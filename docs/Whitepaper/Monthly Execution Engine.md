# 3. Monthly Execution Engine

## 3.1 Overview

YieldLoop operates on a fixed, calendar-based monthly cycle.

Each cycle progresses through five structured phases:

1. Deposit & Strategy Confirmation  
2. Active Trading Window  
3. Cycle Close & Settlement  
4. LOOP Mint & Ratchet Evaluation  
5. Redemption Window  

This structure ensures:

- Clean accounting
- Predictable liability tracking
- No mid-cycle manipulation
- Deterministic profit realization

---

## 3.2 Phase 1 — Deposit & Strategy Confirmation

Before the start of each calendar month:

Users may:

- Create a vault (500 USDT minimum)
- Add capital (250 USDT minimum)
- Adjust token allocation percentages
- Review AI-proposed trade parameters
- Accept or modify execution constraints

The system generates a pre-populated configuration including:

- Arbitrage vs spot mode mix
- Liquidity-based trade sizing limits
- Slippage bounds
- Spread thresholds
- Risk caps
- Exposure limits

User must explicitly confirm the strategy before the cycle begins.

Once the cycle starts, configuration locks.

---

## 3.3 Phase 2 — Active Trading Window

From the 1st to the final day of the month:

The Trading Engine executes eligible opportunities subject to:

- Liquidity-aware sizing
- Positive net expectancy
- Volatility filters
- Drawdown protection
- Gas sanity checks

Trades may occur frequently or remain idle.

YieldLoop does not force activity.

No mid-cycle withdrawals or strategy changes are permitted.

---

## 3.4 Phase 3 — Cycle Close & Settlement

At cycle end:

Vault value is evaluated.

Realized_Profit_E is calculated as:

End_Value - Start_Value - External_Flows

If Realized_Profit_E ≤ 0:
- No LOOP minted
- No performance fee applied

If Realized_Profit_E > 0:
- 10% performance fee applied
- Fee allocated per treasury policy
- User-selected routing applied (USDT or LOOP)

Settlement occurs before the next cycle begins.

---

## 3.5 Phase 4 — LOOP Mint & Ratchet Evaluation

On the 1st of the following month:

For users selecting LOOP:

- LOOP is minted 1:1 from net profit.
- LOOP is tagged to the specific epoch.
- Redemption_price_E initialized (if new epoch).

Ratchet evaluation then executes for all eligible epochs:

- Coverage ratios calculated
- Surplus identified
- Floor increases applied where permitted

Redemption_price_E may increase.
It may never decrease.

---

## 3.6 Phase 5 — Redemption Window

After ratchet execution:

Redemption window opens for eligible epochs.

Eligibility requires:

- 3 full calendar months of holding.

Users may:

- Redeem partial or full LOOP balances.
- Receive USDT based on Redemption_price_E.

Redemptions execute instantly if coverage remains ≥ 1.0.

If coverage would be violated:
- Transaction fails.
- No queue is created.

After window closes:
- New cycle continues.
- Liability remains intact.

---

## 3.7 Zero-Profit Cycle Behavior

If no profit is generated:

- No LOOP minted
- No ratchet growth (unless prior surplus allows)
- Redemption remains available for matured epochs

YieldLoop does not fabricate growth during inactivity.

---

## 3.8 System Stability Over Time

As epochs mature:

- Some LOOP is redeemed.
- LOOP_supply_E decreases.
- Ratchet_pool_E may accumulate.
- Floor increases require smaller transfers.

This creates a natural time-based strengthening effect.

Older epochs become structurally more valuable due to:

- Reduced liability
- Accumulated surplus
- Ratchet progression

This behavior is mathematical, not promotional.

---

## 3.9 Design Intent

The monthly execution engine is built to:

- Eliminate continuous accounting drift
- Prevent opportunistic mid-cycle behavior
- Align risk, profit, and liability cleanly
- Keep system state auditable at all times

Predictability is a feature.

Not a limitation.
