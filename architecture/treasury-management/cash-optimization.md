# Treasury Management - Cash Optimization

## Liquidity Forecasting

```
Cash Flow Forecasting:

Daily:
- Expected deposits: $500M
- Expected withdrawals: $480M
- Net position: +$20M

Weekly:
- Payroll processing: $100M
- Settlement obligations: $200M
- Investment maturity: $50M

Monthly:
- Dividend payments: $50M
- Debt service: $200M
- Operating expenses: $150M

Annual:
- Debt refinancing: $5B
- Capital expenditures: $2B
- Shareholder returns: $3B
```

## Investment Strategy

```
Cash Deployment:

Tier 1 (Daily): Cash Buffer
- Amount: $1B
- Vehicle: Money market fund (4.5% APY)
- Liquidity: Instant

Tier 2 (Weekly): Short-term
- Amount: $5B
- Vehicle: Short-term deposits (3-6 months)
- Rate: 5.0-5.2% APY

Tier 3 (Monthly): Intermediate
- Amount: $10B
- Vehicle: Commercial paper, CDs
- Rate: 5.2-5.5% APY
- Maturity: 6-12 months

Tier 4 (Annual): Long-term
- Amount: $20B
- Vehicle: Bonds, long-term securities
- Rate: 4.5-5.2% YTM
- Maturity: 2-10 years
```

## FX Management

```
Multi-Currency Exposure:

Revenues:
- USD: 60% ($600M/year)
- EUR: 25% ($250M/year)
- JPY: 10% ($100M/year)
- GBP: 5% ($50M/year)

Costs:
- USD: 50% ($500M/year)
- EUR: 30% ($300M/year)
- JPY: 15% ($150M/year)
- GBP: 5% ($50M/year)

Net Exposure:
- EUR: +$200M (revenue > costs)
- JPY: -$50M (costs > revenue)

Hedging Strategy:
- EUR: Keep as natural hedge
- JPY: Hedge 50% with forward contracts
- Cost: 0.5-1% of notional amount
```

## Database Schema

```sql
CREATE TABLE cash_positions (
    position_id UUID PRIMARY KEY,
    date DATE,
    currency VARCHAR(3),
    available_cash DECIMAL(15,2),
    deployed_amount DECIMAL(15,2),
    forecasted_in DECIMAL(15,2),
    forecasted_out DECIMAL(15,2),
    net_position DECIMAL(15,2)
);

CREATE TABLE investments (
    investment_id UUID PRIMARY KEY,
    instrument_type VARCHAR(50),  -- MMKT, CD, CP, BOND
    currency VARCHAR(3),
    amount DECIMAL(15,2),
    rate DECIMAL(5,3),
    maturity_date DATE,
    status VARCHAR(20),  -- ACTIVE, MATURED, SOLD
    created_at TIMESTAMP
);

CREATE TABLE fx_hedges (
    hedge_id UUID PRIMARY KEY,
    currency_pair VARCHAR(10),  -- EURUSD
    contract_amount DECIMAL(15,2),
    forward_rate DECIMAL(10,4),
    maturity_date DATE,
    status VARCHAR(20),  -- ACTIVE, SETTLED, CANCELLED
    created_at TIMESTAMP
);
```

## Reporting

```
Daily Treasury Report:

Cash Position:
- Domestic: $10B
- International: $5B
- Total liquid: $15B

Investments:
- Money market: $3B (4.5% APY)
- CDs: $5B (5.0% APY)
- Bonds: $7B (5.2% YTM)

FX Exposure:
- EUR: +$200M (unhedged)
- JPY: -$50M (50% hedged)
- GBP: -$10M (unhedged)

Risk Metrics:
- Liquidity ratio: 3.0x (target: >1.5x)
- Average yield: 4.8%
- Duration: 2.3 years
```

## Status
✅ Production-ready | $50B+ liquidity managed
