---
description: >-
  The Ithaca Matching Engine (IME) is a multi-product cross-orderbook matching
  engine
---

# Ithaca Matching Engine

Ithaca Matching Engine has 3 key elements.&#x20;

1\) **Replication** - IME decomposes financial contracts into packages of 'atomic' instruments using put-call parity relationships. Therefore, orders are matched at the atomic instrument level.

2\) **Conditional orders** - 'Synthetic' orders are formed by replication principles while the matching engine algorithm ensures simultaneous consistent execution.

3\) **Mixed Integer Programming (MIP) Optimization -** MIP allows searching for clearing prices and associated sets of consistent orders satisfying them that maximise executed volume and satisfy best execution requirements.

The Ithaca Matching Engine (IME) is composed of discrete-time (periodic), uniform-price (one price per instrument per auction), double (orders submitted by buyers and sellers) auctions that allow for the pooling of orders for different underlying instruments and payoffs and thus the utilisation of multiple different sources of liquidity that normally clear in isolation from each other. The IME, therefore, constitutes the foundation of a marketplace in which growth and liquidity generation are supported by IME-intrinsic, multiple-sided network effects.

In the standard continuous order book methodology, price discovery for each instrument happens in the context of each order book as a silo, with "cross order book" or "cross instrument" liquidity delivered haphazardly and, on an ad-hoc basis. The IME is a mechanism for harnessing, in a systematic fashion, all market making tools and making them accessible to every market participant.

Buy and Sell orders, including orders for pre-packaged and structured products and conditional orders are matched based on the principles of maximum matched volume and minimum surplus across all orderbooks covered. If several possible sets of match prices exist that would result in the same amount of total matched volume and the same volume of surplus across all orderbooks, the set of match prices within the group of possible sets that results in the smallest total price deviation to the previous set of reference prices across all orderbooks will be the determined set of match prices.

Once the set of match prices for all contracts is being determined, all orders that are being allocated a fill at the determined prices are filled accordingly and are either removed from the orderbook or their live outstanding size updated.

If there are surpluses of match-able quantity on one side of an orderbook, all orders on that side are partially filled according to their original order size relative to the overall executable quantity. Fully executed orders are removed from the Orderbook at the end of the Matching Phase. Partially filled orders are automatically updated in the Orderbook to appropriately reflect the new and remaining size and state of the order.

## 1 Overview

The MIP approach simultaneously looks for clearing prices and a set of orders satisfying them, which clear fully against each other (i.e. there are no unmatched tokens), while guaranteeing maximal volume execution, and further best execution constraints. In particular, it does this in the presence of conditional orders and with automatic replication.

### 1.1 Challenge

Clearing a single book is conceptually straightforward - regular exchanges do it all the time. The difference in the Ithaca set-up is the presence of Conditional Orders - orders on more than one token, which execute conditional on the net price of the tokens, and conditional on the presence of other orders to clear against. Suddenly, the clearing of one book depends on another — a conditional order may provide a competitively-priced fill, but only if its other leg is also filled in another book, etc.&#x20;

Given prices, it is fairly straightforward to determine a set of clearing orders that maximizes volume. Conversely, given a set of clearing orders, it is straightforward to find a price that satisfies them. What is difficult is to do both at the same time.&#x20;

To overcome this difficulty, Mixed Integer (Linear) Programming is used.

### 1.2 Mixed Integer Programming (MIP)

MIP is an optimisation technique, solving Linear Programming problems, but with the additional stipulation that some variables must be integers, or even just 0 or 1. Such binary variables can be used to encode logical decisions, such as “if price is greater than 100, this order cannot fill”. This [document ](http://web.mit.edu/15.053/www/AMP-Chapter-09.pdf)is a nice introduction to some of the possibilities this opens.



## 2 Formulating the clearing problem as MIP

At the most general way, the clearing problem can be specified as follows:&#x20;

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

Without condition (3), this would be a linear program, however we must also search over prices, and in turn eligibility of the orders for clearing.&#x20;

Assuming for a moment we can solve this problem via some kind of iterative approach, we will find that for any price vector, there are various orders satisfying (3). An iterative solver would try to maximise the _`xj`_ filled amounts, subject to the clearing condition. But then it would also try to adjust the prices _`p`_, to come up with a better subset of orders. This sounds like computationally infeasible.&#x20;

Fortunately, this kind of condition is readily expressible in the MIP framework. Consider the following inequality:

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

where _`M`_ is a large parameter. If _`i = 0`_, the inequality reduces to _`x ≤ y`_, but if _`i = 1`_ and _`M`_ is sufficiently large, the inequality is automatically satisfied, and the condition specifies no relationship between _`x`_ and _`y`_.

The problem above can thus be augmented as follows:

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

Note that all inequalities are linear, so we satisfy the requirements of linearity. Further, conditions (9) and (10) imply that _`wj · p ≤ lj ⇐⇒ ij = 1`_. Indeed, for carefully chosen values of `A` and `B` constraints, if _`wj · p ≤ lj`_ , then (9) implies _`ij = 1`_ (otherwise the inequality cannot be satisfied). Conversely, if _`ij = 1`_, inequality (10) reduces to _`wj · p − lj ≤ 0`_. Finally, inequality (13) implies that if _`ij = 0`_, the associated traded quantity _`xj`_ must also be 0.

Consequently, the following are true:&#x20;

1. The optimisation program is a linear, mixed-integer program, and readily solvable with off-the-shelf solvers
2. The optimisation problem also encodes (part of) the Ithaca rules for clearing books: choose prices and orders satisfying them, such that the orders clear, and such that the overall crossed volume is maximised
3. Thanks to the formulation, the order selection and price determination happen automatically. The search over prices and order sets occurs inside the MIP solver, in a computationally-efficient way.
4. A regular linear program would not be sufficient here; if the variables _`ij`_ are allowed to take fractional values between 0 and 1, the logic encoding no longer works.

The problem is phrased slightly differently in the prototype, to enable other features as well, but the general idea is the same, and this is a good starting point for understanding the approach.



## 3 Full problem specification

In this section, I describe in full how the problem is specified in the solver. It is conceptually the same as the example above, but has some further features.

The clearing process consists of solving a sequence of closely related optimisation processes. This is because the clearing process in fact maximises a number of objectives in sequence: first maximise volume, then minimise surplus subject to maximising volume, then maximise price surplus subject to keeping the other quantities constant. Finally, select a single price satisfying a final disambiguation criterion. The first three problems are MIP problems, the last one is a Quadratic Program (QP), with possibly simpler formulations being possible.

Overall, the problem is split logically as follows:

1. Order data is pre-processed
2. MIP problem is solved to maximise volume
3. MIP problem is solved to minimise surplus, subject to keeping volume constant
4. MIP problem is solved to maximise price surplus, subject to keeping volume and surplus constant
5. QP is solved to choose final price

### 3.1 Data pre-processing

The following steps are taken:

1\. Orders are grouped by side, tokens, and relative weights of the tokens. The following are examples of orders that would get grouped:

* All buy orders for a single token _`X`_
* All conditional orders to buy _`1 X`_, sell _`1 Y`_
* Conditional order to sell _`1 X`_, buy _`1 Y`_, and conditional order to sell _`10 X`_, buy _`10 Y`_

If orders are on opposite sides, or acquire different proportions of tokens, they are not grouped. Each group has weights w associated with it, describing what tokens are being traded in the group, normalised so that

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

For the above examples, the weights will be _`(1), (0.5, −0.5), (−0.5, 0.5)`_. I refer to these associations as groups.

2\. Within a group, I order and sub-group orders by price, from most aggressive to least aggressive. A sub-group of orders in the same group, and with the same price, is referred to as an _interval_. Each interval has a quantity _`q`_ associated with it, which the maximal amount of tokens that can be filled at this price. The price condition is expressed as

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Examples:&#x20;

* For a buy order for at most 100: _`(1) · (p) ≤ 100`_.
* For a sell order for at least 150: _`(−1) · (p) ≤ −150`_.&#x20;
* For a conditional order to buy _`0.5 X`_, sell _`0.5 Y`_, receive 10 premium: _`(0.5, −0.5) · (pX, pY ) ≤ −10`_

3\. Quantities _`A`_ and _`B`_ are also calculated for each interval, to be defined later.
