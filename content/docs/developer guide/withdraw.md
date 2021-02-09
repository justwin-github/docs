---
title: "Withdraw"
description: "Solutions to common problems."
lead: "Solutions to common problems."
date: 2020-11-12T15:22:20+01:00
lastmod: 2020-11-12T15:22:20+01:00
draft: false
images: []
menu: 
  docs:
    parent: "Developer Guide"
weight: 35
toc: true
---

```
function withdraw() public onlyAfterInit() onlyOwner() {
    require(CT.payoutDenominator(condition) != 0, 'Market needs to be resolved');
    uint[] memory dust = new uint256[](numOutcomes);
    uint p = 0;
    for (uint i=0; i<numOutcomes; i++) {
      dust[i] = 1<<i;
      /* console.log('Index', 1<<i); */
      p = CT.getPositionId(IERC20(token), CT.getCollectionId(
        bytes32(0), condition, 1<<i)
        );
    }
    CT.redeemPositions(IERC20(token), bytes32(0), condition, dust);
  }
```

This function can only be called by the user that deployed the contract and after the outcome of the prediction market has been reported. It converts all the tokens that the market maker has into collateral and then withdraws all the collateral. w