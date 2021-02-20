---
title: "Buy"
description: ""
lead: ""
date: 2020-11-12T15:22:20+01:00
lastmod: 2020-11-12T15:22:20+01:00
draft: false
images: []
menu: 
  docs:
    parent: "Developer Guide"
weight: 34
toc: true
---

```
function buy(
    uint256 _outcome,
    int128 _amount
  ) public onlyAfterInit() returns (int128 _price){
    int128 sum_total;
    require(_outcome > 0);
    require(CT.payoutDenominator(condition) == 0, 'Market already resolved');

    for(uint j=0; j<numOutcomes; j++) {
      if((_outcome & (1<<j)) != 0) {
        q[j] = ABDKMath.add(q[j], _amount);
        total_shares = ABDKMath.add(total_shares, _amount);
      }
    }

    b = ABDKMath.mul(total_shares, alpha);

    for(uint i=0; i< numOutcomes; i++) {
      sum_total = ABDKMath.add(sum_total,
        ABDKMath.exp(
          ABDKMath.div(q[i], b)
          ));
    }

    int128 new_cost = ABDKMath.mul(b,ABDKMath.ln(sum_total));
    _price = ABDKMath.sub(new_cost,current_cost);
    current_cost = new_cost;

    uint token_cost = getTokenWei(token, _price);
    uint n_outcome_tokens = getTokenWei(token, _amount);
    require(IERC20(token).transferFrom(msg.sender, address(this), token_cost),
      'Error transferring tokens');
    IERC20(token).approve(address(CT), getTokenWei(token, _amount));
    CT.splitPosition(IERC20(token), bytes32(0), condition,
      getPositionAndDustPositions(_outcome), n_outcome_tokens);
    uint pos = CT.getPositionId(IERC20(token),
    CT.getCollectionId(bytes32(0), condition, _outcome));
    CT.safeTransferFrom(address(this), msg.sender,
      pos, n_outcome_tokens, '');
  }
```
This function is used by users to buy outcome tokens from the automated market maker. It needs to be provided with the outcome that the user wishes to buy tokens for and the amount.

`_outcome` is an integer representation of the bitwise array for the particular outcome that the user is betting on. This is described well within the Gnosis documentation here. We expect it would be easier when implementing this within a project to calculate this value within the front-end interface as it will be complicated for users.

`_amount` is a fixed point decimal representation of the amount of outcome tokens that the user wishes to buy. We note here that 1 outcome token pays off 1 token of the base currency, so there will be significant differences between tokens such as wBTC and dai in terms of their price. If you wish to buy one outcome token, you will need to enter `18446744073709551616` which is 1 * 2^64. Again, we expect this to be abstracted away from users with a front-end interface.

Behind the scenes, the contract is taking some of the collateral token and splitting it into different outcome tokens. For example, if an event has three outcomes and a user is trying to buy 10 Dai worth of tokens, then it will take 10 dai and split that into the following positions: 10Dai:(A), 10Dai:(B) and 10Dai:(C).
If the user chose outcome A, then he will need to buy the 10Dai:(A) tokens from the market maker. The market maker determines the price for this based on the LsLMSR algorithm.

If the user chose the correct outcome, he can convert his tokens into 10Dai and will have made a profit. The profit will be the difference between the price he was charged by the market maker and the value of the 10 Dai. If he is incorrect, then his tokens will be worthless and the market maker will be able to redeem the positions which have value and will have also profited from the price that the user paid for the trade. 
