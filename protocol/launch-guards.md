# Launch Guards

## Summary

FIAT's Collateral Vaults allow a user to deposit assets as collateral and borrow a liquid stable coin (FIAT token) against their illiquid capital. FIAT's Collateral Vaults are the main entry point for a user to interact with the FIAT ecosystem.

This guide will describe at a high level how the Vaults work and will dive deeper into the intricacies of the system and implementation.

## Purpose

A DeFi user may participate in fixed income strategies where their position is locked behind a maturity date. Typically a DeFi Platform will issue the user a token to prove their fixed income position. There is no secondary market for these types of tokens, so this position is considered illiquid. \
\
FIAT's Collateral Vaults give these DeFi users the flexibility to deposit bond tokens issued by other fixed income DeFi platforms as collateral. This collateral can be used to borrow a liquid stable coin known as the FIAT token.

The Collateral Vaults interact with several other components in the FIAT ecosystem. These components are responsible for determining the fair price of illiquid assets, accounting for users' debt positions, and sending collateral to an auction in case of a liquidation event.

