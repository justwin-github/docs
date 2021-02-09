---
title: "Oracles"
description: ""
lead: ""
date: 2020-10-13T15:21:01+02:00
lastmod: 2020-10-13T15:21:01+02:00
draft: false
images: []
menu: 
  docs:
    parent: "overview"
weight: 6
toc: true
---
Within this page, we will describe different approaches to solve the oracle problem. If you want to know how to implement these within the prediction markets, please see the development page.

The oracle problem has been well documented within blockchain technology. Blockchain networks are designed in such a way as to be deterministic. This makes it easy to access data that is stored on-chain (such as checking the balance of a public address) but impossible to access off-chain data (such as the weather in California). 

An oracle acts as a bridge for this real-world data and offers an approach for bringing this data on-chain. There are multiple different oracle solutions and each has their own benefits/issues. We will discuss a number of different oracles that can be used to power prediction markets, although this list is not exhaustive.

### The trusted data source

A user can be nominated as the source of oracle information. This is the easiest oracle solution to implement. To give an example, I could create a prediction market with the question "Who will win the 2022 World Cup?" which has 32 different outcomes. After the tournament is over, I could submit a transaction onto the conditional tokens contract calling the function reportPayouts().

The main issue with this approach is that it relies on a trusted party to honestly submit the correct outcome. It is easy to imagine situations where this architecture can be abused by malicious actors.

### The objective data oracle

[Chainlink](https://chain.link/) is one example of a decentralised solution to the oracle problem and can be implemented with prediction markets to safely determine the outcome of an event. Broadly speaking, chainlink utilises a network of nodes to access off-chain data and submits this information on-chain for smart contracts to utilise. It is easy to implement price feed oracles from chainlink so it is possible to create prediction markets such as "Will the price of Bitcoin be greater than $100,000 by the end of 2021". It is also possible to interface with external adaptors using chainlink in order to obtain any data that can be found in an API.

There are limitations using chainlink in the type of data available. It is easy to obtain discrete data such as price feeds or sports scores, but difficult to establish outcomes where there may be conflicting data sources.

### The subjective data oracle

[Kleros](https://kleros.io/) provides a decentralised dispute resolution service and can also be used as a 'subjective oracle'. When utilising kleros, a number of jurors must vote and decide on the outcome and are financially incentivised for choosing the correct outcome. This is particularly important when there may be conflicting information from different sources of data. Kleros [case 302](https://thedailychain.com/case-302-a-moment-in-decentralized-history/) is the most notable example of this happening.