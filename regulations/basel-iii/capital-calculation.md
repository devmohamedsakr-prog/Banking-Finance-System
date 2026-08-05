# Basel III - Capital Calculation & Requirements

## Capital Tiers

### Common Equity Tier 1 (CET1)
- Paid-in capital
- Retained earnings
- Disclosed reserves
- Minimum: 4.5% of RWA

### Tier 1 Capital
- CET1 + Tier 1 Hybrid instruments
- Minimum: 6.0% of RWA

### Tier 2 Capital
- Subordinated debt
- Loan loss provisions
- Minimum: 2.0% of RWA

Total Capital Ratio:
- (Tier 1 + Tier 2) / RWA
- Minimum: 8.0%

## Risk-Weighted Assets (RWA) Calculation

```
Total RWA = Credit Risk RWA + Market Risk RWA + Op Risk RWA

Credit Risk (Standardized Approach):

Residential Mortgages: 35% weight
- $1M mortgage: $350K RWA

Corporate Loans: 100% weight
- $1M corporate loan: $1M RWA

Government Bonds (AAA): 0% weight
- $1M UST bonds: $0 RWA

Exposures by Risk:

Risk 0%: Government securities (US, AAA)
Risk 20%: Bank exposures, AAA corporates
Risk 50%: Residential mortgages
Risk 100%: Corporate loans, equities
Risk 150%: High-risk corporates

Calculation Example:

Assets:
- US Treasuries: $100M (0% weight) = $0 RWA
- Investment-grade loans: $300M (50% weight) = $150M RWA
- Corporate loans: $400M (100% weight) = $400M RWA
- Equities: $50M (100% weight) = $50M RWA
- Cash: $150M (0% weight) = $0M RWA

Total RWA: $600M
Capital Required: $600M × 8% = $48M Tier 1+2 capital
```

## Leverage Ratio

```
Leverage Ratio = Tier 1 Capital / Total Leverage Exposure

Requirement: Minimum 3%

Example:
- Tier 1 Capital: $50B
- Total Leverage Exposure: $1,500B
- Ratio: $50B / $1,500B = 3.3% ✅ (meets requirement)
```

## Compliance Checklist

- [ ] Monthly capital calculation
- [ ] Quarterly stress testing
- [ ] Annual regulatory filing (FR Y-9C)
- [ ] Board of Directors review
- [ ] Contingency plan if ratio falls below requirement
- [ ] Impact analysis for major transactions

## Status
✅ Production-ready | Quarterly reporting required
