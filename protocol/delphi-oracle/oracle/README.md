---
description: Obtains the on-chain data and it's ready to report it
---

# Oracle

### 🔎 High-level Overview

The Oracle is an `abstract` contract which needs to be inherited and completely implemented. The basic functionality is all there, each Oracle implementation needs to handle their own way of gathering and validating the data. The `abstract` Oracle handles time delays, making the data available, as well as emitting the relevant events.

### 🐣 Initialization

An Oracle has a simple initialization process because most of the complexity will be contained in the Oracle implementations that inherit the `abstract` Oracle.

An oracle needs these parameters on deployment:

* `timeUpdateWindow` - This interval is defined as the minimum number of seconds between subsequent updates. The oracle has to wait at least `timeUpdateWindow` between updates.

All parameters are `immutable` (they can't change) after the contract was deployed.

### 🌈 Execution Flow

#### Update

An external trusted actor needs to call `update` to make the Oracle retrieve the data it needs. The update method is also protected against re-entrancy. First, the Oracle checks if enough time has passed since the previous execution (using `timeUpdateWindow` ), then it proceeds to obtain the on-chain value.

To reduce the problems that might arise from doing external calls to external systems, an external call is simulated towards an `abstract` method that the Oracle implementation contains. This is done by using a [Solidity feature known as `try/catch` ](https://docs.soliditylang.org/en/v0.8.13/control-structures.html#try-catch)but it can also be achieved by doing low-level calls to its own implementation. Having this approach protects the call to `getValue` from unsafe execution, as well as failed calls since it handles all cases in the `try/catch` statement.

Considering all goes well, a new value is obtained and it is saved as the next available value. This next available value becomes active after the next successful call to `update`. A successful execution swaps the next available value as the currently reported value and sets the next available value as the retrieved value. This means the Oracle has a minimum delay set as `timeUpdateWindow` after receiving a new value from the outside source.&#x20;

Having this delay allows approved actors to veto the value, in case the oracle or the data source went wrong.&#x20;

#### Value

Once at least one successful update was performed, the value can be retrieved. This is usually queried by the [Relayer](../relayer.md) and pushed into Collybus.&#x20;

Also, when querying the value, a boolean flag is also returned which signifies if the latest `update` executed successfully. This flag should always be checked for truthness when retrieving the value.

#### Veto

As described in the update section, a value can be vetoed by approved actors. This feature is important just in case the value that is about to be reported by the Oracle is invalid. This could happen because of a compromised data source but also protects from flashloan attacks.

The approved actors have the possibility to `pause`, `reset` and `unpause` the Oracle if they want to veto the value.&#x20;

A pause stops the Oracle from returning a value.

Resetting can be done only when the Oracle is paused, and completely resets the Oracle's state. Once the Oracle's state was cleared, a new update happens immediatelly, meaning the obtained value becomes active right away (without the need to wait the usual `timeUpdateWindow` ).

Once a reset was performed, the Oracle can be unpaused. Having the Oracle is unpaused, it can report the recorded value.

### 📑 Public Methods

These methods can be called by anyone.

#### `getValue`

This is the abstract method that needs to be implemented by each Oracle instance. It will contain the specific logic of getting values and transforming them in a way that makes them usable by Collybus.

#### `value`

Returns the value and if the last retrieval was successful. One must check if the last retrieval was successful to know if the returned value is valid.&#x20;

#### `lastTimestamp`

The last Unix timestamp at which the Oracle was updated.

#### `timeUpdateWindow`

The minimum time in seconds between sequential Oracle updates.

#### `nextValue`

The value that will become the reported one (by `value`) after a new successful `update` is performed.

### 👮 Guarded Methods

#### `update`

Updates the Oracle values if at least `timeUpdateWindow` has passed since the last update.

#### `pause`

Disables calls to `value`. This method is used discretionarily when the Oracle or an adjacent system misbehaves.

#### `unpause`

Enables calls to `value`. This method is used after the Oracle was reset, in case the Oracle or an adjacent system mibehaved.

#### `reset`

Resets the Oracle value if the Oracle or an adjacent system misbehaved. Can only be called when the Oracle is paused.

### 📘 References

* [Implementation](https://github.com/fiatdao/delphi/tree/master/src/oracle)
