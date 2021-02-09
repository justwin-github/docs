---
title: "Price"
description: "Solutions to common problems."
lead: "Solutions to common problems."
date: 2020-11-12T15:22:20+01:00
lastmod: 2020-11-12T15:22:20+01:00
draft: false
images: []
menu: 
  docs:
    parent: "Developer Guide"
weight: 38
toc: true
---

```
function price(
    uint256 _outcome,
    int128 _amount
  ) public view returns (int128) {
    return cost_after_buy(_outcome, _amount) - current_cost;
  }
```
This function takes the same parameters as buy().

It will tell you the price to buy that number of outcome tokens and is denominated in the base currency.

This is also returned as a fixed point integer and so will need to be converted into a human readable value by a front end.

It is worth mentioning that the price here is the price to buy all the tokens, so if you wish to buy 10Dai worth of tokens with three separate outcomes, you may expect the price here to be 3-4Dai.

If you wish to calculate the price per token (and therefore an approximate for the implied probability) you can divide this by the number of tokens.
