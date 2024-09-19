---
nav_order: 10010
title: FIP.10
---

# FIP.10: Strengthen Flare Networks with Participation Requirements

| Type               | Flare Improvement Proposal |
| :----------------- | :------------------------- |
| Author             | Flare Foundation           |
| Created            | 18-Sep-2024                |
| Document Status    | Draft                      |
| Majority Condition | 50% (required to reject)   |

## 1. Brief Description

Currently, Flare data providers can choose to participate in a single protocol within the Flare network instead of participating in all of protocols.
As a result, the incentive structure for each protocol must be considered separately because the providers might participate only in the protocols they believe are sufficiently profitable relative to their unique interests and particular infrastructure costs.
However, broadly speaking, the protocols are intentionally designed with the expectation that providers will participate in all of them. Each provider's weight defines their relative voting power in each protocol, and it is assumed that approximately the full weight of providers will be used to vote in each protocol.
For example, the block-latency feeds sample random providers to provide updates, and if all providers don't respond when they are selected, the feeds will likely lag behind the intended values.

This proposal intends to create a global incentive structure across the Flare network to strengthen it to encourage providers to participate in all Flare protocols in order to:

* Improve the accuracy and security of Flare’s oracles (e.g., the FTSOv2 anchor and block-latency feeds)
* Encourage correct behavior of providersThis benefits Flare’s community
* Bolster the stability of the network

According to the incentive structure, providers are required to actively participate in all protocols in each reward epoch, or they will receive a strike.
If too many strikes in a short period of time are received, rewards for certain epochs will be burnt.
Implementing this structure will extend Flare’s mission as an L1 enshrined-oracle network that offers total security for all protocols.
However, because new behavior and infrastructure requirements will be inmposed on data providers, and because the choice to meet these requirements will impact delegators and stakers in the wider Flare community, this proposal asks the entire Flare community to vote on whether the incentive structure will be implemented.
If this proposal is accepted, data providers will be required to meet a set of minimum requirements for participating in all Flare protocols.
The definition of minimal participation for each existing protocol and an explanation of how strikes are calculated and accrued and their duration are explained in the next section.

## 2. Technical Description

### Example

The following is an example of how a hypothetical provider’s rewards will change in the new system: each time they do not succeed the minimal conditions for all protocols in a given reward epoch they receive a strike; if they do not have any remaining strikes, their rewards for that round are burnt. Each round in which a provider successfully meets all conditions for each protocol, a strike is reimbursed. In this way, only providers who are consistently not participating in Flare’s protocols are punished.

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

## 3. Proposed Implementation Date Range

The Committee would be funded immediately after the passage of the governance proposal with the first month’s emissions allocation process beginning July 6th 2024.

## 5. Voting Details

To pass, the proposal requires a simple majority of votes in favor of it.

## 6. Deadline for Voting

* **Notice period**: 25-June-2024 to 27-June-2024
* **Voting period**: 28-June-2024 to 4-July-2024