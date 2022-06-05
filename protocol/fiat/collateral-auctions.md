---
description: Liquidating and auctioning off Zero-Coupon Bonds with Stablecoin underliers
---

# Liquidations

To ensure that FIAT is properly collateralized at any time the collateral of a [Position](position-management.md) can be confiscated by anyone if the fair value (`collateral *` [`fairPrice`](../collateral-vaults/#fair-price)`)`) of its collateral falls below the position's outstanding debt (den. in FIAT).

**Collateral Auctions**

Confiscated collateral is auctioned off via a dutch auction which over time follows a linear increasing discount on the collateral to be sold. The initial price is the `fairPrice` plus a premium as defined by the `multiplier`. Additionally a `floorPrice` is enforced to guarantee that after all the collateral has been sold - all outstanding debt has been recovered. The current price of an active auction will be reset if either `maxAuctionDuration` has elapsed or the current price is less than the `floorPrice`. Auctions will continuously run until all the debt has been recovered.

**Liquidating Zero-Coupon Bonds**

The primary type of collateral backing FIAT are fixed rate assets which can suffer from insufficiently liquid markets when attempting to directly sell them for a more liquid asset. For some of the collateral types such as Element Finance pTokens or Notional Finance fCash there are native markets for which liquidators can lock in an atomic profit as they would do when participating in liquidations on common lending apps like Aave and Compound.&#x20;

Buyers of liquidated collateral have to make the decision to either hold the yield bearing asset until maturity to then redeem it for its liquid underlying or to factor in price impact which they might encounter when selling it directly on the secondary market.

