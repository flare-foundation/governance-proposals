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

<!--Update Status to Final, Majority Condition obtained, Voting Outcome, and date.-->

[ProposalLink]:
<!--Add link-->

## 1. Brief Description

The Songbird network is currently centralized, because all of its validators are run by Flare.
In addition, Flare leverages the Avalanche network's consensus mechanism and Songbird's validator code is lagging behind both the Flare and Avalanche networks.
This proposal updates the Songbird validator code, effectively forking the Songbird network.
The goal of the fork is to:

* Decentralize the network by allowing anybody to become a validator and to stake on any validator.
* Use the same code on Songbird as Flare to make upgrades to more recent Avalanche versions easier.

## 2. Technical Description

### 2.1 Who Can Be Songbird Validators

Currently, node IDs of validators run by Flare are hard-coded into the Songbird validator code, so that only validators run by Flare can secure the Songbird network.

If this proposal is accepted, the hard-coded node IDs will be removed, and the underlying Proof-of-Stake consensus will be enabled, so that nodes will need to be staked to validate.

### 2.2 Enable Staking on Songbird Validators

If this proposal is accepted, staking parameters will be set to the equivalent of those on the Flare network.

|                         | Proposed for Coston   | Proposed for Songbird | Currently on Flare    |
| :---------------------- | :-------------------- | :-------------------- | :-------------------- |
| Min self-bond           | 100,000 `$CFLR`       | 1,000,000 `$SGB`      | 1,000,000 `$FLR`      |
| Max total stake         | 1,000,000,000 `$CFLR` | 200,000,000 `$SGB`    | 200,000,000 `$SGB`    |
| Min delegation          | 10,000 `$CFLR`        | 50,000 `$SGB`         | 50,000 `$SGB`         |
| Min validator duration  | 24 hours              | 60 days               | 60 days               |
| Max validator duration  | 365 days              | 365 days              |                       |
| Min delegation duration | 1 hour                | 14 days               | 14 days               |
| Delegation factor       | 15                    | 15                    | 15                    |

<!--Question: [FIP.05](https://proposals.flare.network/FIP/FIP_5.html) does not give a maximum validator duration. Do we have one for Flare? What is it? Is it also 365 days? I [asked Marko](https://flarenetworks.slack.com/archives/C02NURDPAQZ/p1720166986299229) July 5.-->

### 2.3 Align Songbird and Flare Code

Currently on the Songbird network, the transfer of funds from the C-chain (where smart contracts are held) to the P-chain (where rewards are managed) is disabled.

If this proposal is accepted, the transfer of funds from the C-chain to the P-Chain will be allowed and staking on validators will be possible, resulting in a fork.

### 2.4 Reset Rewards

To accommodate these changes and the resulting fork, Coston and Songbird P-chain rewards will be set to 0 immediately and rewarding will initially be done manually via grants.
Shortly thereafter, the same rewarding mechanism as on Flare C-chain will be implemented on the Coston and Songbird C-chains.

<!--Is this correct? I presume only Coston and Songbird P-chain rewards are set to 0. Or do the Flare rewards start from scratch too (in such a case, this brings into question why people would lose rewards, so I'm thinking not on Flare)?-->

### 3. Link to Code Repository

Here is the source code repository: [Go-Flare](https://github.com/flare-foundation/go-flare/tree/songbird-support).

The audit report for these changes is available at: [FYEO Secure Code Assessment of Flare Songbird](https://x.com/goFYEO/status/1792599813743161479).

<!--Add this to our Security Audit page? (It's not there yet. Check with Luka.)-->

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

<!--Check with Marko and update dates above and below when we publish: Monday EOD to publish?-->

### 5. Voting Details

To pass, this proposal requires a simple majority of votes in favor of it.

### 6. Deadline for Voting

* **Notice period:** 09-Jul-2024 to 15-Jul-2024
* **Voting period:** 16-Jul-2024 to 22-Jul-2024

<!--Update the repo index page. When do I do that? Right after we publish?-->
