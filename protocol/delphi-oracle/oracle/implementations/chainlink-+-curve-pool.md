---
description: Chainlink Curve Pool Oracle implementation
---

# Chainlink + Curve Pool

### 🔎 High-level Overview

The implementation for the Oracle is done according to the Curve - Chainlink Oracle implementation [guide](https://news.curve.fi/chainlink-oracles-and-curve-pools/).&#x20;

In order to compute the price, we need the Chainlink Data Feeds for each LP token. For example, in order to price the pool in USD, we need the price feeds for each token within the pool in USD, which in this case would be DAI/USD, USDT/USD, and USDC/USD. We also need the addresses of the Curve 3 Pool, the address of the Curve 3 LP Token, and the Curve LUSD Pool.

The resulting spot price is converted to a per-second rate that is used by the [Relayer](../../relayer.md) to push values to [Collybus](../../../fiat/).

List of Curve Pool Oracles:

* `LUSD3CRVValueProvider` - LUSD Curve Pool Oracle.

### 🐣 Initialization

These are the parameters needed to build the `LUSD3CRVValueProvider` Oracle:

* `curve3Pool` - Address of the Curve 3 Pool
* `curveLUSD3Pool` - Address of the LUSD Curve Pool
* `chainlinkLUSD` - Address of the LUSD Chainlink Data Feed
* `chainlinkUSDC` - Address of the USDC Chainlink Data Feed
* `chainlinkDAI` - Address of the DAI Chainlink Data Feed
* `chainlinkUSDT` -  Address of the USDT Chainlink Data Feed

All of the initial parameters are `immutable` which means once they are set they can not be modified.

### 🌈 Execution Flow

Each specific oracle implementation must define the `Oracle.getValue()` function. This function is called when the global execution flow is triggered by the [Relayer](../../relayer.md). &#x20;

In order to compute the price we follow a few steps:

1. We query all the Chainlink Data Feeds for the latest price for the tokens within the 3 Pool(USDC, DAI, USDT) and find the smallest value.
2. Retrieve the virtual price for the Curve 3 Token and multiply it by the minimum value obtained in step 1.
3. Compute the minimum between the value obtained in step 2 and the LUSD price retrieved from the Chainlink Data feed.
4. Compute the final price by retrieving the virtual price for the LUSD Curve Pool LP Token and multiplying it by the value obtained in step 3.

### 📑 Public Methods

These methods can be called by anyone:

**`decimalsUSDC`**&#x20;

Decimals for the Chainlink Data Feed price.

**`decimalsDAI`**

Decimals for the Chainlink Data Feed price.

**`decimalsUSDT`**

Decimals for the Chainlink Data Feed price.

**`decimalsLUSD`**

Decimals for the Chainlink Data Feed price.

**`curve3Pool`**

Address of the Curve 3 Pool.

**`curveLUSD3Pool`**

Address of the Curve LUSD 3 Pool.

**`chainlinkLUSD`**

Address of the Chainlink Data Feed.

**`chainlinkUSDC`**

Address of the Chainlink Data Feed.

**`chainlinkDAI`**

Address of the Chainlink Data Feed.

**`chainlinkUSDT`**

Address of the Chainlink Data Feed.

**`getValue`**

Computes and returns the spot price.

**`description`**

Returns the description of the Oracle.

### 📘 References

* [Implementation](https://github.com/fiatdao/delphi/blob/26c91838d287a27e494c75a834fbafef303c090d/src/oracle\_implementations/spot\_price/Chainlink/LUSD3CRV/LUSD3CRVValueProvider.sol).
* [Deployed Delphi Oracles.](https://github.com/fiatdao/changelog/tree/0693456e1938288734b79a24e9ac3be4a0ef6661/deployment)
* [Curve - Chainlink oracle implementation guide.](https://news.curve.fi/chainlink-oracles-and-curve-pools/)
* [Deployed Chainlink Data Feeds.](https://docs.chain.link/docs/ethereum-addresses/)
