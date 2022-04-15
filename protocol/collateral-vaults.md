# Collateral Vaults

## Summary

**$FIAT** is primarily collateralized with fixed income assets. [`Vaults`](https://www.github.com/fiatdao/vaults) act as collateral adapters by standardizing how other contracts of the system and users interact with those types of assets, since these assets can come in the form of ERC721, ERC1155 or ERC20 tokens. FIAT classifies them via the `vaultType` property which is an identifier describing the token standard used by the fixed income asset followed by an shorthand of the assets protocol (e.g. ERC20:EPT (Element Finance Principal Token), ERC1155:FC (Notional Finance fCash Token)).

In order to avoid having multiple different interfaces for each `vaultType` there's a general [`IVault`](https://github.com/fiatdao/fiat/blob/main/src/interfaces/IFIAT.sol) interface which standardizes deposits and withdrawals via `enter` and `exit` how the protocol can obtain the current collateral value via `fairPrice`.

In order for users to mint new $FIAT by opening a new position, users first have to put up collateral  by depositing assets into the corresponding Vault.
