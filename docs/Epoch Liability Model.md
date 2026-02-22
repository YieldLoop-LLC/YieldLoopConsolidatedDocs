# YieldLoop — Epoch Liability Model  
Version: 1.0  
Status: Core Financial Invariant Specification  
Scope: LOOP Minting, Redemption, Ratchet Mechanics  

---

# 1. Purpose

This document defines the deterministic liability structure governing:

- LOOP minting
- Epoch isolation
- Redemption eligibility
- Ratchet floor increases
- Coverage enforcement

This model ensures:

- No cross-epoch subsidy
- No synthetic yield
- No redemption insolvency
- Deterministic floor progression

All LOOP liabilities must be fully accounted for at all times.

---

# 2. Definitions

Each calendar month constitutes an **Epoch (E)**.

Example:
- E_2026_01
- E_2026_02

For each epoch E:

LOOP_supply_E  
= Total LOOP minted from realized profit during that epoch.

Redemption_pool_E  
= USDT allocated and reserved to back LOOP_supply_E.

Ratchet_pool_E  
= Portion of performance fees allocated to increasing redemption value of LOOP_supply_E.

Redemption_price_E  
= Current redeemable USDT value per LOOP for that epoch.

Coverage_ratio_E  
= Redemption_pool_E / (LOOP_supply_E × Redemption_price_E)

Invariant requirement:

Coverage_ratio_E ≥ 1.0 at all times.

---

# 3. LOOP Minting Rule

At the close of each monthly trading cycle:

If Realized_Profit_E > 0:

    LOOP_supply_E = Realized_Profit_E

LOOP is minted strictly 1:1 with realized profit after trade settlement.

No speculative minting.
No forward minting.
No inflation outside realized profit.

---

# 4. Performance Fee Allocation

Flat performance fee: 10% of realized profit.

Distribution:

- 60% → Ratchet_pool_E
- 20% → Ops/Dev
- 10% → Marketing
- 10% → LoopLab

Ratchet allocation is epoch-specific.

No cross-epoch blending permitted.

---

# 5. Redemption Eligibility

LOOP minted in epoch E becomes eligible for redemption after:

Minimum holding period = 3 full calendar months.

Example:

Minted: January  
Eligible: April 1  

Redemption only allowed during defined monthly redemption window.

Partial redemption permitted.

---

# 6. Initial Redemption Price

Upon mint:

Redemption_price_E_initial = 1.00 USDT

Redemption_pool_E_initial = LOOP_supply_E × 1.00

Coverage_ratio_E_initial = 1.0

---

# 7. Ratchet Mechanism

Ratchet increases Redemption_price_E only when surplus exists.

Condition for ratchet trigger:

If Coverage_ratio_E ≥ Trigger_threshold

Where:

Trigger_threshold = 1.05 (configurable but must exceed 1.0)

Then:

Δ_price_E = predefined increment (deterministic rule)

Required_transfer = Δ_price_E × LOOP_supply_E

Transfer Required_transfer from Ratchet_pool_E to Redemption_pool_E

Update:

Redemption_price_E = Redemption_price_E + Δ_price_E

Recalculate Coverage_ratio_E

Ratchet may never reduce Redemption_price_E.

Floor only moves upward.

---

# 8. Cross-Epoch Isolation Rule

For any two epochs E1 and E2:

Redemption_pool_E1 may not be used to cover LOOP_supply_E2.

Ratchet_pool_E1 may not be used to increase Redemption_price_E2.

Each epoch is financially independent.

System-wide reporting may aggregate totals for transparency, but liabilities remain isolated.

---

# 9. Redemption Execution Rule

At redemption:

User submits LOOP amount from eligible epoch E.

USDT returned = LOOP_redeemed × Redemption_price_E

Redemption_pool_E decreases accordingly.
LOOP_supply_E decreases accordingly.

Coverage_ratio_E recalculated immediately.

Redemption may not proceed if Coverage_ratio_E would fall below 1.0 post-transaction.

Fail-closed enforcement required.

---

# 10. Zero-Profit Scenario

If Realized_Profit_E ≤ 0:

No LOOP minted.
No Ratchet_pool_E created.

Existing epochs continue independently.

Ratchet may stall.
Redemption remains available only if coverage permits.

System does not promise yield.

---

# 11. Invariants Summary

At all times:

1. LOOP_supply_E == cumulative realized profit for epoch E
2. Redemption_price_E ≥ 1.00
3. Coverage_ratio_E ≥ 1.00
4. Redemption_price_E is non-decreasing
5. No cross-epoch subsidy

Violation of any invariant halts redemptions automatically.

---

# 12. Design Philosophy

YieldLoop does not create value through inflation.

All LOOP represents:

Realized, closed, on-chain profit.

Ratchet increases are funded only by allocated surplus.

The system must fail closed before it fails insolvent.
