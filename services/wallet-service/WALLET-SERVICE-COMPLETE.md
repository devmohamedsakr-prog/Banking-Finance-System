# Wallet & Balance Service - Complete Implementation

**Scale:** 500M+ concurrent wallets | 10M+ balance updates/sec | <10ms latency | 99.999% uptime

## API Specification

```
POST /v1/wallets
  Create user wallet
  Body: { userId, currency, accountType }
  Response: { walletId, balance: 0, createdAt }

GET /v1/wallets/{walletId}
  Get wallet details
  Response: { walletId, userId, balance, currency, holds, available }

POST /v1/wallets/{walletId}/deposit
  Add funds to wallet
  Body: { amount, source, description }
  Response: { transactionId, newBalance, timestamp }

POST /v1/wallets/{walletId}/withdraw
  Withdraw funds from wallet
  Body: { amount, destination, description }
  Response: { transactionId, newBalance, timestamp }

POST /v1/wallets/{walletId}/transfer
  P2P transfer between wallets
  Body: { amount, toWalletId, description, sendNotification }
  Response: { transferId, status, timestamp }

POST /v1/wallets/{walletId}/hold
  Place hold on funds (for pending orders)
  Body: { amount, orderId, reason, expiryMinutes }
  Response: { holdId, availableBalance }

POST /v1/wallets/{walletId}/release-hold
  Release hold and restore available balance
  Body: { holdId }
  Response: { success, availableBalance }

GET /v1/wallets/{walletId}/transactions
  Get transaction history
  Query: { limit, offset, startDate, endDate }
  Response: { transactions[], totalCount }

GET /v1/wallets/{walletId}/balance-history
  Get balance over time
  Query: { period: HOURLY|DAILY|MONTHLY }
  Response: { balancePoints[], trend }
```

## Domain Models

```
Wallet {
  walletId: UUID
  userId: UUID
  currency: String (3-letter code)
  balance: Money (total balance)
  available: Money (balance - holds)
  holds: Money (reserved for pending orders)
  status: WalletStatus
  createdAt: DateTime
  updatedAt: DateTime
}

WalletStatus = ACTIVE | FROZEN | SUSPENDED | CLOSED

Transaction {
  transactionId: UUID
  walletId: UUID
  type: TransactionType
  amount: Money
  currency: String
  relatedWalletId: UUID (for transfers)
  status: TransactionStatus
  description: String
  createdAt: DateTime
  settledAt: DateTime
  metadata: Object
}

TransactionType = DEPOSIT | WITHDRAWAL | TRANSFER_OUT | TRANSFER_IN | REFUND | ADJUSTMENT | INTEREST | FEE

Hold {
  holdId: UUID
  walletId: UUID
  amount: Money
  orderId: UUID
  reason: String
  expiresAt: DateTime
  releasedAt: DateTime (null if active)
  status: HoldStatus
}

HoldStatus = ACTIVE | RELEASED | EXPIRED | FORFEITED

BalanceRecord {
  recordId: UUID
  walletId: UUID
  balance: Money
  holds: Money
  available: Money
  timestamp: DateTime
}
```

## Use Cases

**UC-001: Money Movement**
- User deposits $100 (balance: $100)
- Places hold for order ($50 for pending order)
- Available balance: $50
- Order ships: release hold (available: $100 again)

**UC-002: P2P Transfer**
- User A transfers $50 to User B
- Deduct from A's wallet (atomic)
- Add to B's wallet (atomic)
- Both notifications sent
- <100ms latency

**UC-003: Reconciliation**
- Daily batch: sum all wallet balances
- Must equal total in ledger
- If mismatch: alert operations team
- Investigate and correct

**UC-004: Multi-Currency**
- User holds USD, EUR, GBP
- Transfer EUR to USD (exchange rate applied)
- Rate locked for 60 seconds
- FX markup: 1-2%

## Company Scenarios

**PayPal:**
- 500M+ active wallets
- Instant transfers between users
- Holds for pending orders (3-30 days)
- Daily settlement to bank

**Square Cash:**
- Instant peer-to-peer transfers
- Same-day withdrawal to bank
- Holds for payment orders
- Mobile-first experience

**Revolut:**
- Multi-currency wallets (30+ currencies)
- Real-time balance updates
- Instant transfers between users
- Cryptocurrency integration

## Infrastructure

**Database:**
```sql
CREATE TABLE wallets (
  wallet_id UUID PRIMARY KEY,
  user_id UUID,
  balance DECIMAL(12,2),
  available DECIMAL(12,2),
  currency VARCHAR(3),
  status VARCHAR(20),
  created_at TIMESTAMP
);
CREATE INDEX idx_user_id ON wallets(user_id);

CREATE TABLE transactions (
  transaction_id UUID PRIMARY KEY,
  wallet_id UUID,
  type VARCHAR(20),
  amount DECIMAL(12,2),
  balance_after DECIMAL(12,2),
  created_at TIMESTAMP
);
CREATE INDEX idx_wallet_created ON transactions(wallet_id, created_at);

CREATE TABLE holds (
  hold_id UUID PRIMARY KEY,
  wallet_id UUID,
  amount DECIMAL(12,2),
  order_id UUID,
  expires_at TIMESTAMP,
  status VARCHAR(20)
);
```

**Real-time Requirements:**
- Balance cache in Redis (sub-10ms)
- Event stream (Kafka) for all transactions
- Stream updates to mobile apps (WebSocket)
- Atomic balance updates (transaction lock)

**Consistency Model:**
- Strong consistency for balance
- Eventual consistency for transaction history
- Read-your-write consistency

## Testing

```
Unit Tests:
- Balance calculation
- Hold logic
- Transfer validation
- Currency conversion

Integration Tests:
- Multi-user transfers
- Hold and release
- Balance reconciliation
- Concurrent updates

Stress Tests:
- 10M+ TPS balance updates
- 500M+ concurrent wallets
- <10ms p99 latency
- Zero lost transactions
```

## Monitoring

```
Key Metrics:
- Balance consistency (%)
- Hold accuracy (%)
- Transfer success rate (%)
- Latency p50/p95/p99 (ms)
- Concurrent active wallets
- Daily transaction volume
```

