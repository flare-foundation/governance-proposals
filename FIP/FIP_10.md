---
nav_order: 10010
title: FIP.10
---

# FIP.10: Add Incentive Structure for Participating in All Protocols

| Type               | Flare Improvement Proposal |
| :----------------- | :------------------------- |
| Author             | Flare Foundation           |
| Created            | 18-Sep-2024                |
| Document Status    | Draft                      |
| Majority Condition | 50% (required to reject)   |

## 1. Brief Description

Currently, providers in the Flare network can choose to participate in a single Flare protocol instead of all of them.
As a result, the incentive structure for each protocol must be considered separately because providers might participate only in the protocol they believe is sufficiently profitable relative to their unique interests and particular infrastructure costs.

However, all Flare protocols were intentionally designed with the expectation that providers will participate in all of them.
Each provider's weight defines their relative voting power in each protocol, and it is assumed that the approximate full weight of providers will be used to vote in each protocol.
For example, the Flare Time Series Oracle (FTSO) fast-updates (block-latency) feeds sample random providers to provide updates, and if all providers don't respond when they are selected, the feeds will likely lag behind the intended values.

Additionally, the majority of providers are assumed to behave honestly in part because, if they behave dishonestly, the Flare network, and their stake in it, will lose value. For example, all FTSO data providers who have been chilled were also not staking any `$FLR`, which meant they had less to lose from their dishonest behavior. Because the current incentive structure does not include a deterrent, FTSO data providers have the opportunity to misbehave with only minimal risk.

This proposal intends to implement a new incentive structure across the entire Flare network to encourage providers to participate in all Flare protocols.
The incentive structure will improve the accuracy and security of the FTSO feeds, scaling (anchor) and block latency, encourage honest behavior from providers, and extend Flare’s mission as an L1 enshrined-oracle network that offers total security for all protocols.

If this proposal is accepted, the following incentive structure will be implemented:

* Minimum participating requirements in a reward epoch will be defined for every existing Flare protocol.
* In the future, new Flare protocols will be proposed with minimum participation requirements.
* Providers will be required to meet the set of minimum requirements for participating in all existing and new Flare protocols added to the network later.
* Providers who do not participate in all protocols and meet all the requirements will be disincentivized.

Because this structure will impose behavior and infrastructure requirements on providers and because noncompliance might negatively impact delegators and stakers in the Flare community, the community must vote whether to implement the structure.

Before voting, providers should consider the possibility of additional infrastructure and maintenance costs they might face in order to comply with the requirements not only for existing protocols but also for new protocols in the future. Additionally, providers should be aware that building a provider system is a competitive venture, and poor performance and lack of participation in one protocol might cause rewards from other protocols to be withheld even thought the provider was not behaving maliciously.

## 2. Technical Description

In the new incentive structure, providers will gain or lose _strikes_, depending on whether they meet the minimum participation requirements for each protocol.

* All providers, both new and existing, start with zero strikes.
* When a provider meets the minimum requirements in a reward epoch, they gain a strike. Providers can gain a maximun of 3 strikes.
* When a provider fails to meet the minimum requirements in a reward epoch, they lose a strike. and can lose more than one this way per round (up to one per protocol).
* If a provider would lose a strike but doesn’t have any, instead they lose all rewards for the epoch. That is, a provider with 1 strike who fails to meet 1 minimal requirement now has 0 strikes but still achieves rewards; a provider with 0 strikes who fails to achieve a minimal requirement loses all rewards.

Each time a provider fails to achieve the minimum participation criteria in a reward epoch, they lose a strike. If they have no strikes, they instead lose all rewards in that reward epoch for all protocols. Because providers participate in multiple protocols, providers may lose more than one strike in a round.
Each reward epoch for which they meet all participation criteria, they gain a strike back, up to a maximum of three. Providers start with zero strikes, including new providers.

### 2.1 Minimum Participation Requirements for Existing Protocols

Each Flare protocol will implement the following requirements:

* **FTSO anchor prices**: 80% of median submissions must lie within a 0.5% band around the consensus median value.
* **FTSO block-latency prices**: Providers must submit at least 80% of their expected number of updates within a reward epoch, unless they have very low weight, such as <0.2%.
* **Staking**: Providers must meet 80% total uptime in the reward epoch with at least 3M FLR in active self-bond and 15M active stake.
* **Stake mirroring**: At least an 80% mirroring merkle root vote-submission rate and at least 80% participation in mirroring when providers are selected. Stake mirroring is currently not rewarded.

### 2.3 Example: Rewards in the Proposed Incentive Structure

The following example shows how a hypothetical provider’s rewards will change in the proposed incentive structure. Each time the provider does not meet the minimal conditions for all protocols in a given reward epoch, they receive a strike. If they do not have any remaining strikes, their rewards for that round are burned. Each round in which the provider successfully meets all conditions for each protocol, a strike is reimbursed. In this way, only providers who are consistently not participating in Flare’s protocols are punished.

| Epoch | Rewards Earned | Staking Criteria Met | FTSO Criteria Met | Strikes Remaining | Epoch Rewards | Total Rewards (New) | Total Rewards (Old) |
|-------|----------------|----------------------|-------------------|-------------------|---------------|---------------------|---------------------|
| 100   | 1000           | &#x2713;             | &#x2713;          | 3                 | 1000          | 1000                | 1000                |
| 101   | 750            | &#x2715;             | &#x2713;          | 2                 | 750           | 1750                | 1750                |
| 102   | 500            | &#x2715;             | &#x2713;          | 1                 | 500           | 2250                | 2250                |
| 103   | 300            | &#x2713;             | &#x2715;          | 0                 | 300           | 2250                | 2250                |
| 104   | 400            | &#x2715;             | &#x2713;          | 0                 | 0             | 2550                | 2950                |
| 105   | 950            | &#x2713;             | &#x2713;          | 1                 | 950           | 3500                | 3900                |
| 106   | 1100           | &#x2713;             | &#x2713;          | 2                 | 1100          | 4600                | 5000                |
| 107   | 700            | &#x2715;             | &#x2715;          | 0                 | 700           | 5300                | 5700                |
| 108   | 1000           | &#x2713;             | &#x2713;          | 1                 | 1000          | 6300                | 6700                |
| 109   | 900            | &#x2713;             | &#x2713;          | 2                 | 900           | 7200                | 7600                |
| 110   | 600            | &#x2715;             | &#x2713;          | 1                 | 600           | 7800                | 8400                |
| 111   | 200            | &#x2715;             | &#x2715;          | 0                 | 0             | 7800                | 8600                |

This table shows how strikes and rewards are determined in an epoch relative to a provider's participation:

<table>
  <tr>
    <th colspan="3" scope="colgroup">Epoch Behavior</th>
    <th colspan="3" scope="colgroup">Result</th>
  </tr>
  <tr>
    <th scope="col">Staking Participation</th>
    <th scope="col">FTSO Participation</th>
    <th scope="col">Has Strikes?</th>
    <th scope="col">Receives Rewards?</th>
    <th scope="col">Loses Strike?</th>
    <th scope="col">Recovers Strike? (Max is 3)</th>
  </tr>
  <tr>
    <td>&#x2713;</td>
    <td>&#x2713;</td>
    <td>&#x2713;</td>
    <td>&#x2713;</td>
    <td>&#x2715;</td>
    <td>&#x2713;</td>
  </tr>
  <tr>
    <td>&#x2713;</td>
    <td>&#x2713;</td>
    <td>&#x2715;</td>
    <td>&#x2713;</td>
    <td>&#x2715;</td>
    <td>&#x2713;</td>
  </tr>
  <tr>
    <td>&#x2713;</td>
    <td>&#x2715;</td>
    <td>&#x2713;</td>
    <td>&#x2713;</td>
    <td>1</td>
    <td>&#x2715;</td>
  </tr>
  <tr>
    <td>&#x2713;</td>
    <td>&#x2715;</td>
    <td>&#x2715;</td>
    <td>&#x2715;</td>
    <td>&#x2715;</td>
    <td>&#x2715;</td>
  </tr>
  <tr>
    <td>&#x2715;</td>
    <td>&#x2713;</td>
    <td>&#x2713;</td>
    <td>&#x2713;</td>
    <td>1</td>
    <td>&#x2715;</td>
  </tr>
  <tr>
    <td>&#x2715;</td>
    <td>&#x2713;</td>
    <td>&#x2715;</td>
    <td>&#x2715;</td>
    <td>&#x2715;</td>
    <td>&#x2715;</td>
  </tr>
  <tr>
    <td>&#x2715;</td>
    <td>&#x2715;</td>
    <td>&#x2713;</td>
    <td>&#x2713; (if has 2)</td>
    <td>2</td>
    <td>&#x2715;</td>
  </tr>
  <tr>
    <td>&#x2715;</td>
    <td>&#x2715;</td>
    <td>&#x2715;</td>
    <td>&#x2715;</td>
    <td>&#x2715;</td>
    <td>&#x2715;</td>
  </tr>
</table>

## 3. Proposed Implementation Date Range

NEED

## 5. Voting Details

To pass, the proposal requires a simple majority of votes in favor of it.

## 6. Deadline for Voting

* **Notice period**: NEED nn-xxx-nnnn to nn-xxx-nnnn
* **Voting period**: NEED nn-xxx-nnnn to nn-xxx-nnnn
