# 2. System Architecture Overview

## 2.1 High-Level Structure

YieldLoop operates as a modular, layered protocol on BNB Chain composed of:

1. Wallet & Access Layer  
2. Vault Layer  
3. Trading Engine  
4. Epoch Mint Engine  
5. Ratchet & Redemption Pools  
6. Claim & Redemption Portal  
7. NFT Program Layer  
8. Governance & AI Control Layer  

Each layer operates independently but under shared invariants.

---

## 2.2 Wallet & Access Layer

Users may access YieldLoop through:

- Standard Web3 wallet connection (BNB Chain compatible)
- Embedded wallet creation (Privy)
- Fiat on-ramp funding (Transak)

All assets are BEP-20 tokens on BNB Chain.

Supported allocation tokens (wrapped/BEP-20 representations):

- BTC
- ETH
- BNB
- SOL
- XRP
- USDT
- USDC
- DAI

All custody remains user-controlled via wallet ownership.

YieldLoop never takes custodial ownership of user identity keys.

---

## 2.3 Vault Layer

Each user operates through one or more Vaults.

### 2.3.1 Vault Creation

- Minimum 500 USDT to create a vault.
- Subsequent deposits: minimum 250 USDT.

### 2.3.2 Allocation Model

Users allocate capital 0–100% across supported tokens.

Vault allocation is locked per cycle once trading begins.

### 2.3.3 Cycle Participation

Vaults participate in monthly trading cycles.

Deposits after cutoff are queued for next cycle.

Withdrawals permitted only at cycle boundaries.

---

## 2.4 Trading Engine

The Trading Engine executes strategies including:

- Cross-DEX arbitrage (PancakeSwap ↔ BiSwap)
- Spot profit buy/sell execution

Execution is constrained by:

- Liquidity-aware trade sizing
- Positive net expectancy enforcement
- Slippage bounds
- Gas thresholds
- Drawdown guards
- Volatility filters

No trade may execute without passing all enforcement checks.

The trading engine operates only within defined monthly cycle windows.

---

## 2.5 Epoch Mint Engine

At the end of each cycle:

If realized profit > 0:

- Performance fee (10%) is applied.
- 60% of fee allocated to epoch-specific Ratchet_pool_E.
- Remaining allocations distributed per treasury policy.

Users choose profit routing:

- USDT direct claim
- LOOP mint (epoch-tagged)

LOOP is minted strictly 1:1 from realized profit after fees.

Each month forms a new Epoch (E).

---

## 2.6 Ratchet & Redemption Pools

Each epoch maintains isolated pools:

- Redemption_pool_E
- Ratchet_pool_E

These pools:

- Are never blended across epochs.
- Operate under strict coverage invariants.
- Govern Redemption_price_E progression.

Redemption_price_E is non-decreasing.

Ratchet execution occurs only when surplus exists.

---

## 2.7 Claim & Redemption Portal

Users interact with:

- USDT claim balances
- LOOP ledger by epoch
- Eligibility countdown
- Redemption window interface

Redemptions:

- Allowed only after minimum holding period (3 full calendar months).
- Allowed only during defined redemption windows.
- Must preserve coverage invariants.
- Execute instantly or fail.

No queue systems.
No IOUs.

---

## 2.8 System Vault

Protocol may create internal System Vaults when surplus accumulation thresholds are met.

System Vaults:

- Operate under same trading rules.
- Route profits in predefined strengthening split.
- Reinforce Redemption and Ratchet pools.
- Create recursive structural stability.

System Vaults are protocol-owned and transparent.

---

## 2.9 NFT Program Layer

Two NFT tiers exist:

Genesis Lite  
- 12-month benefits  
- Performance fee reduction  
- Deposit credit pool participation  

Genesis Lifetime  
- Lifetime benefits  
- Larger fee reduction  
- Larger deposit credit pool share  

NFTs do not alter:

- Redemption math
- Epoch liability
- Coverage invariants

NFTs influence fee treatment and user incentives only.

---

## 2.10 Governance & AI Control Layer

AI may:

- Propose trade parameters
- Adjust spread thresholds
- Adjust risk caps
- Modify trade sizing constraints within hard bounds

AI may not:

- Change fee splits
- Modify redemption formula
- Mint LOOP
- Access treasury outside rule triggers
- Break epoch isolation

Hard-coded invariants supersede AI decisions.

Administrative override (MultiSig) may:

- Pause trading
- Pause redemption
- Trigger emergency halt

But may not violate liability coverage rules.

---

## 2.11 Design Summary

YieldLoop architecture separates:

- Execution risk
- Liability accounting
- Incentive mechanics
- Governance controls

Each module enforces constraints that protect the whole.

No single layer may override core financial invariants.
