---
nav_order: 30007
title: STP.07
---

# STP.07: Add Support for FTSO Fast Updates

| Type                | Songbird Test Proposal   |
| :------------------ | :----------------------- |
| Author              | Flare Foundation         |
| Created             | 9-Apr-2024               |
| Document Status     | Draft                    |
| Threshold Condition | 75% (required to reject) |
| Majority Condition  | 50% (required to reject) |

## 1. Brief Description

[Flare's FTSO system](https://docs.flare.network/tech/ftso), including its [Scalability upgrade](./STP_6.md), gathers input from several data providers before producing an outcome, which results in safe values but with a low throughput.
This proposal introduces FTSO Fast Updates, a protocol that allows a smaller number of randomly-selected data providers to submit a separate stream of updates in each block, resulting in a much higher rate.
The values from this separate stream (stream values) do not affect the regular FTSO results (anchor values), which continue to be produced in the same way and at the same rate.

The downside of the increased throughput is diminished stability of the fast updates, because a smaller number of data providers, typically one, are participating in each update.
However, the following circumstances limit the extent of this drawback:

* The providers of each update are randomly selected in each block, making it very difficult to take advantage of a submission to manipulate prices.
* Subsequent updates are submitted by different providers and will likely correct any error introduced by previous submissions.
* Each time an anchor value is produced the fast updates reset to it, correcting any possible previous deviations.

This proposal rewards fast update submissions from the same FTSO reward pool as submissions of anchor values.
Since this is a change in the way the current FTSO system works, a governance vote is necessary.

The following sections describe in more detail the proposed changes.

## 2. Technical Description

The FTSO version 2 was introduced in a [whitepaper](https://flare.network/wp-content/uploads/FTSOv2.pdf), which describes both the FTSO Scalability protocol voted on [STP.06](./STP_6.md), and the FTSO Fast Updates protocol proposed in this STP.
This STP highlights the features of the Fast Updates protocol that affect the current FTSO system and therefore require a governance vote.
For the technical details, please refer to the [whitepaper](https://flare.network/wp-content/uploads/FTSOv2.pdf).

### 2.1 Changes to FTSO Rewarding

As of [STP.05](./STP_5.md), A portion of the Songbird network's yearly inflation is reserved for rewarding FTSO data providers.
[STP.06](./STP_6.md) further refined how these FTSO rewards are split among participants in the FTSO Scaling protocol.
STP.07 proposes to split the total FTSO rewards between participants in FTSO Scaling and FTSO Fast Updates.

This STP proposes that the total FTSO rewards are divided in the following manner:

* 70% to participants in the FTSO Scaling protocol, as described in [STP.06](./STP_6.md).
* 30% to participants in the FTSO Fast Updates protocol, as described in [the FTSO v2 whitepaper](https://flare.network/wp-content/uploads/FTSOv2.pdf).

Additionally, consumers of the FTSO data feeds can choose to offer additional rewards to increase the rate and magnitude of the fast updates, resulting in better volatility protection.
These additional rewards are pooled together and evenly distributed among providers, allowing more frequent updates to be submitted, and their value to be bigger so that they can better adjust to sudden value changes.

All rewards for the FTSO Fast Updates protocol are evenly distributed among all data feeds.

### 2.2 Deployment Strategy

Enabling the FTSO Fast Updates protocol does not require a hard fork of the network.
Instead, the new contracts listed in the next section will be deployed, and the inflation allocation updated as previously described.

During the 90 days following the deployment, the Flare Foundation may change some of the system's parameters to fine-tune it.

## 3. Links to Code Repositories and Contracts

* [`FastUpdater`](https://gitlab.com/flarenetwork/flare-smart-contracts-v2/-/blob/fast_updates/contracts/fastUpdates/implementation/FastUpdater.sol?ref_type=heads)

    Entry point for providers submitting fast updates.
    This contract will be registered with the Flare daemon, [the `Submission` contract](./FIP_6.md#211-the-submission-contract), and [the Flare Contract Registry](https://docs.flare.network/dev/getting-started/contract-addresses/#retrieval-from-blockchain).

* [`FastUpdateIncentiveManager`](https://gitlab.com/flarenetwork/flare-smart-contracts-v2/-/blob/fast_updates/contracts/fastUpdates/implementation/FastUpdateIncentiveManager.sol?ref_type=heads)

    Entry point for consumers of the FTSO data feeds willing to provide additional rewards to increase volatility protection.
    This contract will be registered with [the Flare Contract Registry](https://docs.flare.network/dev/getting-started/contract-addresses/#retrieval-from-blockchain).

* [`FastUpdatesConfiguration`](https://gitlab.com/flarenetwork/flare-smart-contracts-v2/-/blob/fast_updates/contracts/fastUpdates/implementation/FastUpdatesConfiguration.sol?ref_type=heads)

    System configuration, only accessible by governance, including, for example, the supported data feeds.

## 4. Proposed Implementation Date Range

If the vote passes, the following actions will be taken:

| Change                                | Date        |
| :------------------------------------ | :---------- |
| Deploy on the Songbird network        | 22-May-2024 |
| No more configuration changes allowed | 20-Aug-2024 |

## 5. Voting Details

An STP is rejection-based, meaning that it is accepted unless certain conditions are met, namely, that the quorum threshold (75%) is reached and at least half of the votes are against it. (See [Governance](https://docs.flare.network/tech/governance/#stps).)

## 6. Deadline for Voting

* **Notice period**: TBD
* **Voting period**: TBD
