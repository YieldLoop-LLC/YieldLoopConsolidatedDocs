# YieldLoop — Monthly Accounting & Execution Cycle Specification  
Version: 1.0  
Status: Core Operating Cycle (BNB Chain)  
Scope: Trading → Profit Realization → Fees → LOOP Mint (Epoch) → Ratchet → Redemption Window  

---

# 1. Purpose

This document defines the deterministic monthly operating cycle for YieldLoop, including:

- Vault deposit timing and eligibility
- Trading start and stop boundaries
- Profit realization and fee assessment
- Epoch creation and LOOP minting
- Ratchet evaluation and floor adjustment
- Redemption eligibility and redemption window rules

This cycle is designed to:

- Keep accounting clean and auditable
- Prevent mid-cycle strategy manipulation
- Ensure LOOP liabilities remain fully covered
- Provide consistent user expectations

---

# 2. Cycle Overview (Calendar-Aligned)

YieldLoop operates on a fixed monthly calendar cycle:

- Trading Cycle Start: **1st day of month @ 00:00 UTC**
- Trading Cycle End: **Last day of month @ 23:59 UTC**
- Settlement / Accounting Close: **Immediately after cycle end**
- LOOP Mint Event (Epoch E): **1st day of next month**
- Ratchet Evaluation: **Before redemption window opens**
- Redemption Window: **Defined window on the 1st of the month (or first X days)**

All times are configurable but must remain deterministic and publicly documented.

---

# 3. Vault Deposit Rules

## 3.1 Vault Creation Deposit
- 500 USDT (BEP-20) to create a vault.

## 3.2 Subsequent Deposits
- 250 USDT (BEP-20) minimum per subsequent deposit.

## 3.3 Deposit Timing Cutoff
Deposits are assigned to the next trading cycle if received after a cutoff time.

Default rule:
- Deposits received **before** the monthly cutoff are included in the upcoming cycle.
- Deposits received **after** the cutoff are queued for the following month.

This prevents late-cycle deposits from free-riding on already-realized results.

---

# 4. Strategy Proposal & Lock-In

## 4.1 Strategy Proposal
Before funds are deployed, the system generates an AI-prepopulated configuration including:

- Mode selection: Arbitrage vs Spot Profit Trading (or mixed)
- Trade sizing limits (liquidity-aware enforcement)
- Slippage tolerance bounds
- Max daily trade count / cadence
- Risk caps and asset exposure limits
- DEX routing (PancakeSwap / BiSwap)

## 4.2 User Acceptance Requirement
User must explicitly:

- Accept default configuration, OR
- Modify and accept a custom configuration within allowed bounds

No strategy execution occurs without explicit acceptance.

## 4.3 Strategy Lock
Once the trading cycle begins, the accepted strategy is locked for that cycle.

Permitted mid-cycle actions:
- View-only reporting
- Risk warnings and status updates

Not permitted mid-cycle:
- Strategy changes
- Allocation changes
- Manual parameter overrides

(Withdrawals may occur only at defined cycle boundaries.)

---

# 5. Trading Execution Window

Trading occurs strictly within the monthly trading cycle window.

Execution constraints:
- Liquidity depth sizing must pass
- Net expectancy must be positive after slippage, fees, and gas
- Fail-closed on oracle uncertainty or abnormal conditions
- Emergency halt possible via Admin/MultiSig controls

---

# 6. Cycle Close: Realized Profit & Fee Assessment

At cycle end, YieldLoop calculates:

Realized_Profit_E = (End_Value - Start_Value - External_Flows)

Where External_Flows include:
- Approved deposits for next cycle (excluded)
- Withdrawals at boundary (if applicable)

If Realized_Profit_E ≤ 0:
- No LOOP minted
- No performance fee charged

If Realized_Profit_E > 0:
- Performance Fee = 10% of Realized_Profit_E

Fee distribution:
- 60% → Ratchet_pool_E
- 20% → Ops/Dev
- 10% → Marketing
- 10% → LoopLab

Ratchet_pool_E is epoch-specific and isolated.

---

# 7. User Profit Routing Choice (Per Cycle)

Each vault chooses a profit routing option for the cycle:

Option A: Claim USDT  
- User profit is paid out to Claim Portal as USDT after settlement.

Option B: Receive LOOP  
- User profit is converted to LOOP (epoch-tagged) 1:1 after settlement.

Notes:
- Profit routing is configurable before cycle end.
- Routing choice applies to realized profit for that cycle only.

---

# 8. LOOP Mint Event (Epoch Creation)

On the 1st day of the next month:

If Realized_Profit_E > 0 and user selected LOOP:

- Mint LOOP_supply_E = User_Profit_After_Fee
- Tag minted LOOP to epoch E
- Record epoch ledger entry:
  - LOOP_supply_E
  - Redemption_price_E_initial (default 1.00)
  - Redemption_pool_E_initial
  - Ratchet_pool_E balance (from fees)

If user selected USDT:
- No LOOP minted for that user for epoch E

LOOP is non-transferable and bound to the originating vault/user identity.

---

# 9. Ratchet Evaluation Sequence (Before Redemption Window)

Before redemptions are allowed each month:

For each eligible epoch E:
1. Compute Coverage_ratio_E
2. If Coverage_ratio_E ≥ Trigger_threshold:
   - Apply ratchet increment per algorithm spec
   - Transfer required funds from Ratchet_pool_E → Redemption_pool_E
   - Update Redemption_price_E
3. Recompute Coverage_ratio_E
4. Stop if threshold no longer met or cap reached

This sequence must complete before redemptions open.

---

# 10. Redemption Eligibility & Window

## 10.1 Minimum Holding Period
LOOP minted in epoch E becomes eligible after:

- 3 full calendar months (default)

Example:
- Minted January → Eligible April 1

## 10.2 Redemption Window
Redemptions are allowed only during the monthly redemption window.

Default:
- Opens after ratchet evaluation completes
- Duration: configurable (e.g., 24–72 hours, or first X days)

## 10.3 Redemption Execution
For eligible epoch E:

USDT_out = LOOP_redeemed × Redemption_price_E

Redemption updates:
- Redemption_pool_E decreases by USDT_out
- LOOP_supply_E decreases by LOOP_redeemed
- Coverage_ratio_E recalculated

Fail-closed rule:
- Redemption cannot execute if it would cause Coverage_ratio_E < 1.0 post-transaction.

---

# 11. System Vault Creation (Overflow Collection)

The protocol may accumulate surplus funds (e.g., from over-buffer conditions or defined collection rules).

## 11.1 Collection Trigger
If system-wide collected surplus reaches:
- 500 USDT → create a System Vault

## 11.2 System Vault Behavior
System Vault is configured to:
- Operate under the same monthly cycle rules
- Route its profits to LOOP at a fixed 50/50 mode:
  - 50% profit routed to Redemption strengthening
  - 50% profit routed to Ratchet strengthening

(Exact routing mechanics must be deterministic and documented.)

## 11.3 System Vault Scaling
If System Vault reaches a configured cap:
- Create an additional System Vault
- Repeat behavior

System vaults are protocol-owned and used only to strengthen:
- Redemption pool health
- Ratchet pool growth

---

# 12. Transparency & Reporting (User-Facing)

Users must be able to view:

- Current cycle dates and status
- Strategy settings (accepted + locked indicator)
- Realized profit for last cycle
- Fees paid (10%) and where they went
- LOOP minted by epoch
- Holding/eligibility countdown by epoch
- Redemption price per epoch
- Coverage ratio per epoch
- Ratchet increases applied per epoch
- Redemption history

---

# 13. Failure Modes (Fail-Closed)

If any of the following occur, the system must fail-closed:

- Oracle failure or extreme deviation
- Coverage_ratio_E < 1.0 risk detected
- Liquidity depth constraints violated
- Abnormal slippage or MEV risk spike
- Admin emergency halt triggered

Fail-closed behavior:
- Pause trading and/or redemptions (scope-specific)
- Preserve funds
- Provide user-visible status and reason code

---

# 14. Invariants

At all times:

1. Trading occurs only within the defined monthly cycle.
2. LOOP minting occurs only from realized profit.
3. LOOP is epoch-tagged and non-transferable.
4. Each epoch’s liabilities are isolated.
5. Redemption cannot violate coverage.
6. Ratchet cannot decrease redemption price.

---

# 15. Design Philosophy

YieldLoop prioritizes:

- Deterministic accounting
- Transparent liabilities
- Conservative execution
- Fail-closed safety

The monthly cycle is the backbone that makes the system auditable, enforceable, and comprehensible.
