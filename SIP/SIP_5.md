---
nav_order: 20005
title: SIP.05
---

# SIP.05: Optimize Rewarding Structure and Adjust Protocol Parameters 

| Type               | Flare Improvement Proposal                  |
| :----------------- | :------------------------------------------ |
| Author             | Flare Foundation                            |
| Created            | 12-Nov-2024                                 |
| Document Status    | Draft                                       |
| Majority Condition | 50% (required)                              |



## 1. Brief Description


This proposal intends to gradually implement certain changes in the rewarding logic of Flare’s enshrined protocols as a reaction to the network’s changing dynamics, which will bring improvements to the current rewarding system.
The goal of these changes is to:

* Ensure participation in the network protocols is sustainable for all honestly behaving data providers.
* Establish a fairer reward structure that compensates a broader set of data providers.
* Align protocol functionality with network developments and changing market conditions.

The proposal will also allow for future adjustments in the parameters outlined below, in line with the network’s growth and evolving needs. The Flare Foundation will directly communicate these future parameter changes to the data providers in a timely manner, before they are submitted for a vote within the [Management Group](https://proposals.flare.network/STP/STP_3.html) of data providers.

The [proposed rewarding changes](#21-proposed-changes) for the core protocols, as well as [future potential parameter adjustments](#22-other-potential-changes) are described in the next section.


## 2. Technical Description

The Flare Foundation developed and maintains scripts for calculating rewards for each reward epoch.
These scripts are publicly available and data providers are encouraged to run them to calculate rewards for each reward epoch.
The rewarding logic contains certain configuration parameters or conditions that require monitoring and may, at times, need to be adjusted.
The purpose of these changes is to fine-tune Flare’s incentive structure, supporting fair rewarding and a generally robust system.

The relevant rewarding changes are described below, split into two subsections: [2.1](#21-proposed-changes) describes changes that will be implemented soon after the proposal passes, while changes that may be implemented later via the Management Group are described in [2.2](#22-other-potential-changes).


### 2.1 Proposed Changes

The [STP.02](https://proposals.flare.network/STP/STP_2.html) governance proposal introduced secondary (PCT) reward bands to the already existing primary (IQR) reward bands, with the aim of increasing the number of data providers that get rewarded.
The size of the PCT bands is currently fixed for each feed, being determined based on the historical accuracy of the individual price submissions from data providers.

However, as market conditions change, certain reward bands can become either too tight or too loose.
Thus, it is crucial to adapt the PCT bands to the underlying market behavior.
Additionally, a better methodology of calculating these bands, such as based on the underlying asset volatility, may be more appropriate than just examining price submissions. 
Thus, the change proposed is:

* **PCT bands**: Introduce an adaptive method for setting the width of the secondary bands, based on volatility and market conditions.

Another feature of the PCT bands is how the inflationary rewards are distributed between the primary and secondary bands in the FTSO Scaling protocol (Anchor Feeds).
The current IQR/PCT ratio is set to 70/30, as per [STP.02](https://proposals.flare.network/STP/STP_2.html).
However, data suggests that this ratio overemphasizes precise proximity to the median value, potentially overlooking valuable, slightly off-median submissions.
This ultimately leads to disproportionate benefits for certain data providers.
As such, we propose:

* **Anchor feeds rewards**: Increase reward allocation for the PCT band to an initial IQR/PCT ratio of 40/60, with further increases to the PCT band allocation planned, pending additional Management Group voting as data from this initial adjustment is evaluated.

With appropriately adjusted PCT bands, this change will not influence the accuracy of the FTSO protocol, while contributing to the decentralization of the system. 


### 2.2 Other Potential Changes

In addition to the changes introduced above, a series of potential changes will be voted directly by the data providers within the [Management Group](https://proposals.flare.network/STP/STP_3.html).
Further details about this [voting process](#3-voting-methodology-for-future-parameter-optimizations) are described in the next section.


**Block-latency feeds rewards**: Accuracy rewards for the FTSO’s block-latency feeds are triggered for a voting epoch if the price at the end of the epoch is within a 0.5% interval around the respective anchor price.
When the system behaves as intended and the two feeds are regularly in good alignment, the width of the interval may need to be changed.
Tightening the interval in this way should provide better incentives for more accurate price guarantees.

**Reward split**: Currently, the FTSO inflationary rewards are split between the anchor and block-latency feeds in proportion 70/30.
This ratio may be revised in the future based on community usage, with a target ratio of 50/50.

**Weight systems**: Various weight systems are used in the core protocols, based on both delegations and stake.
To promote decentralization, lowering the overdelegation cap may be desirable in the future.
An initial 2% cap may be set only after the new IQR/PCT ratio is implemented, to support a smooth transition.
Additionally, using a unified weight system across all protocols, such as the signing policy weight, could lead to a more robust system.

**Block-latency feeds parameters**: The behavior of the block-latency feeds is strongly dependent on the (expected) number of updates per block, as well as the size of each update.
While volatility incentives allow for temporary changes in these parameters, new default values may be desirable depending on underlying changes in block creation times or market behavior.
Additionally, the fee parameters for the volatility incentives may need to be changed accordingly.

**Scaling reward split**: Currently 80% of the rewards are allocated to submission accuracy, 10% to timely signature deposition, and 10% to finalization.
To provide fairer allocation of rewards, these percentages may be changed in the future to values in the ranges: 70-80%, 10-15%, 10-15% respectively.

**Unearned rewards**: Certain protocols within the Flare network are assigned an amount of inflationary rewards per voting epoch, but it is possible (or even likely) that not all the assigned rewards will be earned by the providers.
Currently, these rewards are burnt: however, in the future, it may be beneficial to the network to redirect these pools of unassigned rewards to create grants for data providers to help support their infrastructure requirements.

**Penalization**: When computing the size of penalties for reveal offenses and double signing within FTSO Scaling, a factor of 30 times the expected proportional reward per voting round is currently used.
In order to ensure that this factor correctly disincentivizes undesirable provider behavior, the factor may be changed in the future to values in the interval from 20 to 50.

**Edge cases**: Included in the rewarding code are mechanisms for handling unlikely or unusual edge cases, which typically arise in less than 0.5% of voting rounds.
As more data from the network comes in, such as information on the frequency of such cases, Flare would like to be able to fine-tune the way these are handled by the rewarding logic.
These changes will be transparently laid out to data providers, as they alone are responsible for validating and signing the rewards.

**Flare Systems Protocol**: The current durations of the grace periods for signature deposition and finalization after the end of reveal period are 10s and 20s respectively.
Due to latency issues arising from an increasingly busy network, it may be beneficial to increase these values.
To this end, the grace periods might be increased by up to 10 seconds, to 20 and 30 seconds respectively.


**Minimal participation requirements**: The [SIP.04](https://proposals.flare.network/SIP/SIP_04.html) proposal introduced minimal participation requirements for all core protocols.
The exact requirements may be revised in the future as more data on provider participation is accrued.


## 3. Voting Methodology for Future Parameter Optimizations

The Flare Foundation will directly communicate any future changes in the parameters previously discussed through the data provider [Management Group's forum](https://proposals.flare.network/STP/STP_3.html).
The potential changes will be voted on by the data providers in a rejection-based approach.
Namely, for the proposed changes to be rejected, both of the following conditions must be true:


* **Threshold condition**: A minimum quorum of at least 66% of all data providers must be reached.
* **Majority condition**: More than 50% of the votes cast must be against the proposal.

Thus, the changes will be implemented if the quorum threshold is not reached, or if less than half of the cast votes are against it.


## 4. Proposed Implementation Date Range

If this proposal passes, implementation of the new parameters will start soon after voting ends.
The changes will be implemented gradually, to support a smooth transition and continued reliability.
New changes will be communicated with the data providers and voted by the data provider [Management Group](https://proposals.flare.network/STP/STP_3.html).


## 5. Voting Details

To pass, the proposal requires a simple majority of votes in favor of it.


## 6. Deadline for Voting

* **Notice period**: 14-November-2024 to 17-November-2024
* **Voting period**: 18-November-2024 to 24-November-2024
