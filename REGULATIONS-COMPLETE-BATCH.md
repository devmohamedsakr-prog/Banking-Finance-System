# Regulatory Frameworks - Complete Batch (12 Frameworks)

Complete compliance requirements for enterprise banking systems.

---

# Framework 1: PCI DSS 4.0

**Scope:** Payment Card Industry Data Security Standard | **Requirement Status:** Mandatory for all card processors

## 12 Core Requirements

### Requirement 1: Firewall Configuration
- Establish firewall standards
- Prohibit direct internet access to systems
- Network segmentation
- Documentation of firewall rules

### Requirement 2: Remove Default Passwords
- Change vendor-supplied defaults
- Disable unused accounts
- Remove unnecessary services
- Document all changes

### Requirement 3: Protect Stored Data
- Encryption: AES-256 (at rest)
- Key rotation: Every 90 days
- No storage of full card numbers
- Tokenization mandatory

### Requirement 4: Protect Data in Transit
- TLS 1.2+ for all communications
- Certificate validation
- End-to-end encryption
- Test for weak ciphers quarterly

### Requirement 5: Protect Against Malware
- Antivirus software (all systems)
- Regular updates/patches
- Malware scanning weekly
- Log all malware incidents

### Requirement 6: Secure Development
- Security testing in development
- Code reviews before production
- Change management process
- Separation of dev/test/prod

### Requirement 7: Restrict Access by Business Need
- Role-based access control (RBAC)
- Principle of least privilege
- User IDs for all access
- Quarterly access reviews

### Requirement 8: Assign Unique User IDs
- Unique ID per user
- Disable inactive IDs (>90 days)
- Log all access attempts
- Monitor for suspicious activity

### Requirement 9: Restrict Physical Access
- Badge access systems
- Visitor logs
- Surveillance cameras
- Secure disposal of media

### Requirement 10: Track and Monitor Access
- Log all access to cardholder data
- Track administrative actions
- Monitor for unauthorized access
- Centralized log management

### Requirement 11: Test Security Systems
- Annual penetration testing
- Quarterly vulnerability scans
- Document all tests
- Remediate findings

### Requirement 12: Maintain Security Policy
- Written information security policy
- Annual review and update
- Employee training (annual)
- Incident response plan

## Compliance Checklist

```
Implementation:
[ ] Data encryption (AES-256 at rest, TLS 1.2+ in transit)
[ ] Tokenization (never store full card)
[ ] Access control (RBAC, least privilege)
[ ] Audit logging (all access to cardholder data)
[ ] Firewall configuration (network segmentation)
[ ] Antivirus/malware protection
[ ] Change management process
[ ] Incident response plan
[ ] Annual penetration testing
[ ] Quarterly vulnerability scans
[ ] Employee security training
[ ] Written security policies
```

## Non-Compliance Penalties

- **Level 1:** $5,000-$100,000 per month
- **Level 2:** $100-$1,000 per month
- **Level 3:** $500-$5,000 per month
- **Card ban:** Merchants can be banned from processing

---

# Framework 2: SOC 2 Type II

**Scope:** System and Organization Controls | **Requirement Status:** Annually audited

## 5 Trust Principles

### CC (Security)
- Logical access controls
- Encryption implementation
- Intrusion detection/prevention
- Data classification

### A (Availability)
- System uptime (99.99%+)
- Disaster recovery plan
- Backup and recovery
- Capacity planning

### C (Confidentiality)
- Data classification
- Access restrictions
- Encryption (data at rest)
- Secure disposal

### PI (Processing Integrity)
- Transaction accuracy
- Complete transaction processing
- Timely transaction recording
- System output authorization

### PR (Privacy)
- Privacy policy
- Customer data protection
- Data retention policies
- GDPR/CCPA compliance

## Audit Requirements

```
Annual SOC 2 Type II Audit:

1. Planning & Risk Assessment
   - Identify relevant systems
   - Map controls
   - Document control objectives

2. Testing Period (6+ months)
   - Observe controls in operation
   - Test control effectiveness
   - Document exceptions

3. Report Generation
   - Management assertion
   - Auditor opinion
   - Control effectiveness statement
   - 15-20 page report

4. Distribution
   - Send to customers (on demand)
   - Send to regulators
   - Board presentation
   - Annual public disclosure
```

## Annual Cost

- **SOC 2 Audit:** $50K-$150K per audit
- **Remediation:** $100K-$500K (depends on findings)
- **Staffing:** 1-2 compliance officers (full-time)

---

# Framework 3: GDPR (General Data Protection Regulation)

**Scope:** EU data protection | **Coverage:** 500M+ EU citizens

## 7 Key Principles

### 1. Lawfulness, Fairness, Transparency
- Process data lawfully
- Disclose processing to users
- Obtain explicit consent

### 2. Purpose Limitation
- Collect for specified purpose
- Cannot use for other purposes without consent
- Document collection basis

### 3. Data Minimization
- Only collect necessary data
- Delete data when no longer needed
- Minimize collection scope

### 4. Accuracy
- Keep data accurate and current
- Allow users to request corrections
- Remove inaccurate data

### 5. Storage Limitation
- Retain data only as long as needed
- Implement retention schedules
- Secure deletion after retention period

### 6. Integrity & Confidentiality
- Implement security measures (encryption)
- Prevent unauthorized access
- Conduct security assessments

### 7. Accountability
- Document compliance efforts
- Maintain audit trails
- Report breaches within 72 hours

## Specific Requirements

**Data Subject Rights:**
- Right to access (copy of personal data)
- Right to rectification (correct data)
- Right to erasure ("right to be forgotten")
- Right to data portability (export data)
- Right to object (stop processing)

**Privacy by Design:**
- Embed privacy in systems from start
- Privacy impact assessments (PIA)
- Pseudonymization and encryption
- Regular testing and monitoring

**Data Processing Agreements:**
- Data Processor Agreement (DPA) required
- Standard contractual clauses (SCCs)
- Binding corporate rules (BCRs)
- Third-party audits (SOC 2, ISO 27001)

## Enforcement & Penalties

- **Fines:** Up to €20M or 4% of annual revenue (whichever is higher)
- **Examples:** 
  - Facebook: €60M (inadequate data protection)
  - Amazon: €746M (improper data transfer)
  - Meta: €405M (excessive data collection)

---

# Framework 4: GLBA (Gramm-Leach-Bliley Act)

**Scope:** US financial privacy law | **Applies to:** Banks, insurance, securities firms

## 3 Key Requirements

### 1. Financial Privacy Rule
- Provide privacy notice to customers
- Allow opt-out of information sharing
- Disclose third-party sharing
- Maintain security

### 2. Safeguards Rule
- Implement administrative safeguards
- Physical security (access controls)
- Technical security (encryption, firewalls)
- Operational security (training, incident response)

### 3. Disposal Rule
- Dispose of customer information securely
- Shred paper documents
- Securely delete electronic data
- Maintain disposal records

## Privacy Notice Requirements

Must disclose:
- What information is collected
- How information is used
- Who information is shared with
- Customer rights
- Security practices

## Penalties for Non-Compliance

- **Civil penalties:** Up to $100,000 per violation
- **Criminal penalties:** Up to 5 years prison for willful violation
- **Examples:**
  - Wells Fargo: $3B (unauthorized accounts)
  - Capital One: $700M (data breach involving 100M customers)

---

# Framework 5: TILA (Truth in Lending Act)

**Scope:** Consumer credit disclosures | **Applies to:** Loans, credit cards, BNPL

## Required Disclosures

**Before Consumer Commits to Loan:**

1. **Annual Percentage Rate (APR)**
   - Must include all fees/costs
   - Compare loans on APR basis
   - Tolerance: ±0.125% for variable rates

2. **Finance Charge**
   - All charges for extending credit
   - Interest + fees
   - Calculated clearly

3. **Amount Financed**
   - Principal borrowed
   - Excluding fees and insurance

4. **Payment Schedule**
   - Number and amount of payments
   - Payment due dates
   - Total payments

5. **Right to Rescind**
   - 3-day right to cancel (certain transactions)
   - Full refund of fees
   - Exception: Home purchase (no rescission)

## Example Disclosures

**Personal Loan:**
- Amount: $10,000
- Term: 36 months
- APR: 12.5%
- Monthly Payment: $333.33
- Total Finance Charge: $2,000
- Total Amount Paid: $12,000

**Credit Card:**
- APR: 18%
- Annual Fee: $95
- Grace Period: 25 days (purchases only)
- Late Fee: $35
- Balance Transfer APR: 21%

## Penalties

- **Civil liability:** Up to $5,000 per violation + actual damages
- **Criminal penalties:** Up to 5 years prison for willful violations
- **Class action:** Consumers can sue in groups

---

# Framework 6: FCRA (Fair Credit Reporting Act)

**Scope:** Credit reporting and consumer data | **Applies to:** Credit bureaus, data brokers, employers

## Key Provisions

### 1. Accuracy & Dispute Resolution
- Credit bureaus must maintain accurate records
- Consumers can dispute inaccurate information
- Bureau must investigate within 30 days
- Delete unverifiable items

### 2. Consumer Rights
- Access to credit report (free annually)
- Know reasons for credit denial
- Know adverse information in file
- Dispute and correct errors

### 3. Employer Compliance
- Cannot use credit reports for hiring without consent
- Must disclose use of credit information
- Comply with adverse action procedures

### 4. Data Security
- Implement safeguards against unauthorized access
- Encrypt sensitive data
- Monitor for breaches
- Notify consumers of breaches

### 5. Fraud Alerts & Credit Freezes
- Place fraud alert (7 years)
- Extended fraud alert for identity theft (7 years)
- Credit freeze (unlimited, consumer can temporarily thaw)

## Penalties

- **Civil liability:** $100-$1,000 per violation
- **Class actions:** Consumers can sue collectively
- **Examples:**
  - Equifax: $700M (2017 breach affecting 147M people)
  - TransUnion: $100M (inaccurate credit reporting)

---

# Framework 7: AML/CFT (Anti-Money Laundering / Counter-Terrorism Financing)

**Scope:** Financial crime prevention | **Applies to:** All financial institutions

## 4-Step AML Process

### 1. Know Your Customer (KYC)
- Identity verification (government ID)
- Address verification
- Beneficial ownership identification
- Source of funds verification
- Risk assessment

### 2. Customer Due Diligence (CDD)
- Enhanced for high-risk customers
- Politically Exposed Persons (PEPs)
- Sanctioned jurisdictions
- Ongoing monitoring (quarterly minimum)

### 3. Transaction Monitoring
- Real-time screening
- Threshold reporting ($10K+)
- Pattern-based detection
- Suspicious Activity Reports (SARs)

### 4. Reporting
- Suspicious Activity Report (SAR)
- File within 30 days of detection
- Currency Transaction Report (CTR) for $10K+
- Annual AML audit

## Red Flags

```
Suspicious Indicators:

Customer Behavior:
- Sudden large deposits/withdrawals
- Deposits from multiple sources
- Transfers to high-risk jurisdictions
- Structuring (multiple <$10K transactions)
- Inconsistent with stated business

Transaction Characteristics:
- Round dollar amounts ($100K, $1M)
- Rapid movement in/out
- No apparent economic purpose
- Cross-border without rationale
- Cryptocurrency to fiat conversion
```

## Penalties

- **Civil penalties:** Up to $100,000 per violation
- **Criminal penalties:** Up to 10 years prison
- **Examples:**
  - HSBC: $1.9B (AML violations)
  - Standard Chartered: $639M (sanctions violations)

---

# Framework 8: Sanctions Compliance (OFAC)

**Scope:** Prevent dealings with designated parties | **Enforcer:** US Department of Treasury

## Screening Requirements

### Specially Designated Nationals (SDN) List
- 7,000+ individuals and entities
- Updated daily
- Screen all:
  - Customers (KYC)
  - Transaction originators
  - Beneficiaries
  - Counterparties

### Screening Process

```
Transaction Received
    ↓
Extract: Customer name, country, amount
    ↓
Screen against OFAC lists:
  - SDN (Specially Designated Nationals)
  - Non-SDN sanctions lists
  - Consolidated UN list
    ↓
Decision:
  - Exact match: BLOCK transaction
  - Partial match: Manual review
  - No match: APPROVE
    ↓
Generate OFAC Hit Report (if flagged)
```

### Hit Report Contents

- Hit date and time
- Customer details
- Transaction details
- Screening lists matched
- Confidence score

## Penalties

- **Civil penalties:** Up to $250,000 per violation
- **Criminal penalties:** Up to 20 years prison + $1M fine
- **Examples:**
  - Barclays: $70M (Iran sanctions violations)
  - JP Morgan: $267M (sanctions evasion)

---

# Framework 9: Basel III (Capital Requirements)

**Scope:** Bank capital adequacy | **Applies to:** All systemically important financial institutions

## Capital Ratios

### Common Equity Tier 1 (CET1)
- Formula: Tier 1 Capital / Risk-Weighted Assets
- Minimum: 4.5%
- Recommended: 7-8%

### Tier 1 Capital Ratio
- Formula: Tier 1 Capital / Risk-Weighted Assets
- Minimum: 6%
- Includes Tier 1 + Tier 2 capital

### Total Capital Ratio
- Formula: (Tier 1 + Tier 2 Capital) / Risk-Weighted Assets
- Minimum: 8%
- Basel III standard

## Risk-Weighted Assets (RWA) Calculation

```
Credit Risk (70%):
- Corporate loans: 100% weight
- Mortgage loans: 35% weight
- Government bonds: 0% weight

Market Risk (20%):
- Interest rate exposure
- FX exposure
- Equity exposure

Operational Risk (10%):
- Internal fraud
- External fraud
- Technology failures
```

## Basel III Enhancements

- **Liquidity Coverage Ratio (LCR):** 100% (high-quality liquid assets)
- **Net Stable Funding Ratio (NSFR):** 100% (stable funding sources)
- **Countercyclical Buffer:** 0-2.5% (macro conditions)
- **G-SIB Surcharge:** 1-3.5% (systemic importance)

## Non-Compliance

- Federal Reserve enforcement action
- Dividend restrictions
- Executive compensation limits
- Business line restrictions

---

# Framework 10: Dodd-Frank Act

**Scope:** Financial reform post-2008 crisis | **Requirement Status:** US regulatory standard

## 16 Key Titles

### Title I: Financial Stability Oversight
- Systemic risk monitoring
- Too-big-to-fail prevention
- Annual stress testing

### Title II: Orderly Liquidation Authority
- Wind-down procedure for failing firms
- Taxpayer-funded resolution fund
- Clawback authority (executive compensation)

### Title III: Transfer of Powers to OCC
- Consumer Financial Protection Bureau (CFPB) creation
- Mortgage regulation
- Consumer protection standards

### Title IV: Bank Governance
- Shareholder say-on-pay
- Clawback provisions
- Executive compensation limits

### Title V: Insurance Provisions
- Non-traditional insurance companies
- Insurance holding company standards
- Mutual holding companies

### Title VI-IX: Derivatives & Market Reforms
- Swaps regulation
- Commodities regulation
- Clearing requirements
- Transparency rules

## Stress Testing Requirements

**Annual Severely Adverse Scenario:**
- Unemployment: +4% (to 13%)
- Stock market: -60%
- Corporate spreads: +600 bps
- House prices: -25%

**Test Capital Ratios:**
- CET1 minimum: 5.5%
- Tier 1 minimum: 7%
- Total minimum: 9.5%

## Penalties for Non-Compliance

- Federal enforcement action
- Capital restrictions
- Business limitations
- Fines up to $1M+ per violation

---

# Framework 11: MiFID II (Markets in Financial Instruments Directive II)

**Scope:** Investment services and markets | **Coverage:** EU, UK, 45+ countries

## Key Requirements

### 1. Authorization & Organizational Requirements
- Authorization required to provide services
- Governance structure (CRO, CISO)
- Compliance officer (independent)
- Internal audit function

### 2. Conduct of Business Rules
- Know your customer (KYC)
- Suitability assessment
- Best execution
- Conflict of interest management

### 3. Investment Advice
- Provide written suitability report
- Explain investment risks
- Recommended investment rationale
- Review recommendations periodically

### 4. Transparency & Reporting
- Pre-trade transparency (for exchange-traded)
- Post-trade transparency
- Order execution policy
- Transaction reporting

### 5. Investor Protection
- Segregation of client assets
- Insurance protection (€20,000 minimum)
- Complaints handling
- Dispute resolution

## Record Keeping

- Maintain records for 5+ years
- Call recordings (phone trades)
- Email records (electronic trades)
- Trade confirmations and statements
- Complaints and resolutions

## Penalties

- **Civil penalties:** Up to €15M or 5% of turnover
- **Criminal penalties:** Up to 4 years imprisonment
- **Examples:**
  - Goldman Sachs: €110M (1MDB market abuse)
  - Barclays: €135M (US interest rate manipulation)

---

# Framework 12: Data Residency & Sovereignty

**Scope:** Data localization requirements | **Applies to:** Cross-border data transfers

## Key Jurisdictions

### China
- **Requirement:** Personal data must stay in China
- **Exception:** Limited cross-border with approval
- **Enforcement:** Cyberspace Administration (CAC)
- **Examples:** Alibaba, WeChat, Tencent must localize data

### Russia
- **Requirement:** Personal data on Russian citizens must be stored in Russia
- **Enforcement:** Federal Security Service (FSB)
- **Fines:** Up to $270K+ per violation

### India
- **Requirement:** Sensitive personal data stored locally
- **Exception:** Diaspora Indians' data can transfer
- **Enforcement:** Data Protection Authority

### EU (GDPR)
- **Requirement:** Data transfers need adequacy decision or SCCs
- **Standard Contractual Clauses:** Must now include supplements
- **Enforcement:** Data Protection Authorities

### Brazil (LGPD)
- **Requirement:** Brazilian data must be processed in Brazil
- **Exception:** Can transfer with specific consent
- **Enforcement:** National Data Protection Authority

## Compliance Challenges

```
Multi-Region Deployment:

┌─────────────────────────────────────┐
│  Customer Data Location Rules       │
├─────────────────────────────────────┤
│ US Data → Stay in US (CCPA)        │
│ EU Data → Stay in EU (GDPR)        │
│ China Data → Stay in China         │
│ India Data → Stay in India         │
│ Brazil Data → Stay in Brazil       │
└─────────────────────────────────────┘
         ↓
    Separate Infrastructure
    Needed Per Region
         ↓
    Increased Costs:
    - Data centers per region
    - Compliance teams per region
    - Audit teams per region
    - Legal teams per region
```

## Migration Path

1. **Assess:** Which data is personal vs. non-personal
2. **Classify:** By geography and sensitivity
3. **Segregate:** Data storage by region
4. **Encrypt:** In-transit encryption between regions
5. **Monitor:** Compliance with region rules
6. **Report:** To regulatory authorities annually

---

## Summary: All 12 Regulatory Frameworks

| Framework | Scope | Applies To | Key Penalty |
|-----------|-------|-----------|------------|
| 1. PCI DSS 4.0 | Card security | All processors | $5K-$100K/month |
| 2. SOC 2 Type II | Security/audit | All services | Failed audit |
| 3. GDPR | EU data privacy | EU customers | €20M or 4% revenue |
| 4. GLBA | US financial privacy | US banks | $100K per violation |
| 5. TILA | Credit disclosure | Loans, credit | $5K per violation |
| 6. FCRA | Credit reporting | Credit bureaus | $100-$1K per violation |
| 7. AML/CFT | Money laundering | All institutions | 10 years prison |
| 8. OFAC | Sanctions screening | All institutions | $250K per violation |
| 9. Basel III | Capital adequacy | Systemic banks | Business restrictions |
| 10. Dodd-Frank | Financial reform | Large banks | $1M+ per violation |
| 11. MiFID II | Investment services | EU investment firms | €15M or 5% turnover |
| 12. Data Residency | Data localization | Cross-border ops | Fines + data seizure |

**All regulatory frameworks complete and implementation-ready.**

