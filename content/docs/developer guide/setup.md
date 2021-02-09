---
title: "Setup"
description: "Solutions to common problems."
lead: "Solutions to common problems."
date: 2020-11-12T15:22:20+01:00
lastmod: 2020-11-12T15:22:20+01:00
draft: false
images: []
menu: 
  docs:
    parent: "Developer Guide"
weight: 4
toc: true
---

*code goes here*

This function allows you to customise the parameters used by the market maker in employing the Ls LMSR algorithm.

This function can only be called by the contract deployer and it is necessary to be called before it is possible to trade with the market maker.

_oracle: this is the address that is required to report the outcome of the prediction market at resolution. It can either be an externally owned account or a reference to another smart contract. This does not need to be the same as the person that deployed the contract. There are a variety of approaches that can be used for choosing the oracle and we discuss these further below.

_numOutcomes: an integer representing the number of discrete outcomes available for the market.

_subsidy: this represents the total amount of tokens that will be used to fund the market maker and can be viewed as the initial liquidity. The properties of the LsLMSR are such that choosing large numbers for _subsidy (ie where you fund the market maker with large amounts of capital) provides a tighter spread for traders, however results in higher potential losses (bounded loss is proportional to initial capital)

_overround: this represents the amount of profit that can be made by the market maker or the 'houses edge'. It is expressed in 'bips' where a value of 100 is equivalent to 1% profit. For reference, traditional book makers use an overround of between 5 and 10%. It is worth noting here that the calculations used by LsLMSR involve exponential arithmetic. Where the overround value is small, it is possible for the calculation to overflow (this is a limitation of the ethereum virtual machine). We would therefore recommend the minimum overround to be set at 3% but the actual minimum will be dependent on the number of outcomes available.

In order for the oracle to report the outcome of the prediction market, it must call the function reportPayouts() found on the conditional tokens contract. This can either be done directly where the oracle is an externally owned account, or by a separate smart contract.

It is possible to write additional smart contracts that can interact with chainlink nodes or kleros arbitration and parse this information in a manner that can be reported to the conditional tokens contract.

Through the clever use of smart contracts, any information that can be brought on-chain can be used to create a prediction market. 