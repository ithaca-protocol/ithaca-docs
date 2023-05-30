# Trading Facility

**End of Trading**

Trading in a Elektro contract will typically cease 2 hours before the Maturity of the contract. After the End of Trading the Elektro Platform will reject any new orders for such contract.

**Orders and Orderbooks**

All orders, BUY or SELL, are required to be Limit Orders. The platform does not allow for Market Orders to be submitted into the Orderbook.&#x20;

As the Ecosystem is based on decentralized on-chain settlement of cash funds, participants are required to fulfil certain collateral and premium funding requirements before submitting any new order, or change an existing order on the platform. Orders can be valid for different time frames. A Day Order is cancelled if it did not get executed by the end of the trading day on which it was submitted into the orderbook.&#x20;

Unless otherwise specified, every order is a Day Order. A Good-Til-Cancelled Order (GTC) will remain in the Orderbook as a live order until it is fully executed or the order is cancelled. GTC orders that are not fully executed will automatically be cancelled when a Contracts stops trading and the orderbook is closed prior to the Contract’s expiry. An Immediate-Or-Cancel (IOC) order requires all or part of the order to be executed in the next Frequent Batch Auction.&#x20;

Any unexecuted parts of the order are cancelled. Partial executions are accepted.

**Multi-Leg Conditional Orders**

Elektro allows for the submission of conditional orders

Such orders consist of two or more legs. Each leg of the conditional order enters the matching engine as a separate order. Legs of a conditional order can be BUY or SELL orders. The order legs can be orders for any contract tradable on the platform. A conditional order can be fully executed if all its legs can be fully executed; or it can be partially executed if all its legs can be executed in the same pro-rata fraction.

A conditional order has one net limit price across all legs (or depending on the interface used, one net limit premium). A leg with a buy order is counted positively while a leg with a sell order is to be counted negatively. Therefore, in similar fashion to plain single orders, the net limit price of a conditional order represents the price of a normalized quantity of 1 unit. The normalized quantity of 1 unit for a conditional order relates to the same quantity across all legs.

**Example 1**:&#x20;

An order to buy a 1x2 call spread can be submitted as a conditional order with two legs:&#x20;

1\) buy 1 contract of Call(K1)&#x20;

2\) sell 2 contracts of Call(K2)&#x20;

The net limit price of the conditional order then relates to the price for the execution of 1/3 of Call(K1) and 2/3 of Call(K2).&#x20;

If during an auction, Call(K1) clears at c1 and Call(K2) clears at c2 then the conditional order would clear at

**\[ 1 x c1 - 2 x c2 ] / \[ 1 + 2 ]**

The net limit price of a conditional order can be positive or negative. A positive net limit price indicates that execution of the conditional order will lead to the price, or less, to be paid (user is a net buyer), a negative net limit price on a conditional order will lead to the price, or more, to be received (user is a net seller)

**Example 2:**

An order to buy a risk reversal may be submitted as a conditional order with two legs:

1\) sell 1 contract of Put (K1) 2)&#x20;

buy 1 contract of Call (K2)

An order sent at a net price of -45 USDC means that the user wants to be net receiving (put over). If the Put(K1) clears at 1,100 USDC and the Call(K2) clears at 1,000 USDC then this implies a clearing net price for the risk reversal at \[ 1,000 - 1,100 ] / \[ 1 + 1 ] = - 50 USD and the previous conditional order would thus be filled.

The net total premium of a conditional order can be calculated by multiplying the net limit price of the order with the sum of the order quantities of all legs.

In the above risk reversal example, the net total premium would be - 45 x 2 or - 90 USDC
