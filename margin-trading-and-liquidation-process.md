# Margin Trading and Liquidation Process

Under a trustless model, option sellers post collateral at the time of order submission to cover the maximum payoff of the option they are selling.

For example, a Call Seller sells a call option on 1 BTC against USDC and posts 1 BTC as collateral. This guarantees the pay-out to the counterparty. In this example, under margin trading, 1 BTC of collateral will still need to be locked up, however, part of that (say 25%) will be posted by the option seller and the remainder (75%) will be financed by a lender (the “Margin Lender”). The Margin Lender will receive a premium for protecting the derivative counterparty on the other side of the trade against the risk of collateral not being enough to satisfy his obligations.

The Margin Loan borrower will get a margin call when the value of his derivative positions falls below a certain threshold. To avoid liquidation, the borrower can transfer more funds to his Margin Lock account. The risk for the lender is related to potential slippage when liquidating a portfolio which is underwater. We refer to that risk as Liquidation Risk.

**Liquidation Risk**

Liquidation Risk is defined as the risk that once the liquidation process has been initiated, the Linked Derivatives in the related Sub-Portfolio cannot be liquidated at a value that is sufficient to repay all the Margin Loan Principal. This is implemented via a requirement for the user’s Margin Balance (net asset value) to cover the Maintenance Margin. All else equal, higher Maintenance Margin equates to lower Liquidation Risk.&#x20;
