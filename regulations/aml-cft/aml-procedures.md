# AML/CFT - Procedures & Implementation

## Customer Due Diligence (CDD)

```
Standard Customers:

1. Identity Verification
   - Government ID (driver's license, passport)
   - ML OCR scan
   - Liveness check

2. Address Verification
   - Utility bill (electricity, water)
   - Or government database cross-check
   - Alternate: Verify during opening

3. Beneficial Ownership (if business)
   - Identify true owners (>25% stake)
   - Verify each beneficial owner
   - Document in BO register

4. Source of Funds
   - Interview customer
   - Request documentation
   - Log answers in file

Enhanced Due Diligence (EDD):

Risk Factors:
- Politically Exposed Person (PEP)
- High-risk country (North Korea, Iran, Syria)
- Business in high-risk sector (gambling, drugs)
- Unusual transaction patterns
- Adverse media (criminal record)

EDD Process:
1. Senior management approval required
2. Gather additional documentation
3. Quarterly re-screening
4. Risk committee review
5. May refuse customer if risk too high
```

## Transaction Monitoring

```
Real-Time Screening:

Algorithm:
1. Transaction occurs
2. Extract: Amount, country, merchant, time
3. Compare to customer profile
4. Calculate risk score (0-100)
5. Decision:
   - 0-30: Approve immediately
   - 30-70: Queue for manual review
   - 70+: Block and investigate

Manual Review Queue:
- Reviewed by 2 analysts
- Assessment: Legitimate? Suspicious?
- If suspicious: Escalate to compliance officer
- If approved: Document and proceed
- Timeline: 24 hours

Examples:
- Structuring: 10 x $9,999 transfers (trying to avoid $10K reporting threshold)
- Rapid velocity: 100 transactions in 1 hour
- Unusual merchant: Arms dealer, money services
```

## Reporting Requirements

```
Suspicious Activity Report (SAR):

When to file:
- Suspected money laundering
- Terrorist financing
- Fraud involving $5K+
- Structuring detected

Timeline: Within 30 days of detection

Contents:
- Customer information
- Description of suspicious activity
- Transactions involved
- Evidence/documentation
- Investigation findings

Retention: 5 years (confidential)
Do NOT notify customer (violates law)

Currency Transaction Report (CTR):

When: Every cash transaction >$10K

Contents:
- Depositor information
- Amount and date
- Type of transaction

Retention: 5 years
```

## Status
✅ Production-ready | Real-time monitoring active
