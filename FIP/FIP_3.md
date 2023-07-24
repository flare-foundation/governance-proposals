---
nav_order: 10003
title: FIP.03
---

# FIP.03: Add a secondary band to FTSO reward calculation

| Type               | Flare Improvement Proposal                  |
| :----------------- | :------------------------------------------ |
| Author             | Flare Foundation                            |
| Created            | 13-June-2023                                |
| Status             | Final                                       |
| Majority Condition | 50% (required) 91.76% (obtained)            |
| Voting Outcome     | [**Accepted**][ProposalLink] on 30-Jun-2023 |
| Timeline           | 10-Jul-2023 Deployment started on Coston2.<br>12-Jul-2023 Deployment started on Flare<br>20-Jul-2023 Deployment complete. |

[ProposalLink]: https://portal.flare.network/proposal/view/102346860651209103299352271877494040880208493959912687607164289066303939599680?chainId=14

## 1. Brief Description

The Flare Time Series Oracle (FTSO) is a smart contract running on the Flare network that provides continuous estimations for different types of data. It does so in a decentralized manner (no single party is in control of the process) and securely (it takes a lot of effort to disrupt the process).

To ensure decentralization and security, a set of independent data providers retrieves data from external sources, like centralized and decentralized exchanges, and supplies the data to the FTSO system. This information is then weighted according to each provider's voting power, and the median is calculated to produce the final estimate.

The current FTSO system rewards data submissions that are close to the median price, resulting in extremely tight reward bands where only about 25% of the submissions are typically rewarded. The low number of providers that have access to the rewards put the rest at risk of not being able to cover their infrastructure costs, which in turn poses a centralization risk to the network.

**This proposal adds a second, wider reward band aiming at increasing the number of data providers that get rewarded, while still giving a higher allocation to submissions closer to the vote-power-weighted median value.**

A [version of this proposal for Songbird](../STP/STP_2.md) was implemented in March 2023. Since then, the Flare Analytics Team conducted [a study to evaluate its impact on the system](https://flare.network/wp-content/uploads/Flare-Analytics-Report-02-Impact-of-the-secondary-FTSO-reward-band.pdf). As explained in the report, results show that the secondary reward band significantly bolsters decentralization by increasing the percentage of rewarded data providers without negatively impacting the quality of the data.

This proposal successfully illustrates the central purpose of the Songbird network, where the experiment to test the effects of another reward band was run before the band was added on the Flare network.

## 2. Technical Description

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

### 2.1 Secondary Reward Band

For each asset, a secondary reward band is defined as a percentage _d_ around the median price.
This percentage is initially fixed for each asset and is defined in [Annex A](#annex-a) of this proposal.

Any data submitted within the secondary band will be rewarded, proportionally to its vote power.

This new band is not meant to be a reward for every submitter, though: Even though the secondary band is wider and therefore rewards more participants than the primary band, submissions still need to be close enough to the median to be included.

Both reward bands are independent, so if a submission falls within both bands, it receives both rewards.

### 2.2 Reward Splitting between Primary and Secondary Bands

To avoid sudden changes that affect the stability of the system, the secondary reward band will initially receive 30% of all FTSO rewards, whereas the primary reward band will receive the remaining 70%.

In choosing these percentages, the Flare Foundation considered the current server maintenance costs so that data providers in the secondary reward band can cover their infrastructure costs.

Depending on the evolution of the system, these values might be revised later through the submission of another proposal.

## 3. Link to Code Repository

In the source code the primary and secondary bands are called the _IQR_ (for Inter-Quartile) and the _elastic_ bands respectively, since these were the working names used during development.

A number of smart contracts, including the [`FtsoManager`](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/master/contracts/ftso/implementation/FtsoManager.sol) and all [`FTSO`](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/master/contracts/ftso/implementation/Ftso.sol) contracts will be redeployed.
The new code can be found in [this repository branch](https://gitlab.com/flarenetwork/flare-smart-contracts/-/tree/master/contracts).

Calculations regarding the secondary reward band can be found in [the FTSO contract, starting on line 1038](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/master/contracts/ftso/implementation/Ftso.sol#L1038).

Once these contracts have been redeployed, all other contracts containing their addresses will be updated too.

The [`PriceFinalized`](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/master/contracts/userInterfaces/IFtso.sol#L25) event in the `IFtso` interface has also been updated to include information regarding the secondary band.

Then, the following governance calls will happen:

* [`FTSOManager.setGovernanceParameters()`](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/master/contracts/ftso/implementation/FtsoManager.sol#L391) will be called to set the `_elasticBandRewardBIPS` parameter to 3000, meaning that the secondary reward band will receive 30% of the total FTSO rewards, and the primary band will receive the remaining 70%.

* [`FTSOManager.setElasticBandWidthPPMFtsos()`](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/master/contracts/ftso/implementation/FtsoManager.sol#L444) will be called to set the band width for each asset to the **Proposed _d_** values specified in [Annex A](#annex-a) of this proposal.

## 4. Proposed Implementation Date Range

At the start of the next reward epoch after the voting ends the new contracts will be deployed and tested on [the Coston2 network](https://docs.flare.network/dev/reference/network-configs/).

Then the proposal will be implemented on the Flare network.

## 5. Voting Details

To pass, the proposal requires a simple majority of votes in favor of it.

## 6. Deadline for Voting

One week after the proposal is published in [the Flare Portal](https://portal.flare.network/).

## 7. Annex A

| Asset  | Primary _d_ | Primary Recipients % | Proposed Secondary _d_ | Estimated Secondary Recipients % |
| :----- | ----------: | :------------------- | ---------------------: | :------------------------------- |
| `XRP`  |     0.0023% | 22.4%                |                 0.005% | 57.1%                            |
| `LTC`  |     0.0019% | 26.1%                |                 0.005% | 53.2%                            |
| `XLM`  |     0.0065% | 26.7%                |                 0.015% | 71.9%                            |
| `DOGE` |     0.0047% | 32.4%                |                 0.005% | 59.4%                            |
| `ADA`  |     0.0028% | 22.6%                |                 0.010% | 68.1%                            |
| `ALGO` |     0.0062% | 24.7%                |                 0.010% | 52.5%                            |
| `BCH`  |     0.0033% | 27.4%                |                 0.010% | 60.0%                            |
| `DGB`  |     0.0347% | 35.4%                |                 0.050% | 66.0%                            |
| `BTC`  |     0.0009% | 24.0%                |                 0.005% | 64.6%                            |
| `ETH`  |     0.0009% | 24.2%                |                 0.005% | 64.9%                            |
| `FIL`  |     0.0024% | 26.7%                |                 0.010% | 66.6%                            |
| `FLR`  |     0.0241% | 28.2%                |                 0.040% | 73.0%                            |

* **Primary _d_**: Average width of the primary band resulting from [the mechanism currently in use](https://docs.flare.network/tech/ftso/).
* **Primary Recipients %**: Average percentage of data providers which currently receive rewards.
* **Proposed Secondary _d_**: Proposed secondary band width.
* **Estimated Secondary Recipients %**: Average percentage of data providers (based on the past data) that are expected to receive rewards from the secondary band.
    The **Proposed Secondary _d_** values have been chosen so that this column is close to 50%.

Note that the **Proposed _d_** band width is higher than the **Primary _d_**, meaning that the secondary reward band is wider than the primary.
