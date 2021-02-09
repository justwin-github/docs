---
title: "Conditional Token Framework"
description: ""
lead: ""
date: 2020-10-13T15:21:01+02:00
lastmod: 2020-10-13T15:21:01+02:00
draft: false
images: []
menu: 
  docs:
    parent: "overview"
weight: 5
toc: true
---

The [conditional tokens framework](https://docs.gnosis.io/conditionaltokens/) is a set of audited contracts published by gnosis which can allow for the creation of prediction markets. These contracts have their own extensive [documentation](https://docs.gnosis.io/conditionaltokens/docs/devguide01/). 

In essence, a user is able to create a condition. This condition represents a particular question and has a number of parameters. These parameters are set using the setup() function and explained below:

-   Oracle. This is an address and represents how the outcome of the question will be determined. This can either be an externally owned account or a smart contract. We discuss this further on the _oracle_ page.

-   Outcome slot count.  This is an integer and represents how many different discrete outcomes are available for the question.