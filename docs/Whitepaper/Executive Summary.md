# Executive Summary

## Overview

YieldLoop is a structured, epoch-based profit engine operating on BNB Chain.

It converts realized DeFi trading profit into a time-layered, non-transferable redemption certificate (LOOP), backed 1:1 by closed profit and strengthened through a deterministic ratchet mechanism.

YieldLoop does not promise yield.

It enforces surplus-backed growth under strict liability isolation and liquidity-aware execution constraints.

---

## Core Mechanism

Each calendar month forms a separate Epoch.

For each epoch:

- Trading occurs under strict risk controls.
- Realized profit is calculated at cycle close.
- A flat 10% performance fee is applied.
- Net profit may be taken as USDT or minted 1:1 into LOOP.
- LOOP minted in that month is tagged to its epoch.
- Each epoch maintains its own redemption pool and ratchet pool.
- No cross-epoch subsidy is permitted.

LOOP is non-transferable and redeemable only after a 3-month holding period.

---

## Redemption Structure

Each epoch begins with:

Redemption_price = 1.00 USDT

Redemption_price may increase over time via a surplus-funded ratchet mechanism.

Redemption price may never decrease.

Redemption can only execute if full coverage remains intact.

Coverage is defined as:

Redemption_pool ≥ LOOP_supply × Redemption_price

If this condition would be violated, redemption fails.

---

## Ratchet Mechanism

60% of performance fees from each epoch are allocated to that epoch’s ratchet pool.

When surplus exceeds a defined threshold, the protocol increases the redemption floor.

Floor increases are:

- Surplus-funded
- Deterministic
- Non-decreasing
- Epoch-isolated

Older epochs may strengthen structurally over time as liabilities shrink and surplus accumulates.

---

## Trading Discipline

YieldLoop trades only when:

- Liquidity depth supports position size.
- Net expectancy remains positive after all costs.
- Slippage and volatility fall within defined bounds.
- Drawdown limits are respected.

Trade sizing scales sub-linearly with AUM to preserve edge.

The system prefers inactivity over forced execution.

---

## NFT Incentive Layer

Genesis Lite and Genesis Lifetime NFTs provide:

- Performance fee reductions
- Deposit credit participation
- Governance access within bounded limits

NFTs do not alter liability math, redemption coverage, or epoch isolation.

---

## AI Governance

AI operates within hard-coded constraints.

AI may optimize parameters such as:

- Spread thresholds
- Risk caps
- Liquidity fraction limits

AI may not:

- Mint LOOP
- Alter redemption math
- Override coverage invariants
- Blend epochs

Deterministic contract rules supersede AI decisions.

---

## Sustainability Model

YieldLoop remains solvent under:

- Zero-profit months
- Elevated redemptions
- Volatility spikes
- Flat market conditions

Because:

- LOOP expands only from realized profit.
- Redemption coverage is enforced.
- Ratchet moves only when surplus exists.
- No emission schedule exists.
- No reflexive token market exists.

The system slows before it fails.

---

## Design Intent

YieldLoop replaces inflationary yield models with:

- Realized profit accounting
- Epoch-based liability isolation
- Surplus-driven floor increases
- Liquidity-respecting execution
- Fail-closed safeguards

Growth is a byproduct of discipline.

Stability is the primary objective.
