---
nav_order: 20003
title: SIP.03
---

# SIP.3: Songbird Decentralization and Code Upgrade

| Type               | Songbird Improvement Proposal               |
| :----------------- | :------------------------------------------ |
| Author             | Flare Foundation                            |
| Created            | 4-Jul-2024                                  |
| Document Status    | In progress                                 |
| Majority Condition | 50% (required) xx.x % (obtained)            |
| Voting Outcome     | [**Accepted**][ProposalLink] on dd-Jul-2024 |

<!--Created? This file or publication of the proposal?
Status: In progress? Change to Final when done.
Majority Condition: Need %age after the vote.
Voting Outcome: Need outcome, link, and date.-->

[ProposalLink]: 
<!--Add link-->

## 1. Brief Description

The Songbird network is currently centralized, because all of its validators are run by Flare, and its validator code is lagging behind the Flare and Avalanche networks.
This proposal updates the Songbird validator code, effectively forking the network. The goal of the fork is to:

* Use the same code on Songbird as Flare to make upgrades to more recent Avalanche versions easier.
* Decentralize the network by allowing anybody to become a validator and to stake on any validator.

## 2. Technical Description

### 2.1 Align Songbird and Flare Code

Currently on the Songbird network, node IDs of validators run by Flare are hard-coded in the validator code.
The transfer of funds from the C-chain (where smart contracts are held) to the P-chain (where rewards are managed) is also disabled.

### 2.2 Who Can Be Validators

As a result, only validators run by Flare can secure the Songbird network.
Moreover, without the ability to transfer funds from the C-chain to the P-chain, staking on validators is not possible.

If this proposal is accepted, the Songbird network will be forked.
The transfer of funds from the C-chain to the P-Chain will be allowed and staking parameters will be set to the values specified below. P-chain rewards are set to 0 and rewarding will be set to 0.

### 2.3 Staking on Songbird Validators

Proposed staking parameters are:

|        | Coston      | Songbird    |
| :----- | :---------- | :---------- |
| T0     | 23-Jul-2024 | 20-Aug-2024 |
| T1     | 30-Jul-2024 | 03-Sep-2024 |

### 3. Link to Code Repository

Here is the source code repository: [Go-Flare](https://github.com/flare-foundation/go-flare/tree/songbird-support).

<!--Are there other relevant repos?-->

### 4. Proposed Implementation Date Range

Both Coston (Songbird's testing network) and the Songbird network itself will need to be forked.
Changes to the code include two important timestamps:

* Enable transfer of funds from C-chain to P-chain at time **T0**. All nodes must be upgraded up to T0.
* Enable staking from a proposed time, which should be T0 + approximately 14 days.

Proposed fork times for T0 and T1 are:

|        | Coston      | Songbird    |
| :----- | :---------- | :---------- |
| T0     | 23-Jul-2024 | 20-Aug-2024 |
| T1     | 30-Jul-2024 | 03-Sep-2024 |

### 5. Voting Details

To pass, the proposal requires a simple majority of votes in favor of it.

### 6. Deadline for Voting

* **Notice period:** 09-Jul-2024 to 15-Jul-2024
* **Voting period:** 16-Jul-2024 to 22-Jul-2024

<!--i surmised these dates, giving 7 days to each period, leaving 2 more days to review the proposal.-->
