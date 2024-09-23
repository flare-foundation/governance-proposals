---
nav_order: 10010
title: FIP.10
---

# FIP.10: Add Protocol Participation Requirements

| Type               | Flare Improvement Proposal |
| :----------------- | :------------------------- |
| Author             | Flare Foundation           |
| Created            | 18-Sep-2024                |
| Document Status    | Draft                      |
| Majority Condition | 50% (required to reject)   |

## 1. Brief Description

Currently, providers in the Flare network can choose to participate in a single Flare protocol instead of all of them.
As a result, the incentive structure for each protocol must be considered separately because providers might participate only in the protocol they believe is sufficiently profitable relative to their unique interests and particular infrastructure costs.
However, broadly speaking, the Flare protocols are intentionally designed with the expectation that providers will participate in every protocol in the network.
Each provider's weight defines their relative voting power in each protocol, and it is assumed that approximately the full weight of providers will be used to vote in each protocol.
For example, the block-latency feeds sample random providers to provide updates, and if all providers don't respond when they are selected, the feeds will likely lag behind the intended values.

This proposal intends to create an incentive structure across the entire Flare network to encourage providers to participate in all Flare protocols.
The incentive structure will:

* Improve the accuracy and security of the Flare Time Series Oracle feeds, scaling (anchor) and fast-updates (block latency)
* Encourage good behavior from providers
* Extend Flare’s mission as an L1 enshrined-oracle network that offers total security for all protocols.

If this proposal is accepted:

* Minimum participating requirements in a reward epoch will be defined for every existing Flare protocol.
* New Flare protocols will be proposed with minimum participation requirements.
* Providers will be required to meet the set of minimum requirements for participating in all existing and new Flare protocols.

## 2. Technical Description

Each Flare protocol will implement the following requirements:

* **FTSO anchor prices**: 80% of median submissions must lie within a 0.5% band around the consensus median value.
* **FTSO block-latency prices**: Providers must submit at least 80% of their expected number of updates within a reward epoch, unless they have very low weight, such as <0.2%.
* **Staking**: Providers must meet 80% total uptime in the reward epoch with at least 3M FLR in active self-bond and 15M active stake.
* **Stake mirroring**: At least an 80% mirroring merkle root vote-submission rate and at least 80% participation in mirroring when providers are selected. Stake mirroring is currently not rewarded.

According to the incentive structure, if providers do not meet the minimal participation requirements for every protocol during each reward epoch, they will receive a penalty in the form of a strike.
If too many strikes in a short period of time are received, rewards for certain epochs will be burned.

Each time a provider fails to achieve the minimum participation criteria in a reward epoch, they lose a strike. If they have no strikes, they instead lose all rewards in that reward epoch for all protocols. Because providers participate in multiple protocols, providers may lose more than one strike in a round.
Each reward epoch for which they meet all participation criteria, they gain a strike back, up to a maximum of three. Providers start with zero strikes, including new providers.

### Example

The following example shows how a hypothetical provider’s rewards will change in the new system. Each time the provider does not meet the minimal conditions for all protocols in a given reward epoch, they receive a strike. If they do not have any remaining strikes, their rewards for that round are burned. Each round in which the provider successfully meets all conditions for each protocol, a strike is reimbursed. In this way, only providers who are consistently not participating in Flare’s protocols are punished.

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

* **Notice period**: 25-June-2024 to 27-June-2024 NEED
* **Voting period**: 28-June-2024 to 4-July-2024 NEED
