---
nav_order: 30006
title: STP.06
---

# STP.06: Add Support for the Flare Systems Protocol and FTSO Scaling

| Type                | Songbird Test Proposal   |
| :------------------ | :----------------------- |
| Author              | Flare Foundation         |
| Created             | 14-Feb-2024              |
| Document Status     | Draft                    |
| Threshold Condition | 75% (required to reject) |
| Majority Condition  | 50% (required to reject) |

## 1. Brief Description

In this proposal, Flare will:

* [1. Add a new protocol, the Flare Systems Protocol (FSP)](#21-the-flare-systems-protocol-fsp), to which all other Flare protocols will connect
* [2. Introduce FTSO scaling](#22-ftso-scaling-updates), an improved version of the current FTSO protocol that will allow up to 1000 data feeds
* [3. Update the list of signers](#23-the-list-of-signers) that define the default set of attestation providers of the State Connector protocol

Because these updates require a hard fork of the network, a governance vote is necessary.

This proposal executes the above changes, which are described in more detail in the next sections.

## 2. Technical Description

### 2.1 The Flare Systems Protocol (FSP)

The FSP will be the core protocol that provides basic functionality to support future Flare protocols, and it will decentralize and automate many operations.
Most users will not need to interact with the FSP directly.
The FSP will provide, among other functions, the infrastructure for future Flare protocols, hereafter called enshrined data protocols, like the FTSO, with which users and dapps will interact.

Data providers for all enshrined data protocols, such as [FTSO](https://docs.flare.network/tech/ftso/) data providers and the [State Connector](https://docs.flare.network/tech/state-connector/)'s attestation providers, will benefit from a common procedure for submitting their data, regardless of the protocol for which they are submitting.

The FSP will provide:

* A system protocol for defining participants in each of the enshrined data protocols and their voting weights for each reward epoch.
* A system protocol for collecting each participant's contribution, aggregating them and submitting them on-chain.
* A system protocol for rewarding and claiming, per each reward epoch, that is unified across all future Flare enshrined data protocols that use the FSP.
* A mechanism for relaying enshrined data protocols’ results to other EVM blockchains.

Because the FSP is extensive, it will be implemented in parts over time by additional governance proposals.

#### 2.1.1 The `Submission` Contract

The `Submission` contract contains functions that enable eligible voters, which are the top 100 data providers with the most vote power, to make subsidized calls not only for the FTSO but for all enshrined data protocols.
Subsidization works through a refund mechanism.

The contract will allow already-submitted data to be resubmitted, but only the first transaction will be refunded and only the last transaction will be considered valid.

#### 2.1.2 The Validator Fork to Subsidize Transactions through the `Submission` Contract

The current FTSO system uses the subsidized contract `PriceSubmitter`, which guarantees that the fees for the transactions sent to it are refunded too, and allows one refund and one transaction per voting epoch.

To subsidize the new `Submission` contract too, the validator code must be updated, and the network must undergo a hard fork.

After this proposal is accepted, validator nodes will be required to upgrade to the latest release of the validator code.
After the new codebase is deployed, data providers can submit their data simultaneously to the new `Submission` contract and to the old `PriceSubmitter` contract for a while.

In this proposal, data providers that continue to submit through the `PriceSubmitter` contract are called **current data providers**, whereas data providers that submit through the new `Submission` contract are called **upgraded data providers**.

### 2.2 FTSO Scaling Updates

This section lists the most relevant changes that this update involves.
A more detailed description of the FTSO Scaling protocol is being prepared.

The FTSO Scaling protocol will operate in 4 phases, similar to how the current FTSO operates:

* **Commit**: Data providers commit hashes of their data.
* **Reveal**: Data providers submit the data to which they committed in the previous phase.
* **Sign**: Data providers calculate the resulting median price from the above submissions, and pack it along with the necessary information to prove it.
    They sign and submit this information.
* **Finalize**: When enough signatures have been received, a data provider triggers finalization on the FSP, making the results available.
  Any data provider at any time can submit the finalization transaction.

The length of the Commit phase will be 90 seconds, but the rest of phases could be shorter depending on how fast data providers perform submissions.
The length of a reward epoch will be 3.5 days.

This updated procedure offloads computation-intensive operations from the smart contracts to the data providers, allowing for greater scalability.

#### 2.2.1 Deployment Phases

Scaling the FTSO system to allow up to 1000 data feeds requires a series of substantial updates.
To provide time for the Flare Foundation to test them and for data providers to adapt to the changes, the update will consist of a trial phase, a beta phase, and a deprecation phase.

During these phases, current and upgraded data providers will coexist.
100% of the network's total inflation still goes to FTSO rewards, but it will be split among data providers in the following fashion:

* During the **trial phase**, reward allocation will not change:
current data providers will continue to receive all the FTSO rewards, whereas upgraded data providers will receive no rewards.

* During the **beta phase**, the Flare Foundation will update the `Inflation` contract so that current data providers will receive 50% of the of the total reward allocation for the FTSO, and upgraded data providers will receive the other 50%.
At this time, all data providers will be able to claim their rewards.

* During the **deprecation phase**, the Flare Foundation will update the `Inflation` contract again so that only upgraded data providers will receive rewards.

In summary:

| Phase             | Rewards for<br> current data providers | Rewards for<br> upgraded data providers |
| ----------------- | :------------------------------------: | :-------------------------------------: |
| Trial phase       |                  100%                  |                   0%                    |
| Beta phase        |                  50%                   |                   50%                   |
| Deprecation phase |                   0%                   |                  100%                   |

#### 2.2.2 Rewards Split

FTSO rewards for the upgraded data providers will be further divided according to the operations they perform.
Submitting data close to the median is still a requirement to get most of the rewards, as is the case with the current FTSO protocol.
Submitting signatures and triggering finalization receive additional rewards.

The following table shows how the total FTSO rewards are split among data providers:

| Reward Designation      | Rewards |
| ----------------------- | :-----: |
| Median closeness        |   80%   |
| Signature submission    |   10%   |
| Successful finalization |   10%   |

#### 2.2.3 Penalties

The protocol will penalize data providers in the following circumstances:

* **Withholding reveals**: A commit not followed by a reveal, or followed by an invalid reveal, impacts the system negatively and will be considered a reveal withholding.

* **Double-signing**: Providing invalid signatures or signatures copied from other providers or other data protocols, and sending signatures for different results in the same voting round will be considered double-signing.

In both cases, the penalty will be 30 times the offending data provider's expected relative share of rewards in that voting round, and it will be deducted from the total amount of rewards at the end of the reward epoch.
The maximum amount that can be deducted is equal to the data provider's total reward in the epoch.
The deducted amount will be burned.

#### 2.2.4 New Random Number Generation

This update to the FTSO system will introduce a new random number generation mechanism, based on the new submission procedure.
This mechanism is more stable, and will be immediately available to dapps.
However, the usage of the new random numbers by Flare protocols like vote-power block selection, governance, and other enshrined data protocols will be implemented by different proposals later.

### 2.3 The List of Signers

The State Connector currently relies on a default set of attestation providers managed by a set of governance addresses.
This proposal will update these addresses.

Note that updating the list of signers is unrelated to adding support for the FSP and FTSO scaling.
It is included in this proposal so that all updates that require a hard fork in the near term will be done simultaneously.
Because these updates will be conveniently packaged together, the network will undergo only one hard fork.

## 3. Links to Code Repositories and Contracts

* [Validator code changes](https://github.com/flare-foundation/go-songbird/tree/prioritised-submitter-contract)
* [New smart contracts](https://github.com/flare-foundation/flare-smart-contracts-v2)

## 4. Proposed Implementation Date Range

If the vote passes, the following actions will be taken:

| Change                  | Date              |
| :---------------------- | :---------------- |
| Hard fork Songbird      | March 15, 2024    |

## 5. Voting Details

An STP is rejection-based, meaning that it is accepted unless certain conditions are met, namely, that the quorum threshold (75%) is reached and at least half of the votes are against it. (See [Governance](https://docs.flare.network/tech/governance/#stps).)

## 6. Deadline for Voting

* **Notice period**: 20-Feb-2024 to 24-Feb-2024
* **Voting period**: 27-Feb-2024 to 5-Mar-2024
