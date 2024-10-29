---
nav_order: 20004
title: SIP.04
---

# SIP.04: Add an Incentive Structure for Participating in All Protocols

| Type               | Songbird Improvement Proposal               |
| :----------------- | :------------------------------------------ |
| Author             | Flare Foundation                            |
| Created            | 14-Oct-2024                                 |
| Document Status    | Final                                       |
| Majority Condition | 50% (required) 96.85% (obtained)            |
| Voting Outcome     | [**Accepted**][ProposalLink] on 27-Oct-2024 |

[ProposalLink]: https://portal.flare.network/proposal/view/89563537990950394808839286241744183822382857868418985395103648546243888827892?chainId=19

## 1. Brief Description

To achieve Flare’s mission as a layer 1 enshrined-oracle network where all protocols operate under the same trust assumptions as the network, this proposal, or more formally its counterpart [FIP.10](../FIP/FIP_10.md), intends to implement a new incentive structure across the Flare network. 
The purpose of this proposal is to implement the corresponding rewarding logic on Songbird, maintaining Songbird’s status as a canary network for Flare’s mainnet. 

The new incentive for Flare structure aims to:

* Encourage providers to participate in all Flare protocols.
* Improve the accuracy and security of the Flare Time Series Oracle (FTSO) feeds across scaling (anchor) and fast updates (block latency) sub-protocols.
* Motivate providers to behave honestly.

This SIP introduces the same incentive structure on Songbird; because this will impose behavior and infrastructure requirements on providers and because noncompliance might impact delegators and stakers in the Songbird community, the community must vote on whether to implement it.

As staking is not yet live on Songbird, the impact of this proposal is limited. However, it lays the groundwork for upcoming network deployments.

The [incentive structure](#21-new-incentive-structure), [minimum participation requirements](#22-minimum-participation-requirements-for-existing-protocols), and an [example that explains how rewards will work](#23-example-rewards-in-the-proposed-incentive-structure) are described in the next section.

## 2. Technical Description

Although providers can currently choose which protocols to participate in, all Flare protocols were intentionally designed with the expectation that all providers will participate in all of them. 
Each provider’s weight defines their relative voting power in each protocol, and it is assumed that the full weight of providers will be used to vote in each protocol.

Consider several examples that illustrate problematic outcomes of partial participation on Flare:

* The FTSO block-latency feeds sample random providers to provide updates, and if some of the selected providers fail to respond, the feeds may lag behind the intended values.
* In every instance where an FTSO data provider was chilled on Flare mainnet, the provider in question was not participating in staking. As a result, these providers had less to lose from dishonest behavior compared to those actively involved in staking.

The current incentive structure does not provide a sufficient deterrent, leaving FTSO data providers with the opportunity to misbehave with limited sanction. 
The new structure, described in the corresponding [FIP.10](../FIP/FIP_10.md), will encourage better behavior and bolster the stability and security of the entire Flare network. 
Below, the proposed changes to the rewarding logic on Flare are summarized, and the corresponding changes to Songbird are described in more detail.

### 2.1 New Incentive Structure

If the corresponding Flare proposal is accepted, the following incentive structure will be implemented on mainnet:

* Minimum participation requirements in a reward epoch will be defined for every existing Flare protocol.
* Providers will be required to meet the set of minimum requirements for participating in all existing Flare protocols.
* Providers who do not participate in all protocols and meet all the requirements will be penalized.
* In the future, new Flare protocols will be proposed with minimum participation requirements.

The SIP lifts these requirements around broad participation to Songbird. 
For pre-existing Songbird protocols, the same definitions of minimum participation on Flare and Songbird are used. 
However, for future protocols, explicit definitions of minimum participation may differ between the two networks in order to reflect differences between Flare and Songbird. 
In such cases, the respective proposals introducing the protocols onto Flare and Songbird will contain the two different definitions.

The new incentive structure introduces the concept of passes. 
Each provider will have a number of passes and can gain or lose passes, depending on whether they meet the minimum participation requirements for each protocol.

* All providers, both new and existing, start with zero passes.
* When a provider meets the minimum requirements in a reward epoch, they gain a pass. Providers can hold a maximum of 3 passes.
* When a provider fails to meet the minimum requirements in a reward epoch, they lose a pass.
* Providers can lose one pass per protocol per reward epoch.
* If a provider would lose a pass but doesn’t have any, they lose all rewards for the epoch.
 
That is, a provider with 1 pass who fails to meet 1 minimal requirement will have 0 passes but will still achieve rewards, and a provider with 0 passes who fails to achieve a minimal requirement loses all rewards.

### 2.2 Minimum Participation Requirements for Existing Protocols

Each Songbird protocol will implement the minimum participation requirements, defined across each reward epoch independently. 
For existing protocols, the requirements are defined as follows:

* **FTSO anchor feeds**: Providers must submit a value estimate that lies within a 0.5% band around the consensus median value in 80% of voting rounds within a reward epoch.
* **FTSO block-latency feeds**: Providers must submit at least 80% of their expected number of updates within a reward epoch, unless they have very low weight, defined as < 0.2% of the total active weight.

Note that the above requirements apply specifically to the FTSOv2 protocol, as the FTSOv1 protocol will soon be deprecated.

### 2.3 Example: Rewards in the Proposed Incentive Structure

The following example shows how the proposed incentive structure will affect a hypothetical provider’s rewards.

* Each time the provider does not meet the minimal conditions for a protocol in a given reward epoch, they lose a pass. 
If they did not have any remaining passes, their rewards for that round are burned.
* Each round in which the provider successfully meets all conditions for each protocol, a pass is reimbursed. 
In this way, only providers who are consistently not participating in Songbird’s protocols are penalized.


As you examine the table, be sure to scroll to the right to see all the data.

| Epoch | Rewards Earned | Anchor Feeds Requirements | Block-latency Feeds Requirements | Passes Remaining | Rewards Received | Total Rewards (New) | Total Rewards (Old) |
|-------|----------------|---------------------------|----------------------------------|------------------|------------------|---------------------|---------------------|
| 100   | 1000           | &#x2713;                  | &#x2713;                         | 3                | 1000             | 1000                | 1000                |
| 101   | 750            | &#x2715;                  | &#x2713;                         | 2                | 750              | 1750                | 1750                |
| 102   | 500            | &#x2715;                  | &#x2713;                         | 1                | 500              | 2250                | 2250                |
| 103   | 300            | &#x2713;                  | &#x2715;                         | 0                | 300              | 2550                | 2550                |
| 104   | 400            | &#x2715;                  | &#x2713;                         | 0                | 0                | 2550                | 2950                |
| 105   | 950            | &#x2713;                  | &#x2713;                         | 1                | 950              | 3500                | 3900                |
| 106   | 1100           | &#x2713;                  | &#x2713;                         | 2                | 1100             | 4600                | 5000                |
| 107   | 700            | &#x2715;                  | &#x2715;                         | 0                | 700              | 5300                | 5700                |
| 108   | 1000           | &#x2713;                  | &#x2713;                         | 1                | 1000             | 6300                | 6700                |
| 109   | 900            | &#x2713;                  | &#x2713;                         | 2                | 900              | 7200                | 7600                |
| 110   | 600            | &#x2715;                  | &#x2713;                         | 1                | 600              | 7800                | 8400                |
| 111   | 200            | &#x2715;                  | &#x2715;                         | 0                | 0                | 7800                | 8600                |

This table shows how passes and rewards are determined in an epoch relative to a provider’s participation:

<table>
  <tr>
    <th colspan="3" scope="colgroup">Epoch Behavior</th>
    <th colspan="3" scope="colgroup">Result</th>
  </tr>
  <tr>
    <th scope="col">Anchor Feeds Requirements</th>
    <th scope="col">Block-latency Feeds Requirements</th>
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
    <td>&#x2713; (if 2 passes)</td>
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

If this proposal passes, implementation of the new rewarding logic will start shortly after the voting ends.


## 4. Voting Details

To pass, the proposal requires a simple majority of votes in favor of it.

Before voting, providers should consider the following factors:

* The possibility of additional infrastructure and maintenance costs associated with meeting the minimum requirements not only for existing protocols but also for new protocols in the future.
* Building a provider system is a competitive venture, and poor performance and lack of participation in one protocol may cause rewards from other protocols to be withheld even though no explicitly malicious behavior occurred.

Additionally, providers should align their vote on this proposal with their vote on the corresponding [FIP.10](../FIP/FIP_10.md).


## 5. Deadline for Voting

* **Notice period**: 14-October-2024 to 20-October-2024
* **Voting period**: 21-October-2024 to 27-October-2024
