---
description: Describing the current ways in which FIAT borrow rates operate
---

# Borrow Rates

Each supported asset type is represented within the FIAT protocol via _Collateral Vaults_. Collateral vault smart contracts contain the risk parameters that are to be applied to the specific asset types. Among these parameters is the borrow `rate`, or the cost of minting FIAT against that specific collateral type.

## Status Quo

**How are borrow rates set?**

Borrow rates are currently set by FIAT DAO governance. When a new collateral vault is deployed, it is assigned a static but not immutable borrowing rate value. This value is generally determined via a risk assessment framework discussed by the community. The `rate` value can be changed at any time either through **a)** on-chain token holder vote, or **b)** on-chain guardian multisig intervention.

**Why do different collateral types have different borrow rates?**

Each collateral type represents a different risk profile to FIAT. As the protocol currently takes on full exposure to deposited collateral, it must be compensated for doing so.

**How does borrowing interest accrue?**

Borrowing interest accrues on a per block basis that annualizes out to the borrow rate figure reflected on the FIAT DAO user interface. This debt accrues in terms of $FIAT, meaning a user must source more than the original amount of $FIAT minted to pay off their debt.&#x20;

**What happens to paid down interest?**

Interest paid down by users accrues to the FIAT DAO treasury. FDT token-holders are the only entities capable of deploying these proceeds.

## Future Implementation

Core development is underway on an algorithmic interest rate model for FIAT collateral vaults. Details will be shared throughout Q3 2022 but essentially, borrow rates will become a function of capital committed to liquidating bad debt positions from a given collateral vault.

