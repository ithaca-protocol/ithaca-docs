# FundLock



`FundLock` is a smart contract managing client deposits. Clients may deposit and withdraw directly into/from `FundLock`. Only clients can withdraw funds from `FundLock`, no one else.

`FundLock` also acts as a ‘gateway’ for what can enter and exit the system. Clients may only deposit whitelisted tokens. The whitelisting process for token addresses is currently executable only by the Java backend. Upon expiry of a financial instrument, a client’s payout is made available from `FundLock`.

`FundLock` keeps track of balances of all users.

## Client Interactions

### deposit()

Used to deposit tokens into FundLock. If Ether is used to deposit it needs to be sent with a transaction and then works under the hood as WETH. Important to know that WETH and ETH are essentially the same thing inside the Ithaca Protocol trading logic. If a user doesn’t have WETH, he can send regular ETH (If on Ethereum main chain) which will be converted by FundLock into WETH. The client will also get his funds back as ETH.

### withdraw()

Used to send ‘request to withdraw’ to mark an amount of tokens client wishes to release later when it is allowed.

{% hint style="info" %}
No tokens are transferred at this point!
{% endhint %}

{% hint style="info" %}
A maximum of 5 withdraw requests can be stored of the same token from one client! Before adding any new ones a client must release some of the existing ones.
{% endhint %}

{% hint style="info" %}
Client's balance will change after this operation, but his marked funds can still be used to cover his trades if his remaining balance is not enough!
{% endhint %}

&#x20;See "Withdrawal flow" below for more details.

### release()

Used to release tokens (!) marked for withdrawal previously (!) from FundLock to the user, `withdrawTimestamp` parameter is used to determine which funds should be withdrawn as there can be multiple withdrawal requests for the same token and client (5 requests max).

## Client Withdrawal Process

Withdrawing funds is a two step process split into:

1. **Withdraw** - in essence a ‘request to withdraw’ which marks funds and moves balance from one mapping to another while introducing some limitations on these funds usage (`releaseLock` and `tradeLock`).
2. **Release** - with a time lock (`releaseLock`) in between. This allows the backend to react and ensure all options are always covered. The time lock also ensures the client has eventual access to its funds if the backend were to cease being operational. The backend cannot prevent a withdrawal indefinitely.

### Trade Lock Period

`tradeLock` outlines a timeframe during which funds marked for withdrawal can still be used to cover client’s trades before actual release. `tradeLock` is a state variable on `FundLock` that is set at construction and can be changed later by a direct setter call.

{% hint style="info" %}
When release() is called, there’s no option to pass amount to the function, which means that client will get the amount that is left written to storage after everything that happened while lock timeframes were enforced.
{% endhint %}

#### Example

* A client has 100 WETH in his `balanceSheet.`
* He calls `withdraw()` to mark 50 WETH from his balance to be released later.
* His `balanceSheet` remainder will be 50 WETH and 50 WETH will be recorded to his `fundsToWithdraw` balance.
* `tradeLock` and `releaseLock` are enforced blocking him from calling release until the required time has passed.
* He makes a trade and settlement requires him to cover a collateral of 70 WETH.
* `FundLock` checks his `balanceSheet` first, figures out he does not have enough funds, so it checks his `fundsToWithdraw` balance where he finds 20 WETH available to add to his collateral, then it checks that `tradeLock` time is still ongoing which signifies, that 20 WETH can be subtracted from his `fundsToWithdraw`.
* `FundLock` covers his trade making his `balanceSheet` = 0 and his `fundsToWithdraw` balance = 30 WETH (remainder).
* Client calls `release()` with a timestamp of his previous withdraw request (taken from the `Withdraw` event timestamp).
* Client receives 30 WETH to his wallet.

At any point during this process if any of the requirements are not met a revert is performed negating the transaction.

### Release Lock Period

`releaseLock` is a timeframe set by Java Backend to ensure that a client can not release his funds before all transactions and trades are settled. This prevents cheating, front-running and ensure that any backend or network delays will not affect trade settlement or cause errors in calculations.

This is a static, configurable variable. The backend calculates the estimated block number based on the time lock which is passed to the contract.

{% hint style="info" %}
A client can not release his funds until releaseLock preiod has passed!
{% endhint %}



### State variables

* **Funds** – struct containing data for funds marked for withdrawal:
  * **value** – the amount of funds marked for withdrawal
  * **timestamp** – block timestamp for when the funds were marked (necessary for releasing them later)
* **ALLOWED\_WITHDRAWAL\_LIMIT** - maximum amount of ‘requests to withdrawal’ can be active (funds not yet released) at a time. _Set to a max of 5 requests!_
* **fundsToWithdrawS** - mapping to store user ‘request to withdrawal’. User address => token address => (amount of tokens to withdraw, timestamp of withdrawal request)
* **tokenManager** - address of TokenManager contract needed for FundLock logic
* **registry** - address of Registry contract needed for FundLock logic
* **balanceSheetS** - actual user balances of funds deposited in FundLock (user address => token address => amount). _This value does NOT include fundsToWithdraw._
* **releaseLock** - time interval that has to be passed between calling _withdraw_ and _release_
* **tradeLock** - time interval after which funds marked as to be withdrawn can be used for trading
