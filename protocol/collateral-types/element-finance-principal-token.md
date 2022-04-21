# Element Finance Principal Token

The protocol enables FIAT to be minted against Element Finance Principal Tokens. Principal Token represent a claim on a future cashflow. Principal Tokens can be bought at a discount on Element Finances AMMs. Given the time value of money - Principal Tokens usually trade at a discount and thus can be viewed as Zero Coupon Bonds.

More info on pTokens: [https://docs.element.fi/element/principal-tokens](https://docs.element.fi/element/principal-tokens)

How to acquire pTokens: [https://docs.element.fi/getting-started/buying-fixed-rates](https://docs.element.fi/getting-started/buying-fixed-rates)

## Vault

For each supported maturity of a Principal Token there's a Minimal Proxy based Vault deployed which delegate-calls to the actual implementation of Principal Token Vault ([VaultEPT](https://github.com/fiatdao/vaults/blob/main/src/VaultEPT.sol)) which is deployed for each [`Wrapped Position`](https://docs.element.fi/element/element-smart-contracts/core-protocol-contracts/wrapped-position).&#x20;

## Fair Price

#### Discount Rate

Currently all Element Finance PToken vaults use the same precomputed fixed discount rate for computing the fair price of the deposited assets.

$$
rate = (1.1^{\frac{1}{365*86400}}-1) * 10^{18} 
\\ rate = 3022265993
$$

## Actions

The following methods wrap multiple actions into a single transaction for proxy users.

#### [buyCollateralAndModifyDebt](https://github.com/fiatdao/actions/blob/main/src/vault/VaultEPTActions.sol#L137)

Enables minting FIAT with the underlier (e.g. USDC) directly. It swaps the underlying token for the corresponding PToken and enters the PToken into the Vault (e.g. ePyvUSDC Vault).

#### [sellCollateralAndModifyDebt](https://github.com/fiatdao/actions/blob/main/src/vault/VaultEPTActions.sol#L177)

Enables burning FIAT and withdrawing the underlier (e.g. USDC) directly. It exits the corresponding PToken from the Vault and swaps it for the underlier. Selling PToken for the underlying works even if the PToken has matured. Though the user should in this case use `redeemCollateralAndModify` to avoid high price impacts due to liquidity constraints on the PToken AMM.

#### [redeemCollateralAndModifyDebt](https://github.com/fiatdao/actions/blob/main/src/vault/VaultEPTActions.sol#L218)

Enables burning FIAT and withdrawing the underlier (e.g. USDC) directly after maturity. It exits the corresponding PToken from the Vault and redeems it for the underlier. This only works if the PToken has matured - otherwise it will revert.&#x20;

## Properties

| Token        | Address                                    | Name                            | Liquidation Ratio | Interest Per Second | Debt Floor | Debt Ceiling     | Multiplier | Max Auction Duration | Auction Debt Floor |
| ------------ | ------------------------------------------ | ------------------------------- | ----------------- | ------------------- | ---------- | ---------------- | ---------- | -------------------- | ------------------ |
| ePyvLUSD3CRV | 0x222dee6192f946040f97aadb386fafa4e6310cdc | VaultEPT\_ePyvLUSD3CRV\_29APR22 | \~95.23%          | 1000000000317097919 | 250 $FIAT  | 3,500,000 $FIAT  | 1.05x      | 324000 Seconds       | 250.25 $FIAT       |
| ePyvUSDC     | 0xd9080bac070cf47cbdb7223d2440cf8e978e6b45 | vaultEPT\_ePyvUSDC\_29APR22     | \~95.23%          | 1000000000317097919 | 250 $FIAT  | 25,000,000 $FIAT | 1.05x      | 324000 Seconds       | 250.25 $FIAT       |
| ePyvDAI      | 0xea016f6a5eaac396baa3aa712e8d3f20764cbb1f | VaultEPT\_ePyvDAI\_29APR22      | \~95.23%          | 1000000000317097919 | 250 $FIAT  | 3,500,000 $FIAT  | 1.05x      | 324000               | 250.25 $FIAT       |
