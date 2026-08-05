# Payment Processing Service - Complete Implementation

**Scale:** 1M+ TPS | $10T+ annual volume | <50ms latency | 99.99% uptime

## API Specification

```
POST /v1/payments/authorize
  Authorize payment (reserve funds)
  Body: { cardToken, amount, currency, merchantId, orderId, 3dsRequired }
  Response: { authId, status: APPROVED|DECLINED, code, message }
  Idempotent: Yes (via idempotency-key header)

POST /v1/payments/capture
  Capture authorized funds
  Body: { authId, captureAmount }
  Response: { transactionId, status: CAPTURED }

POST /v1/payments/void
  Cancel authorization (before capture)
  Body: { authId }
  Response: { success, fundsPending: 0 }

POST /v1/payments/refund
  Refund transaction (after capture)
  Body: { transactionId, refundAmount, reason }
  Response: { refundId, status: REFUND_PENDING }

POST /v1/payments/3ds-verify
  3D Secure verification
  Body: { authId, verificationCode }
  Response: { verified: true|false }

POST /v1/payments/recurring
  Setup recurring payment
  Body: { cardToken, amount, frequency, startDate }
  Response: { subscriptionId, nextChargeDate }

GET /v1/payments/transaction/{txnId}
  Get transaction details
  Response: { txnId, status, amount, timestamp, fee }
```

## Domain Models

```
Payment {
  paymentId: UUID
  transactionId: UUID
  cardToken: String (tokenized)
  amount: Money
  currency: String (3-letter code)
  status: PaymentStatus
  merchantId: UUID
  orderId: UUID
  authorizationCode: String
  authorizationId: UUID
  createdAt: DateTime
  capturedAt: DateTime
  settledAt: DateTime (T+1)
  failureReason: String (optional)
}

PaymentStatus = PENDING | AUTHORIZED | CAPTURED | FAILED | REFUNDED | DISPUTED | CHARGEDBACK | SETTLED

Authorization {
  authId: UUID
  amount: Money
  approvalCode: String
  expiresAt: DateTime (7 days)
  capturedAmount: Money (0 to amount)
  status: AuthStatus (ACTIVE | CAPTURED | VOID | EXPIRED)
}

Refund {
  refundId: UUID
  transactionId: UUID
  refundAmount: Money
  reason: RefundReason
  status: RefundStatus
  initiatedAt: DateTime
  completedAt: DateTime
}

RefundReason = CUSTOMER_REQUEST | DUPLICATE | FRAUDULENT | ORDER_CANCELLED | PARTIAL_RETURN

PaymentMethod {
  methodId: UUID
  type: PaymentMethodType (CREDIT_CARD | DEBIT_CARD | ACH | WALLET | CRYPTO)
  maskedValue: String
  expiryDate: Date (optional)
  holderName: String
}
```

## Use Cases

**UC-001: Complete Payment Flow**
- Authorization (hold funds, <50ms)
- Order processing (1 sec - 24 hours)
- Capture (funds charged)
- Settlement (T+1, funds to merchant)

**UC-002: Payment Failure & Retry**
- Authorization declined (insufficient funds)
- Notify customer
- Retry with alternate method
- Handle timeout/network errors

**UC-003: Recurring Payments**
- Setup subscription (store payment method)
- Auto-charge on schedule
- Retry failed charges (3 attempts)
- Handle subscription pause/cancel

**UC-004: Dispute Handling**
- Customer disputes charge
- Collect evidence
- Submit to processor
- Win/lose determination

## Company Scenarios

**JPMorgan Chase:**
- 1M+ TPS peak capacity
- <20ms authorization (p99)
- Multi-processor routing (redundancy)
- Proprietary risk scoring
- Same-day settlement for corporate

**Stripe:**
- Developer-friendly API
- Instant payouts option (1% fee)
- Webhook notifications (real-time)
- PCI compliance handled by Stripe
- 50+ payment methods

**PayPal:**
- Buyer/seller protection integrated
- Alternative payment methods (wallet, bank)
- Buyer authentication (simple)
- Instant transfer option
- Daily settlement (established sellers)

## Infrastructure

**PCI DSS Compliance:**
- TLS 1.2+ encryption
- Tokenization (never store full card)
- Network segmentation (isolated payment)
- Annual audits (SOC 2 Type II)

**Database:**
```sql
CREATE TABLE payments (
  payment_id UUID PRIMARY KEY,
  card_token VARCHAR(255),
  amount DECIMAL(10,2),
  status VARCHAR(20),
  merchant_id UUID,
  created_at TIMESTAMP,
  captured_at TIMESTAMP,
  settled_at TIMESTAMP
);
CREATE INDEX idx_merchant_created ON payments(merchant_id, created_at);
```

**Processors Supported:**
- Visa/Mastercard (60%)
- American Express (15%)
- Discover (5%)
- ACH/Bank transfer (15%)
- Digital wallets (5%)

## Testing

```
Unit Tests:
- Authorization validation
- Amount verification
- Card token processing
- Refund logic

Integration Tests:
- Full auth → capture → settle flow
- Processor failure handling
- 3D Secure verification
- Refund processing

Load Tests:
- 1M+ TPS sustained
- <50ms latency p99
- Zero transaction loss
- Concurrent processing
```

## Monitoring

```
Key Metrics:
- Authorization success rate (%)
- Transaction latency (ms p50/p95/p99)
- Settlement accuracy (%)
- Chargeback rate (bps)
- System uptime (%)
- Processor availability (%)
```

