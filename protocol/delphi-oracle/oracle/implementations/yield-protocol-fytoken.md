---
description: Yield Oracle Implementation
---

# Yield

### 🔎 High-level Overview

The Oracle is implemented as a simple [Uniswap V2 Oracle](https://docs.uniswap.org/protocol/V2/guides/smart-contract-integration/building-an-oracle). The discount rate is computed by taking the difference between the cumulative price at the beginning and end of the period and then divided by the elapsed time between them in seconds. The resulting price is converted to a per-second rate that is used by the [Relayer](../../relayer.md) to push values to [Collybus](../../../fiat/).

### 🐣 Initialization

These parameters are needed in order to define a `YieldValueProvider` Oracle:

* `poolAddress` - Address of the Yield Pool
* `maturity` - Maturity date of the Pool
* `timeScale` - A precomputed time scale in 18 digit precision

The `timeScale` is obtained from the Yield Pool time-stretch property `ts` by applying the following formula:

$$
timescale = {\dfrac{ts}{2^{64}} * 10^{18}}
$$

All of the initial parameters are `immutable` which means once they are set they can not be modified.

### 🌈 Execution Flow

Each specific oracle implementation must define the `Oracle.getValue()` function. This function is called when the global execution flow is triggered by the [Relayer](../../relayer.md). &#x20;

To obtain the new price we compute the delta between the current and the previous cumulative balance ratio which we keep in storage as`cumulativeBalanceRatioLast` and `blockTimestampLast`. The last step is to scale the delta by `timeScale` and convert the rate to an 18-digit precision fixed-point number.

If `getValue` is called after the maturity date the call will revert. In this scenario, the `update()` process will continue but the current value will be considered invalid.

For more information on working and building Uniswap V2 oracles, you can read this [article](https://docs.uniswap.org/protocol/V2/guides/smart-contract-integration/building-an-oracle) and check this Oracle [example](https://docs.uniswap.org/protocol/V2/guides/smart-contract-integration/building-an-oracle).

### 📑 Public Methods

These methods can be called by anyone:

**`cumulativeBalanceRatioLast`**

Returns the last computed balance ratio.

**`blockTimestampLast`**

Returns the timestamp at which the last balance ratio was computed

**`poolAddress`**

Returns the address of the Yield Pool

**`maturity`**

Returns the maturity of the Yield Pool

**`timeScale`**

Returns the formatted time scale of the Yield Pool

**`getValue`**

Computes and returns the discount rate.

### 📘 References

* [Implementation](https://github.com/fiatdao/delphi/blob/26c91838d287a27e494c75a834fbafef303c090d/src/oracle\_implementations/discount\_rate/Yield/YieldValueProvider.sol)
* [Yield Protocol documentation](https://docs.yieldprotocol.com/#/)
* [Deployed Delphi Oracles](https://github.com/fiatdao/changelog/tree/0693456e1938288734b79a24e9ac3be4a0ef6661/deployment)
