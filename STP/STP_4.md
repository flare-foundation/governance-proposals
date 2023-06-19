---
nav_order: 30004
title: STP.04
---

# STP.04: Update FTSO price pairs

| Type                | Songbird Test Proposal                      |
| :------------------ | :------------------------------------------ |
| Author              | Flare Foundation                            |
| Created             | 17-May-2023                                 |
| Status              | Final                                       |
| Threshold Condition | 75% (required) 2.39% (obtained)             |
| Majority Condition  | 50% (required) 0.80% (obtained)             |
| Voting Outcome      | [**Accepted**][ProposalLink] on 05-Jun-2023 |
| Timeline            | 7-Jun-2023 Deployment started on Coston.    |

[ProposalLink]: https://portal.flare.network/proposal/view/66778465481645690887041505836796293365286885579605118369844165722336569178460?chainId=19

## 1. Brief Description

Following requests from the Flare and Songbird communities, this proposal updates the price pairs currently provided by the [FTSO system](https://docs.flare.network/tech/ftso/).

All prices are against the `USD` currency.
The current 12 price pairs are `ADA`, `ALGO`, `BCH`, `BTC`, `DGB`, `DOGE`, `ETH`, `FIL`, `LTC`, `SGB`, `XLM`, and `XRP`.

The proposed changes are as follows:

* Remove 2 pairs: `BCH` and `DGB`.

* Add 8 pairs: `ARB`, `AVAX`, `BNB`, `MATIC`, `SOL`, `USDC`, `USDT`, and `XDC`.

After those pairs are added or removed, the list of supported price pairs is: `ADA`, `ALGO`, `ARB`, `AVAX`, `BNB`, `BTC`, `DOGE`, `ETH`, `FIL`, `LTC`, `MATIC`, `SGB`, `SOL`, `USDC`, `USDT`, `XDC`, `XLM`, and `XRP`.

Please note that, due to current network limitations, some pairs need to be removed to free the network bandwidth for the new ones.
The Flare Foundation is working to overcome these limitations.

## 2. Technical Description

* To handle each new price pair, 8 new clones of the `Ftso` contract will be deployed and configured.

* Method `RemoveFtso` on the `FtsoManager` contract will be called for each of the removed pairs.

* Method `addFtsosBulk` on the `FtsoManager` contract will be called to add the 8 new pairs.

Once these calls are made, the `FtsoRegistry` contract will reflect the new list of price pairs, and the FTSO data providers can start submitting them.

Note that the _indices_ associated with the removed pairs, as returned by `FtsoRegistry.getSupportedIndices` for example, will not be used anymore and will remain unused. This means that the rest of the existing pairs will keep their current indices and the added pairs will receive indices from 12 onwards.

The `FtsoAdded` event is fired from the `FtsoManager` every time a price pair is added or removed, and can be used by data providers to start submitting the new pairs at exactly the right block height. Failure to do so will result in reverted submissions.

[Instructions to retrieve the addresses of the above contracts](https://docs.flare.network/dev/getting-started/contract-addresses/).

### 2.1 Secondary Reward Band

[STP.02](./STP_2.md) introduced a secondary reward band to benefit more FTSO data providers. The width of this band is chosen for each price pair to reward a fixed percentage of all providers.

However, immediately after the new price pairs are added by this proposal, enough information about them will not be available to calculate their secondary band widths. As a result, initially the secondary reward bands for these pairs will be disabled.

In approximately 2 weeks, after a few reward epochs have passed and enough submissions have been analyzed, secondary reward bands for the new pairs will be enabled.

## 3. Link to Code Repository

There is no new code involved. Only governance calls to modify the network's configuration.

[Current configuration can be found here](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/songbird_network_deployed_code/deployment/chain-config/songbird.json#L60).

## 4. Proposed Implementation Date Range

Implementation is expected to start shortly after the voting finishes. The exact date will be announced in time for FTSO data providers to prepare.

## 5. Voting Details

The proposal will be accepted unless it proves to be extremely unpopular with the community. Therefore, it will be rejected only if a majority (>50%) of the votes cast are against it, with the additional threshold condition that at least 75% of all possible votes are cast.

## 6. Deadline for Voting

One week after the proposal is published in [the Flare Portal](https://portal.flare.network/).
