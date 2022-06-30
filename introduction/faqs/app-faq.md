---
description: Responses to common questions regarding the FIAT DAO application
---

# App FAQ

**What is a proxy contract?**

* Interacting with the primary FIAT DAO user interface requires that users first deploy a proxy smart contract. This allows for the streamlining of protocol interactions, ultimately saving the user gas costs and allowing for more complex transactions.

**What does "Face Value" mean?**

* The face value reported by the application is the value of one unit of the collateral present in the CDP at maturity. For most supported collateral types, the face value is $1.00.

**What is a "Collateralization Threshold"?**

* The collateralization threshold refers to the minimum ratio between the value of your CDP and the amount of $FIAT minted against it. This parameter is set by FIAT DAO governance. For example, a CDP with a 105% collateralization threshold would be eligible for liquidation if more than 100 FIAT were minted against only $105 worth of collateral.

**What is a "Health Factor"?**

* The health factor of a given CDP is its current collateralization threshold. For low volatility collateral like those denominated in stablecoins and with steadily shrinking discount rates, users can choose to carry health factors barely above 1.00.&#x20;

**How do I determine my maximum Loan-to-Value ratio?**

* To calculate the current maximum amount of $FIAT a user can mint at a given point in time, they must calculate the discount rate assessed against the face value of their collateral, and then divide that product by the associated collateralization threshold.&#x20;
