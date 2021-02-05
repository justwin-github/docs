---
title: "Automated Market Makers"
description: "A brief summary on automated marketmakers."
date: 2020-10-06T08:48:57+00:00
lastmod: 2020-10-06T08:48:57+00:00
draft: false
images: []
menu:
  docs:
    parent: "overview"
weight: 2
toc: true
---
One of the important trends of 2020 was the rise of [automated market makers](https://medium.com/dragonfly-research/what-explains-the-rise-of-amms-7d008af1c399). These automated market makers have been successful in functioning as decentralised exchanges and have attracted billions of dollars in trading volume.

The appeal of these automated market makers is that they are computationally more simple than decentralised orderbook exchanges (such as Etherdelta) and thus allow for more gas efficient trading. Importantly, they are permissionless, allowing anyone to list asset pairs or trade.

Most popular decentralised exchanges use [constant product market makers](https://medium.com/bollinger-investment-group/constant-function-market-makers-defis-zero-to-one-innovation-968f77022159) to provide price quotes for assets. These have their drawbacks, such as impermanent loss, but have overall found a strong product market fit. Many decentralised prediction markets, such as [Omen](http://omen.eth.link/) and [Polymarket](https://polymarket.com/), have adopted the same constant product market maker approach.

However, the issue with constant product market makers in prediction markets is that since the markets tend to resolve to 0 or 1 in a sudden manner, liquidity providers are often stuck with significant losses. This means that liquidity providers have little incentive to provide liquidity to these prediction market pools, leading to illiquid markets that are not comparable to centralised order book based prediction markets.

In current implementations, liquidity providers are effectively paying for users to divulge information about their beliefs on the outcome of a particular event. This provides an effective means for market makers to crowdsource information about a particular topic such as guessing the weight of Galton's ox. However, this does not incentivise trading volume. If liquidity providers are rewarded only with a percentage of trading fees, then the majority of liquidity providers will lose money from their initial capital.

In order for prediction markets to be successful, we need to promote liquid markets by ensuring that LPs are adequately rewarded for providing liquidity. We believe that the best way to do this is by designing AMMs that reward the creation of markets and minimise the risks associated with providing liquidity .Constant product automated market makers are just one type of automated market maker, and it is possible that different families of automated marketmaker serve the prediction market model better.