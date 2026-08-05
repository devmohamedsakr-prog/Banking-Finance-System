# Banking Services - Complete Batch (Services 3-7)

Complete specifications for remaining 5 core banking services.

---

# Service 3: Banking Gateway Service

**Scale:** 100K+ bank connections | <1 second latency | 99.95% availability

## API Specification

```
POST /v1/gateway/ach-transfer
  Initiate ACH transfer to bank account
  Body: { accountNumber, routingNumber, amount, description }
  Response: { transferId, status: PENDING, estimatedArrival }

POST /v1/gateway/wire-transfer
  Send wire transfer (SWIFT)
  Body: { beneficiaryBank, beneficiaryAccount, amount, purpose }
  Response: { wireId, status: PENDING_APPROVAL, amount, fee }

POST /v1/gateway/account-link
  Link external bank account (Plaid integration)
  Body: { publicToken, accountType }
  Response: { linkedAccountId, accountMasked }

GET /v1/gateway/balance/{linkedAccountId}
  Get balance of linked bank account
  Response: { balance, currency, lastUpdated }

POST /v1/gateway/recurring-transfer
  Setup automatic transfers on schedule
  Body: { linkedAccountId, amount, frequency, startDate }
  Response: { recurringId, nextTransferDate }
```

## Domain Models

```
ExternalBankAccount {
  accountId: UUID
  userId: UUID
  accountNumber: String (masked)
  routingNumber: String
  bankName: String
  accountType: BankAccountType (CHECKING | SAVINGS)
  status: AccountStatus
  linkedAt: DateTime
  verifiedAt: DateTime
}

Transfer {
  transferId: UUID
  fromWallet: UUID
  toBankAccount: UUID
  amount: Money
  status: TransferStatus
  initiatedAt: DateTime
  settledAt: DateTime (T+0 for wire, T+1-2 for ACH)
  fee: Money
}

TransferStatus = PENDING | APPROVED | PROCESSING | SETTLED | FAILED | CANCELLED
```

## Use Cases

**UC-001: Link Bank Account (Plaid)**
- User initiates bank link
- Redirected to Plaid UI
- Authenticate with bank
- Account linked automatically
- Micro-deposits for verification (optional)

**UC-002: ACH Transfer**
- User initiates $1000 transfer out
- Verification (Plaid confirms account)
- Schedule for next business day (T+1)
- Funds deducted from wallet
- ACH file generated
- Settled next day

**UC-003: Wire Transfer**
- User initiates international wire
- Compliance checks (AML, sanctions)
- Manager approval (if >$10K)
- SWIFT message sent
- Beneficiary bank receives in 2-4 hours
- Fee charged to wallet

## Infrastructure

**Integrations:**
- Plaid (bank account linking)
- FedACH (ACH submissions)
- SWIFT (wire transfers)
- Compliance API (AML/sanctions)

**Database:**
```sql
CREATE TABLE external_accounts (
  account_id UUID PRIMARY KEY,
  user_id UUID,
  account_number VARCHAR(255),
  routing_number VARCHAR(10),
  bank_name VARCHAR(255),
  linked_at TIMESTAMP
);

CREATE TABLE transfers (
  transfer_id UUID PRIMARY KEY,
  wallet_id UUID,
  external_account_id UUID,
  amount DECIMAL(12,2),
  status VARCHAR(20),
  initiated_at TIMESTAMP,
  settled_at TIMESTAMP
);
```

---

# Service 4: Lending & Credit Service

**Scale:** $10T+ portfolio | 100M+ active loans | 1M+ loan applications/day

## API Specification

```
POST /v1/lending/apply
  Submit loan application
  Body: { applicantId, loanAmount, term, loanType, purpose }
  Response: { applicationId, status: PENDING, estimatedDecision }

GET /v1/lending/application/{appId}
  Get application status
  Response: { applicationId, status, estimatedDecision, terms }

POST /v1/lending/verify-income
  Upload income verification (W-2, pay stubs, tax returns)
  Body: { applicationId, documents[] }
  Response: { verified: true|false, verifiedAmount }

POST /v1/lending/accept-terms
  Accept loan offer
  Body: { applicationId, acceptTOU: true }
  Response: { loanId, disbursingAt, firstPaymentDate }

POST /v1/lending/disburse
  Fund approved loan
  Body: { loanId }
  Response: { disbursementId, amount, fundingMethod }

GET /v1/lending/loan/{loanId}
  Get active loan details
  Response: { loanId, balance, rate, nextPaymentDate, totalPaid }

POST /v1/lending/payment
  Make loan payment
  Body: { loanId, amount }
  Response: { paymentId, newBalance, nextPaymentDate }

POST /v1/lending/bnpl-checkout
  Buy Now Pay Later at checkout
  Body: { orderId, amount, selectedPlan }
  Response: { bnplId, installments[], startDate }
```

## Domain Models

```
LoanApplication {
  applicationId: UUID
  applicantId: UUID
  loanAmount: Money
  term: Integer (in months)
  loanType: LoanType (PERSONAL | BUSINESS | MORTGAGE | BNPL)
  purpose: String
  status: ApplicationStatus
  score: Integer (300-850 FICO)
  approvalDecision: ApprovalDecision
  createdAt: DateTime
  decidedAt: DateTime
}

Loan {
  loanId: UUID
  applicantId: UUID
  originalAmount: Money
  currentBalance: Money
  rate: Decimal (APR %)
  term: Integer (months)
  status: LoanStatus
  nextPaymentDate: DateTime
  originalTerm: Integer
  originalStartDate: DateTime
}

LoanStatus = ACTIVE | DELINQUENT (30/60/90 days) | DEFAULT | PAID_OFF | FORECLOSURE

Payment {
  paymentId: UUID
  loanId: UUID
  amount: Money
  principal: Money
  interest: Money
  status: PaymentStatus
  scheduledDate: DateTime
  actualDate: DateTime
}
```

## Use Cases

**UC-001: Loan Application**
- User applies for $10,000 personal loan
- ML model scores instantly (300-850)
- Income verified via Plaid
- Decision in <5 minutes
- Offer: $10K at 12% APR for 36 months ($332/month)

**UC-002: Loan Disbursement**
- User accepts terms
- Funds immediately available in wallet
- Can transfer to bank via ACH
- First payment due in 30 days

**UC-003: BNPL Checkout**
- User selects $600 item (4 payments)
- Payment plan: $150/month
- Instantly approved
- First payment at checkout, 3 more follow

## Infrastructure

**ML Model:**
- Income verification (Plaid data)
- Credit score (Equifax API)
- Alternative data (utility payments, phone records)
- Fraud detection (device, location)

**Compliance:**
- Fair Lending Act (prevent discrimination)
- TILA (Truth in Lending disclosure)
- ECOA (Equal Credit Opportunity)
- FCRA (Fair Credit Reporting)

---

# Service 5: Financial Settlement Service

**Scale:** $10T+ daily volume | T+0 or T+1 settlement | 100% reconciliation

## API Specification

```
POST /v1/settlement/schedule
  Schedule payment settlement
  Body: { merchantId, amount, settlementDate }
  Response: { settlementId, status: SCHEDULED, expectedAmount }

GET /v1/settlement/{settlementId}
  Get settlement status
  Response: { settlementId, amount, status, settledAt }

POST /v1/settlement/batch
  Process daily batch settlement
  Body: { settlementDate }
  Response: { batchId, totalAmount, count, status: PROCESSING }

POST /v1/settlement/reconcile
  Reconcile settlements vs. actual transactions
  Body: { batchId }
  Response: { reconciled: true|false, discrepancies[] }

GET /v1/settlement/report/{reportId}
  Get settlement report (daily/weekly/monthly)
  Response: { reportId, totalSettled, settlementsByMerchant[], period }
```

## Domain Models

```
Settlement {
  settlementId: UUID
  merchantId: UUID
  amount: Money
  status: SettlementStatus
  settlementDate: Date (T+0 or T+1)
  bankAccount: BankAccountDetails
  fee: Money (0.5-1%)
  netAmount: Money (amount - fee)
}

SettlementStatus = SCHEDULED | PENDING_BATCH | PROCESSING | CONFIRMED | SETTLED | FAILED

SettlementBatch {
  batchId: UUID
  settlementDate: Date
  totalAmount: Money
  settlementCount: Integer
  status: BatchStatus
  createdAt: DateTime
  processedAt: DateTime
}

SettlementReport {
  reportId: UUID
  period: ReportPeriod (DAILY | WEEKLY | MONTHLY)
  startDate: DateTime
  endDate: DateTime
  totalSettled: Money
  settlementsByMerchant: Map<MerchantId, SettlementAmount>
}
```

## Use Cases

**UC-001: Daily Batch Settlement**
- End of day: collect all transactions
- Group by merchant (100K+ merchants)
- Calculate fees (2-3% platform fee)
- Prepare batch file
- Submit to ACH network
- T+1: funds appear in merchant accounts

**UC-002: Instant Settlement**
- Premium merchants (1% fee)
- Settlements within 2 hours
- Real-time transfer
- Higher cost but cash flow benefit

**UC-003: Reconciliation**
- Expected vs. actual transactions
- Detect discrepancies
- Generate adjustment entries
- Audit trail for 7 years

## Infrastructure

**Batch Processing:**
- 100K+ merchant settlements
- Parallel processing (Spark clusters)
- FTP/SFTP delivery
- Retry logic (3 attempts)

**Ledger System:**
- Double-entry accounting
- Immutable transaction log
- Real-time balance verification
- 7-year retention

---

# Service 6: Risk & Compliance Service

**Scale:** Enterprise-grade | 100% regulatory coverage | Real-time detection

## API Specification

```
POST /v1/compliance/kyc-verify
  Verify customer identity (KYC)
  Body: { userId, fullName, dob, ssn, address }
  Response: { kycId, verified: true|false, riskLevel }

POST /v1/compliance/aml-check
  Check against sanctions lists (OFAC)
  Body: { userId, fullName }
  Response: { screeningId, matched: true|false, matches[] }

POST /v1/compliance/scoring
  Calculate risk score for transaction
  Body: { transactionAmount, userId, merchantId, location }
  Response: { riskScore: 0-100, recommendation: APPROVE|DECLINE|REVIEW }

POST /v1/compliance/suspicious-activity
  Report suspicious activity (SAR)
  Body: { userId, description, evidence[] }
  Response: { sarId, status: FILED }

GET /v1/compliance/user/{userId}
  Get customer risk profile
  Response: { userId, kycStatus, amlStatus, riskLevel, lastReview }
```

## Domain Models

```
KycVerification {
  kycId: UUID
  userId: UUID
  fullName: String
  dob: Date
  ssn: String (masked)
  address: Address
  status: VerificationStatus
  riskLevel: RiskLevel
  verifiedAt: DateTime
  expiresAt: DateTime (annually)
}

VerificationStatus = PENDING | VERIFIED | FAILED | EXPIRED

RiskLevel = LOW | MEDIUM | HIGH

AmlScreening {
  screeningId: UUID
  userId: UUID
  matched: Boolean
  matches: List<SanctionMatch>
  screened: DateTime
}

SuspiciousActivityReport {
  sarId: UUID
  userId: UUID
  description: String
  evidence: List<Evidence>
  filed: DateTime
  filedTo: Regulator
}
```

## Use Cases

**UC-001: Customer Onboarding**
- New user registration
- KYC verification (2-3 minutes)
- AML screening (real-time)
- If approved: account active
- If flagged: manual review (24 hours)

**UC-002: Transaction Monitoring**
- Every transaction scored (0-100)
- Score >75: manual review
- Score >90: decline automatically
- Alerts to compliance team

**UC-003: Sanctions Compliance**
- Hourly updates from OFAC
- Customer list screening
- Transaction originator/beneficiary check
- Hit report to compliance

## Infrastructure

**Regulatory Data:**
- OFAC Specially Designated Nationals (SDN)
- BIS Entity List
- EU sanctions list
- Country-specific lists

**ML Risk Scoring:**
- Transaction amount
- User history
- Device/location
- Time of day
- Merchant category

---

# Service 7: Financial Analytics Service

**Scale:** Real-time dashboards | 1B+ events/day | <1 second query latency

## API Specification

```
GET /v1/analytics/dashboard/overview
  Get executive dashboard
  Query: { period: TODAY|WEEK|MONTH|YEAR }
  Response: { totalVolume, totalUsers, activeAccounts, platformFees, chartData }

GET /v1/analytics/dashboard/merchant
  Get merchant-specific analytics
  Query: { merchantId, period }
  Response: { totalTransactions, totalVolume, avgTransactionSize, trends }

GET /v1/analytics/dashboard/risk
  Get risk metrics dashboard
  Query: { period }
  Response: { fraudRate, chargebackRate, failureRate, riskTrends }

GET /v1/analytics/custom-report
  Generate custom report
  Query: { startDate, endDate, dimensions, metrics }
  Response: { reportId, data, format }

GET /v1/analytics/real-time
  Get real-time metrics (last 5 minutes)
  Response: { currentTps, avgLatency, activeUsers, errorRate }
```

## Domain Models

```
TransactionMetric {
  timestamp: DateTime
  transactionCount: Long
  totalVolume: Money
  avgAmount: Money
  successRate: Decimal
  fraudRate: Decimal
  chargebackRate: Decimal
}

UserMetric {
  timestamp: DateTime
  newUsers: Long
  activeUsers: Long
  totalWallets: Long
  avgWalletBalance: Money
}

PerformanceMetric {
  timestamp: DateTime
  avgLatency: Long (ms)
  p95Latency: Long
  p99Latency: Long
  errorRate: Decimal
  uptime: Decimal
}

CustomReport {
  reportId: UUID
  createdAt: DateTime
  startDate: DateTime
  endDate: DateTime
  dimensions: List<String>
  metrics: List<String>
  data: TimeSeriesData
}
```

## Use Cases

**UC-001: Executive Dashboard**
- Real-time volume ($10M/hour peak)
- Active users (5M concurrent)
- Revenue ($5M/day platform fees)
- Trend indicators (↑↓ vs. yesterday)

**UC-002: Merchant Reporting**
- Daily settlement report
- Monthly revenue breakdown
- Fraud/chargeback metrics
- Downloadable (PDF/CSV)

**UC-003: Risk Dashboard**
- Fraud rate (0.1-0.5%)
- Chargeback rate (0.05-0.1%)
- Failure rate (0.5-1%)
- Geographic heatmaps

## Infrastructure

**Data Pipeline:**
- Kafka streams (1B+ events/day)
- ClickHouse (time-series DB)
- Elasticsearch (log analytics)
- Grafana dashboards

**Reporting:**
- Scheduled reports (daily/weekly/monthly)
- On-demand custom reports
- Export to S3 (data lake)
- Tableau/Looker integration

---

## Summary

**All 7 Services Complete:**

| Service | TPS | Users | Scale |
|---------|-----|-------|-------|
| 1. Payment | 1M+ | - | $10T+ annual |
| 2. Wallet | 10M+ | 500M+ | 500B+ transactions |
| 3. Gateway | 100K+ | - | 100K+ banks |
| 4. Lending | - | 100M+ | $10T+ portfolio |
| 5. Settlement | - | 100K+ | $10T+ daily |
| 6. Risk | - | 1B+ | Real-time |
| 7. Analytics | - | - | 1B+ events/day |

**Ready to extract:** Create individual files for each service from this batch.

