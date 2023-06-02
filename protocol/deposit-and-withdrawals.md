# Deposit & Withdrawals



Participants need to deposit assets as a collateral in order to execute trades. Those assets are deposited with the FundLock smart contract.

If and when an order gets executed and settled as a contract position, the premium gets settled to and from the participants FundLock accounts and the Collateral related to the new positions gets taken out of the FundLock account into the Ithaca Collateral pool. Users may query their FundLock account balances directly on the blockchain.

As soon as the cash deposit has settled on the network, the balance in the FundLock address will reflect the deposit and the funds are available to be used to fund new orders.

Participants can also always request a withdrawal of their funds from their FundLock account, however, if the withdrawal would reduce the balance below the amount that is currently marked and locked for live open orders, the withdrawal will be delayed until such orders are automatically cancelled on the trading Platform freeing up the amount of cash that the participant attempts to withdraw. When a user borrows, funds are moved from FundLock to MarginLock contract. Users may deposit to MarginLock directly on-chain.&#x20;

A user may withdraw from MarginLock to FundLock up to the Maintenance Margin minimum threshold. A user may not withdraw from MarginLock directly to its wallet, but must instead transfer from MarginLock to FundLock and withdraw from FundLock to its wallet
