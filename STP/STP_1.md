---
nav_order: 20001
---

# STP-1: Reduce FTSO vote power cap to 2.5%

| Type      | Songbird test proposal |
| :-------- | :--------------------- |
| Author    | Flare Foundation       |
| Created   | 16-Dec-2022            |
| Status    | Draft                  |
| Threshold | 75%                    |
| Majority  | 50%                    |

## Brief Description

The Flare Time Series Oracle (FTSO) system is used to obtain time series data (signals) from data providers in a decentralized manner.
A vote power cap is used to limit the impact of an individual data provider in the final decision for the median price thus helping to enforce decentralization.
Currently, the vote power cap is set to 10% of the total vote power, which in practice means that six or more of the top data providers can decide to collude with the goal of guaranteeing rewards for all of them or to control the data produced by the FTSO system.

As the number of reliable data providers has grown, we propose to lower the vote power cap to 2.5%.
This will provide further decentralization and a better safeguard against the mentioned threat.
Although it will impact the rewards earned by the seven data providers that currently have the most vote power, it will not affect their delegators, since there are a lot of other data providers with similar reward rates to choose from.
Moreover, the reward rates of most of the other data providers will increase, further contributing to the diversification of delegations among data providers.

## Technical Description

The FTSO system is a decentralized time series voting mechanism where every 3 minutes data providers cast their votes on price signals.
Currently, the Songbird network supports twelve price signals for various cryptocurrencies priced in USD.
Each 3-minute voting interval is called a *price epoch*.
A *data provider* is an entity behind a Songbird account that, in each price epoch, submits what it deems is the best price for the supported cryptocurrencies.
Signal voting is done through the FTSO smart contracts using a so-called commit and reveal scheme.
After the prices for a price epoch are revealed, each data provider's vote power is used to calculate the weighted median price and the rewards.

See more details on the [Flare Documentation site](https://docs.flare.network/tech/ftso/).

To explain the influence of the vote power cap, a few definitions are needed:

- The *total vote power* is the number of `$WSGB` (wrapped Songbird) tokens in circulation.
- The *balance vote power* of a Songbird account is the amount of `$WSGB` tokens held in that account.
- The *vote power* of an account is the sum of its balance vote power and all balance vote powers *delegated* to it by other accounts.
- The *active vote power* of an account is its vote power at a specific block, called the *vote power block*.
- The *active total vote power* is the total vote power (from all accounts) at the vote power block.
- A *reward epoch* is a 7-day period in which the active vote powers of data providers are fixed.
  When a new reward epoch is started, its vote power block is chosen randomly from the past epoch and it stays the same for the whole epoch.
- The *vote power cap* specifies the percentage of the active total vote power to which active vote powers of all data providers are capped.

Active vote powers are capped on two occasions:

1. When used as voting weights in calculating a signal's weighted median for a price epoch.
2. When used as shares in the reward distribution.

Hence, the impact of lowering the vote power cap is dual as well:

1. It lowers the maximum power a data provider can have in deciding the median price of a signal (from 10% to 2.5%).
2. It lowers the minimum share of the reward that a data provider, which reaches the vote power cap, can receive with a successful price prediction (from 10% to 2.5%).

### Example

To see the impact of the proposed change, consider the following situation:

- The network has a total vote power of 1000 WSGB.
- There is a group A of 7 data providers, each with an active vote power of 100 WSGB.
  This is 10% of the total, so they are all over the 2.5% cap but not over the 10% cap.
- There is a group B of 10 data providers, each with an active vote power of 20 WSGB.
  This is 2% of the total, so they are not over any cap.
- There is a group C of 10 data providers, each with an active vote power of 10 WSGB.
  This is 1% of the total, so they are not over any cap.

| Group | Group size | Vote power | VP capped to 10% | VP capped to 2.5% |
| ----- | ---------: | ---------: | ---------------: | ----------------: |
| A     |          7 |        100 |              100 |                25 |
| B     |         10 |         20 |               20 |                20 |
| C     |         10 |         10 |               10 |                10 |
| TOTAL |         27 |       1000 |             1000 |               475 |

With the 10% vote power cap, group A can control the median selection process with a 70% weight, whereas with the 2.5% vote cap the group can only exert a 37% weight in the process:

| Group | Group weight with 10% cap | Group weight with 2.5% cap |
| ----- | ------------------------: | -------------------------: |
| A     |                       70% |                        37% |
| B     |                       20% |                        42% |
| C     |                       10% |                        21% |

Regarding rewards, for the sake of simplicity let us assume that all data providers are entitled to a reward.
Each provider's share of the reward then equals its group weight divided by the group size:

| Group | Reward share with 10% cap | Reward share with 2.5% cap |
| ----- | ------------------------: | -------------------------: |
| A     |                       10% |                      5.27% |
| B     |                        2% |                      4.21% |
| C     |                        1% |                      2.10% |

## Link to Code Repository

Only a function call to `setGovernanceParameters` will be made on the `FtsoManager` contract ([Explorer link](https://songbird-explorer.flare.network/address/0xbfA12e4E1411B62EdA8B035d71735667422A6A9e)), where the first two parameters (`_maxVotePowerNatThresholdFraction` and `_maxVotePowerAssetThresholdFraction`) will be set to 40, meaning that the vote power cap will be set at 1/40th of the total vote power (i.e. 2.5%).

## Proposed Implementation Date Range

At the start of the next reward epoch after the voting ends.

## Voting Details

The proposal will be accepted unless it proves to be extremely unpopular with the community. It will therefore be rejected only if a majority (>50%) of the votes cast are against it, with the additional threshold condition that at least 75% of all possible votes are cast.

## Deadline for Voting

One week after the proposal is published.
