# Banking-System: Integration Guide

**Version:** 2.0 | **Status:** Production | **Updated:** 2026-08-05

---

## Third-Party Integrations

### Payment Networks

**Visa/Mastercard:**
- Settlement: Daily batch
- Volume: $5T+ annually
- Integration: Proprietary API
- Fees: 0.3-0.5% per transaction

**American Express:**
- Settlement: Daily batch
- Volume: $500B+ annually
- Integration: Proprietary API
- Fees: 0.6-0.8% per transaction

### Bank Connections

**Plaid (Account Linking):**
- Status: 12K+ financial institutions
- Latency: <2 seconds
- Accuracy: 99.9%
- Endpoint: https://api.plaid.com

**Stripe (Payment Processing):**
- Status: 190+ countries supported
- Latency: <100ms
- Accuracy: 99.99%
- Endpoint: https://api.stripe.com

### Lending Integrations

**Equifax (Credit Scoring):**
- API: Real Integrations Score
- Response: <1 second
- Score: 300-850 FICO
- Cost: $5-10 per inquiry

**LexisNexis (Verification):**
- API: Identity verification
- Response: <500ms
- Coverage: 99% of US adults
- Cost: $2-3 per check

### Compliance APIs

**OFAC Screening:**
- SDN List: Updated daily
- Match Confidence: 0-100%
- Threshold: Alert if >85%
- Cost: Included in bank license

**AML Monitoring:**
- Real-time transaction screening
- Multiple data sources
- Threshold: Auto-flag if >75%
- Cost: $100K+/year

---

## API Integration Patterns

### Payment Processing Flow

```
1. Customer initiates payment
   ↓
2. Call Payment Service API
   POST /v1/payments
   {
     amount: 100.00,
     currency: "USD",
     merchant_id: "abc123",
     customer_id: "xyz789"
   }
   ↓
3. Payment Service validates
   ├─ Amount > 0? ✓
   ├─ Account active? ✓
   ├─ Sufficient funds? ✓
   └─ AML/fraud checks? ✓
   ↓
4. Route to payment network
   ├─ Visa/Mastercard/Amex
   ├─ Route optimization
   └─ Real-time decision
   ↓
5. Response
   {
     payment_id: "pay_123",
     status: "completed",
     amount: 100.00,
     timestamp: "2024-01-15T10:30:00Z"
   }
```

### Account Linking (Plaid)

```
1. User clicks "Link Account"
   ↓
2. Redirect to Plaid Link UI
   ↓
3. User authenticates with bank
   ↓
4. Plaid returns public_token
   ↓
5. Exchange token for access_token
   POST /account_identity/match
   {
     public_token: "public-sandbox-...",
     user_id: "user_123"
   }
   ↓
6. Store access_token
   ↓
7. Account linked ✓
```

### Loan Application (Equifax)

```
1. Customer applies for loan
   ↓
2. Collect application data
   ↓
3. Pull credit score
   POST https://equifax.api.com/scoring
   {
     ssn: "123-45-6789",
     first_name: "John",
     last_name: "Doe",
     dob: "1990-01-01"
   }
   ↓
4. Response
   {
     score: 720,
     factors: ["Payment history: Good"]
   }
   ↓
5. ML model scores
   ↓
6. Decision: APPROVED, APR: 12%
```

---

## Partner APIs

### 100+ Banking Partners

**Connection Types:**

FIX Protocol:
- Protocol: Financial Information eXchange
- Use: Stock trading, derivatives
- Latency: <10ms
- Volume: 1M+ messages/day

REST APIs:
- Format: JSON
- Auth: OAuth 2.0
- Rate Limit: 1000 req/min
- Responses: <200ms

SFTP:
- Protocol: Secure File Transfer
- Use: Batch settlements
- Frequency: Daily at midnight
- Encryption: AES-256

---

## Webhook Integration

### Inbound Webhooks (Receiving Events)

```
Event: Settlement Completed

POST https://banking-api.prod/webhooks/settlement
{
  "event_id": "evt_123",
  "event_type": "settlement.completed",
  "timestamp": "2024-01-15T23:00:00Z",
  "data": {
    "settlement_id": "sett_456",
    "amount": 1000000,
    "merchant_count": 5000,
    "status": "completed"
  }
}

Response:
{
  "status": "received",
  "webhook_id": "wh_789"
}
```

### Outbound Webhooks (Sending Events)

**Supported Events:**
- payment.completed
- payment.failed
- settlement.scheduled
- settlement.completed
- account.created
- transfer.settled

**Retry Logic:**
```
Attempt 1: Immediate
Attempt 2: +5 minutes
Attempt 3: +30 minutes
Attempt 4: +2 hours
Attempt 5: +24 hours
Give up after 5 attempts
```

---

## Rate Limiting & Throttling

### API Rate Limits

```
Tier 1 (Free):
  Requests: 100/minute
  Burst: 200
  Cost: $0

Tier 2 (Pro):
  Requests: 1000/minute
  Burst: 2000
  Cost: $500/month

Tier 3 (Enterprise):
  Requests: Unlimited
  Burst: Custom
  Cost: Custom SLA

Response Headers:
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 847
X-RateLimit-Reset: 1693598400
```

---

## Error Handling

### API Error Codes

```
Success:
  200 OK
  201 Created

Client Errors:
  400 Bad Request
  401 Unauthorized
  403 Forbidden
  404 Not Found
  429 Too Many Requests

Server Errors:
  500 Internal Server Error
  503 Service Unavailable

Banking-Specific:
  2001 Insufficient Funds
  2002 Account Suspended
  2003 Daily Limit Exceeded
  2004 Invalid Account
```

---

## Data Synchronization

### Real-time Sync (Kafka)

```
Balance Update Flow:

Payment Service → Kafka Topic
    ↓ (Topic: payment.completed)
├─ Wallet Service (subscribes)
├─ Analytics Service (subscribes)
├─ Settlement Service (subscribes)
└─ Ledger Service (subscribes)
```

### Batch Sync (SFTP/S3)

```
Daily Settlement Reconciliation:

1. Generate settlement file
   settlement_YYYYMMDD.csv

2. Upload to partner SFTP
   sftp://partner.bank.com/in/

3. Partner processes

4. Download confirmation
   sftp://partner.bank.com/out/

5. Reconcile in system
```

---

## Partner Testing

### Sandbox Environment

```
URL: https://sandbox.banking-api.io
Credentials: Separate from production

Test Cards:
  4242424242424242 → Success
  4000000000000002 → Decline
  4111111111111111 → 3D Secure

Test Amounts:
  $1.00-$99.99 → Use as is
  $100.00+ → 3D Secure required
```

### Pre-Production Testing

- Test all happy paths
- Test error scenarios
- Load testing (10K concurrent)
- Failover testing
- Rollback procedures

---

## Monitoring Integrations

### Partner Health Checks

```
Plaid Status:
  Last check: 2 minutes ago
  Status: ✓ UP
  Response time: 234ms
  
Equifax Status:
  Last check: 5 minutes ago
  Status: ✓ UP
  Response time: 456ms

Stripe Status:
  Last check: 1 minute ago
  Status: ✓ UP
  Response time: 89ms
```

---

## Status

✅ Production-ready | 100+ partners connected | Real-time sync
