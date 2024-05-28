---
nav_order: 10008
title: FIP.08
---

# FIP.08: Update FTSO Data Feeds and Define Process to Add New Ones

| Type               | Flare Improvement Proposal |
| :----------------- | :------------------------- |
| Author             | Flare Foundation           |
| Created            | 28-May-2024                |
| Document Status    | Draft                      |
| Majority Condition | 50% (required to reject)   |

## 1. Brief Description

The community approval of [FIP.06](./FIP_6.md) significantly increased the number of data feeds that [Flare's FTSO system](https://docs.flare.network/tech/ftso) can support through the introduction of the FTSO Scaling protocol.
This proposal starts making use of this increased capacity.

The purpose of this proposal is twofold:

1. Add 19 more cryptocurrency prices to the current list of 19 data feeds.
2. Define a process for the future inclusion of more data feeds, including automatic monitoring and community requests.

The following sections describe in more detail the proposed changes.

## 2. Technical Description

### 2.1 Addition of New Data Feeds

All data feeds are price pairs against the `USD` currency.
The current 18 feeds are `ADA`, `ALGO`, `ARB`, `AVAX`, `BNB`, `BTC`, `DOGE`, `ETH`, `FIL`, `FLR`, `LTC`, `MATIC`, `SOL`, `USDC`, `USDT`, `XDC`, `XLM`, and `XRP`.

This proposal adds the following 19 new feeds: `TRX`, `LINK`, `ATOM`, `DOT`, `TON`, `ICP`, `SHIB`, `DAI`, `BCH`, `NEAR`, `LEO`, `UNI`, `ETC`, `WIF`, `BONK`, `JUP`, `ETHFI`, `ENA`, and `PYTH`.

For transparency's sake, these are the reasons that these feeds were chosen:

* Shortlisted by surveying responses from the FTSO community: `TRX`, `LINK`, `ATOM`, and `DOT`.
* Currently ranked in the top 25 of [CoinMarketCap](https://coinmarketcap.com): `TON`, `ICP`, `SHIB`, `DAI`, `BCH`, `NEAR`, `LEO`, `UNI`, and `ETC`.
* Launched on [Binance](https://binance.com) in the past 3 months, with strong narrative and market capitalization: `WIF`, `BONK`, `JUP`, `ETHFI`, `ENA`, and `PYTH`.

### 2.2 Process to Add More Data Feeds

Two methods are proposed to streamline the inclusion of new data feeds in the future.
These methods will work in parallel with governance proposals initiated by the Flare Foundation, such as [FIP.04](./FIP_4.md).

#### 2.2.1 Automatic Inclusion

A cryptocurrency showing exceptional market performance can trigger its automatic inclusion in the FTSO system using the following process:

1. The Flare Foundation will identify any token that reaches a 24h trading volume of $10 million during each of the first 3 days after its launch, as reported by [CoinMarketCap](https://coinmarketcap.com), on any of the 5 major exchanges: [Binance](https://binance.com), [Coinbase](https://coinbase.com), [Bybit](https://bybit.com), [OKX](https://okx.com), and [UPbit](https://upbit.com).
2. The Flare Foundation will announce the identified tokens on Twitter and the dedicated FTSO groups, in order for the data providers to prepare.
3. A week later, the identified tokens will be added to the FTSO system, without a governance vote.

#### 2.2.2 Through the FTSO Management Group

Cryptocurrencies not added automatically by the previous method, or non-cryptocurrency data feeds, which might incur additional costs for data providers, can still be included by submitting a vote to the FTSO Management Group created in [FIP.02](./FIP_2.md).

The process is as follows:

1. A request to include a data feed must be made through one of these channels:
    1. A tweet with the hashtag `#NewpairsonFlare` that gathers at least 100 likes.
    2. A message on [Flare's Discord server](https://discord.com/invite/flarenetwork), in the `Discuss-Ideas` developer channel, explaining the business justification for the request.
    3. A web form (URL to be announced), explaining the business justification for the request.
2. The Flare Foundation will forward the request to the [FTSO Management Group's forum](./FIP_2.md#3-link-to-code-repository) and request a vote.
3. If the vote passes, the requested data feed will be added to the FTSO system.

### 2.3 Secondary Reward Band

[FIP.03](./FIP_3.md) introduced a secondary reward band to benefit more FTSO data providers. The width of this band is chosen for each price pair to reward a fixed percentage of all providers.

However, immediately after a new price pair is added, enough information about it is not available to calculate its secondary band width. As a result, the secondary reward bands are initially disabled for all newly-added data feeds.

After a few weeks, when a few reward epochs have passed and enough submissions have been analyzed, secondary reward bands for the new feeds will be enabled.

Similarly, currently existing data feeds might need to have their secondary reward bands recalculated to make sure that enough providers are covered after [FIP.06](./FIP_6.md) enabled the FTSO Scalability protocol.

### 2.4 Relation to FTSO Fast Updates

Proposal [FIP.07](./FIP_7.md), currently being voted on, introduces the FTSO Fast Updates protocol, which provides a separate stream of FTSO updates.
If that proposal is accepted, any new data feeds added directly by FIP.08, or by the mechanisms defined in FIP.08, will also be available through the FTSO Fast Updates protocol.

Given that the initialization of new fast update streams requires a starting value provided by the FTSO Scaling protocol, a delay of a few reward epochs is to be expected before new feeds are available through the Fast Updates protocol.

## 3. Links to Code Repositories and Contracts

There is no new code involved. Only governance calls to modify the network's configuration.

## 4. Proposed Implementation Date Range

Implementation is expected to start shortly after the voting finishes. The exact date will be announced in time for FTSO data providers to prepare.

## 5. Voting Details

To pass, the proposal requires a simple majority of votes in favor of it.

## 6. Deadline for Voting

* **Notice period**: 3-June-2024 to 9-June-2024
* **Voting period**: 12-June-2024 to 18-June-2024
