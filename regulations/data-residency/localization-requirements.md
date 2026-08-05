# Data Residency - Localization Requirements

## Country-Specific Rules

### China (CNY)

```
Requirements:
- Personal data must stay in China
- Cross-border transfer banned
- Exception: Limited transfer with approval
- Enforcement: Cyberspace Administration (CAC)

Penalties:
- Fines: 1-10% revenue
- License revocation

Examples:
- Alibaba, WeChat, Tencent: China-only data centers
- Apple: iCloud data in China (Guizhou data center)
```

### Russia (RUB)

```
Requirements:
- Russian citizen data = Russia-only storage
- Exception: With explicit consent
- Enforcement: Federal Security Service (FSB)

Penalties:
- €27K per violation (as of 2024)
- Service blocking

Tech Companies Affected:
- Facebook: Blocked 2021
- Twitter: Slowed 2021
- Telegram: Ongoing restrictions
```

### India (INR)

```
Requirements:
- Sensitive personal data (biometric, financial): India-only
- Non-sensitive can transfer with conditions
- Enforcement: Data Protection Authority

Sensitive Data:
- Financial data
- Health data
- Government ID
- Biometric data
```

### Brazil (BRL)

```
Requirements:
- Brazilian citizen data = Brazil-only (by default)
- Transfer allowed with specific consent
- Enforcement: ANPD (National Data Protection Authority)

Penalties:
- Up to 2% annual revenue (max €15M)
- Service suspension
```

### EU (GDPR)

```
Requirements:
- EU citizen data: Cannot transfer outside EU
- Exception: Countries with adequacy decision
- Exception: Standard Contractual Clauses (SCCs)

Approved Countries (Adequacy):
- Switzerland
- UK (with agreement)
- Canada (partial)
- Israel (partial)

Restrictions:
- US Schrems II ruling restricts SCCs
- Supplementary measures required
- Enhanced encryption
- Legal review
```

## Compliance Strategy

### Multi-Region Architecture

```
Data Centers Required:

┌─────────────────────────────────────┐
│ Customer Data by Region             │
├─────────────────────────────────────┤
│ US Data → US Data Centers           │
│ EU Data → EU Data Centers           │
│ China Data → China Data Centers     │
│ India Data → India Data Centers     │
│ Brazil Data → Brazil Data Centers   │
└─────────────────────────────────────┘

Costs:
- Additional data centers: $50-200M per region
- Compliance teams: 5-10 per region
- Legal review: Ongoing
- Audit costs: $500K-2M per region annually
```

### Data Classification

```
Process:

1. Identify Data Type
   - Personal data
   - Financial data
   - Health data
   - Government ID
   - Biometric data

2. Identify Subject Geography
   - Where is customer?
   - Where is employee?
   - Where is processing?

3. Apply Residency Rules
   - China: Stay in China
   - EU: Stay in EU
   - India: Check sensitivity
   - Brazil: Check sensitivity

4. Route to Appropriate DC
   - Query from China server (China data)
   - Query from Frankfurt server (EU data)
   - Query from Mumbai server (India data)
```

## Implementation Checklist

- [ ] Data classification completed
- [ ] Geographic origin identified
- [ ] Residency rules documented
- [ ] DC locations identified
- [ ] Transfer policies updated
- [ ] Compliance training done
- [ ] Audit process defined
- [ ] Incident response plan created

## Status
✅ Complex | Multi-jurisdiction compliance required
