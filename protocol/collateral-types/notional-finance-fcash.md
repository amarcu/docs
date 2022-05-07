# Notional Finance fCash

The protocol enables FIAT to be minted against Notional Finance fCash tokens. fCash tokens represent a claim on a future cashflow. fCash can be bought at a discount on Notional Finances AMMs. Given the time value of money - fCash usually trade at a discount and thus can be viewed as Zero Coupon Bonds.

More info on fCash: [https://docs.notional.finance/notional-v2/notional-v2-basics/fcash](https://docs.notional.finance/notional-v2/notional-v2-basics/fcash)

How to acquire fCash: [https://notional.finance/lend](https://notional.finance/lend)

## fCash Vault

For each supported tenor (e.g. 3 months, 6 months, 12 months) of an fCash asset (e.g. fUSDC) there's a Vault deployed called [VaultFC](https://github.com/fiatdao/vaults/blob/main/src/VaultFC.sol). So for one underlier (e.g. USDC) a single vault accepts different maturing fCash tokens.

## Pricing fCash

#### Fair Price

see [Collateral Vaults](../collateral-vaults.md)

#### Discount Rate

see [Delphi Oracle](../delphi-oracle/oracle/implementations/notional-finance.md)

## Convenience Methods / Zaps

The following methods wrap multiple actions into a single transaction for proxy users.

#### [buyCollateralAndModifyDebt](https://github.com/fiatdao/actions/blob/main/src/vault/VaultFCActions.sol#L327)

Enables minting FIAT with the underlier (e.g. USDC) directly. It swaps the underlying token for the corresponding fCash token and enters the fCash into the Vault (e.g. fcUSDC\_3MONTH Vault).

#### [sellCollateralAndModifyDebt](https://github.com/fiatdao/actions/blob/main/src/vault/VaultFCActions.sol#L372)

Enables burning FIAT and withdrawing the underlier (e.g. USDC) directly. It exits the corresponding fCash tokens from the Vault and swaps it for the underlier. Selling fCash for the underlying does not works if the fCash asset has matured. The user should in this case use `redeemCollateralAndModify`.

#### [redeemCollateralAndModifyDebt](https://github.com/fiatdao/actions/blob/main/src/vault/VaultFCActions.sol#L417)

Enables burning FIAT and withdrawing the underlier (e.g. USDC) directly after maturity. It exits the corresponding fCash token from the Vault and redeems it for the underlier. This only works if the fCash asset has matured - otherwise it will revert.
