# Position

For each asset supported by FIAT a user can create a [`Position`](https://github.com/fiatdao/fiat/blob/main/src/Codex.sol#L70) which is comprised of the current absolute amounts of collateral (of the supported asset) and normalized debt (realized as $FIAT). A position is [identified](https://github.com/fiatdao/fiat/blob/main/src/Codex.sol#L70) by its collateral asset (Address of the Vault + TokenId) and the original creator of the position (Address of the creator).
