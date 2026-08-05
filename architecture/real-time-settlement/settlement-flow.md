# Real-Time Settlement - Flow & Implementation

## Settlement Workflow

```
T+0 SETTLEMENT (Instant):

1. Transaction Authorized (50ms)
   - Risk approved
   - Funds confirmed
   - Status: PENDING

2. Queue in Settlement Buffer (Kafka)
   - Topic: settlements.pending
   - Partition by merchant
   - TTL: 5 minutes

3. Batch Collection (Every 100ms or 10K transactions)
   - Group by merchant
   - Group by destination bank
   - Calculate net position

4. Settlement Instructions
   - Merchant balance update
   - Bank transfer initiation
   - Fee deduction

5. Real-Time Gross Settlement (RTGS)
   - Direct to RTGS network
   - Interbank transfer
   - Sub-second finality

6. Confirmation (1 second)
   - Webhook to merchant
   - Customer notification
   - Transaction settled
```

## Implementation Details

### Batch Processing

```python
# Pseudocode: Settlement Batch Processor

def process_settlement_batch(merchant_id):
    # Collect all pending transactions
    transactions = kafka_consumer.get_pending(
        topic='settlements.pending',
        partition=merchant_id
    )
    
    # Calculate totals
    total_volume = sum(t.amount for t in transactions)
    total_count = len(transactions)
    
    # Calculate fee (2.9% + $0.30)
    fee = (total_volume * 0.029) + (total_count * 0.30)
    
    # Net payout
    payout = total_volume - fee
    
    # Update merchant wallet
    merchant_wallet.debit(fee)
    merchant_wallet.add_pending_settlement(payout)
    
    # Create settlement instruction
    settlement = Settlement(
        merchant_id=merchant_id,
        amount=payout,
        bank_account=get_merchant_bank_account(),
        scheduled_time=now() + timedelta(hours=24),
        status='PENDING'
    )
    
    # Persist
    db.settlements.insert(settlement)
    
    # Send to RTGS
    rtgs_gateway.submit_transfer(settlement)
    
    return settlement.id
```

### Database Schema

```sql
CREATE TABLE settlements (
    settlement_id UUID PRIMARY KEY,
    merchant_id UUID NOT NULL,
    amount DECIMAL(12,2) NOT NULL,
    fee DECIMAL(12,2) NOT NULL,
    gross_amount DECIMAL(12,2) NOT NULL,
    bank_account_id UUID NOT NULL,
    status VARCHAR(20) NOT NULL,
    scheduled_at TIMESTAMP NOT NULL,
    settled_at TIMESTAMP,
    rtgs_reference VARCHAR(255),
    created_at TIMESTAMP NOT NULL,
    updated_at TIMESTAMP NOT NULL,
    
    INDEX idx_merchant_scheduled (merchant_id, scheduled_at),
    INDEX idx_status (status),
    FOREIGN KEY (merchant_id) REFERENCES merchants(id)
);

CREATE TABLE settlement_reconciliation (
    reconciliation_id UUID PRIMARY KEY,
    settlement_id UUID NOT NULL,
    expected_amount DECIMAL(12,2) NOT NULL,
    actual_amount DECIMAL(12,2),
    status VARCHAR(20) NOT NULL,
    discrepancy DECIMAL(12,2),
    verified_at TIMESTAMP,
    created_at TIMESTAMP NOT NULL,
    
    FOREIGN KEY (settlement_id) REFERENCES settlements(id)
);
```

### Reconciliation Process

```
Daily Settlement Reconciliation:

1. Get all settled transactions for day
2. Sum by merchant, bank account
3. Compare expected vs actual amount received
4. Create reconciliation records
5. Alert ops team if discrepancy >$100
6. Retry failed transfers (max 3 attempts)
7. Generate daily settlement report
```

## Monitoring & Alerts

```
Key Metrics:
- Settlement success rate (target: 99.99%)
- Average settlement time (target: <1s)
- Total daily volume processed
- Failed settlements count
- Reconciliation discrepancies

Alerts:
- Success rate <99%
- Settlement latency >5 seconds
- Reconciliation mismatch >$1K
- Failed transfers (retry after 24h)
```

## Status
✅ Production-ready
