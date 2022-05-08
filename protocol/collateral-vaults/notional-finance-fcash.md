---
description: >-
  Notional Finance allows users to engaged in fixed rate over-collateralized
  borrowing and lending
---

# Notional Finance fCash

Notional Finance is the second collateral partner for FIAT. Notional Finance's _fCash_ represents a claim on the future redemption of both **a)** the initial principal lent & **b)** the fixed yield for having done so, and can thus be viewed as Zero Coupon Bond-like assets. Users are able to earn a fixed yield on them through one of two ways:

* **Minting fCash:** When lending on Notional Finance, users receive a nominal amount of fCash (e.g. fUSDC, fDAI) amounting to the sum of their principal and the fixed yield they are to receive. While fCash is redeemable for the corresponding underlier at a 1:1 ratio, one cannot do so until maturity has been reached.
* **Purchasing fCash at a discount:** Notional Finance supports liquidity pools for each of its fCash series, meaning users can purchase these assets at a discount to face value due to the time value of money and hold them until maturity.

**fCash Basics:** [https://docs.notional.finance/notional-v2/notional-v2-basics/fcash](https://docs.notional.finance/notional-v2/notional-v2-basics/fcash)

**Minting fCash:** [https://notional.finance/lend](https://notional.finance/lend)

**Buying fCash:** [https://docs.notional.finance/traders/notional-basics/liquidity-pools](https://docs.notional.finance/traders/notional-basics/liquidity-pools)

## fCash Vault

For each supported tenor (e.g. 3 months, 6 months, 12 months) of an fCash asset (e.g. fUSDC) there's a Vault deployed called [VaultFC](https://github.com/fiatdao/vaults/blob/main/src/VaultFC.sol). So for one underlier (e.g. USDC) a single vault accepts different maturing fCash tokens.

## Pricing fCash

fCash is priced using [Notional's Interest Rate Oracles](https://docs.notional.finance/notional-v2/fcash-valuation/interest-rate-oracles) to fetch a dampened price that converges to the last traded rate over a window of time. After the price is retrieved, we format and convert the rate to a per-second value that is used by the [Relayer](../delphi-oracle/relayer.md) to push values to [Collybus](../fiat/).

* **Fair Price:** See [Collateral Vaults](./)
* **Discount Rate:** See [Delphi Oracle](../delphi-oracle/oracle/implementations/notional-finance-fcash.md)

## Convenience Methods / Zaps

The following methods wrap multiple actions into a single transaction for proxy users.

#### [buyCollateralAndModifyDebt](https://github.com/fiatdao/actions/blob/main/src/vault/VaultFCActions.sol#L327)

Mints FIAT with the underlier (e.g. USDC) directly. It swaps the underlying token for the corresponding fCash token and enters the fCash into the Vault (e.g. fcUSDC\_3MONTH Vault).

#### [sellCollateralAndModifyDebt](https://github.com/fiatdao/actions/blob/main/src/vault/VaultFCActions.sol#L372)

Burns FIAT and withdraws the underlier (e.g. USDC) directly. It exits the corresponding fCash tokens from the Vault and swaps it for the underlier. Selling fCash for the underlying does not work if the fCash asset has matured. The user should in this case use `redeemCollateralAndModify`.

#### [redeemCollateralAndModifyDebt](https://github.com/fiatdao/actions/blob/main/src/vault/VaultFCActions.sol#L417)

Burns FIAT and withdraws the underlier (e.g. USDC) directly after maturity. It exits the corresponding fCash token from the Vault and redeems it for the underlier. This only works if the fCash asset has matured - otherwise it will revert.
