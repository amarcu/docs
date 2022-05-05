---
description: Pushes the data obtained from Oracles into Collybus
---

# 🗣 Relayer

### 🔎 High-level Overview

A Relayer is the glue between the data sources and the core FIAT DAO system. Its purpose is to determine when the values should be updated and do the necessary actions in order to push the new rates into Collybus. It is a manager that stands between oracles and the Collybus.

### 🐣 Initialization

A Relayer has to know a few details about the overall system in order to function correctly. Most of its parameters are saved at deploy time (some parameters can be changed later).

A Relayer needs these parameters on deployment:

* `collybus` - Collybus address; where here the Relayer pushes the retrieved values;
* `type` - determines the Relayer type; we have two types: [DiscountRate](https://github.com/fiatdao/delphi/blob/67d77e1a46995456ada05c25d1eade9029ba068e/src/relayer/IRelayer.sol#L6), [SpotPrice](https://github.com/fiatdao/delphi/blob/67d77e1a46995456ada05c25d1eade9029ba068e/src/relayer/IRelayer.sol#L7); based on the type, it calls different methods in Collybus;
* `oracle` - address of the oracle the Relayer needs to query;
* `tokenId` - key of the asset for which it tracks the value; this is used when pushing the value into Collybus;
* `minimumPercentageDeltaValue` - minimum percentage delta between the last pushed value and the current value to push a new update. The value saved in the Relayer is divided by 10000 thus a value of 5000 represents 50%, and 25 represents 0.25%.

All of the initial parameters are `immutable` (they can't change) except for `minimumPercentageDeltaValue` which can be updated by a governance vote.

### 🌈 Execution Flow

![](<../../.gitbook/assets/Collybus Diagram.png>)

The execution starts with a [Gelato Network](https://www.gelato.network) task trying to execute [Relayer.executeWithRevert()](https://github.com/fiatdao/delphi/blob/67d77e1a46995456ada05c25d1eade9029ba068e/src/relayer/Relayer.sol#L107-L114) successfully at each block. Most of the time, the execution will revert because the value reported by the oracle does not differ enough (by at least `minimumPercentageDeltaValue` ).&#x20;

The execution must revert if it should not commit any change in the system. Reverting is important to integrate with Gelato correctly. The type of task we use tries to trigger a successful execution at each block. Their private relayers simulate the execution, and a transaction is not sent to the chain if the execution reverts. However, if the execution does not revert, a transaction is relayed through their private relayer system, and the Relayer pushes a new value into Collybus.

First, the oracle is updated, and the value is retrieved. Collybus is updated if the retrieved value is far enough from the previously pushed value.

Execution ends early if the oracle doesn't need an update, the value is not far enough from the previously pushed value, or the oracle's next value will not trigger an update. This is all encoded in the `execute` method, making it return `false`. The value of execute is checked in `executeWithRevert()`, making it revert if `execute()` returns `false`, and allowing successful execution otherwise.

### 📑 Public Methods

These methods can be called by anyone.

#### `collybus`

Returns the Collybus address

#### `relayerType`

Relayer type:`DiscountRate` or `SpotPrice`

#### `oracle`

Oracle address

#### `encodedTokenId`

key used in Collybus when pushing a value

#### `minimumPercentageDeltaValue`

minimum threshold deviation enabling updates

#### `execute`

Triggers oracle updates and pushes new rates into Collybus.

Returns `false` if no state change should be preserved. This is in case the Oracle did not obtain a new significant value or the currently reported value is not significant. To be significant, the value needs to pass the defined `minimumPercentageDeltaValue`.

#### `executeWithRevert`

Calls `execute` and reverts if `execute` returns false

### 👮‍♂️ Guarded Methods

These methods can be called by approved actors.

#### `setParam`

changes `minimumPercentageDeltaValue`

This value is used to determine if the newly read value from the Oracle is different enough to be pushed into Collybus

### 📘 References

* [Implementation](https://github.com/fiatdao/delphi/tree/master/src/relayer)

