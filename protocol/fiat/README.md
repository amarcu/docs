# 🌅 FIAT v1

## What is FIAT?

The FIAT protocol allows users to mint a single ERC-20 token, $FIAT, against a universe of accepted fixed income asset collateral. By providing users with fungible liquidity for the duration of their fixed term deposits in other protocol, FIAT reduces the opportunity cost of underwriting DeFi debt and aggregates secondary liquidity around a singular focal point.

## How is FIAT Implemented?

The first iteration of the FIAT protocol is based on a collateralized debt position design. Users are able to mint $FIAT against their collateralized positions at some discount rate to the underlying such that the total $FIAT debt outstanding is fully backed by deposited collateral value. Furthermore, the system maintains an internal $1 price target for $FIAT such that arbitrage opportunities for either minting or burning the token arise when there is an imbalance between the circulating supply and market demand for it.

![](<../../.gitbook/assets/CORE diagram updated.png>)

**For greater detail on specific elements of the FIAT protocol, please refer to the following subpages.**

{% content-ref url="position-management.md" %}
[position-management.md](position-management.md)
{% endcontent-ref %}

{% content-ref url="collateral-auctions.md" %}
[collateral-auctions.md](collateral-auctions.md)
{% endcontent-ref %}

{% content-ref url="borrow-rates.md" %}
[borrow-rates.md](borrow-rates.md)
{% endcontent-ref %}

{% content-ref url="surplus-and-debt.md" %}
[surplus-and-debt.md](surplus-and-debt.md)
{% endcontent-ref %}
