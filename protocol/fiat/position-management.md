# Position Management

For each asset supported by FIAT a user can create a [`Position`](https://github.com/fiatdao/fiat/blob/main/src/Codex.sol#L70) which is comprised of the current absolute amounts of collateral (of the supported asset) and normalized debt (realized as $FIAT). A position is uniquely [identified](https://github.com/fiatdao/fiat/blob/main/src/Codex.sol#L70) by its collateral asset (Address of the Vault + `TokenId`) and the original creator of the Position.

#### Depositing and Withdrawing collateral assets in and out of FIAT

Each approved Vault in Codex is able to call `modifyBalance` which records deposited collateral amounts for each depositor. For more information see [Collateral Vaults](../collateral-vaults/).

#### Terms

* `collateral`: amount of collateral units corresponding to a deposited asset, identified by the address of the Vault it has been deposited in and a `TokenId` (0 for ERC20 tokens)
* `normalDebt`: amount of gross debt of a position where the borrow rate accumulator has not been applied to
* `debt`: amount of net debt of a position where the borrow rate accumulator has been applied to (amount of debt which the owner has to be repay to retrieve the full collateralized amount)
* `credit`: amount of credit units which can be transferred between accounts and realized in form of $FIAT via `Moneta`- is minted to / burned from the `creditor` by taking out or repaying debt on a Position
* `unbackedDebt:` amount of debt which is not backed by collateral which can be canceled out via a surplus of `credit`

#### Adjusting a Position's collateral-to-debt ratio

[`modifyCollateralAndDebt`](https://github.com/fiatdao/fiat/blob/main/src/Codex.sol#L297) allows for adjusting `collateral` and `normalDebt` of a Position. Whereby `user` is the owner of the Position, `collateralizer` is the account from which collateral is transferred to or from the Position, `creditor` account from which `credit`is transferred to or from the Position, depending on if the `deltaCollateral` and `deltaNormalDebt` amounts are positive or negative.

#### Depositing and Withdrawing $FIAT in and out of FIAT

After collateralizing a Position and minting `credit` by generating `debt` in a Position, the user is able to redeem it for $FIAT by calling `exit` on `Moneta` which transfers the internal `credit` to `Moneta` and in return mints $FIAT to the users. Likewise if a user wants repay a portion or all of the debt for a Position he calls `enter` on `Moneta` which burns the users $FIAT and in return transfers an equal amount of internal `credit` to the user which can then be used to cancel out an equal amount of `debt`. Before exiting $FIAT out of FIAT the users has to call `grantDelegate` to allow `Moneta` to transfer the internal credit on their behalf. Before entering $FIAT the user has to approve `Moneta` by calling `setAllowance` on $FIAT.

#### Delegating ownership of a Position

The owner of a Position can delegate the right to call `ModifyCollateralAndDebt` for managing collateral-to-debt ratios of the owned Positions, `transferBalance` to transfer internal collateral assets and `transferCredit` to transfer internal `credit` to another account by calling `grantDelegate` and `revokeDelegate`.
