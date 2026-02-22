# 5. Ratchet Growth Mechanism

## 5.1 Purpose

The Ratchet Mechanism exists to convert surplus into permanent increases in redemption value.

It does not generate yield.

It reallocates accumulated surplus from performance fees into a higher redemption floor for each epoch.

The ratchet may only move upward.
It may never reverse.

---

## 5.2 Source of Ratchet Funding

For each epoch E:

60% of the 10% performance fee is allocated to Ratchet_pool_E.

No other capital source may feed the ratchet.

There are:

- No emissions.
- No treasury injections.
- No cross-epoch borrowing.

Ratchet growth is surplus-driven only.

---

## 5.3 Trigger Condition

A ratchet increase may occur only when:

Coverage_ratio_E ≥ Trigger_threshold

Default Trigger_threshold = 1.05

This means the redemption pool must hold at least 5% surplus relative to current liability before the floor may rise.

If this condition is not met, the floor remains unchanged.

---

## 5.4 Increment Logic

When trigger conditions are satisfied:

- A deterministic increment (Δ_price_E) is calculated.
- Required transfer = Δ_price_E × LOOP_supply_E.
- Funds are moved from Ratchet_pool_E to Redemption_pool_E.
- Redemption_price_E increases accordingly.

After adjustment:

Coverage_ratio_E must remain ≥ 1.0.

If post-adjustment coverage would fall below safety threshold, the ratchet increment is reduced or skipped.

---

## 5.5 Non-Decreasing Floor

Redemption_price_E is strictly non-decreasing.

It may:

- Increase.
- Remain constant.

It may never decline.

This ensures:

- No retroactive value reduction.
- No floor manipulation.
- No negative repricing of older LOOP.

---

## 5.6 Acceleration Effect Over Time

As LOOP_supply_E decreases due to redemptions:

- The required transfer per increment decreases.
- Ratchet_pool_E may continue to accumulate.
- Floor increases become easier to achieve.

This creates a natural strengthening effect for older epochs.

This effect is structural, not promotional.

---

## 5.7 Maximum Growth Constraints

To prevent instability during unusually large profit cycles:

The protocol may define:

- Maximum floor increase per month.
- Maximum cumulative increase per evaluation cycle.

These limits prevent runaway repricing and preserve long-term sustainability.

---

## 5.8 Zero-Profit Behavior

If an epoch generates no additional surplus:

- Ratchet_pool_E does not grow.
- Floor remains unchanged unless prior surplus allows movement.
- No artificial floor increase occurs.

The ratchet never fabricates value.

---

## 5.9 System Stability Considerations

The ratchet mechanism is designed to:

- Strengthen older epochs gradually.
- Avoid sudden liability expansion.
- Ensure coverage is always maintained.
- Convert surplus into durable value.

Because each epoch is isolated:

A stalled epoch does not affect another.

---

## 5.10 Economic Outcome

Over time:

- Older LOOP may redeem at higher values.
- Newer LOOP begins at baseline value.
- Patience aligns with structural advantage.

This design encourages:

- Time alignment.
- Reduced redemption volatility.
- Sustainable surplus deployment.

The ratchet is not an incentive program.

It is a surplus allocation rule.
