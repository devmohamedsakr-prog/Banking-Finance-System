# OFAC Sanctions - Screening Procedure

## Sanction Lists

```
Primary Lists Screened:

1. OFAC SDN List (Specially Designated Nationals)
   - 7,000+ individuals and entities
   - Regular updates
   - Updated daily
   - $250K+ in blocked assets

2. Non-SDN Sanctions Lists
   - Sectoral sanctions (Russian oil, Iranian entities)
   - Country-specific lists
   - Consolidated UN list

3. Additional Screening
   - EU sanctions lists
   - UK sanctions lists
   - Multiple jurisdictions
```

## Screening Process

```
For New Customers:

1. Collect Information
   - Full name (as spelled on ID)
   - Date of birth
   - Nationality/country of residence
   - Business name (if applicable)

2. Normalize Name
   - Remove titles (Mr., Jr., Sr.)
   - Remove accents
   - Parse first/middle/last
   - Handle aliases

3. Fuzzy Matching
   - Match against SDN list
   - Calculate match score (0-100%)
   - 85%+ confidence: FLAG
   - 95%+confidence: AUTO-BLOCK

4. Review & Decision
   - Manual review by compliance officer
   - Judgment call (especially 85-95% range)
   - Document decision
   - Escalate if uncertain

5. Quarterly Re-screening
   - Re-screen all active customers
   - New lists may add customers
   - Detect previously unknown matches
   - Update sanctions status
```

## Hit Report

```
If Match Found:

Generate Hit Report:
- Customer name (from database)
- SDN name (from list)
- Match confidence: 92%
- Sanction program: OFAC SDN
- Source entity: Treasury Department
- Listed since: 2023-01-15

Decision Options:
1. APPROVED (false positive)
2. BLOCKED (genuine match)
3. REVIEWED (awaiting decision)

If Blocked:
- Account frozen immediately
- Assets held in segregated account
- Report filing required
- Never notify customer (breaks law)
```

## Blocked Transactions

```
Actions:

Immediate:
- Stop transaction
- Freeze account
- Alert compliance team
- Document in system

Within 24 Hours:
- File OFAC report
- Preserve transaction records
- Notify senior management
- Begin investigation

Within 30 Days:
- Compile evidence file
- Prepare OFAC filing
- Submit to Treasury
- Maintain confidentiality
```

## Compliance Checklist

- [ ] Screening system tested and validated
- [ ] SDN list updated daily
- [ ] All customers screened (100%)
- [ ] New customers screened before activation
- [ ] Quarterly re-screening procedure
- [ ] Hit reports filed timely
- [ ] Staff training completed annually
- [ ] Audit conducted annually

## Status
✅ Mandatory | Daily compliance required
