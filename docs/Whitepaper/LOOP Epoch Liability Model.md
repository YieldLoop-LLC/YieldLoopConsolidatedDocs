# 4. LOOP Epoch Liability Model

## 4.1 Overview

YieldLoop structures all LOOP issuance into isolated monthly epochs.

Each epoch represents:

- A discrete period of realized profit.
- A self-contained liability pool.
- An independently ratcheting redemption structure.

There is no blending of liabilities across months.

Each epoch stands on its own balance sheet.

---

## 4.2 Epoch Definition

An Epoch (E) corresponds to one completed calendar month of trading.

For each epoch, the protocol records:

- LOOP_supply_E  
- Redemption_pool_E  
- Ratchet_pool_E  
- Redemption_price_E  
- Coverage_ratio_E  

These values are tracked independently for each month.

---

## 4.3 LOOP Minting Rule

LOOP is minted only when:

Realized_Profit_E > 0

Minting occurs strictly 1:1 with net realized profit (after performance fee).

There is:

- No emission schedule.
- No staking inflation.
- No speculative minting.
- No forward yield.

If profit is zero, no LOOP is created.

This ensures LOOP represents realized economic activity.

---

## 4.4 Redemption Pool

Each epoch begins with:

Redemption_price_E_initial = 1.00 USDT

Redemption_pool_E_initial = LOOP_supply_E × 1.00

The redemption pool backs the liability of that epoch.

At all times:

Redemption_pool_E ≥ LOOP_supply_E × Redemption_price_E

This condition defines full coverage.

---

## 4.5 Coverage Ratio

For each epoch:

Coverage_ratio_E =  
Redemption_pool_E ÷ (LOOP_supply_E × Redemption_price_E)

System invariant:

Coverage_ratio_E ≥ 1.0

If a redemption would reduce coverage below 1.0, it fails.

Coverage cannot be compromised.

---

## 4.6 Ratchet Pool

60% of performance fees from epoch E are allocated to Ratchet_pool_E.

Ratchet_pool_E is used exclusively to increase Redemption_price_E.

It may not:

- Cover another epoch’s liability.
- Be blended into global reserves.
- Offset unrelated deficits.

Each epoch’s surplus belongs to that epoch.

---

## 4.7 Epoch Isolation Rule

YieldLoop enforces strict isolation:

- Redemption_pool_E1 cannot support LOOP_supply_E2.
- Ratchet_pool_E1 cannot increase Redemption_price_E2.
- Cross-subsidization is prohibited.

This prevents dilution and hidden liability blending.

Isolation creates transparency and structural integrity.

---

## 4.8 Redemption Price Behavior

Redemption_price_E:

- Starts at 1.00 USDT.
- May increase via ratchet mechanism.
- May never decrease.

Redemption price increases only when surplus exists.

No artificial floor adjustments are permitted.

---

## 4.9 Holding Requirement

LOOP minted in epoch E becomes eligible for redemption after:

3 full calendar months.

Example:
Minted January → Eligible April 1.

This delay:

- Allows ratchet accumulation.
- Stabilizes liability turnover.
- Encourages time alignment with system growth.

---

## 4.10 Partial Redemption Effects

When LOOP from epoch E is redeemed:

- LOOP_supply_E decreases.
- Redemption_pool_E decreases proportionally.
- Ratchet_pool_E remains unchanged.
- Coverage_ratio_E recalculates.

As LOOP_supply_E declines:

Future ratchet increments require smaller transfers.

Older epochs naturally strengthen over time.

---

## 4.11 Zero-Profit Epochs

If no profit is generated in a given month:

- No LOOP minted.
- No Ratchet_pool_E created.
- Existing epochs remain unaffected.

The system does not fabricate growth.

---

## 4.12 Liability Transparency

For each epoch, users may view:

- LOOP_supply_E
- Redemption_pool_E
- Ratchet_pool_E
- Redemption_price_E
- Coverage_ratio_E

Users must be able to independently verify:

Redemption_pool_E ≥ LOOP_supply_E × Redemption_price_E

Transparency is mandatory.

---

## 4.13 Design Outcome

The Epoch Liability Model ensures:

- LOOP supply grows only from realized profit.
- Older LOOP accrues structural advantage.
- Redemption remains fully backed.
- No hidden blending of liabilities occurs.
- Growth is surplus-driven, not emission-driven.

This structure replaces narrative incentives with mathematical discipline.
