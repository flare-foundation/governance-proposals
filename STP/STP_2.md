---
nav_order: 30002
title: STP.02
---

# STP.02: Add a secondary band to FTSO reward calculation

| Type                | Songbird Test Proposal                      |
| :------------------ | :------------------------------------------ |
| Author              | Flare Foundation                            |
| Created             | 20-Jan-2022                                 |
| Status              | Final                                       |
| Threshold Condition | 75% (required to reject) 7.06% (obtained)   |
| Majority Condition  | 50% (required to reject) 5.22% (obtained)   |
| Voting Outcome      | [**Accepted**][ProposalLink] on 16-Feb-2023 |
| Timeline            | 21-Feb-2023 Deployment started on Coston.<br> 24-Feb-2023 Deployment started on Songbird.<br>3-Mar-2023 Deployment complete. |

[ProposalLink]: https://portal.flare.network/proposal/view/99877686589051291159439816214444308681891669617969610362053207962914539941917?chainId=19

## Brief Description

The Flare Time Series Oracle (FTSO) is a smart contract running on the Songbird network that provides continuous estimations for different types of data. It does so in a decentralized manner (no single party is in control of the process) and securely (it takes a lot of effort to disrupt the process).

To ensure decentralization and security, a set of independent data providers retrieves data from external sources, like centralized and decentralized exchanges, and supplies the data to the FTSO system. This information is then weighted according to each provider's voting power, and the median is calculated to produce the final estimate.

The current FTSO system rewards data submissions that are close to the median price, resulting in extremely tight reward bands where only about 25% of the submissions are typically rewarded. The low number of providers that have access to the rewards put the rest at risk of not being able to cover their infrastructure costs, which in turn poses a centralization risk to the network.

**This proposal adds a second, wider reward band aiming at increasing the number of data providers that get rewarded, while still giving a higher allocation to submissions closer to the vote-power-weighted median value.**

## Technical Description

The current FTSO system is described in the [Flare White Paper](https://flare.network/wp-content/uploads/Flare-White-Paper-v2.pdf), and a high-level summary can be found in the [FTSO technical documentation page](https://docs.flare.network/tech/ftso/).

The system contains a single reward mechanism, where 50% of submissions, weighted by their vote power and centered around the median price, are rewarded:

![price](../assets/ftso_reward_mechanism.png)

Some data providers have become so proficient at estimating this median that the reward band, i.e., the price range inside which submissions are rewarded is sometimes shrunk to ±0.005% of the median value.
In other words, in order to be rewarded, a data provider usually needs to predict the median value with a precision of four decimal places.

This situation typically results in rewards going to only 25% of the data providers that submit data in each epoch.
Data providers that are systematically left out struggle to keep their service cost-effective.
The FTSO community has [repeatedly flagged](https://twitter.com/ftso_au/status/1617352195137236994) that these conditions are very hard on them.

The FTSO system intends for data providers to be able to cover their infrastructure costs with their rewards in order to recruit as many data providers as possible and increase network decentralization.

Therefore, the Flare Foundation wants to improve the current situation by implementing a distribution of the FTSO rewards that covers more data providers, while still rewarding primarily those that submit values closest to the median.

This proposal maintains the current rewarding mechanism and renames it to _primary reward band_.
However, to achieve the aforementioned goals, a _secondary reward band_ will be added which is described next.

### Secondary Reward Band

For each asset, a secondary reward band is defined as a percentage _d_ around the median price.
This percentage is initially fixed for each asset and is defined in [Annex A](#annex-a) of this proposal.

Any data submitted within the secondary band will be rewarded, proportionally to its vote power.

This new band is not meant to be a reward for every submitter, though: Even though the secondary band is wider and therefore rewards more participants than the primary band, submissions still need to be close enough to the median to be included.

Both reward bands are independent, so if a submission falls within both bands, it receives both rewards.

### Reward Splitting between Primary and Secondary Bands

To avoid sudden changes that affect the stability of the system, the secondary reward band will initially receive 30% of all FTSO rewards, whereas the primary reward band will receive the remaining 70%.

In choosing these percentages, the Flare Foundation considered the current server maintenance costs so that data providers in the secondary reward band can cover their infrastructure costs.

Depending on the evolution of the system, these values might be revised later through the submission of another proposal.

## Link to Code Repository

In the source code the primary and secondary bands are called the _IQR_ (for Inter-Quartile) and the _elastic_ bands respectively, since these were the working names used during development.

A number of smart contracts, including the [`FTSOManager`](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/636-songbird-upgrade/contracts/ftso/implementation/FtsoManager.sol), the [`FtsoRegistry`](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/636-songbird-upgrade/contracts/utils/implementation/FtsoRegistry.sol), and all [`FTSO`](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/636-songbird-upgrade/contracts/ftso/implementation/Ftso.sol) contracts will be redeployed.
The new code can be found in [this repository branch](https://gitlab.com/flarenetwork/flare-smart-contracts/-/tree/636-songbird-upgrade).

Calculations regarding the secondary reward band can be found in [the FTSO contract, starting on line 1078](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/636-songbird-upgrade/contracts/ftso/implementation/Ftso.sol#L1078).

Once these contracts have been redeployed, all other contracts containing their addresses will be updated too.

The [`PriceFinalized`](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/636-songbird-upgrade/contracts/userInterfaces/IFtso.sol#L30) event in the `IFtso` interface has also been updated to include information regarding the secondary band.

Then, the following governance calls will happen:

* [`FTSOManager.setGovernanceParameters()`](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/636-songbird-upgrade/contracts/ftso/implementation/FtsoManager.sol#L360) will be called to set the `_elasticBandRewardBIPS` parameter to 3000, meaning that the secondary reward band will receive 30% of the total FTSO rewards, and the primary band will receive the remaining 70%.

* [`FTSOManager.setElasticBandWidthPPMFtsos()`](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/636-songbird-upgrade/contracts/ftso/implementation/FtsoManager.sol#L413) will be called to set the band width for each asset to the **Proposed _d_** values specified in [Annex A](#annex-a) of this proposal.

## Proposed Implementation Date Range

At the start of the next reward epoch after the voting ends the new contracts will be deployed and tested on [the Coston network](https://docs.flare.network/dev/reference/network-configs/).

Then the proposal will be implemented on the Songbird network.

## Voting Details

The proposal will be accepted unless it proves to be extremely unpopular with the community. It will therefore be rejected only if a majority (>50%) of the votes cast are against it, with the additional threshold condition that at least 75% of all possible votes are cast.

## Deadline for Voting

One week after the proposal is published in [the Flare Portal](https://portal.flare.network/).

## Annex A

| Asset  | Primary _d_ | Primary Recipients % | Proposed Secondary _d_ | Estimated Secondary Recipients % |
| :----- | ----------: | :------------------- | ---------------------: | :------------------------------- |
| `XRP`  |     0.0055% | 25.6%                |                 0.015% | 49.2%                            |
| `LTC`  |     0.0065% | 27.3%                |                 0.020% | 52.6%                            |
| `XLM`  |     0.0079% | 26.3%                |                 0.020% | 50.3%                            |
| `DOGE` |     0.0069% | 25.6%                |                 0.020% | 51.4%                            |
| `ADA`  |     0.0062% | 26.1%                |                 0.020% | 51.4%                            |
| `ALGO` |     0.0094% | 26.6%                |                 0.020% | 48.2%                            |
| `BCH`  |     0.0069% | 27.7%                |                 0.020% | 51.0%                            |
| `DGB`  |     0.0214% | 31.2%                |                 0.050% | 51.7%                            |
| `BTC`  |     0.0036% | 25.1%                |                 0.010% | 47.4%                            |
| `ETH`  |     0.0042% | 25.2%                |                 0.015% | 52.4%                            |
| `FIL`  |     0.0080% | 27.6%                |                 0.020% | 48.1%                            |
| `SGB`  |     0.0329% | 32.7%                |                 0.050% | 45.0%                            |

* **Primary _d_**: Average width of the primary band resulting from [the mechanism currently in use](https://docs.flare.network/tech/ftso/).
* **Primary Recipients %**: Average percentage of data providers which currently receive rewards.
* **Proposed Secondary _d_**: Proposed secondary band width.
* **Estimated Secondary Recipients %**: Average percentage of data providers (based on the past data) that are expected to receive rewards from the secondary band.
    The **Proposed Secondary _d_** values have been chosen so that this column is close to 50%.

Note that the **Proposed _d_** band width is higher than the **Primary _d_**, meaning that the secondary reward band is wider than the primary.
