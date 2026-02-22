# 6. Liquidity-Aware Trade Sizing & Risk Controls

## 6.1 Overview

YieldLoop’s sustainability depends on disciplined execution.

Profit generation is not assumed.

It must be earned under strict constraints.

The Trading Engine operates under mandatory enforcement rules that protect:

- Capital
- Liquidity integrity
- Long-term expectancy
- Liability stability

No trade may execute unless all constraints are satisfied.

---

## 6.2 Liquidity-Aware Position Sizing

Trade size is bounded relative to real-time liquidity depth.

For any candidate trade:

- Size must not exceed a fixed percentage of available liquidity within an acceptable price impact window.
- Size must not exceed a defined percentage of vault capital.
- Size must scale conservatively as total AUM grows.

YieldLoop must never dominate a liquidity pool.

It must remain a participant, not the market maker.

---

## 6.3 Positive Net Expectancy Requirement

Every trade must satisfy:

Expected Profit > Total Execution Cost

Execution cost includes:

- Slippage
- DEX fees
- Gas
- Price impact
- Safety buffer for MEV and timing risk

If net expectancy is zero or negative, the trade is rejected.

There is no discretionary override.

---

## 6.4 Arbitrage Enforcement

For cross-DEX arbitrage:

- Spread must exceed cost stack plus safety buffer.
- Liquidity must support both legs.
- Atomic or near-atomic execution required.
- If second leg fails, transaction reverts.

Arbitrage is executed only when structurally favorable.

Not when marginal.

---

## 6.5 Spot Profit Mode Enforcement

For directional trading:

- Entry requires defined margin above total cost stack.
- Exit strategy must be defined before execution.
- No open-ended “hold and hope” behavior permitted.
- Trades that fail expectancy criteria are rejected.

YieldLoop is not a speculative momentum engine.

It is a profit-constrained execution engine.

---

## 6.6 Exposure Caps

Per vault:

- Maximum capital exposure per day is bounded.
- Full capital cycling during abnormal volatility is restricted.

This prevents cascading losses during unexpected conditions.

---

## 6.7 Drawdown Guard

If cycle drawdown exceeds defined threshold:

- Trade size automatically reduces.
- Or strategy shifts to conservative mode.
- Or trading pauses for that vault.

Drawdown controls are deterministic and visible.

---

## 6.8 Volatility & Market Condition Filters

During extreme volatility:

- Spread thresholds increase.
- Position sizes reduce.
- Safety buffers widen.

If volatility exceeds defined limits, trading may pause.

The system prefers inactivity over forced execution.

---

## 6.9 Gas & Cost Sanity Checks

Trades execute only when:

Projected profit meaningfully exceeds gas cost.

If gas spikes reduce expectancy:

Trades are skipped.

Profit must remain real after full cost stack.

---

## 6.10 Oracle & Data Integrity

Execution requires:

- Valid, current price feeds.
- Acceptable deviation bounds between DEX and reference pricing.
- No stale or anomalous data.

If data integrity fails:

Trading halts automatically.

---

## 6.11 Emergency Halt Conditions

Trading may pause if:

- Coverage invariants are threatened.
- Smart contract anomaly detected.
- Oracle failure occurs.
- Administrative emergency halt activated.

Pause protects capital before profit.

---

## 6.12 Scaling Discipline

As AUM grows:

- Trade size must scale sub-linearly.
- Risk must not increase proportionally with capital.
- Edge preservation takes priority over volume.

YieldLoop is designed to protect expectancy over time, not maximize short-term throughput.

---

## 6.13 Design Philosophy

Profit generation is constrained by:

- Liquidity respect
- Mathematical expectancy
- Volatility discipline
- Deterministic drawdown controls

This module exists to prevent YieldLoop from eroding its own structural advantage as it scales.

Stability precedes growth.
