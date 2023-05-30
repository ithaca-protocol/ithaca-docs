# Trading Patterns and Collateral Implications

**Trading Strategies and conditional orders**

Elektro enables participants to implement diverse and complex trading strategies involving many different products using pre-packaged and structured products and most notably using conditional orders.&#x20;

Conditional orders eliminate the execution risk between different orders while implementing trading strategies and position patterns. Another notable benefit of using conditional orders comes from the fact that there might be collateral optimization benefit (initially at order entry only) compared to what would be required for the corresponding orders sent simultaneously but as independent orders. When sending conditional orders, the Trading Platform will run a collateral optimization on the conditional order as a package and only the optimized collateral will be needed at order entry.&#x20;

For the corresponding orders sent independently, Elektro would require the full collateral on each order independently at order submission. Any inter-dependencies between independent orders that lead to a smaller overall maximum liability compared with the linear sum of the maximum liabilities of all the legs will only take effect after the trades are executed and settled but are not being considered in the initial collateral requirements. This means that initially, at order submission collateral orders might require more collateral to be locked up than the overall portfolio will economically require. The excess collateral will be freed up again during settlement immediately after trades are executed.&#x20;

**Netting and Unwind trades**

When a participant submits a new order, the collateral requirements for such order at the time of order entry are being calculated independently of any existing settled positions that a participant might already have in the same or any other contract.

That means that any new order that will lead to the economic unwind of an existing position will still initially require the same collateral as if it was an independent order leading to a completely new position. Similarly, orders that, once executed, will lead to an overall reduction of the economically required collateral of a participant’s portfolio, will initially require full independent collateralisation at order entry.

As soon as unwind and collateral reducing trades are executed, the Elektro Collateral Optimisation process will immediately release any excess collateral back to the participant’s FundLock cash balances. As a result of these system mechanics however, the closeout or unwind of existing positions typically do require the temporary provision of capital to fund the on-order-entry collateral requirements.&#x20;

**Leveraged Margin Trading**

Elektro enables participants to borrow portion of the required collateral related to derivative orders and positions using Margin Loan Contracts. As described in the section on Margin Loan Trading below, such contracts can be traded as legs of a conditional order attached to derivative contract orders. Such a conditional Margin Loan order will reduce the collateral requirement of the overall conditional order according to the amount to be borrowed via the margin loan.&#x20;

Similarly to unwind or close-out orders of derivatives or other types of contracts however, if and when a participant would like to trade out of an existing Margin Loan position prior to the expiry, such order to effectively ‘lend’ via a Margin Loan, will require the full notional of the margin loan to be temporarily posted as collateral on order entry.&#x20;

As a consequence, the early ‘pay back’ or ‘redemption’ of a Margin Loan in the secondary market, does require full collateralization of the full loan notional at order entry for the period between the order submission and the settlement of the trade.
