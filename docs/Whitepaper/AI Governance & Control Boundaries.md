# 8. AI Governance & Control Boundaries

## 8.1 Overview

YieldLoop integrates AI as a parameter optimization and risk adaptation layer.

AI enhances execution efficiency.

AI does not override financial invariants.

The protocol is governed by deterministic rules first, adaptive optimization second.

---

## 8.2 AI Scope of Authority

AI may:

- Propose strategy configurations for each vault.
- Adjust spread thresholds within allowed bounds.
- Modify liquidity fraction caps.
- Adjust slippage tolerance within limits.
- Modify volatility sensitivity parameters.
- Adjust risk caps based on drawdown state.
- Shift arbitrage vs spot allocation ratios.
- Recommend temporary trading pauses under abnormal conditions.

All AI actions must remain within predefined hard bounds.

---

## 8.3 Hard-Bound Enforcement Layer

AI cannot:

- Change performance fee percentage.
- Alter fee allocation splits.
- Modify redemption formulas.
- Modify ratchet trigger thresholds beyond governance limits.
- Mint LOOP.
- Access or transfer treasury funds.
- Cross-subsidize epochs.
- Override coverage invariants.
- Reduce Redemption_price_E.
- Bypass holding requirements.

Financial invariants are enforced at contract level.

AI cannot bypass smart contract constraints.

---

## 8.4 Human Oversight & Emergency Controls

A defined administrative multi-signature authority may:

- Pause trading.
- Pause redemptions.
- Trigger emergency halt.
- Upgrade contract modules (subject to governance policy).

Administrative authority may not:

- Withdraw user funds.
- Modify epoch liabilities.
- Alter redemption balances.
- Break isolation rules.

Emergency powers exist only for safety, not economic manipulation.

---

## 8.5 Governance Parameter Bounds

Governance (via NFT holders or defined mechanism) may vote within bounded ranges for:

- Risk cap ranges
- Liquidity fraction limits
- Spread threshold floors
- Ratchet trigger thresholds (within safety band)
- Maximum monthly ratchet increment

Governance may not:

- Set parameters that violate Coverage_ratio_E ≥ 1.0
- Remove epoch isolation
- Override holding requirement
- Convert LOOP into transferable token
- Change minting from realized-profit-only model

Governance operates within guardrails.

---

## 8.6 Deterministic First, Adaptive Second

YieldLoop follows this hierarchy:

1. Hard-coded invariants
2. Smart contract enforcement
3. Governance-approved parameter bounds
4. AI optimization within bounds

If conflict arises:

Lower-numbered layer prevails.

AI cannot overrule invariants.

---

## 8.7 Transparency Requirements

Users must be able to view:

- Current active AI parameter set
- Historical parameter adjustments
- Reason codes for strategy shifts
- Volatility mode activation
- Drawdown mode activation

AI must not operate as a black box.

Execution transparency builds trust.

---

## 8.8 Design Philosophy

AI is used to:

- Improve efficiency
- Adapt to market conditions
- Reduce human reaction delay

AI is not used to:

- Manufacture yield
- Modify liabilities
- Override coverage protection

YieldLoop is a rule-based financial engine with adaptive tuning.

Not an AI-driven discretionary fund.
