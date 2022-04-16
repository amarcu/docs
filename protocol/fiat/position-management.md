# Position Management

For each asset supported by FIAT a user can create a [`Position`](https://github.com/fiatdao/fiat/blob/main/src/Codex.sol#L70) which is comprised of the current absolute amounts of collateral (of the supported asset) and normalized debt (realized as $FIAT). A position is uniquely [identified](https://github.com/fiatdao/fiat/blob/main/src/Codex.sol#L70) by its collateral asset (Address of the Vault + TokenId) and the original creator of the position (Address of the creator).

#### Depositing and Withdrawing assets in and out of FIAT

see [Collateral Vaults](../collateral-vaults.md)

#### Terms

* `collateral`:  amount of collateral units corresponding to a deposited asset, identified by the address of the Vault it has been deposited in and a `TokenId` (0 for ERC20 tokens)
* `normalDebt`: amount of gross debt of a position where the borrow rate accumulator has not been applied to
* `debt`: amount of net debt of a position where the borrow rate accumulator has been applied to (amount of debt which the owner has to be repay to retrieve the full collateralized amount)
* `credit`: amount of credit units which can be transferred between accounts and realized in form of $FIAT via `Moneta`- is minted to / burned from the `creditor` by taking out  or repaying debt on a Position
* `unbackedDebt:` amount of debt which is not backed by collateral which can be canceled out via `credit`

#### Adjusting a Position's collateral-to-debt ratio

`ModifyCollateralAndDebt(vault, tokenId, user, collateralizer, creditor, deltaCollateral, deltaNormalDebt)` allows for adjusting `collateral` and `normalDebt` of a Position. Whereby `user` is the owner of the Position, `collateralizer` is the account from which collateral is transferred to or from the Position, `creditor` account from which `credit`is transferred to or from the Position, depending on if the `deltaCollateral` and `deltaNormalDebt` amounts are positive or negative.

#### Delegating ownership of a Position

The owner of a Position can delegate the right to call `ModifyCollateralAndDebt` to another address for managing collateral-to-debt ratios by calling `grantDelegate` and `revokeDelegate` .
