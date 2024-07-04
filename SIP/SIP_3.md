---
nav_order: 20003
title: SIP.03
---

# SIP.3: Songbird Decentralization and Code Upgrade

| Type               | Songbird Improvement Proposal               |
| :----------------- | :------------------------------------------ |
| Author             | Flare Foundation                            |
| Created            | 4-Jul-2024                                  |
| Document Status    | Draft                                 |
| Majority Condition | 50% (required) xx.x % (obtained)            |
| Voting Outcome     | [**Accepted**][ProposalLink] on dd-Jul-2024 |

<!--Question 1: How do we handle tracking info that we can't know until we all agree to publish it?
Created? This file or publication of the proposal (maybe 09 Jul, see below)?
Status: In progress? Change to Final when done.
Majority Condition: Need %age after the vote.
Voting Outcome: Need outcome, link, and date.-->

[ProposalLink]:
<!--Add link-->

## 1. Brief Description

The Songbird network is currently centralized, because all of its validators are run by Flare, and its validator code is lagging behind the Flare and Avalanche networks.
This proposal updates the Songbird validator code, effectively forking the Songbird network.
The goal of the fork is to:

* Use the same code on Songbird as Flare to make upgrades to more recent Avalanche versions easier.
* Decentralize the network by allowing anybody to become a validator and to stake on any validator.

<!--Question 2: What can I safely say to explain why we would want to remain current with Avalanche versions. Some possibilities from our doc include:
- "All Flare networks are a fork of the Avalanche project, which runs the Ethereum Virtual Machine."
- "Validators agree on the state of the ledger using a consensus algorithm that varies for each blockchain [Does "each blockchain" refer to Coston, Songbird, and Flare?]. For example, Flare uses the Snowman++ consensus protocol from Avalanche." [Does this update put them in sync?]-->

## 2. Technical Description

### 2.1 Who Can Be Validators

Currently, node IDs of validators run by Flare are hard-coded in the Songbird validator code, so that only validators run by Flare can secure the Songbird network.

If this proposal is accepted, the hard-coded node IDs will be removed, and the underlying proof-of-stake consensus will be enabled, so that nodes will need to be staked to validate.

### 2.2 Align Songbird and Flare Code

Currently on the Songbird network, the transfer of funds from the C-chain (where smart contracts are held) to the P-chain (where rewards are managed) is disabled.

If this proposal is accepted, the transfer of funds from the C-chain to the P-Chain will be allowed and staking on validators will be possible, resulting in a fork.

### 2.3 Staking on Songbird Validators

If this proposal is accepted, staking parameters will be set to the equivalent of those on the Flare network.
P-chain rewards will be set to 0 immediately and rewarding will initially be done manually via grants.
Later, the same mechanism as on Flare will be implemented on the C-chain.

|                         | Proposed for Coston   | Proposed for Songbird | Currently on Flare    |
| :---------------------- | :-------------------- | :-------------------- | :-------------------- |
| Min self-bond           | 100,000 `$CFLR`       | 1,000,000 `$SGB`      | 1,000,000 `$FLR`      |
| Max total stake         | 1,000,000,000 `$CFLR` | 200,000,000 `$SGB`    | 200,000,000 `$SGB`    |
| Min delegation          | 10,000 `$CFLR`        | 50,000 `$SGB`         | 50,000 `$SGB`         |
| Min validator duration  | 24 hours              | 60 days               | 60 days               |
| Max validator duration  | 365 days              | 365 days              |                       |
| Min delegation duration | 1 hour                | 14 days               | 14 days               |
| Delegation factor       | 15                    | 15                    | 15                    |

<!--Question 5: [FIP.05](https://proposals.flare.network/FIP/FIP_5.html) does not give a maximum validator duration. Do we have one for Flare? What is it? Ask Marko.-->

### 3. Link to Code Repository

Here is the source code repository: [Go-Flare](https://github.com/flare-foundation/go-flare/tree/songbird-support).

Audit report is available at: [FYEO Secure Code Assessment of Flare Songbird](https://x.com/goFYEO/status/1792599813743161479).

<!--Question 6: Should I add this to our Security Audit page?-->

### 4. Proposed Implementation Date Range

Both Coston (Songbird's testing network) and the Songbird network itself will need to be forked.
Changes to the code include two important timestamps:

* Enable transfer of funds from the C-chain to the P-chain at time **T0**. All nodes must be upgraded up to T0.
* Enable staking from a proposed time, which should be **T0 + approximately 14 days**.

Proposed fork times for T0 and T1 are:

|        | Coston      | Songbird    |
| :----- | :---------- | :---------- |
| T0     | 23-Jul-2024 | 20-Aug-2024 |
| T1     | 30-Jul-2024 | 03-Sep-2024 |

<!--Question 7: This is another part of question 1. I surmised the dates below from the proposed T0 and T1, giving 7 days to each period, which means we'd have to be ready by Monday EOD to publish.-->

### 5. Voting Details

To pass, this proposal requires a simple majority of votes in favor of it.

### 6. Deadline for Voting

* **Notice period:** 09-Jul-2024 to 15-Jul-2024
* **Voting period:** 16-Jul-2024 to 22-Jul-2024

<!--Question 8: Looks like I have to update the repo index page as well, yes? When do I do that? Right after we publish?-->
