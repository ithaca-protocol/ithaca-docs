# Ledger

Each `Ledger` contract represents a market for a specific currency pair. When a new currency pair market is needed a new ELedger contract is deployed. Within each market trades for different contractId’s which use this currency pair can be made. I.e. We can trade CALL and PUT options with different expiration dates using the same contract. The storage of each Ledger contract has respective addresses of underlying and strike currency which together represent a currency pair used by this market.

```solidity
    address public underlyingCurrency;
    address public strikeCurrency;
```

Within each Ledger contract client positions are represented by respective mapping. Positions are per contractId per trader. Each position denotes amount of option contracts each trader has in respective contractId (size). The size can be either negative (Sell) or positive (Buy).

```solidity
    mapping(uint32 => mapping(address => int64)) public clientPositions;
```



## Ledger contract update flow

Orders settlement flow `updatePositions` function accepts an arrays of arguments. Logically these arrays can be split onto 2 virtual data structures: "Fund Movement" and "Position". Those will be represented by array groups passed as arguments.

```solidity
  function updatePositions(
    // Position update part of arguments
    address[] memory positionClients,
    uint32[] memory positionContractIds,
    int64[] memory positionSizes,
    // Fund Movement part of arguments
    address[] memory fundMovementClients,
    int64[] memory underlyingAmounts,
    int64[] memory strikeAmounts,
    // backend tracking arg
    uint64 backendId
  ) external;
```

* The purpose of **Position** part is to record position information to smart contract state.
* The purpose of **FundMovement** part is to move funds in the `FundLock` without recording any information into `Ledger` state. We represent things like premiums, collaterals, spot trades, margin loan underlying transfers as `FundMovement`.

Please also note that arrays of `positionClients`, `positionContractIds`, `positionSizes` should always have the same lengths and this property is enforced by smart contract. Similarly arrays `fundMovementClients`, `underlyingAmounts`, `strikeAmounts`, would also have the same length which is also enforced within smart contract. On the contrary `positionClients` would not necessarily have the same length as `fundMovementClients` as they do represent different things. Also, it is possible for Backend to not send one part or the other (e.g. Spot Trading would only have **Fund Movements** part and arrays for positions will be empty), however smart contract will revert if both `positionClients` and `fundMovementClients` arrays are empty signifying an error on the backend or the fact that a TX is being sent with no data in it, for which we would still have to pay gas.

First three arguments represent a **Position** part with single structure sharing the same array index.

```solidity
address[] memory positionClients,
uint32[] memory positionContractIds,
int64[] memory positionSizes,
```

The following three arguments represent **Fund Movement** part with single structure sharing the same array index.

```solidity
address[] memory fundMovementClients,
int64[] memory underlyingAmounts,
int64[] memory strikeAmounts,
```

Lastly the `uint64 backendId` argument is meant for injecting a backend generated transaction id to be able to immediately assign it to transaction without waiting for txHash to become available. This `backendId` is always used to emit an appropriate event with the following signature:

```solidity
event PositionsUpdated(
    uint64 indexed backendId
);
```
