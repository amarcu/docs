# 🏺 Collateral Vaults

**$FIAT** is primarily collateralized with fixed income assets. [`Vaults`](https://www.github.com/fiatdao/vaults) act as collateral adapters by standardizing how other contracts of the system and users interact with those types of assets within the system, since these assets can come in the form of ERC721, ERC1155 or ERC20 tokens. FIAT classifies them via the [`vaultType`](https://github.com/fiatdao/fiat/blob/main/src/interfaces/IVault.sol#L20) property which is an identifier describing the token standard used by the fixed income asset followed by a shorthand of the assets originating protocol (e.g. ERC20:EPT (Element Finance Principal Token), ERC1155:FC (Notional Finance fCash Token)).

All vaults no matter the `vaultType` adhere to the general [`IVault`](https://github.com/fiatdao/fiat/blob/main/src/interfaces/IVault.sol) interface which standardizes deposits and withdrawals via [`enter`](https://github.com/fiatdao/fiat/blob/main/src/interfaces/IVault.sol#L36) and [`exit`](https://github.com/fiatdao/fiat/blob/main/src/interfaces/IVault.sol#L42) and how the protocol can obtain the current collateral value via [`fairPrice`](https://github.com/fiatdao/fiat/blob/main/src/interfaces/IVault.sol#L30).

Before users are able to mint new $FIAT (by opening a position), users first have to put up collateral  by depositing assets into the corresponding Vault. The Vault outsources the stateful accounting of ownership of the assets to [`Codex`](https://github.com/fiatdao/fiat/blob/main/src/Codex.sol) . After a successful deposit or withdrawal the users [`balances`](https://github.com/fiatdao/fiat/blob/main/src/Codex.sol#L73) are updated within `Codex`. A user is now able to use their deposited assets to collateralize a new $FIAT position via [`ModifyCollateralAndDebt`](fiat/position-management.md#adjusting-a-positions-collateral-to-debt-ratio). `Codex` will call back into the corresponding Vault to fetch the current `fairPrice` to determine the new `LTV` (loan-to-value) ratio of the position to ensure that the position is collateralized.

Before users are able to withdraw collateral from the system, users first have to transfer the collateral out of the corresponding Position by calling `ModifyCollateralAndDebt`. This will only succeed if the collateral-to-loan ratio of the Position is still safe after the transfer. Users will have to repay a portion or all of their outstanding debt to be able to do so. After the internal collateral transfer from their Position to their internal balance (`balances`) they are able to call exit on the respective Vault.

#### Fair Price

Fair price for the deposited assets are determined using the following formula:

$$
fairPrice = \frac{10^{18}}{(1+discountRate)^{maturity - timestamp}}
$$

The discount rate can vary depending on the vault type.

#### Maximum Debt Position

The maximum amount of debt a user take in a position is enforced via the following formula:

$$
debt \leq collateral * \frac{fairPrice}{liquidationRatio}
$$





