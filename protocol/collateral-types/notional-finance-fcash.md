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
