---
description: Responses to common questions regarding protocol functionality
---

# Protocol FAQ

**What is a Collateralized Debt Position (CDP)?**

* CDPs represent your deposits of supported collateral assets into the FIAT protocol. Users are able to mint $FIAT agains the balances present in their CDPs.&#x20;

**How are collateral types added to the FIAT protocol?**

* Currently, the addition of collateral assets is managed via discrete FIAT DAO governance votes following risk assessments. Future iterations of the protocol may allow for permissionless addition of collateral types through ambient risk management utility for the $FDT token.

**Is there a cost for minting $FIAT?**

* Each collateral vault has an assigned interest rate that is accrued per block. Currently, interest rates for each collateral type are set discretely by FIAT DAO governance. Future iterations of the protocol may allow for dynamic interest rates as a function of total $FIAT circulating supply minted against a specific collateral vault.

**By when do I need to repay my $FIAT debt?**

* Debts can be settled at any time with $FIAT, even after the collateral has matured. Open CDPs will accrue interest until they are either fully repaid or at the point where accrued interest forces the position into liquidation.

**How is collateral liquidated?**

* Liquidation occurs when a given CDP falls below a certain marginal collateralization ratio. The collateral is seized by the protocol and put up for public auction in order to acquire and burn an equivalent amount of $FIAT as had been previously minted against it. Anyone can participate in a collateral auction, even if they have not themselves minted $FIAT.
