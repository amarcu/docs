---
description: Yield Protocol allows users to borrow and lend tokens at a fixed rate
---

# Yield Protocol fyToken

Yield Protocol is the third collateral partner for FIAT. Yield Protocol's _fyTokens_ represent a claim on the future redemption of both **a)** the initial principal lent & **b)** the fixed yield for having done so, and can thus be viewed as Zero Coupon Bond-like assets. Users are able to earn a fixed yield on them through one of two ways:

* **Minting fyTokens:** When lending on Yield Protocol, users receive a nominal amount of fyToken (e.g. fyUSDC, fyDAI) amounting to the sum of their principal and the fixed yield they are to receive. While fyTokens are redeemable for the corresponding underlier at a 1:1 ratio, one cannot do so until maturity has been reached.
* **Purchasing fyTokens at a discount:** Yield Protocol supports liquidity pools for each of its fyTokens series, meaning users can purchase these assets at a discount to face value due to the time value of money and hold them until maturity.

**Yield Protocol Basics:** [https://docs.yieldprotocol.com/#/](https://docs.yieldprotocol.com/#/)

**Minting fyTokens:** [https://app.yieldprotocol.com/lend](https://app.yieldprotocol.com/lend)

**YieldSpace Whitepaper:** [https://yieldprotocol.com/YieldSpace.pdf](https://yieldprotocol.com/YieldSpace.pdf)

## Fixed Yield Token Vault

For each supported maturity of a Fixed Yield Token, there is a Minimal Proxy-based Vault deployed which delegate-calls into the actual implementation of the fyToken Vault (VaultFY).

## Pricing Fixed Yield Tokens

#### Fair Price

See [Collateral Vaults](./)

#### Discount Rate

Currently all Yield Protocol Fixed Yield Token vaults use the same precomputed fixed discount rate for computing the fair price of the deposited assets.

$$
rate = (1.1^{\frac{1}{365*86400}}-1) * 10^{18} = 3022265993
$$

## Convenience Methods / Zaps

The following methods wrap multiple actions into a single transaction for proxy users.

#### [buyCollateralAndModifyDebt](https://github.com/fiatdao/actions/blob/main/src/vault/VaultFYActions.sol#L81)

Mints FIAT with the underlier (e.g. USDC) directly. It swaps the underlying token for the corresponding fyToken and enters the fyToken into the Vault (e.g. fyUSDC Vault).

#### [sellCollateralAndModifyDebt](https://github.com/fiatdao/actions/blob/main/src/vault/VaultFYActions.sol#L123)

Burns FIAT and withdraws the underlier (e.g. USDC) directly. It exits the corresponding fyToken from the Vault and swaps it for the underlier. It is not possible to sell the fyToken for the underlying  post maturity.

{% hint style="warning" %}
Calling this method risks high price impact in the event of AMM liquidity constraints
{% endhint %}

#### [redeemCollateralAndModifyDebt](https://github.com/fiatdao/actions/blob/main/src/vault/VaultFYActions.sol#L166)

Burns FIAT and withdraws the underlier (e.g. USDC) after maturity. It exits the corresponding fyToken from the Vault and redeems it for the underlier. This only works if the fyToken has matured - otherwise it will revert.

