---
order: 18
---


# Ithaca Matching Engine (IME)

!!!info
EVM-compatible, verifiable, provably optimal, auction based, derivatives 
!!!

- Options ( Vanilla & Digitals )
- Forwards | Forward / Forward Swaps
- Borrowing & Lending
- Structured Products & Prepackaged Strategies 
- Margin Loans 

**Matching engine that powers an**
- Algorithmic Market Maker and a 
- Collateral Management & Settlement Protocol

**Incorporating**
- Replication & VAR trigger margin lending accommodating conditional orders

**Currently:**
- Different Derivative instruments on the same underlying asset are traded across different order books, on different exchanges and protocols 
    - Operational risk
    - counterparty risk
    - funding risk
- Liquidity pooling across instruments and venues is delivered by market markers extracting value from end users. 
- Margin lending markets are operating on an OTC basis accompanied by inefficient capital deployment and lack of transparency. 
- Exotic derivatives trading desks earn high margins managing option path dependence and barrier risk.

IME is empowering users to directly access sophisticated derivative structuring and execution capabilities as well as the associated PNL, by embedding market makers’ risk taking financial engineering logic onto an algorithmic market maker.

## Characteristics

Auctions 
- Frequent Batch ( -> MEV resistance )
- Discrete-time (periodic)
- Uniform-price (one price per instrument per auction)
- Double (orders submitted by buyers and sellers)


## Risk Sharing Building Blocks (RSBBs)

- ‘ Spot Cash ‘; Per-auction settled collateralized forward (‘MEV resistant’ spot)
    - ‘within auction‘ option settlement
    - Delta- hedged option execution 
- ‘Forward Cash’; Defined maturity collateralized forward; 
    - ability to trade collateral swaps, spot/forward swaps, forward/forward swaps as standard conditional orders; 
- European Call Option (by put/call parity, Put Options); 
- European Binary Call Option (by put/call parity, Binary Put Options)

## Matching Engine Optimization Setup

- Synchronous matching across RSBBs 
-    Decompose statically replicable derivatives into RSBBs using put-call parity and funding-option equivalence relationships 
    - Collateralized borrowing & lending enforced by no-arbitrage
    - Reconstitute derivatives from RSBB constituent parts 
- Conditional orders
    - ‘Synthetic’ orders formed by replication involve multiple interdependent legs with matching engine algorithm ensuring simultaneous consistent execution. 
- Matching engine instances can run across multiple orderbooks with different products, underlyings and maturities
- DIY statically replicable derivative payoffs embedded within matching engine
    - Structured Products rendered uniquely accessible as auction orders 
    - Variance swaps as multi-legged conditional orders

## VaR Engine
- VaR trigger-based, simultaneously clearing, derivatives margin loan market
    - Price discovery for risky funding curves


## Liquidation Engine
-  Ongoing margin monitoring; ‘within-engine’ liquidity boosting,replication-based liquidation process

## Portfolio Collateral Optimization
- Derivative portfolio collateral management 





## Mixed Integer Linear Programming Optimization
- Allows searching for clearing prices and associated sets of consistent orders satisfying these that maximize executed volume and satisfy best execution requirements.
- MILP utilizes advanced heuristics to perform an efficient search of the solution space; for binary integer constraints, the branch and bound algorithm is used recursively.