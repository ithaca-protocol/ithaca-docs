# Liquidation Engine

**For Ithaca’s margined products, the Ithaca Liquidation Engine (ILE) continuously monitors users’ margin balances against their maintenance margin requirements**

* If a user’s margin balance falls below its maintenance margin, the ILE will initiate a liquidation
* As liquidations occur when the market has moved against the user, this may involve the ILE having to liquidate an in-the-money option, where there is typically less liquidity in a typical options market
* The Ithaca Matching Engine will execute these orders by replicating, for example, in-the-money puts into forwards and out-of-the money calls, thereby creating additional liquidity
