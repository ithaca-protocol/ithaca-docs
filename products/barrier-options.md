# Barrier Options

Reduce option premium compared to premium for call and put options by making option payoff contingent on ETH price either reaching or not a predetermined barrier. User buys (+) an ETH equivalent number ( size ) of calls ( UP ) or puts ( DOWN ), at a strike user can choose that knock IN or knock OUT at a barrier user can choose.

1. An Up and Out call is cheaper than a call with the same strike as the call upside is not enjoyed beyond the barrier; user prefers an UaO call to a call if he thinks that the underlying will not move beyond the barrier.
2. An Up and In call is cheaper than a call with the same strike as the call upside is only enjoyed beyond the barrier; user prefers an UaI call to a call if he thinks that the underlying will move beyond the barrier.
3. A Down and Out put is cheaper than a put with the same strike as the put downside is not enjoyed beyond the barrier; user prefers a DaO put to a put if he thinks that the underlying will not move beyond the barrier.
4. An Down and In put is cheaper than a put with the same strike as the put downside is only enjoyed beyond the barrier; user prefers a DaI put to a put if he thinks that the underlying will move beyond the barrier. Protocol feeds indicative barrier pricing calculated by a proprietary pricing engine. ( User will be presented in a later release with the choice between, the pricing engine or a delta-adjusted last filled order, feeds.)

* User will need to input his order level
