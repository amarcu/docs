---
description: Chainlink Oracle Implementation
---

# Chainlink

### 🔎 High-level Overview

The Chainlink Oracle acts as a wrapper over a [Chainlink Data Feed](https://docs.chain.link/docs/get-the-latest-price/) by fetching the latest price for a single token. Each Chainlink Oracle is updating the spot price for an underlier used by the FIAT protocol.&#x20;

List of deployed Chainlink Oracles:

* DAI Oracle
* USDC Oracle
* LUSD Oracle

### 🐣 Initialization

In order to deploy a Chainlink Oracle we only need the address of the Data Feed:

* `chainlinkAggregatorAddress` - the address of the deployed chainlink aggregator contract

All of the initial parameters are `immutable` which means once they are set they can not be modified.

### 🌈 Execution Flow

Each specific oracle implementation must define the `Oracle.getValue()` function. This function is called when the global execution flow is triggered by the [Relayer](../../relayer.md). &#x20;

To obtain the new price the Oracle will retrieve the latest data from the Chainlink Data Feed by calling the `latestRoundData()`. After the price is retrieved it is converted to an 18-digit fix-point number.

### 📑 Public Methods

These methods can be called by anyone:

**`underlierDecimals`**

Returns the decimal value for the Chainlink Data Feed.

**`chainlinkAggregatorAddress`**&#x20;

Returns the address of the Chainlink Data Feed.

**`getValue`**&#x20;

Computes and returns the spot price.

**`description`**&#x20;

Retrieves and returns the description of the Chainlink Data Feed.

### 📘 References

* [Implementation](https://github.com/fiatdao/delphi/blob/26c91838d287a27e494c75a834fbafef303c090d/src/oracle\_implementations/spot\_price/Chainlink/ChainlinkValueProvider.sol).
* [Deployed Delphi Oracles](https://github.com/fiatdao/changelog/tree/0693456e1938288734b79a24e9ac3be4a0ef6661/deployment).
* [Chainlink docs](https://docs.chain.link).
* [Deployed Chainlink Data Feeds](https://docs.chain.link/docs/ethereum-addresses/).
