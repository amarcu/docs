---
description: >-
  Element Finance allows users to strip yield-bearing positions into Principal
  and Yield Tokens
---

# Element Finance pToken

The first supported collateral partner of FIAT is Element Finance. Element Finance's _principal tokens_ represent a claim on a deposit made into a yield-bearing market at a known point in the future and can thus be viewed as Zero Coupon Bond-like assets. Users are able to earn a fixed yield on them through one of two ways:

* **Minting the "PT" to sell the "YT":** When creating a principal token, the user also receives a yield token that retains ownership of all subsequent yield to be earned by the associated deposit. By selling the YT, the user locks in a known income and then simply waits for the maturity of the principal token's to withdraw the initial deposit.
* **Purchasing the PT at a discount:** Principal Tokens can be bought at a discount to face value on specific YieldSpace AMMs spun up by Element Finance. By holding until maturity, the user is able to realize the discount as yield on the original purchase amount.&#x20;

**More info on Principal Tokens:** [https://docs.element.fi/element/principal-tokens](https://docs.element.fi/element/principal-tokens)

**How to acquire Principal Tokens:** [https://docs.element.fi/getting-started/buying-fixed-rates](https://docs.element.fi/getting-started/buying-fixed-rates)

**Providing liquidity for Principal Tokens:** [https://docs.element.fi/element/providing-liquidity](https://docs.element.fi/element/providing-liquidity)

## Principal Token Vault

For each supported maturity of a Principal Token, there is a Minimal Proxy-based Vault deployed which delegate-calls into the actual implementation of the Principal Token Vault ([VaultEPT](https://github.com/fiatdao/vaults/blob/main/src/VaultEPT.sol)) which is deployed for each [`Wrapped Position`](https://docs.element.fi/element/element-smart-contracts/core-protocol-contracts/wrapped-position).

## Pricing Principal Tokens

#### Fair Price

See [Collateral Vaults](./)

#### Discount Rate

Currently all Element Finance Principal Token vaults use the same precomputed fixed discount rate for computing the fair price of the deposited assets.

$$
rate = (1.1^{\frac{1}{365*86400}}-1) * 10^{18} = 3022265993
$$

This is due to Element Finance's YieldSpace implementation lacking a price oracle.

## Convenience Methods / Zaps

The following methods wrap multiple actions into a single transaction for proxy users.

#### [buyCollateralAndModifyDebt](https://github.com/fiatdao/actions/blob/main/src/vault/VaultEPTActions.sol#L137)

Mints FIAT with the underlier (e.g. USDC) directly. It swaps the underlying token for the corresponding pToken and enters the pToken into the Vault (e.g. ePyvUSDC Vault).

#### [sellCollateralAndModifyDebt](https://github.com/fiatdao/actions/blob/main/src/vault/VaultEPTActions.sol#L177)

Burns FIAT and withdraws the underlier (e.g. USDC) directly. It exits the corresponding pToken from the Vault and swaps it for the underlier. Selling the pToken for the underlying works even if the pToken has matured.&#x20;

{% hint style="warning" %}
Calling this method risks high price impact in the event of AMM liquidity constraints
{% endhint %}

#### [redeemCollateralAndModifyDebt](https://github.com/fiatdao/actions/blob/main/src/vault/VaultEPTActions.sol#L218)

Burns FIAT and withdraws the underlier (e.g. USDC) directly after maturity. It exits the corresponding pToken from the Vault and redeems it for the underlier. This only works if the pToken has matured - otherwise it will revert.

## Collateral Parameters

| Name                            | Address                                    | Liquidation Ratio | Borrow Rate (Annualized) | Debt Floor | Debt Ceiling    | Multiplier | Auction Duration (Max.) | Auction Debt Floor |
| ------------------------------- | ------------------------------------------ | ----------------- | ------------------------ | ---------- | --------------- | ---------- | ----------------------- | ------------------ |
| VaultEPT\_ePyvUSDC\_29APR22     | 0xd9080bac070cf47cbdb7223d2440cf8e978e6b45 | \~95.23%          | 1%                       | 250 FIAT   | 25,000,000 FIAT | 1.05x      | 90 hours                | 250.25 FIAT        |
| VaultEPT\_ePyvLUSD3CRV\_29APR22 | 0x222dee6192f946040f97aadb386fafa4e6310cdc | \~95.23%          | 1%                       | 250 FIAT   | 3,500,000 FIAT  | 1.05x      | 90 hours                | 250.25 FIAT        |
| VaultEPT\_ePyvDAI\_29APR22      | 0xea016f6a5eaac396baa3aa712e8d3f20764cbb1f | \~95.23%          | 1%                       | 250 FIAT   | 3,500,000 FIAT  | 1.05x      | 90 hours                | 250.25 FIAT        |
| VaultEPT\_ePyvUSDC\_16SEP22     | 0x4465476275E1201F2E5Ab9479E6a51c7875Ce700 | \~95.23%          | 1%                       | 250 FIAT   | 25,000,000 FIAT | 1.05x      | 90 hours                | 250.25 FIAT        |
| VaultEPT\_ePyvDAI\_16SEP22      | 0xb6922A39C85a4E838e1499A8B7465BDca2E49491 | \~95.23%          | 1%                       | 250 FIAT   | 3,500,000 FIAT  | 1.05x      | 90 hours                | 250.25 FIAT        |
| VaultEPT\_ePyvLUSD3CRV\_16SEP22 | 0xE486a875367875a7Ec1DfE40510B6Ed217849737 | \~95.23%          | 1%                       | 250 FIAT   | 3,500,000 FIAT  | 1.05x      | 90 hours                | 250.25 FIAT        |

