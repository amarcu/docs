---
description: Oracle Implementations
---

# Implementations

### 🔎 High-level Overview

Abstract Oracle implementations fall into two categories. We have oracles that compute discount rates and are used by Discount Rate Relayers to push rates to Collybus and spot price Oracles that are used by Spot Relayers to push spot prices to Collybus.

List of Discount Rate Oracles:

* [Notional Finance ](notional-finance-fcash.md)- `NotionalFianceValueProvider`
* [Yield](yield-protocol-fytoken.md) - `YieldValueProvider`

List of Spot Oracles:

* [Chainlink](chainlink-underlier-feed.md) - `ChainlinkValueProvider`
* [Chainlink + Curve Pool](chainlink-+-curve-lp-token-feed.md) - `LUSD3CRVValueProvider`

### 📘 References

* [Deployed Delphi Oracles](https://github.com/fiatdao/changelog/tree/0693456e1938288734b79a24e9ac3be4a0ef6661/deployment)
