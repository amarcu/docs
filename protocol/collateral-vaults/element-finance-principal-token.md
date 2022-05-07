# Element Finance pTokens

The protocol enables FIAT to be minted against Element Finance Principal Tokens. Principal Token represent a claim on a future cashflow. Principal Tokens can be bought at a discount on Element Finances AMMs. Given the time value of money - Principal Tokens usually trade at a discount and thus can be viewed as Zero Coupon Bonds.

More info on Principal Tokens: [https://docs.element.fi/element/principal-tokens](https://docs.element.fi/element/principal-tokens)

How to acquire Principal Tokens: [https://docs.element.fi/getting-started/buying-fixed-rates](https://docs.element.fi/getting-started/buying-fixed-rates)

## Principal Token Vault

For each supported maturity of a Principal Token there's a Minimal Proxy based Vault deployed which delegate-calls into the actual implementation of Principal Token Vault ([VaultEPT](https://github.com/fiatdao/vaults/blob/main/src/VaultEPT.sol)) which is deployed for each [`Wrapped Position`](https://docs.element.fi/element/element-smart-contracts/core-protocol-contracts/wrapped-position).

## Pricing Principal Tokens

#### Fair Price

see [Collateral Vaults](./)

#### Discount Rate

Currently all Element Finance Principal Token vaults use the same precomputed fixed discount rate for computing the fair price of the deposited assets.

$$
rate = (1.1^{\frac{1}{365*86400}}-1) * 10^{18} = 3022265993
$$

## Convenience Methods / Zaps

The following methods wrap multiple actions into a single transaction for proxy users.

#### [buyCollateralAndModifyDebt](https://github.com/fiatdao/actions/blob/main/src/vault/VaultEPTActions.sol#L137)

Enables minting FIAT with the underlier (e.g. USDC) directly. It swaps the underlying token for the corresponding pToken and enters the pToken into the Vault (e.g. ePyvUSDC Vault).

#### [sellCollateralAndModifyDebt](https://github.com/fiatdao/actions/blob/main/src/vault/VaultEPTActions.sol#L177)

Enables burning FIAT and withdrawing the underlier (e.g. USDC) directly. It exits the corresponding pToken from the Vault and swaps it for the underlier. Selling pToken for the underlying works even if the pToken has matured. Though the user should in this case use `redeemCollateralAndModify` to avoid high price impacts due to liquidity constraints on the pToken AMM.

#### [redeemCollateralAndModifyDebt](https://github.com/fiatdao/actions/blob/main/src/vault/VaultEPTActions.sol#L218)

Enables burning FIAT and withdrawing the underlier (e.g. USDC) directly after maturity. It exits the corresponding pToken from the Vault and redeems it for the underlier. This only works if the pToken has matured - otherwise it will revert.

## Collateral Parameters

| Name                            | Address                                    | Liquidation Ratio | Borrow Rate (Annualized) | Debt Floor | Debt Ceiling    | Multiplier | Auction Duration (Max.) | Auction Debt Floor |
| ------------------------------- | ------------------------------------------ | ----------------- | ------------------------ | ---------- | --------------- | ---------- | ----------------------- | ------------------ |
| VaultEPT\_ePyvUSDC\_29APR22     | 0xd9080bac070cf47cbdb7223d2440cf8e978e6b45 | \~95.23%          | 1%                       | 250 FIAT   | 25,000,000 FIAT | 1.05x      | 90 hours                | 250.25 FIAT        |
| VaultEPT\_ePyvLUSD3CRV\_29APR22 | 0x222dee6192f946040f97aadb386fafa4e6310cdc | \~95.23%          | 1%                       | 250 FIAT   | 3,500,000 FIAT  | 1.05x      | 90 hours                | 250.25 FIAT        |
| VaultEPT\_ePyvDAI\_29APR22      | 0xea016f6a5eaac396baa3aa712e8d3f20764cbb1f | \~95.23%          | 1%                       | 250 FIAT   | 3,500,000 FIAT  | 1.05x      | 90 hours                | 250.25 FIAT        |
| VaultEPT\_ePyvUSDC\_16SEP22     | 0x4465476275E1201F2E5Ab9479E6a51c7875Ce700 | \~95.23%          | 1%                       | 250 FIAT   | 25,000,000 FIAT | 1.05x      | 90 hours                | 250.25 FIAT        |
| VaultEPT\_ePyvDAI\_16SEP22      | 0xb6922A39C85a4E838e1499A8B7465BDca2E49491 | \~95.23%          | 1%                       | 250 FIAT   | 3,500,000 FIAT  | 1.05x      | 90 hours                | 250.25 FIAT        |
| VaultEPT\_ePyvLUSD3CRV\_16SEP22 | 0xE486a875367875a7Ec1DfE40510B6Ed217849737 | \~95.23%          | 1%                       | 250 FIAT   | 3,500,000 FIAT  | 1.05x      | 90 hours                | 250.25 FIAT        |

