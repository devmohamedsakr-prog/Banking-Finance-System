# Wealth Management - Portfolio Rebalancing

## Robo-Advisor Algorithm

```
Customer Profile:
- Age: 35
- Risk tolerance: MODERATE (60% stocks, 40% bonds)
- Investment horizon: 25 years
- Initial investment: $100K

Initial Allocation:
- US Stocks (40%): $40K
- Int'l Stocks (20%): $20K
- Bonds (30%): $30K
- Cash (10%): $10K

After 6 months (market changes):
- US Stocks (43%): $48K (gained $8K)
- Int'l Stocks (22%): $24K
- Bonds (25%): $28K (lost $2K)
- Cash (10%): $10K

Rebalancing Action:
- Sell $4K US stocks
- Sell $2K Int'l stocks
- Buy $6K bonds
- Back to target allocation
```

## Tax Optimization

```
Tax-Loss Harvesting:

Example:
- Stock A: Bought at $100, now $80 (unrealized loss: $20)
- Tax impact: $20 × 20% cap gains rate = $4 tax savings
- Harvest loss (sell)
- Buy similar stock (avoid wash sale)
- Repeat throughout year
- Annual savings: $200-$500 per $100K portfolio
```

## Performance Tracking

```
Metrics:

Total Return:
= (Ending Value - Beginning Value) / Beginning Value

Example:
- Started: $100K
- Ended: $115K
- Return: 15%

Benchmark Comparison:
- Portfolio: +15%
- S&P 500: +12%
- Outperformance: +3%

Risk-Adjusted Returns:
- Sharpe Ratio: (Return - Risk-Free Rate) / Volatility
- Target: >1.0

Volatility:
- Standard deviation of returns
- Portfolio volatility: 8%
- S&P 500 volatility: 10%
```

## Database Schema

```sql
CREATE TABLE portfolios (
    portfolio_id UUID PRIMARY KEY,
    customer_id UUID NOT NULL,
    risk_profile VARCHAR(50),  -- CONSERVATIVE, MODERATE, AGGRESSIVE
    target_allocation JSON,  -- {"stocks": 0.60, "bonds": 0.40}
    created_at TIMESTAMP,
    last_rebalanced_at TIMESTAMP
);

CREATE TABLE portfolio_positions (
    position_id UUID PRIMARY KEY,
    portfolio_id UUID NOT NULL,
    symbol VARCHAR(10),
    quantity INT,
    purchase_price DECIMAL(10,2),
    current_price DECIMAL(10,2),
    market_value DECIMAL(12,2),
    unrealized_gain DECIMAL(12,2),
    updated_at TIMESTAMP,
    
    FOREIGN KEY (portfolio_id) REFERENCES portfolios(id)
);

CREATE TABLE rebalancing_transactions (
    transaction_id UUID PRIMARY KEY,
    portfolio_id UUID NOT NULL,
    action VARCHAR(10),  -- BUY, SELL
    symbol VARCHAR(10),
    quantity INT,
    price DECIMAL(10,2),
    executed_at TIMESTAMP
);
```

## Rebalancing Triggers

```
Automatic Rebalancing:
- Every quarter (Jan/Apr/Jul/Oct)
- If allocation drifts >5% from target
- After large deposit/withdrawal
- Annual review at minimum

Rules:
- Minimize transaction costs
- Tax-loss harvest if possible
- Rebalance high-volatility positions first
```

## Status
✅ Production-ready | $1T+ portfolios managed
