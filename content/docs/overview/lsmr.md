---
title: "LSMR & LS-LSMR"
description: ""
lead: ""
date: 2020-11-16T13:59:39+01:00
lastmod: 2020-11-16T13:59:39+01:00
draft: false
images: []
menu:
  docs:
    parent: "overview"
weight: 7
toc: true
---

## LMSR

The [Logarithmic Market Scoring Rules](http://mason.gmu.edu/~rhanson/mktscore.pdf) (LMSR) market maker has a rich academic history  and was designed specifically with prediction markets in mind.

According to the [Gnosis Developer Portal](https://docs.gnosis.io/conditionaltokens/docs/introduction3/) the advantages of the LSMR compared to the CPMM are:

-   LMSR has more recognition in academic work, and its properties are much more studied than CPMM. It is easier to find papers on the properties of LMSR, or to leverage existing research on this market maker.

-   LMSR breaks down into self-similar components when applied to combinatorial prediction markets. Its analysis in those scenarios has been quite explored in the literature. 

-   Finally, the closed form expressions for buying and selling with the LMSR allow calculating a net cost for a batch of buys and sells done simultaneously. The CPMM does not admit such an expression for the prediction market use case, so buying and selling is limited on the contract to one outcome token at a time.

Nonetheless, there are only two projects that we know of that have implemented an LMSR market maker. These are:

-   [Gnosis implementation](https://github.com/gnosis/conditional-tokens-market-makers)

-   [Charm.fi implementation](https://github.com/charmfinance/charm-options)

## LS - LMSR

The liquidity sensitive LMSR (LS-LMSR) AMM algorithm was described by Othman et al. 2013 ([A Practical Liquidity-Sensitive Automated Market Maker](https://www.cs.cmu.edu/~./sandholm/liquidity-sensitive%20automated%20market%20maker.teac.pdf)) and offers an improvement on the traditional LMSR algorithm.

For our purposes, this algorithm has the following desirable properties: 

-   Can be used as a price oracle for tokens using the conditional token framework (CTF).

-   Bounded loss can be set as a parameter, such that the downside to liquidity providers is limited. 

-   Overrounding can also be set as a parameter. This is the main function that helps LPs achieve profits.


### Advantages

-   Similar UX to Uniswap LP-ing, familiar to DeFi users. However, the downside is much less complex to estimate than in Omen or in Uniswap

-   Market depth increases with trading volume -> less spread for more popular markets

-   User that creates market does not need to set initial probabilities of outcomes

-   Functions well in both low and high liquidity markets (dynamically adjusts the b parameter from the LMSR based on liquidity such that low liquidity markets have larger spread)

### Disadvantages

-   Does not easily allow users to sell their outcome tokens back to the AMM

-   Bounded loss makes for a better UX, but is capital inefficient. 

-   Does not respond to market shocks well (ie if a presidential candidate pulls out)

-   Requires the initial probabilities to be equal at initialisation - there will be immediate arbitrage opportunities for traders at initialisation

-   Prices do not converge to a well defined probability estimate, but instead fluctuate about an equilibrium price

-   Profit-taking is fixed regardless of liquidity -> therefore in the larger popular markets (such as the result of an election), prices will be greater than those offered on centralised exchanges 

-   AMM cannot re-buy tokens from users at the same price

As far as we know, no live project currently uses an LS-LMSR. The Charm.Fi team implemented one for their decentralised trading platform, but decided to use the LMSR instead.
