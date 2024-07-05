---
nav_order: 20003
title: SIP.03
---

# SIP.03: Songbird Decentralization and Code Upgrade

| Type               | Songbird Improvement Proposal               |
| :----------------- | :------------------------------------------ |
| Author             | Flare Foundation                            |
| Created            | 4-Jul-2024                                  |
| Document Status    | Draft                                       |
| Majority Condition | 50% (required)                              |

## 1. Brief Description

The following proposal refers to the Songbird network, but should be understood to include both Songbird and its testing network, Coston.
The Songbird network is currently centralized, because all of its validators are run by Flare.
In addition, Flare leverages the Avalanche network's consensus mechanism and the validator code is lagging behind both the Flare and Avalanche networks.
This proposal updates the validator code, effectively forking the network.
The goal of the fork is to:

* Decentralize the network by allowing anybody to become a validator and anyone to stake on a validator.
* Use the same code on Songbird as Flare to make upgrades to more recent Avalanche versions easier.

## 2. Technical Description

### 2.1 Who Can Be Songbird Validators

Currently, node IDs of validators run by Flare are hard-coded into the Songbird validator code, so that only validators run by Flare can secure the Songbird network.

If this proposal is accepted, the hard-coded node IDs will be removed, and the underlying proof-of-stake consensus will be enabled, so that nodes will need to be staked to validate.

### 2.2 Enable Staking on Songbird Validators

If this proposal is accepted, staking parameters will be set to the equivalent of those on the Flare network.

|                         | Proposed for Coston   | Proposed for Songbird | Currently on Flare |
| :---------------------- | :-------------------- | :-------------------- | :----------------- |
| Min self-bond           | 100,000 `$CFLR`       | 1,000,000 `$SGB`      | 1,000,000 `$FLR`   |
| Max total stake         | 1,000,000,000 `$CFLR` | 200,000,000 `$SGB`    | 200,000,000 `$SGB` |
| Min delegation          | 10,000 `$CFLR`        | 50,000 `$SGB`         | 50,000 `$SGB`      |
| Min validator duration  | 24 hours              | 60 days               | 60 days            |
| Max validator duration  | 365 days              | 365 days              | 365 days           |
| Min delegation duration | 1 hour                | 14 days               | 14 days            |
| Delegation factor       | 15                    | 15                    | 15                 |

### 2.3 Align Songbird and Flare Code

Currently on the Songbird network, the transfer of funds from the C-chain (where smart contracts are held) to the P-chain (where rewards are managed) is disabled.

If this proposal is accepted, the transfer of funds from the C-chain to the P-Chain will be allowed and staking on validators will be possible, resulting in a fork.

### 2.4 Reset Rewards

To accommodate these changes and the resulting fork, P-chain rewards are set to 0 and rewarding will initially be done manually via grants.
Shortly thereafter, the same rewarding mechanism as on the Flare C-chain will be implemented on the Coston and Songbird C-chains.

### 3. Link to Code Repository

Here is the source code repository: [Go-Flare](https://github.com/flare-foundation/go-flare/tree/songbird-support).

The audit report for these changes is available at: [FYEO Secure Code Assessment of Flare Songbird](https://x.com/goFYEO/status/1792599813743161479).

### 4. Proposed Implementation Date Range

Both Coston and the Songbird networks will need to be forked.
Changes to the code include two important timestamps:

* Enable transfer of funds from the C-chain to the P-chain at time **T0**. All nodes must be upgraded up to T0.
* Enable staking from a proposed time, which should be **T0 + approximately 14 days**.

Proposed fork times for T0 and T1 are:

|        | Coston      | Songbird    |
| :----- | :---------- | :---------- |
| T0     | 23-Jul-2024 | 20-Aug-2024 |
| T1     | 30-Jul-2024 | 03-Sep-2024 |

### 5. Voting Details

To pass, this proposal requires a simple majority of votes in favor of it.

### 6. Deadline for Voting

* **Notice period:** 09-Jul-2024 to 15-Jul-2024
* **Voting period:** 16-Jul-2024 to 22-Jul-2024
