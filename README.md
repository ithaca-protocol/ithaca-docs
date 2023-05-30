---
description: Composable derivatives
---

# Introduction

## What is Ithaca?

Elektro is a non-custodial derivatives trading and settlements protocol. At its core, Elektro is a technical framework that will allow users to create fully collateralized, composable derivatives. The Elektro protocol leverages Frequent Batch Auctions (FBAs). In FBAs, time is treated as discrete instead of continuous, and orders are processed in auction batches instead of serially. This eliminates front-running and creates a uniform clearing price. This isn’t the cool part.

By utilizing FBAs to clear derivatives, we can **decompose the clearing of financial contracts into packages of ‘atomic’ instruments**, using **put-call parity** relationships. This enables order matching at the atomic instrument level, and for ‘synthetic’ orders to be formed via replication principles involving multiple interdependent legs.&#x20;



**Market Disruption**

Traditionally/currently, derivative instruments on the same underlying clear in independent markets, usually, not only in digital assets, on different exchanges, with all the concomitant operational, counterparty, funding risk implications. Margin loan and funding markets are also operating independently. Liquidity across these markets is fragmented by definition and gets delivered by market participants that extract the value associated with providing these liquidity pooling services across instruments, venues and market structure arrangements.

Specifically:

1. Traders, arbitrageurs, market makers broadly, enforce the linkages between independent markets
2. Traders of listed derivatives, funding repo, 1 delta products extract revenue associated with spot vs vanilla options delta hedging at inception, lifetime, expiry as well as creating implied ‘OTC equivalent‘ markets on funding rates, collateral posting and collateral swaps
3. Structured products and associated structuring efforts and intermediaries managing internally option path dependence and binary/barrier risk and earning the associated high margins

Elektro is disrupting the web of interlocking ad hoc, haphazard, lucrative (but also risky for the undertakers) constraints to trading activity, by embedding the financial logic associated with the activities onto the rules governing matching within the protocol itself.
