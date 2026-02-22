# 9. Sustainability & Coverage Model

## 9.1 Overview

YieldLoop is designed to remain structurally solvent under:

- Low-profit conditions
- Flat market conditions
- Elevated redemption activity
- Volatility spikes
- AUM growth phases

Sustainability is not dependent on continuous profit.

It is dependent on invariant enforcement and surplus discipline.

---

## 9.2 Liability Structure Recap

For each epoch E:

LOOP_supply_E = realized profit minted 1:1  
Redemption_pool_E = backing reserve  
Redemption_price_E ≥ 1.00  
Coverage_ratio_E = Redemption_pool_E / (LOOP_supply_E × Redemption_price_E)

System invariant:

Coverage_ratio_E ≥ 1.0

Redemptions cannot violate this.

This prevents insolvency by design.

---

## 9.3 Zero-Profit Scenario

If multiple consecutive months produce:

Realized_Profit_E = 0

Then:

- No new LOOP is minted.
- No new ratchet surplus is created.
- Redemption_price_E remains stable.
- Eligible redemptions may still execute.

System behavior:

- Growth pauses.
- Liability does not expand.
- Coverage remains intact.

YieldLoop does not depend on exponential growth to remain solvent.

---

## 9.4 Elevated Redemption Scenario

If many users redeem at eligibility:

Effects per epoch:

- LOOP_supply_E decreases.
- Redemption_pool_E decreases proportionally.
- Coverage_ratio_E remains ≥ 1.0.
- Ratchet_pool_E remains intact.

As LOOP_supply_E shrinks:

Future ratchet increments require smaller capital transfers.

Older epochs may strengthen structurally after partial redemptions.

There is no bank-run dynamic because:

Redemptions are limited to eligible epochs and enforced by coverage rules.

---

## 9.5 High AUM Growth Scenario

As vault capital increases:

- Trade sizing scales sub-linearly.
- Liquidity caps prevent dominance.
- Edge compression is mitigated.

YieldLoop is designed to prioritize expectancy preservation over volume maximization.

Growth without discipline destroys edge.

Edge preservation ensures long-term viability.

---

## 9.6 Ratchet Sustainability

Ratchet increases occur only when:

Coverage_ratio_E ≥ Trigger_threshold

If surplus is insufficient:

- Ratchet does not move.
- Floor remains stable.

No artificial appreciation occurs.

This ensures:

Redemption_price_E growth is surplus-backed, not narrative-driven.

---

## 9.7 Stress Scenario: Combined Conditions

Consider:

- 2–3 months of low profit.
- Moderate redemptions.
- Volatility spike.

System response:

1. No new LOOP minted.
2. Ratchet pauses.
3. Coverage remains ≥ 1.0.
4. Drawdown guard reduces trade size.
5. Trading may pause if risk bounds breached.

Liability does not expand.
Floor does not decrease.
Redemptions remain coverage-bound.

The system slows.
It does not implode.

---

## 9.8 System Vault Contribution

Protocol-owned System Vaults may:

- Recycle accumulated surplus.
- Strengthen redemption pools.
- Strengthen ratchet pools.

System Vaults operate under same execution constraints.

They are reinforcement tools, not inflation tools.

---

## 9.9 No Reflexive Spiral Risk

YieldLoop avoids reflexive collapse because:

- LOOP is non-transferable.
- No secondary market panic pricing exists.
- No rebasing mechanics exist.
- No emission schedule exists.
- No cross-epoch blending exists.

Liability expansion is directly tied to realized profit only.

This prevents structural overextension.

---

## 9.10 Long-Term Stability Outcome

Over time:

- Older epochs may strengthen.
- New epochs begin at baseline.
- Liability grows only with realized profit.
- Redemption coverage remains enforceable.

Sustainability emerges from constraint.

Not promise.

---

## 9.11 Design Conclusion

YieldLoop remains solvent if:

1. Coverage invariants are enforced.
2. Trade sizing discipline is maintained.
3. Ratchet only converts real surplus.
4. Governance does not override guardrails.

The system is built to:

Slow down safely before it ever fails.
