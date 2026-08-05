# KYC/AML - Verification Process

## Customer Onboarding Flow

```
1. KYC VERIFICATION (2-3 minutes)

Step 1: Collect Information
  - Full name
  - Date of birth
  - Address
  - SSN/Tax ID
  - Employment info
  - Source of funds

Step 2: Identity Verification
  - Government ID upload (driver's license, passport)
  - ML OCR scan
  - Liveness check (selfie video)
  - Verify details match

Step 3: Address Verification
  - Cross-reference government DB
  - Or send verification letter (5-7 days)

Step 4: Risk Assessment
  - Calculate risk score (LOW/MEDIUM/HIGH)
  - Check PEP database (politically exposed persons)
  - Review adverse media

Result: APPROVED or FLAGGED or MANUAL REVIEW
```

## AML Screening

```
2. AML SANCTIONS SCREENING (Real-time)

Screening Against:
- OFAC SDN list (7,000+ entities)
- EU sanctions list
- UN consolidated list
- FATF grey list

Process:
1. Extract name, country, DOB
2. Normalize (remove accents, titles)
3. Fuzzy match against lists
4. Calculate match score (0-100)
5. If >85: FLAG for manual review
6. If >95: AUTO-BLOCK

Hit Report Fields:
- Full name
- Match confidence
- Sanction program
- Date added to list
- Listing agency
```

## Ongoing Monitoring

```
3. QUARTERLY RE-SCREENING

Every 90 days:
- Re-screen all active customers
- Check for adverse media
- Monitor transaction patterns
- Update risk profiles

Rules:
- New sanctions lists (updated daily)
- Adverse media (quarterly review)
- Transaction anomalies (real-time)
```

## Database Schema

```sql
CREATE TABLE kyc_verifications (
    kyc_id UUID PRIMARY KEY,
    customer_id UUID NOT NULL,
    full_name VARCHAR(255),
    dob DATE,
    ssn VARCHAR(255),  -- encrypted
    address TEXT,
    status VARCHAR(20),  -- PENDING, VERIFIED, FAILED, EXPIRED
    risk_level VARCHAR(20),  -- LOW, MEDIUM, HIGH
    verified_at TIMESTAMP,
    expires_at TIMESTAMP,  -- 1 year
    created_at TIMESTAMP NOT NULL,
    
    UNIQUE(customer_id),
    INDEX idx_status (status)
);

CREATE TABLE aml_screenings (
    screening_id UUID PRIMARY KEY,
    customer_id UUID NOT NULL,
    full_name VARCHAR(255),
    matched BOOLEAN,
    match_score INT,  -- 0-100
    sanction_lists VARCHAR(500),  -- OFAC, EU, UN
    created_at TIMESTAMP NOT NULL,
    
    INDEX idx_customer_created (customer_id, created_at)
);

CREATE TABLE sanctions_lists (
    list_id UUID PRIMARY KEY,
    entity_name VARCHAR(500),
    entity_type VARCHAR(50),  -- INDIVIDUAL, ORGANIZATION
    country VARCHAR(100),
    sanction_program VARCHAR(255),  -- OFAC SDN, EU_SANCTIONS, etc
    listed_date DATE,
    updated_at TIMESTAMP,
    
    INDEX idx_entity_name (entity_name)
);
```

## Compliance Reporting

```
SAR Filing (Suspicious Activity Report):

Triggers:
- Transaction >$10K (CTR)
- Structuring pattern detected
- Sanction match
- Unusual activity

Process:
1. Alert compliance team
2. Investigate (24-48 hours)
3. Generate SAR (FinCEN form)
4. File with regulator
5. Maintain confidentiality (no customer notification)

Retention: 5 years
```

## Status
✅ Production-ready | Enterprise-grade compliance
