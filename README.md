# Monte Carlo Risk Calculator for Compound V3

Real-time liquidation probability estimation for Compound V3 borrowers using Monte Carlo simulation.

## What It Does

Calculates your precise liquidation risk over different time horizons (7/30/90 days) using probabilistic modeling instead of basic health factor ratios. Shows you "98.5% safe" instead of just "1.5 health factor."

## Technical Architecture

**Smart Contract Integration**
- Direct Comet proxy queries for real-time position data
- `borrowBalanceOf()` + `collateralBalanceOf()` for current positions
- `getAssetInfo()` for collateral factors and oracle addresses
- Chainlink price feeds for collateral valuation

**Risk Engine**
- Geometric Brownian Motion (GBM): `dS = σS dW`
- 10,000 Monte Carlo simulations per calculation
- Historical volatility: 30-day rolling windows
- Multiple time horizons: 7, 30, 90 days

**Data Sources**
- On-chain: Comet contracts (Ethereum, Base, Arbitrum)
- Historical prices: CoinGecko API
- Oracles: Chainlink price feeds

## Implementation

**Backend API**
```
POST /api/v1/risk-score
{
  "userAddress": "0x...",
  "market": "usdc-mainnet",
  "timeHorizon": 30
}

Response:
{
  "liquidationProbability": 0.015,
  "healthScore": 98.5
}
```

**Frontend Widget**
- Embeddable iframe: `<iframe src="https://risk-calc.xyz/widget?address=0x...">`
- Traffic-light indicators: Green (<5%), Yellow (5-15%), Red (>15%)
- Simple probability displays
- Mobile-responsive

## Differentiation

**What Gauntlet Does**: Protocol-level VaR for governance  
**What This Does**: Individual position risk for borrowers

**What DeFi Saver Does**: Reactive alerts when threshold breached  
**What This Does**: Predictive probabilities before you borrow

**What Current Tools Show**: "1.5 health factor"  
**What This Shows**: "98.7% probability of safety over 30 days"

## Supported Markets

Launch: Ethereum USDC, Base USDC, Arbitrum USDC

## Tech Stack

- Smart Contracts: Compound V3 Comet ABIs
- Backend: Node.js, Express, TypeScript
- Computation: Python (NumPy) for Monte Carlo
- Frontend: React, Recharts, Tailwind CSS
- Data: CoinGecko API, Chainlink oracles

## Security Considerations

- Read-only contract interactions (no transaction signing)
- Oracle price validation (staleness checks)
- No user fund custody or private key handling
