# YieldLoop — Liquidity-Aware Trade Sizing & Risk Control Module  
Version: 1.0  
Status: Mandatory Execution Constraint Layer  
Scope: Arbitrage & Spot Trading Enforcement  

---

# 1. Purpose

This module defines mandatory execution constraints that ensure:

- Positive net expectancy after full cost stack
- No self-induced slippage collapse
- No liquidity dominance risk
- Sustainable scaling as AUM grows
- Fail-closed protection during abnormal market conditions

This module sits between:

Strategy Proposal → Trade Execution

No trade may execute without passing these constraints.

---

# 2. Core Principle

YieldLoop must never become the liquidity it attempts to harvest.

Trade size must always be bounded relative to real-time executable liquidity depth.

Edge must remain positive after:

- Slippage
- DEX swap fees
- Gas
- Price impact
- Adverse selection risk

If Net_Expectancy ≤ 0:

Trade is rejected.

---

# 3. Definitions

For a candidate trade T:

AUM_vault = Capital allocated to vault  
D_x = Available liquidity depth within X% price impact window  
Impact_estimate = Projected price movement from trade size  
Spread_estimate = Arbitrage or profit margin opportunity  
Cost_stack = Slippage + Gas + DEX fees + MEV risk estimate  

Net_Expectancy = Spread_estimate - Cost_stack - Impact_estimate  

---

# 4. Liquidity Depth Constraint

Maximum trade size per pair:

Trade_size ≤ min(
    AUM_vault × Risk_cap,
    D_x × Liquidity_fraction_cap
)

Where:

Risk_cap = Configurable % of vault capital (e.g., 5–20%)  
Liquidity_fraction_cap = Max % of depth within allowed price window (e.g., 10–20%)

Example:

If D_x = 100,000 USDT  
Liquidity_fraction_cap = 15%  

Max trade size = 15,000 USDT  

Trade may not exceed this even if vault capital allows more.

---

# 5. Sub-Linear Scaling Rule (AUM Protection)

As total deployed capital increases:

Trade sizing must scale sub-linearly.

Example implementation:

Effective_trade_size = Base_size × sqrt(AUM_current / AUM_reference)

Purpose:

- Prevent compression of arbitrage edge
- Avoid becoming dominant liquidity
- Preserve long-term expectancy

---

# 6. Arbitrage Mode Constraints

For cross-DEX arbitrage (PancakeSwap ↔ BiSwap):

Trade executes only if:

Spread_estimate ≥ Min_spread_threshold

Where:

Min_spread_threshold > Total_cost_stack + Safety_buffer

Safety_buffer accounts for:

- Block delay risk
- MEV front-run risk
- Price movement between legs

If second leg cannot execute at expected rate:

Trade aborts (atomic where possible).

---

# 7. Spot Profit Mode Constraints

For directional buy/sell profit:

Trade only executes if:

Expected_exit_price - Entry_price ≥ Required_margin

Required_margin ≥ Cost_stack + Profit_buffer

Profit_buffer must exceed minimum threshold (configurable).

No “hope-based” trades allowed.

---

# 8. Daily Exposure Cap

Per vault:

Max_daily_capital_exposure ≤ Exposure_limit × AUM_vault

Example:

Exposure_limit = 50%

This prevents full capital cycling during abnormal volatility.

---

# 9. Drawdown Guard

If cumulative cycle drawdown exceeds:

Drawdown_threshold (e.g., 8–15%)

Then:

- Reduce Risk_cap automatically
- Or pause new trades
- Or switch to conservative strategy mode

Drawdown logic must be deterministic and visible.

---

# 10. Volatility Filter

If real-time volatility exceeds defined bounds:

- Increase Min_spread_threshold
- Reduce Liquidity_fraction_cap
- Increase Safety_buffer

Extreme volatility mode must reduce aggressiveness automatically.

---

# 11. Gas & Execution Sanity Check

Before execution:

Projected_profit ≥ (Gas_cost × Gas_multiplier)

Gas_multiplier ensures trades are not marginal after gas spike.

If gas > threshold:

Trade suspended.

---

# 12. Oracle & Data Integrity Requirements

Trade execution requires:

- Valid price feed
- Acceptable deviation between DEX pricing and oracle
- No stale data

If deviation > Oracle_deviation_limit:

Trade rejected.

Fail-closed rule applies.

---

# 13. Blacklist & Pair Restrictions

System may enforce:

- Approved token whitelist
- Liquidity floor requirement per pair
- Volume floor requirement per pair

Pairs below minimum liquidity threshold:

Not eligible for execution.

---

# 14. Emergency Halt Conditions

Trading automatically halts if:

- Coverage_ratio_E threatened
- Smart contract anomaly detected
- Excessive slippage detected
- Oracle outage
- Governance-triggered pause

Funds remain idle and secure until condition resolves.

---

# 15. Transparency Requirements

Users must be able to view:

- Current Risk_cap
- Liquidity_fraction_cap
- Active spread threshold
- Current volatility mode
- Drawdown state
- Trade rejection reason logs

No black-box execution.

---

# 16. Invariants

At all times:

1. Trade_size must respect liquidity depth bound.
2. Net_Expectancy must be positive after full cost stack.
3. Exposure limits must not be exceeded.
4. Drawdown rules must trigger automatically.
5. Trading must fail-closed before coverage risk emerges.

---

# 17. Design Philosophy

Profit is not generated by aggression.

It is generated by:

- Liquidity inefficiencies
- Controlled execution
- Conservative sizing
- Discipline under volatility

This module exists to prevent YieldLoop from destroying its own edge as it grows.
