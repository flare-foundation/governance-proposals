---
nav_order: 20003
title: SIP.03
---

# SIP.03: Songbird Decentralization and Code Upgrade

| Type               | Songbird Improvement Proposal               |
| :----------------- | :------------------------------------------ |
| Author             | Flare Foundation                            |
| Created            | 4-Jul-2024                                  |
| Revision           | 2                                           |
| Document Status    | Draft                                       |
| Majority Condition | 50% (required)                              |

## 1. Brief Description

This proposal opens up Songbird validation to allow anyone to become a validator and bring the Songbird validator code up to speed with Flare's.

These updates to the validator code, necessarily fork the network with the goal of:

* Decentralizing the network by allowing anybody to become a validator and anyone to stake on a validator.
* Using the same code on Songbird as Flare to make future upgrades easier.

## 2. Technical Description

### 2.1 Who Can Be Songbird Validators

Currently, node IDs of validators run by Flare are hard-coded into the Songbird validator code, so that only validators run by Flare can secure the Songbird network.

If this proposal is accepted, the hard-coded node IDs will be removed, and the underlying proof-of-stake consensus will be enabled, so that nodes will need to be staked to validate.

As a first step, Songbird validators will not be required to run an FTSO data provider as Flare validators are. This restriction will be introduced at a later stage.

### 2.2 Enable Staking on Songbird Validators

If this proposal is accepted, staking parameters will be set to the equivalent of those on the Flare network.

|                         | Proposed for Coston   | Proposed for Songbird | Currently on Flare |
| :---------------------- | :-------------------- | :-------------------- | :----------------- |
| Min self-bond           | 100,000 `$CFLR`       | 1,000,000 `$SGB`      | 1,000,000 `$FLR`   |
| Max total stake         | 1,000,000,000 `$CFLR` | 200,000,000 `$SGB`    | 200,000,000 `$FLR` |
| Min delegation          | 10,000 `$CFLR`        | 50,000 `$SGB`         | 50,000 `$FLR`      |
| Min validator duration  | 24 hours              | 60 days               | 60 days            |
| Max validator duration  | 365 days              | 365 days              | 365 days           |
| Min delegation duration | 1 hour                | 14 days               | 14 days            |
| Delegation factor       | 15                    | 15                    | 15                 |

### 2.3 Align Songbird and Flare Code

Currently, the transfer of funds from the C-chain (where smart contracts are held) to the P-chain (where rewards are managed) is disabled.

If this proposal is accepted, the transfer of funds from the C-chain to the P-Chain will be allowed and staking on validators will be possible.

### 2.4 Rewarding

P-chain rewards are set to 0 and rewarding will initially be done manually via grants.
Shortly thereafter, the same rewarding mechanism as on the Flare C-chain will be implemented on the Songbird C-chain.

### 3. Link to Code Repository

Here is the source code repository: [Go-Flare (songbird-support branch)](https://github.com/flare-foundation/go-flare/tree/songbird-support).

The audit report for these changes is available at: [FYEO Secure Code Assessment of Flare Songbird](https://x.com/goFYEO/status/1792599813743161479).

### 4. Proposed Implementation Date Range

The proposed code changes will cause a fork of both the Coston and Songbird networks.
Because all nodes are currently managed by Flare and all will be updated, the fork should not produce any conflicts.
Changes to the code include two important timestamps:

* Enable transfer of funds from the C-chain to the P-chain at time **T0**. All nodes will be upgraded up to T0.
* Enable staking at time **T1**, which will be a few days after T0.

Proposed fork times for T0 and T1 are:

|        | Coston      | Songbird    |
| :----- | :---------- | :---------- |
| T0     | 23-Jul-2024 | 20-Aug-2024 |
| T1     | 30-Jul-2024 | 03-Sep-2024 |

### 5. Voting Details

To pass, this proposal requires a simple majority of votes in favor of it.

### 6. Deadline for Voting

* **Notice period:** 09-Jul-2024 to 13-Jul-2024
* **Voting period:** 16-Jul-2024 to 22-Jul-2024

### 7. Revision History

|  Revision | Changes                                                                                       |
| --------: | :-------------------------------------------------------------------------------------------- |
| [1][rev1] | Initial document.                                                                             |
|    [2](#) | This revision clarifies that the requirement for Songbird validators to run an FTSO data provider will not be introduced until a later stage.        |

[rev1]: https://github.com/flare-foundation/governance-proposals/blob/86fbc0/STP/STP_03.md
