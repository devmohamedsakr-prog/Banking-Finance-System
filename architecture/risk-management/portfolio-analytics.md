# Risk Management - Portfolio Analytics

## Value-at-Risk (VaR) Calculation

```
VaR Formula: VaR = Portfolio Value × Z-score × Volatility

Example:
Portfolio value: $100M
Z-score (95% confidence): 1.645
Volatility: 2% daily
VaR = $100M × 1.645 × 0.02 = $3.29M

Interpretation: 95% chance daily loss <$3.29M
```

## Greeks Calculation

```
Delta (Δ): How option price changes with stock price
- Range: 0 to 1
- Delta = 0.5 means 1% stock move = 0.5% option move

Gamma (Γ): How delta changes
- Measures risk of large moves
- High gamma = risky

Vega (ν): How option price changes with volatility
- Range: 0 to Infinity
- High vega = volatility risk

Theta (Θ): Time decay
- How much option loses per day
- Positive for sellers, negative for buyers

Rho (ρ): How option price changes with interest rates
- Lower importance (less volatile)
```

## Position Management

```
Portfolio Tracking:

Equities:
├── Long positions (own shares)
├── Short positions (borrowed shares)
├── Dividends paid/received
└── Realized P&L

Fixed Income:
├── Bonds held
├── Duration risk (interest rate)
├── Credit risk (issuer default)
└── Yield to maturity

Derivatives:
├── Calls and puts
├── Futures contracts
├── Swaps
└── Notional exposure

Real-time Dashboard:
- Mark-to-market valuation
- Daily P&L
- Risk metrics (Greeks, VaR)
- Limit compliance
```

## Risk Limits Framework

```
Counterparty Limits:
- JPMorgan Chase: $100M exposure max
- Bank of China: $50M exposure max
- Alert at 75%: $75M (JPM example)

Sector Limits:
- Technology: 20% max portfolio
- Financials: 15% max
- Energy: 10% max

Product Limits:
- Derivatives: 10% of portfolio
- Commodities: 5% of portfolio

Individual Limits:
- Single stock: 5% max
- Single bond: 2% max

Breach Procedure:
1. Automatic alert
2. Risk committee review
3. 24-hour remediation required
4. Escalation if not resolved
```

## Stress Testing

```
Scenarios:

2008 Financial Crisis:
- S&P 500: -50%
- Credit spreads: +600 bps
- VIX: 80
- Predicted loss: $50M

COVID-19 Scenario:
- Equity: -30%
- Tech: -20%
- Energy: -50%
- Predicted loss: $30M

Interest Rate Shock:
- Rates +200 bps
- Bond portfolio: -15%
- Duration risk realized

Historical Backtesting:
- Compare predicted vs actual
- Accuracy >90% required
- Retrain model if <85%
```

## Status
✅ Production-ready | Real-time monitoring
