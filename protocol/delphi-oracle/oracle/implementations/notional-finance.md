---
description: Notional Finance Oracle Implementation
---

# Notional Finance

### 🔎 High-level Overview

The Oracle implementation uses [Notional's Interest Rate Oracles](https://docs.notional.finance/notional-v2/fcash-valuation/interest-rate-oracles) to fetch a dampened price that converges to the last traded rate over a window of time. After the price is retrieved we format and convert the rate to a per-second value that is used by the [Relayer](../../relayer.md) to push values to [Collybus](../../../fiat/).

### 🐣 Initialization

The Oracle uses active markets that are retrieved from the [deployed Notional contract](https://docs.notional.finance/developer-documentation/#deployed-contract-addresses) via a [View interface](https://github.com/notional-finance/contracts-v2/blob/d89be9474e181b322480830501728ea625e853d0/interfaces/notional/NotionalViews.sol).&#x20;

These are the parameters needed to define a `NotionalFianceValueProvider` Oracle:

* `timeUpdateWindow` - the minimum time between updates for the oracle rate
* `notional` - the address of the Notional deployed oracle
* `currencyId` - the id of each currency as defined in the Notional [protocol](https://docs.notional.finance/developer-documentation/off-chain/subgraph-reference#particularities-of-notionals-subgraph)
* `oracleRateDecimals`- the decimal precision of the Notional provided oracle rate
* `maturityDate`- the maturity date of the pool.

All of the initial parameters are `immutable` which means once they are set they can not be modified.

### 🌈 Execution Flow

Each specific oracle implementation must define the `Oracle.getValue()` function. This function is called when the global execution flow is triggered by the [Relayer](../../relayer.md). &#x20;

The oracle rate is retrieved by calling `getMarket(currencyId, maturityDate, settlementDate)` on the Notional contract.&#x20;

After the value is retrieved it goes through a few processing steps that convert it to the format that is required by Collybus. First, the value is converted to an 18-digit(WAD) fixed-point number, after that, it is transformed from an annual rate to a per-second rate then the last step is to convert it from a continuous compounding to a discrete compounding rate.

If `getValue` is called after the maturity date the call will revert. In this scenario, the `update()` process will continue but the current value will be considered invalid.

### 📑 Public Methods

These methods can be called by anyone:

**`notional`**

Returns the address of the deployed Notional contract that is needed in order to fetch the oracle rate and compute the per second rate

**`currencyId`**

Returns the Currency ID, ETH = 1, DAI = 2, USDC = 3, WBTC = 4

**`maturityDate`**

Returns the maturity date of the Pool

**`oracleRateDecimals`**

Returns the precision of the Notional oracle rate

**`getValue`**

Computes and returns the discount rate.

**`getSettlementDate`**

Computes and returns the pool settlement date

### 📘 References

* [Implementation](https://github.com/fiatdao/delphi/blob/26c91838d287a27e494c75a834fbafef303c090d/src/oracle\_implementations/discount\_rate/NotionalFinance/NotionalFinanceValueProvider.sol)
* [Notional documentation](https://docs.notional.finance/notional-v2/)
* [Primer on how Notional works](https://blog.notional.finance/how-notional-works/)
* [Deployed Delphi Oracles](https://github.com/fiatdao/changelog/tree/0693456e1938288734b79a24e9ac3be4a0ef6661/deployment)
