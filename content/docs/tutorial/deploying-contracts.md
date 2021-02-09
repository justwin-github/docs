---
title: "Deploying Contracts"
description: ""
date: 2020-10-06T08:48:57+00:00
lastmod: 2020-10-06T08:48:57+00:00
draft: false
images: []
menu:
  docs:
    parent: "tutorial"
weight: 23
toc: true
---

In order to deploy this contract, you will first need to compile the source code. To do this, you will need to open the compiler tab and then select a compiler version from the drop down box. As you can see from the screenshot, we are using version 0.6.6.

When you click the compile button you should get a number of yellow cautions, but no warning messages. 


<img src="https://lh4.googleusercontent.com/O-eJE005w53BHnEP8eFHw6vKU5wQ1lOY-F3RcqdR_ycOPD-BqtwPU5gxtthyJiIWaQm0_0C-qQFfO_ulNvXRwqqQXtTN-oMJx2exRR4B7vFF68qrFyeqlYFYswyM1QMhgIHG0Be-" alt="compile" width="100%"/>


Once you have successfully compiled the contract, you can begin to deploy the contracts.

To do this, you can click on the deploy and run transactions tab. There is a dropdown menu here called 'contract' which has all the different contracts within the source code. The important contract here is the LsLMSR contract which you can select.

<img src="https://lh6.googleusercontent.com/QP0pXWZToc1G08Smm6hmfgkwCu1O4Dsc_bXsw99xNZeiZHw6LBJkvvmlkb6NQoyDBPGpQ8kJUtTgrYp7dPWwf88c95VWKGsBS7Q7Qho-JnW3PA57GgNPs6EBXcTErxh-W4B33nWi" alt="deploy" width="100%"/>


You will notice when you select the LsLMSR that a text box appears. You can click the down arrow and it will expand to show you some more information. When deploying the LsLMSR contract, you are required to provide two addresses. The first address is for the conditional tokens contract, and the second address is for the token that you are using for the base currency.

We will be testing the contract locally first, so we will need to create our own copy of the conditional tokens contract. To do this, we need to select the conditional tokens contract from the drop down box. There will be a text field asking you for the gas limit. You need to set this to 7000000 otherwise you will get an error message.


<img src="https://lh3.googleusercontent.com/qHEPo-V0DnLM-6zn_jg1koHA1xwa3mAi6HoAQ2fmtLBFaZ4XpPO19xTuCehDn9Esr7ovkirvKsZO1mRAlzWNWocbdIJwAOT1mHDnIR9H6a6dbrlMTctzVW2w6okQvhHy7l3Yw4AX" alt="error_msg" width="100%"/>


Once you have deployed the contract, you should see the following:


<img src="https://lh5.googleusercontent.com/piN72EirtelwEq_ze3r3NS8SEnm4vmzfQdqY_DYugqVlgltIm2I9B_cbBqykbGmDpmp5xyCjIMMMPgqEr7hir1RiI1slTIYX_WPzzzGt6lz1gjRVQB-_FJTIMvKVT5CmUwiX23wf" alt="after_deploy" width="100%"/>


You can click on the clipboard to copy the address for this contract.

You will need to repeat the same process with the FakeDai contract. If you have done this successfully it should look like:

<img src="https://lh4.googleusercontent.com/EQxkFUTi4XqWmLxIZZk7XDGgnR02Sv2tMxXbb68hQDAtPg-_PbO2NqWohTs59wBDLnsmGtBclQm7sZOmUOeDXWb6kOJnlmbT6Xyz-s8JgwkPqU0i2s8mTqGFQ33Tfj0bVp_MMJZU" alt="fake_dai" width="100%"/>


Please note that the addresses are determined randomly, so your addresses should look different to mine.

Now that you have the addresses for the conditional tokens contract and your ERC20 currency contract, you will be able to deploy your prediction market contract!

To do this, go back to the LsLMSR contract and then input the addresses into the textfield before you deploy. It should look something like this:


<img src="https://lh5.googleusercontent.com/Mx_YDwOfAmGjwJ5chsY-gWS6TPgWrQ1MtSO1rs24ta1gu1osEYPuIg2l0O3Vu_OKpvLATxuucevd4Yj0bLtnpU3tyIejSOqLLuIuM-7aXdx0mOriDPUfk_JyAuZoaefBpEaY3KnQ" alt="lsLMSR" width="100%"/>


Once you submit this transaction, you will have successfully deployed the necessary contracts! You can now move onto the next page to interact with the contracts.
