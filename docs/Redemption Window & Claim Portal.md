# YieldLoop — Redemption Window & Claim Portal Specification  
Version: 1.0  
Status: User-Facing Settlement Layer  
Scope: LOOP Redemption, USDT Claims, Eligibility Enforcement  

---

# 1. Purpose

This document defines:

- How users redeem eligible LOOP
- How USDT claims are processed
- How redemption windows are enforced
- How epoch isolation is preserved during settlement
- How fail-closed protections are applied

This layer connects:

Liability math ↔ User experience

---

# 2. Core Principles

1. LOOP is non-transferable.
2. LOOP is epoch-tagged.
3. Redemption is eligibility-gated.
4. Redemption is coverage-constrained.
5. No queue systems.
6. No IOUs.
7. No pending liability spillover.

Redemption must always be backed at time of execution.

---

# 3. Claim Portal Overview (UI Layer)

The Claim Portal must display:

## 3.1 USDT Claim Balance
- Available USDT from most recent cycle (if selected)
- Claimable immediately after settlement

## 3.2 LOOP Ledger by Epoch
Table view:

| Epoch | LOOP Minted | Held Duration | Eligible | Redemption Price | Redeemable USDT | Status |

Example:

Jan 2026 | 120 LOOP | 3 Months | Yes | $1.05 | $126.00 | Redeem  
Feb 2026 | 90 LOOP | 2 Months | No | $1.02 | Locked | Countdown  

---

# 4. Redemption Eligibility Rules

For epoch E:

Eligible if:

Current_Date ≥ (Epoch_Mint_Date + 3 full calendar months)

Eligibility is calculated deterministically.

No early unlock.
No manual override.

---

# 5. Redemption Window Rules

Redemptions may occur only during the defined monthly redemption window.

Default configuration:

- Opens after ratchet evaluation completes.
- Closes after configured duration (e.g., 72 hours).

Outside window:
- Redemption function disabled.
- UI displays countdown to next window.

This prevents continuous liability drain and stabilizes liquidity.

---

# 6. Redemption Execution Logic

User selects:

- Epoch
- Amount of LOOP to redeem (partial or full)

System calculates:

USDT_out = LOOP_redeemed × Redemption_price_E

Before execution:

Simulate post-redemption state:

Redemption_pool_E_post = Redemption_pool_E - USDT_out  
LOOP_supply_E_post = LOOP_supply_E - LOOP_redeemed  

Compute:

Coverage_ratio_E_post = Redemption_pool_E_post / (LOOP_supply_E_post × Redemption_price_E)

If Coverage_ratio_E_post ≥ 1.0:

    Execute redemption
    Transfer USDT_out to user
    Burn redeemed LOOP
    Update epoch ledger

Else:

    Reject transaction (fail-closed)

---

# 7. Partial Redemption Behavior

Partial redemption is allowed.

Effects:

- LOOP_supply_E decreases
- Redemption_pool_E decreases proportionally
- Ratchet_pool_E remains unaffected
- Coverage ratio recalculated

As LOOP_supply_E shrinks, required ratchet transfer per increment decreases.

This may accelerate floor increases over time.

This behavior is intentional and sustainable.

---

# 8. USDT Direct Claim Behavior

If user selected USDT routing for that cycle:

- Profit credited to USDT Claim Balance
- Available immediately after settlement
- No holding requirement
- No ratchet participation

Claimed USDT does not affect LOOP liability.

---

# 9. Redemption Limits & Safeguards

## 9.1 Per-Transaction Limits (Optional)
Protocol may define max % of epoch supply redeemable per transaction.

Example:
- Max 25% of epoch supply per wallet per window.

Purpose:
- Prevent shock redemptions.
- Smooth liquidity drain.

## 9.2 System Halt Condition
If system-wide abnormal conditions detected:

- Redemption window may be paused
- Reason code must be displayed
- Funds remain secure

---

# 10. Redemption Queue Policy

There is no queue system.

If coverage exists → redemption executes instantly.

If coverage insufficient → transaction fails immediately.

No pending states.

---

# 11. Transparency Requirements

Claim Portal must display:

- Redemption_pool_E
- Ratchet_pool_E
- Coverage_ratio_E
- Redemption_price_E history
- Ratchet increases applied

Users must be able to independently verify:

Redemption_pool_E ≥ LOOP_supply_E × Redemption_price_E

This builds confidence.

---

# 12. Failure Modes

Redemption must fail-closed if:

- Coverage invariant would be violated
- Oracle failure
- Contract state inconsistency
- Epoch isolation rule breach

Under no circumstances may redemption:

- Create negative coverage
- Borrow from another epoch
- Create IOU debt

---

# 13. Edge Case: Full Epoch Redemption

If LOOP_supply_E → 0:

Then:

- Redemption_pool_E should also approach 0
- Remaining Ratchet_pool_E may be:
    - Burned
    - Rolled into protocol surplus pool
    - Used to seed System Vault

This must be predefined in treasury policy.

---

# 14. Design Philosophy

The Claim Portal is not cosmetic.

It is a liability control surface.

Every redemption must preserve:

- Mathematical coverage
- Epoch isolation
- Deterministic behavior

User experience must reflect real-time system state.

No abstraction.
No hidden smoothing.
