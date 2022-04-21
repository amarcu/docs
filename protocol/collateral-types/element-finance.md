# Element Finance

## Vaults

#### Supported Tokens

When a user locks DAI, USDC, or LUSD3CRV-f in an Element Finance fixed rate term contract, Element finance will mint Principal Tokens that are specific to the tranche the user entered. Each tranche is differentiated by the underlying token and maturity date.&#x20;

While the underlying assets are locked for a fixed term, a user can deposit Principal Tokens in a FIAT vault that matches the correct asset type and maturity date.

The FIAT protocol has vaults that support deposits for 3 Element Finance asset types:

* DAI Principal Token
* USDC Principal Token
* LUSD3CRV-f Principal Token

More info on pTokens: [https://docs.element.fi/element/principal-tokens](https://docs.element.fi/element/principal-tokens)

How to acquire pTokens: [https://docs.element.fi/getting-started/buying-fixed-rates](https://docs.element.fi/getting-started/buying-fixed-rates)

#### Token vs Underlier

In the contract code for VaultEPT.sol there are public view methods for `token()` and `underlierToken()`.&#x20;

`token()` returns the address of the principal token, and `underlierToken()` returns the address of the token deposited in the Element Finance tranche (DAI, USD, or LUSD3CRV-f)

## Collybus Discount Rate

All Element Finance vaults use the same precomputed fixed discount rate for computing the fair price of the deposited assets.&#x20;

$$
rate = (1.1^{\frac{1}{365*86400}}-1) * 10^{18} 
\\ rate = 3022265993
$$



## Actions

The following actions wrap multiple transactions into a single transaction for the user.

#### [buyCollateralAndModifyDebt](https://github.com/fiatdao/actions/blob/main/src/vault/VaultEPTActions.sol#L137)

Swaps the underlying token for a pToken to be used as collateral. The pToken is entered into a vault and the user's collateral is updated. A calculated amount of FIAT is minted and sent to the user.

#### [sellCollateralAndModifyDebt](https://github.com/fiatdao/actions/blob/main/src/vault/VaultEPTActions.sol#L177)

Sells pTokens for the underlying token after modifying the position's collateral and debt balances. This method allows for selling pTokens after the maturity date.

#### [redeemCollateralAndModifyDebt](https://github.com/fiatdao/actions/blob/main/src/vault/VaultEPTActions.sol#L218)

Redeems pTokens for the underlying token after it modifies a position's collateral and debt balances.&#x20;

## Properties

| Token        | Address                                    | Name                            | Liquidation Ratio | Interest Per Second | Debt Floor | Debt Ceiling     | Multiplier | Max Auction Duration | Auction Debt Floor |
| ------------ | ------------------------------------------ | ------------------------------- | ----------------- | ------------------- | ---------- | ---------------- | ---------- | -------------------- | ------------------ |
| ePyvLUSD3CRV | 0x222dee6192f946040f97aadb386fafa4e6310cdc | VaultEPT\_ePyvLUSD3CRV\_29APR22 | \~95.23%          | 1000000000317097919 | 250 $FIAT  | 3,500,000 $FIAT  | 1.05x      | 324000 Seconds       | 250.25 $FIAT       |
| ePyvUSDC     | 0xd9080bac070cf47cbdb7223d2440cf8e978e6b45 | vaultEPT\_ePyvUSDC\_29APR22     | \~95.23%          | 1000000000317097919 | 250 $FIAT  | 25,000,000 $FIAT | 1.05x      | 324000 Seconds       | 250.25 $FIAT       |
| ePyvDAI      | 0xea016f6a5eaac396baa3aa712e8d3f20764cbb1f | VaultEPT\_ePyvDAI\_29APR22      | \~95.23%          | 1000000000317097919 | 250 $FIAT  | 3,500,000 $FIAT  | 1.05x      | 324000               | 250.25 $FIAT       |
