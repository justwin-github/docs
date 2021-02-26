---
title: "Interacting with Contracts"
description: ""
date: 2020-10-06T08:48:57+00:00
lastmod: 2020-10-06T08:48:57+00:00
draft: false
images: []
menu:
  docs:
    parent: "tutorial"
weight: 25
toc: true
---

You should now have deployed three contracts: The LsLMSR, FakeDai, and ConditionalTokens contracts. When we set up the LsLMSR contract, we will need to fund the contract with some initial liquidity. Therefore we will need to get some money before we can proceed with the set up!

### Creating tokens to play with

If you click on the deployed instance of the FakeDai contract, you should have a number of functions available. The orange functions are transactions that you can send, whilst the blue functions are view functions. 

<img src="https://lh3.googleusercontent.com/o1a_A8BGcumEd6-MIYGyZcHO28DRCn37lR2WTkrw80cR0vcbfCUgQ-FN_LINXLlIlNOvEwQW-0IWgtDFofyW7NiFdoDvnoQ81g6feTFhFNuVxxCIAH9heF812758c13xli66lUii" alt="compile" width="100%"/>

These functions are found on all ERC20 token contracts with the exception of the mint function. This mint function is a debugging function and takes two parameters, the 'account' and the 'amount'. You will need to enter your address in order to receive the tokens, this address can be found near the top of the remix interface. The 'amount' that you enter will need to include the appropriate number of decimals for the token you are using. If we want to create 1000 Dai (which has 18 decimals), we will therefore need to put the amount as 100000000000000000000. 

You can confirm that this worked by calling the balanceOf function and inputting the same address. This should output the same number.

Now that you have minted 1000 Dai, you need to allow the LsLMSR contract to spend this money. You will need to call the `approve()` function and input the address of the LsLMSR contract as the spender and the amount as the total amount that you will want to spend. Let us approve 1000 Dai. For me this looks like:

![](https://lh6.googleusercontent.com/AiSB8MrkMiYJ51SSUAS0kQNP_Xm5qoMphZ1P0ui6quTNC5XwkMD6qcv22ZMT65YaGiOLk2AL6S_vGkpAW58jzOwocaAiDv44Nn5V40vuUZ2ZfDQpAFXkDl9UDdBB8vlZqjhsTB7P)


### Setting up the LsLMSR

Now you will need to select the LsLMSR and run the `setup()` function. This has a number of parameters which we will discuss:

<img src="https://raw.githubusercontent.com/justwin-github/docs/main/images/setup0.png" width="100%">


-   `_oracle` will be an address that refers to either an externally owned address, or a smart contract. The prediction market will remain open until this oracle address tells the prediction market what the outcome of the event was. We invite you to read the oracle page for different ideas of what to put here. For our testing purposes, we can use the address for our account. This will be easy for us to report the outcome at the end of our tutorial

-   `_questionId` is a unique ID that refers to this particular market. You can input anything here as long as it conforms to the bytes32 type. This ID will be used by the oracle to report the outcome.

-   `_numOutcomes` is an integer which represents the number of different outcomes that are available. For example, the number of outcomes for a football match would be 3: home team win, away team win, draw. It is worth mentioning at this point, that the conditional tokens allow users to bet on combinations such as either home team winning or draw occurring however the number of discrete outcomes remains 3.

-   `_subsidy` is an integer which represents how much initial funding is available for the prediction market in order to create the prices. There is a trade-off to be made within the LsLMSR algorithm. More initial funding results in a more liquid market, however it increases the potential losses as the bounded loss is proportional to the initial liquidity.

-   `_overround` is an integer which represents how much 'profit' will be taken by the market maker. This number is represented in 'bips' where 100 is equal to 1% profit. If you wish to add 4.5% overround, you would therefore need to enter 450. Traditional book makers would tend to use values between 5 and 10% depending on the nature of the market.

Let us make a prediction market for the Superbowl.

`_oracle` will be our address. This means that we will be responsible for correctly reporting on the outcome after the superbowl.

`_questionId` will be the bytes32 representation of the word 'test'.

`_numOutcomes` will be 3: For Buccaneers win, Chiefs win, draw. 

`_subsidy` will be 1000000000000000000000 - this represents 1000 Dai in initial funding

`_overround` will be 450. This books us 4.5% profit

Your setup will look like this:

<img src="https://raw.githubusercontent.com/justwin-github/docs/main/images/setup1.png" width="100%">

Once you submit this transaction, the prediction market will be ready for users to interact with!

### Buying outcome tokens

The `buy()` function is used to purchase outcome tokens. This function has two parameters which require some additional steps to calculate: the outcome, and the amount.

In order to know what to put for `_outcome`, we need to briefly discuss how the conditional tokens contract represents each outcome. We have established that we have chosen three discrete outcomes for our event. If we assume the following outcomes:

Outcome 1: Buccaneers win

Outcome 2: Chiefs win

Outcome 3: Draw

We can therefore construct a position based on any combination of these outcomes. If we think that the chiefs will win but also that there may be a draw, then we will purchase tokens for outcomes 2 and 3. Conditional tokens allow us to have a position in both these outcomes by combining the outcomes into a collection. Outcomes that will be included within the collection will be represented in binary with a 1. We can therefore construct:

Outcome 1: 0

Outcome 2: 1

Outcome 3: 1

This can be expressed in binary as 0b110. Note that this is backwards where the first 1 represents outcome 3. 0b110 as an integer is 6.Therefore, if we wish to purchase tokens for Chiefs winning or there being a draw, we will need to put _outcome as 6.

The `_amount` represents the amount of tokens that we wish to purchase. It is worth mentioning at this point that 1 outcome token pays out 1 of the base currency. Although the price of underlying base currency can fluctuate, the outcome tokens will always be proportional to the price of the underlying. The `_amount` is submitted as a fixed point integer. This is the desired number multiplied by 2^64. If you wish to purchase one outcome token, you will therefore need to enter `18446744073709551616`.

Let us purchase outcome tokens that will pay out 10 Dai if the Chiefs win or draw. We will therefore need to enter:

![](https://lh4.googleusercontent.com/8vIHGuNt9scsc71rT-ERen_IrgOAvYAxvnf9DwMtdOe3cDSCuC0GQyriaLsP_0SQA5NjMdoZQjx7bAhb768McoauIRZK1JQbXlJmBNidH50NGG9cDP8bpWz_CgWZMvGch0DuNvOV)

Note the 0 at the end of _amount to signify ten outcome tokens.

If you followed the tutorial, you should have received an error message telling you "ERC20: transfer amount exceeds balance". See if you can mint some more dai, and then approve this to be spent again and re-try!

Congratulations, you've just bought your tokens from the market maker.

### Reporting payout and claiming rewards

As we set the oracle for the contract to be our address, we are able to set the outcome of the market. We do this by calling the `reportPayouts()` function found within the conditional tokens contract. We must input the questionId and the outcome.

The questionId is the ID that we used earlier. 

The payout variable takes an array of the relative payout for each outcome. As we chose three outcomes, we will need to enter an array with three variables here. If you wanted to report that the Chiefs won, you will enter this outcome as a 1 (which means each outcome token can be redeemed for 1 base currency) and the rest as zero. 

<img src="https://raw.githubusercontent.com/justwin-github/docs/main/images/reportpayouts.png" width="100%">

Now that you have done this, you can claim your reward for the successful portion of your position. You do this with the redeemPositions() function. This function takes your outcome tokens and then converts them back into the base currency token. The `redeemPositions()` function takes the following parameters:

-   collateralToken: the address for the ERC-20 token used

-   parentCollectionId: this will be `0x0000000000000000000000000000000000000000000000000000000000000000`

-   conditionId: this can be found by clicking on the 'condition' button within the LsLMSR contract or calculated by the `getConditionId()` function on conditional tokens

-   indexSets: this will be the same number as the integer representation of your positions, displayed as an array. For us this will be [6]

If you check your balance of collateral token, you should notice that you have more Dai than when you started!
