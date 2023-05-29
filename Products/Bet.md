---
order: 10
---

# Bet


Risk X$ to earn Y$ if ETH within specified range at expiry; earn Y/X % return

Risk X$ to earn Y$ if ETH outside specified range at expiry; earn Y/X % return

### Inputs
User chooses whether he wants to bet
- on an ‘inside range’ ( ETH price landing at expiry between chosen ETH price levels )
or
- on an ‘outside range’ ( ETH price landing at expiry outside chosen ETH price levels )

Protocol feeds indicative pricing and indicative % return calculated by a proprietary pricing
engine.

User will be presented in a later release with the choice between, the pricing engine or a delta-adjusted last filled order, feeds.
User will need to input his order level, which will be comprised of after choosing the strike:
- Entering the amount of USDC user wants to risk
- Entering the amount of USDC user will be satisfied in earning for the amount of risk and the range he has already chosen.

Earn % will be automatically calculated