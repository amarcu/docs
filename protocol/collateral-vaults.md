# Collateral Vaults

## Summary

FIAT's Collateral Vaults allow a user to deposit assets as collateral and borrow a liquid stable coin (FIAT token) against their illiquid capital. FIAT's Collateral Vaults are the main entry point for a user to interact with the FIAT ecosystem.

This guide will describe at a high level how the Vaults work and will dive deeper into the intricacies of the system and implementation.

## Purpose

A DeFi user may participate in fixed income strategies where their position is locked behind a maturity date. Typically a DeFi Platform will issue the user a token to prove their fixed income position. There is no secondary market for these types of tokens, so this position is considered illiquid. \
\
FIAT's Collateral Vaults give these DeFi users the flexibility to deposit bond tokens issued by other fixed income DeFi platforms as collateral. This collateral can be used to borrow a liquid stable coin known as the FIAT token.

The Collateral Vaults interact with several other components in the FIAT ecosystem. These components are responsible for determining the fair price of illiquid assets, accounting for users' debt positions, and sending collateral to an auction in case of a liquidation event.

## Interaction

### Enter

Vaults allow a user to enter a vault by depositing an asset supported by a specific vault. Entering a vault will update the Codex (accounting contract) with the amount of deposited assets for a specific vault type.

### Exit

A user can exit a vault and their deposited assets will be returned to them. As long as the user's LTV does not exceed a certain threshold, their funds will remain locked in the vault until exiting.

### Fair Price

Vault contracts interact with a contract named Collybus. Collybus will estimate a fair price of the illiquid assets. The vault contracts will return this fair price value to the Codex to calculate the maximum amount of debt a user can borrow against their collateral.
