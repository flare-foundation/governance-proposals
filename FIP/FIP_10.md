---
nav_order: 10010
title: FIP.10
---

# FIP.10: Add an Incentive Structure for Participating in All Protocols

| Type               | Flare Improvement Proposal |
| :----------------- | :------------------------- |
| Author             | Flare Foundation           |
| Created            | 18-Sep-2024                |
| Document Status    | Draft                      |
| Majority Condition | 50% (required to reject)   |

## 1. Brief Description

To achieve Flare’s mission as a layer 1 enshrined-oracle network that offers total security for all protocols, this proposal intends to implement a new incentive structure across the entire Flare network.

The new incentive structure aims to:

* Encourage providers to participate in all Flare protocols.
* Improve the accuracy and security of the Flare Time Series Oracle (FTSO) feeds across scaling (anchor) and fast updates (block latency) sub-protocols.
* Motivate providers to behave honestly.

Because this incentive structure will impose behavior and infrastructure requirements on providers and because noncompliance might impact delegators and stakers in the Flare community, the community must vote on whether to implement it.

The [incentive structure](#21-new-incentive-structure), [minimum participate requirements](#22-minimum-participation-requirements-for-existing-protocols), and an [example that explains how rewards will work](#23-example-rewards-in-the-proposed-incentive-structure) are described in the next section.

## 2. Technical Description

Although currently providers can choose which protocols to participate in, all Flare protocols were intentionally designed with the expectation that all providers will participate in all of them. 
Each provider’s weight defines their relative voting power in each protocol, and it is assumed that the full weight of providers will be used to vote in each protocol.

Consider several examples that illustrate problematic outcomes of partial participation on Flare:

* The FTSO block-latency feeds sample random providers to provide updates, and if some of the selected providers fail to respond, the feeds may lag behind the intended values.
* In every instance where an FTSO data provider was chilled on Flare mainnet, the provider in question was not participating in staking. As a result, these providers had less to lose from dishonest behavior compared to those actively involved in staking.

The current incentive structure does not provide a sufficient deterrent, leaving FTSO data providers with the opportunity to misbehave with limited sanction. The new structure, described below, will encourage better behavior and bolster the stability and security of the entire Flare network.

### 2.1 New Incentive Structure

If this proposal is accepted, the following incentive structure will be implemented:

* Minimum participation requirements in a reward epoch will be defined for every existing Flare protocol.
* Providers will be required to meet the set of minimum requirements for participating in all existing Flare protocols.
* Providers who do not participate in all protocols and meet all the requirements will be penalized.
* In the future, new Flare protocols will be proposed with minimum participation requirements.

In the new incentive structure, providers will gain or lose passes, depending on whether they meet the minimum participation requirements for each protocol.

* All providers, both new and existing, start with zero passes.
* When a provider meets the minimum requirements in a reward epoch, they gain a pass. Providers can hold a maximum of 3 passes.
* When a provider fails to meet the minimum requirements in a reward epoch, they lose a pass.
* Providers can lose one pass per protocol per reward epoch.
* If a provider would lose a pass but doesn’t have any, they lose all rewards for the epoch. 
That is, a provider with 1 pass who fails to meet 1 minimal requirement will have 0 passes but will still achieve rewards, and a provider with 0 passes who fails to achieve a minimal requirement loses all rewards.


### 2.2 Minimum Participation Requirements for Existing Protocols

Each Flare protocol will implement the minimum participation requirements, defined across each reward epoch independently. 
For existing protocols, the requirements are defined as follows:

* **FTSO anchor prices**: Providers must submit a value estimate that lies within a 0.5% band around the consensus median value in 80% of voting rounds within a reward epoch.
* **FTSO block-latency prices**: Providers must submit at least 80% of their expected number of updates within a reward epoch, unless they have very low weight, defined as < 0.2% of the total active weight.
* **Staking**: Providers must meet 80% total uptime in the reward epoch with at least 1M FLR in active self-bond. 
However, in order to earn passes, the provider must have at least 3M FLR in active self-bond and 15M in active stake. 
Providers with 80% total uptime and at least 1M FLR in active self-bond but not meeting both the 3M FLR active self-bond and 15M active stake requirements neither earn nor lose passes, and still receive eligible rewards.


### 2.3 Example: Rewards in the Proposed Incentive Structure

The following example shows how the proposed incentive structure will affect a hypothetical provider’s rewards.

* Each time the provider does not meet the minimal conditions for all protocols in a given reward epoch, they lose a pass. 
If they did not have any remaining passes, their rewards for that round are burned.
* Each round in which the provider successfully meets all conditions for each protocol, a pass is reimbursed. 
In this way, only providers who are consistently not participating in Flare’s protocols are penalized.

As you examine the table, be sure to scroll to the right to see all the data.

| Epoch | Rewards Earned | Staking Criteria Met | FTSO Criteria Met | Passes Remaining | Epoch Rewards | Total Rewards (New) | Total Rewards (Old) |
|-------|----------------|----------------------|-------------------|------------------|---------------|---------------------|---------------------|
| 100   | 1000           | &#x2713;             | &#x2713;          | 3                | 1000          | 1000                | 1000                |
| 101   | 750            | &#x2715;             | &#x2713;          | 2                | 750           | 1750                | 1750                |
| 102   | 500            | &#x2715;             | &#x2713;          | 1                | 500           | 2250                | 2250                |
| 103   | 300            | &#x2713;             | &#x2715;          | 0                | 300           | 2550                | 2550                |
| 104   | 400            | &#x2715;             | &#x2713;          | 0                | 0             | 2550                | 2950                |
| 105   | 950            | &#x2713;             | &#x2713;          | 1                | 950           | 3500                | 3900                |
| 106   | 1100           | &#x2713;             | &#x2713;          | 2                | 1100          | 4600                | 5000                |
| 107   | 700            | &#x2715;             | &#x2715;          | 0                | 700           | 5300                | 5700                |
| 108   | 1000           | &#x2713;             | &#x2713;          | 1                | 1000          | 6300                | 6700                |
| 109   | 900            | &#x2713;             | &#x2713;          | 2                | 900           | 7200                | 7600                |
| 110   | 600            | &#x2715;             | &#x2713;          | 1                | 600           | 7800                | 8400                |
| 111   | 200            | &#x2715;             | &#x2715;          | 0                | 0             | 7800                | 8600                |

This table shows how passes and rewards are determined in an epoch relative to a provider’s participation:

<table>
  <tr>
    <th colspan="3" scope="colgroup">Epoch Behavior</th>
    <th colspan="3" scope="colgroup">Result</th>
  </tr>
  <tr>
    <th scope="col">Minimal Staking Participation (1M)</th>
    <th scope="col">Full Staking Participation (5M/15M)</th>
    <th scope="col">FTSO Participation</th>
    <th scope="col">Has Passes?</th>
    <th scope="col">Receives Rewards?</th>
    <th scope="col">Loses Pass?</th>
    <th scope="col">Recovers Pass? (Max is 3)</th>
  </tr>
  <tr>
    <td>&#x2713;</td>
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
    <td>&#x2713;</td>
    <td>&#x2715;</td>
    <td>&#x2713;</td>
    <td>&#x2715;</td>
    <td>&#x2713;</td>
  </tr>
  <tr>
    <td>&#x2713;</td>
    <td>&#x2713;</td>
    <td>&#x2715;</td>
    <td>&#x2713;</td>
    <td>&#x2713;</td>
    <td>1</td>
    <td>&#x2715;</td>
  </tr>
  <tr>
    <td>&#x2713;</td>
    <td>&#x2713;</td>
    <td>&#x2715;</td>
    <td>&#x2715;</td>
    <td>&#x2715;</td>
    <td>&#x2715;</td>
    <td>&#x2715;</td>
  </tr>
  <tr>
    <td>&#x2713;</td>
    <td>&#x2715;</td>
    <td>&#x2713;</td>
    <td>&#x2713;</td>
    <td>&#x2713;</td>
    <td>&#x2715;</td>
    <td>&#x2715;</td>
  </tr>
  <tr>
    <td>&#x2713;</td>
    <td>&#x2715;</td>
    <td>&#x2713;</td>
    <td>&#x2715;</td>
    <td>&#x2713;</td>
    <td>&#x2715;</td>
    <td>&#x2715;</td>
  </tr>
  <tr>
    <td>&#x2715;</td>
    <td>&#x2715;</td>
    <td>&#x2713;</td>
    <td>&#x2713;</td>
    <td>&#x2713;</td>
    <td>1</td>
    <td>&#x2715;</td>
  </tr>
  <tr>
    <td>&#x2715;</td>
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
    <td>&#x2715;</td>
    <td>&#x2713;</td>
    <td>&#x2713; (if 2 strikes)</td>
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
    <td>&#x2715;</td>
  </tr>
</table>

## 3. Proposed Implementation Date Range

NEED

## 4. Voting Details

To pass, the proposal requires a simple majority of votes in favor of it.

Before voting, providers should consider the following factors:

* The possibility of additional infrastructure and maintenance costs associated with meeting the minimum requirements not only for existing protocols but also for new protocols in the future.
* Building a provider system is a competitive venture, and poor performance and lack of participation in one protocol may cause rewards from other protocols to be withheld even though no explicitly malicious behavior occurred.

## 5. Deadline for Voting

* **Notice period**: 14-October-2024 to nn-xxx-nnnn
* **Voting period**: NEED nn-xxx-nnnn to nn-xxx-nnnn
