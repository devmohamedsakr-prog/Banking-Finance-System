# Financial Products - Complete Batch (5 Products)

Complete specifications for all financial products.

---

# Product 1: Checking Account

**Users:** 500M+ | **Daily Active:** 100M+ | **APY:** 0-0.5%

## Features

- **Debit Card** (instant access, worldwide)
- **Check Writing** (unlimited, no fee)
- **Bill Pay** (auto-pay setup, 1000+ billers)
- **Mobile Banking** (balance, transfer, deposit)
- **ATM Access** (50K+ nationwide)
- **Direct Deposit** (2-day early arrival)
- **Transaction History** (7 years searchable)

## Account Tiers

**Basic Checking**
- Min balance: $0
- Monthly fee: $0 (waived with $500+ direct deposit)
- Overdraft: $35/occurrence
- Debit card: Yes
- Features: Basic

**Premium Checking**
- Min balance: $2,500
- Monthly fee: $0 (waived if minimum met)
- Overdraft: $0 (covered up to $200)
- Debit card: Premium (metal)
- Features: Priority support, fee waiver

**Business Checking**
- Min balance: $5,000
- Monthly fee: $30
- ACH transfers: Unlimited
- Wire transfers: $15 each
- Features: Accounting integration, business insights

## Use Cases

**UC-001: Account Opening**
- Customer opens account (2 minutes online)
- Identity verified (ID + SSN)
- Debit card ordered (2-3 days delivery)
- Account active immediately
- First transaction: ACH from prior bank

**UC-002: Bill Pay**
- Customer sets up auto-pay (mortgage, utilities)
- System connects to 1000+ billers
- Payment amount fixed or variable
- Payment processed 2 days early
- Confirmation email/SMS

**UC-003: Overdraft Protection**
- Customer spends $50 over balance
- Overdraft covered ($35 fee)
- Low balance alert sent
- 2-day grace period to deposit
- Auto-transfer from linked savings

## Integrations

- **Direct Deposit:** NACHA ACH
- **Bill Pay:** BillServ (1000+ billers)
- **ATM Network:** Allpoint (50K+ ATMs)
- **Mobile:** Apple Pay, Google Pay, Samsung Pay
- **Accounting:** QuickBooks, Xero (business)

---

# Product 2: Savings Account

**Users:** 300M+ | **AUM:** $5T | **APY:** 0.5-5.0% (tiered)

## Account Types

**Basic Savings**
- Min balance: $0
- APY: 0.5% (base rate)
- Withdrawals: 6/month (regulatory limit)
- Fee: $0 if min balance >$100

**High-Yield Savings (HYS)**
- Min balance: $1,000
- APY: 4.5% (varies with Fed rate)
- Withdrawals: Unlimited
- Fee: $0
- Compound: Daily

**Money Market Account**
- Min balance: $2,500
- APY: 4.0% (tiered based on balance)
- Check writing: 3/month
- Debit card: Yes
- Fee: $25 (waived if min maintained)

**Certificate of Deposit (CD)**
- Min deposit: $1,000
- Terms: 3mo, 6mo, 1yr, 3yr, 5yr
- Early withdrawal penalty: Interest loss + 1%
- APY: 5.0-5.5% (longer terms higher)
- Laddering option: Auto-renew

## Rates Structure

```
HYS Rate Schedule:
Balance          APY
$0-$1K           4.0%
$1K-$10K         4.3%
$10K-$100K       4.5%
$100K-$1M        4.6%
$1M+             4.7%
```

## Use Cases

**UC-001: Savings Growth**
- Customer deposits $10K into HYS
- APY: 4.5% → $450/year
- Daily compound: $38/month interest
- After 1 year: $10,450
- Customer can withdraw anytime

**UC-002: CD Ladder**
- Customer invests $10K across 5 CDs
- $2K each: 1yr (5.0%), 2yr (5.1%), 3yr (5.2%), 4yr (5.3%), 5yr (5.4%)
- Each year: $2K matures at higher rate
- Reinvest or withdraw
- Average return: 5.2%

**UC-003: Emergency Fund**
- Customer maintains $5K in HYS
- Earns $225/year (4.5% APY)
- Instant access if needed
- No lock-in period

## Integrations

- **Fed Funds:** Sync with daily Fed rate changes
- **Interest Accrual:** Daily compounding (automated)
- **Tax Reporting:** 1099-INT for >$10 interest
- **Transfers:** Linked checking account (3/month free)

---

# Product 3: Lending Products

**Portfolio:** $10T | **Active Loans:** 100M+ | **APR Range:** 6-36%

## Loan Types

**Personal Loan**
- Amount: $1,000-$50,000
- Term: 12-60 months
- APR: 8-36% (based on credit score)
- Use: Debt consolidation, home improvement, vacation
- Approval: <5 minutes (ML scoring)

**Business Loan**
- Amount: $10,000-$1,000,000
- Term: 1-7 years
- APR: 6-18% (based on revenue, credit)
- Use: Working capital, equipment, expansion
- SBA-backed options available

**Auto Loan**
- Amount: $5,000-$100,000
- Term: 36-72 months
- APR: 4-10% (based on vehicle, credit)
- Use: New/used vehicle purchase
- Rate holds: 30 days

**Mortgage**
- Amount: $50,000-$1,000,000+
- Term: 10/15/20/30 years
- APR: 3-8% (varies with rates)
- Use: Home purchase/refinance
- Rate lock: 45-60 days
- Closing: 7-10 days

**Buy Now Pay Later (BNPL)**
- Amount: $50-$5,000 (at checkout)
- Term: 4-12 weeks (typically 4 payments)
- Fee: $0 if paid on time, 25% APR if late
- Use: E-commerce purchases
- Instant approval

**Credit Line**
- Amount: $1,000-$100,000
- Term: Revolving (3-5 year review)
- APR: 7-25% (based on credit)
- Use: Emergency funds, flexible borrowing
- Interest-only payments available

## Approval Workflow

```
Application → Income Verification → Credit Check → ML Scoring → Decision (< 5 min)
    ↓                   ↓                 ↓              ↓
  Form                Plaid            Equifax      Algorithm         
  Entry               API              Score        (300-850)
                                                         ↓
                                              Approved / Declined / Manual Review
                                                    ↓
                                              Offer Letter → Accept → Funds Disbursed
```

## Rate Factors

- **Credit Score:** 760+: -2% APR, 660-759: Base, <660: +5% APR
- **Loan Term:** Longer term: +0.5% APR per year
- **Debt-to-Income:** <20%: Base, 20-40%: +1%, >40%: +3%
- **Income Verification:** Verified: Base, Unverified: +2%

## Use Cases

**UC-001: Debt Consolidation**
- Customer has 3 credit cards ($15K @ 18% APR)
- Takes personal loan ($15K @ 12% APR, 36mo)
- Saves $2,880 in interest over 3 years
- Monthly payment: $417 (vs. $500 across 3 cards)

**UC-002: Home Purchase**
- Customer buys home ($400K)
- 30-year mortgage @ 6.5% APR
- Down payment: 20% ($80K)
- Loan: $320K
- Monthly payment: $2,024 (PITI)
- Total interest: $410K over 30 years

**UC-003: BNPL at Checkout**
- Customer wants $1,000 laptop
- Selects 4 payments: $250 each
- First payment: checkout
- Remaining 3 due every 2 weeks
- No interest if on-time

## Integrations

- **Income Verification:** Plaid, ADP, Guidepoint
- **Credit Bureaus:** Equifax, Experian, TransUnion
- **Fraud Detection:** ML models, device checking
- **Compliance:** TILA (Truth in Lending), ECOA

---

# Product 4: Investment Products

**Users:** 50M+ | **AUM:** $2T | **Commission:** 0-0.5%

## Investment Options

**Stock Trading**
- 5,000+ stocks (US/international)
- Commission: $0 (zero commission)
- Fractional shares: Yes ($1 minimum)
- Margin available: Up to 2x
- Mobile-first interface

**ETF Investing**
- 3,000+ ETFs (equity, bond, commodity)
- Commission: $0
- Dividend reinvestment: Automatic
- Portfolio tracking: Real-time

**Bonds**
- Corporate bonds (2-20 year)
- Municipal bonds (tax-free)
- Treasury bonds (backed by US govt)
- Yield: 4-7% depending on term
- Ladder builder: Automated

**Cryptocurrency (Optional)**
- Bitcoin, Ethereum, Solana (20+ coins)
- Wallets: Custody by regulated partner
- Staking rewards: 4-8% APY
- Trading: 24/7 markets

**Robo-Advisor**
- Automated portfolio management
- Asset allocation based on risk profile
- Rebalancing: Quarterly
- Fee: 0.25-0.50% AUM
- Tax-loss harvesting: Automatic

## Account Types

**Taxable Brokerage**
- No contribution limits
- Taxes on gains/dividends
- Unlimited trading

**Retirement Accounts**
- Traditional IRA: $6,500/year contribution limit, tax-deductible
- Roth IRA: $6,500/year, tax-free growth
- 401(k) rollover: $23,500/year
- SEP-IRA (self-employed): $66,000/year

**529 Education Plan**
- College savings: $235K+ per beneficiary
- Tax-free growth (education expenses)
- State tax deduction: Up to $35K/year

## Use Cases

**UC-001: Getting Started Investor**
- Customer: $5K to invest
- Riskometer: 70% stocks, 30% bonds
- Robo-advisor: Auto-allocate
- Monthly contributions: $500
- After 10 years: ~$72K (8% avg return)

**UC-002: Retirement Savings**
- Customer: 30 years old, income $100K
- Contributes $500/month to 401(k)
- Company match: 3% ($300/month)
- After 35 years: ~$1.2M (7% avg return)

**UC-003: Education Funding**
- Parent: 2 kids, $18K/year college
- 529 plan: $100K deposit
- Tax-free growth @ 6%: $150K in 10 years
- Covers both kids' first 4 years

---

# Product 5: Insurance Products

**Premium:** $500B+ annually | **Customers:** 200M+ | **Claims:** 99% paid

## Insurance Types

**Life Insurance**

*Term Life (20-30 year)*
- Death benefit: $100K-$1M
- Monthly premium: $15-$50 (age/health dependent)
- Payout: Tax-free to beneficiary
- No cash value

*Whole Life*
- Death benefit: $100K-$500K
- Monthly premium: $100-$300
- Cash value: 70% of premium after year 1
- Lifetime coverage

**Auto Insurance**

*Liability Coverage*
- Bodily injury: $100K-$500K
- Property damage: $100K-$500K
- Premium: $500-$1,500/year

*Collision & Comprehensive*
- Deductible: $250-$1,000
- Covers accidents, theft, weather
- Premium: $200-$800/year

*Uninsured Motorist*
- Covers accidents with uninsured drivers
- Premium: $50-$200/year

**Home Insurance**

*Homeowner Policy*
- Dwelling: Replacement cost
- Contents: 70% of dwelling
- Liability: $100K-$500K
- Deductible: $500-$2,500
- Premium: $1,000-$3,000/year

*Flood Insurance*
- Separate policy (standard homeowner excludes)
- Coverage: Up to $250K dwelling
- Premium: $500-$3,000/year
- 30-day waiting period

**Health Insurance**

*Bronze Plan*
- Premium: $200-$400/month
- Deductible: $5,000-$7,000
- Out-of-pocket max: $8,700
- Coverage: 60% of costs

*Silver Plan*
- Premium: $300-$500/month
- Deductible: $2,000-$4,000
- Out-of-pocket max: $8,000
- Coverage: 70% of costs

*Gold Plan*
- Premium: $400-$700/month
- Deductible: $500-$1,500
- Out-of-pocket max: $6,500
- Coverage: 80% of costs

**Travel Insurance**

*Trip Protection*
- Cost: $50-$200 per trip
- Covers: Cancellation, delay, medical
- Max reimbursement: $10K

*Annual Pass*
- Cost: $300-$500/year
- Unlimited trips
- Same coverage per trip

## Use Cases

**UC-001: Young Family**
- Customer: 35, married, 2 kids
- Term life: $500K @ $25/month
- Auto insurance: $1,200/year
- Homeowner: $1,500/year
- Total: ~$300/month protection

**UC-002: Retirement Planning**
- Customer: 55, wants guaranteed income
- Whole life: $250K
- Death benefit + cash value = security
- Premium: $150/month
- At 65: ~$40K cash value

**UC-003: Travel Risk**
- Customer: Frequent flyer (12 trips/year)
- Annual travel insurance: $400
- Per-trip cost: $33 (vs. $150 if per trip)
- Covers cancellation, delay, medical

---

## Summary: All 5 Products

| Product | Users | Scale | Revenue |
|---------|-------|-------|---------|
| Checking | 500M+ | $500B+ AUM | Deposits |
| Savings | 300M+ | $5T+ AUM | 4.5% APY |
| Lending | 100M+ | $10T+ portfolio | 15% avg APR |
| Investment | 50M+ | $2T+ AUM | 0.25-0.50% fee |
| Insurance | 200M+ | $500B+ premiums | 15-20% margin |

**All products complete and ready for implementation.**

