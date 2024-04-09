---
nav_order: 10005
title: FIP.05
---

# FIP.05: Update Services, Limits, and Rewards Required for Staking

| Type               | Flare Improvement Proposal                                         |
| :----------------- | :----------------------------------------------------------------- |
| Author             | Flare Foundation                                                   |
| Created            | 16-Aug-2023                                                        |
| Revision           | 2                                                                  |
| Document Status    | Final                                                              |
| Majority Condition | 50% (required) 92.02% (obtained)                                   |
| Voting Outcome     | [**Accepted**][ProposalLink] on 8-Sep-2023                         |
| Timeline           | 22-Sep-2023 Deployed on Coston2.<br>25-Sep-2023 Deployed on Flare. |

[ProposalLink]: https://portal.flare.network/proposal/view/115140506966436016229428851010197993705384354938653006064424080138950501877188?chainId=14

## 1. Brief Description

The Flare Network is transitioning to a [proof of stake](https://docs.flare.network/tech/glossary#proof_of_stake) consensus mechanism as explained in the [Validator Nodes documentation page](https://docs.flare.network/tech/validators).
This transition requires:

* Implementing necessary infrastructure services.
* Modifying the current staking limits.
* Increasing the portion of inflation destined to validation rewards.

This proposal executes all the above changes, as detailed next.

## 2. Technical Description

### 2.1 New Infrastructure Service

In a manner similar to Avalanche, the Flare network is composed of [two independent blockchains](https://docs.avax.network/learn/avalanche/avalanche-platform): The platform chain (P-chain) and the contract chain (C-chain).
The P-chain manages platform-handling operations, like validator staking, whereas the C-chain implements the [EVM](https://docs.flare.network/tech/glossary#evm) and runs all the smart contracts.

Because validators stake on the P-chain and rewards are distributed through smart contracts on the C-chain, a _mirroring service_ will be developed.
This service will track, in a secure and decentralized manner, the amounts staked on the P-chain in a contract on the C-chain.
This service will enable validators to claim their validation rewards in the same way FTSO rewards are claimed, for example, and benefit from facilities like [automatic claiming](https://docs.flare.network/tech/automatic-claiming).

### 2.2 New Staking Limits

To simplify the onboarding of as many validators as possible, the following staking settings will be changed.

Note that some of these settings require changing the P-chain configuration and hence a network fork.

Some other settings affect only the mirroring service and are described here for the first time.
All limits enforced by the P-chain will also be enforced by the mirroring service.

| Setting                                                                                       | Current value | Proposed value |
| --------------------------------------------------------------------------------------------- | ------------: | -------------: |
| [Minimum self-bond amount](#minimum-self-bond-amount)                                         |    10M `$FLR` |      1M `$FLR` |
| [Minimum delegation amount](#minimum-delegation-amount)                                       |    10M `$FLR` |     50K `$FLR` |
| [Minimum stake duration](#minimum-stake-duration)                                             |       2 weeks |       2 months |
| [Minimum delegation duration](#minimum-delegation-duration)                                   |       2 weeks |        2 weeks |
| [Stake delay](#stake-delay)                                                                   |       11 days |      immediate |
| [Delegation factor](#delegation-factor)                                                       |       5 times |       15 times |
| [Maximum total stake](#maximum-total-stake)                                                   |    50M `$FLR` |    200M `$FLR` |
| [Maximum validators per infrastructure entity](#maximum-validators-per-infrastructure-entity) |           N/A |              4 |

Setting descriptions:

<a name="minimum-self-bond-amount"></a>

* **Minimum self-bond amount**: Minimum stake that an entity must provide to become a validator.
    This setting reduces staking requests, which frees network resources.

<a name="minimum-delegation-amount"></a>

* **Minimum delegation amount**: Validators can accept stake delegations from any `$FLR` holder, so their stake and validation rewards are increased.
    The specified value is the minimum stake amount that can be delegated.
    This setting reduces delegation requests, which frees network resources.

    Validators can set a staking fee and will earn staking rewards for its own self-bond and a fee on any stake delegated on them.

    This stake delegation is not related to [FTSO delegation](https://docs.flare.network/tech/ftso#delegation) nor [governance delegation](https://docs.flare.network/tech/governance#vote-transfer).

<a name="minimum-stake-duration"></a>

* **Minimum stake duration**: Minimum amount of time that self-bond funds must remain staked, and therefore locked.

<a name="minimum-delegation-duration"></a>

* **Minimum delegation duration**: Minimum amount of time that delegated funds must remain staked, and therefore locked.
    This setting does not change.
    It is included in this list for clarity.

<a name="stake-delay"></a>

* **Stake delay**: Duration that passes between the time when funds are staked and the time when they become effective.
    During this period funds are locked.
    This delay provided added security when staking was enabled for the first time and stakes were small, since any suspicious stake could be analyzed before it became effective.
    Now that larger stakes are in place this restriction will be lifted to simplify onboarding of new validators.

    Note that there is no such delay involved when withdrawing staked funds once the staking period expires.

<a name="delegation-factor"></a>

* **Delegation factor**: The total amount that can be staked to a validator is limited to its self-bond times this factor.
    For example, with the proposed value of 15, if a validator has a self-bond stake of 1M `$FLR`, the total sum of all stakes, including delegations, cannot exceed 15M `$FLR`.
    This allows for 14M `$FLR` of delegations.

<a name="maximum-total-stake"></a>

* **Maximum total stake**: Maximum stake per validator, including self-bond and delegations.

<a name="maximum-validators-per-infrastructure-entity"></a>

* **Maximum validators per infrastructure entity**: A single [infrastructure entity](https://docs.flare.network/tech/validators) can include up to this amount of validators.

    Infrastructure entities also include FTSO data providers.
    For example, a single FTSO data provider can create up to 4 validators, each one with its own stake, and claim the validation rewards for all of them.

    This restriction is not enforced by the P-chain staking mechanism but by the mirroring service, and will not take effect until [phase 3](https://docs.flare.network/tech/validators#phase-3).

    If an infrastructure entity has more than one validator, each of the validators must fulfill the above requirements.

### 2.3 New Validation Rewards Distribution

The yearly inflation is used to reward FTSO data providers and validators in the following manner:

| Provider            | Current value | Proposed value |
| ------------------- | :-----------: | :------------: |
| FTSO data providers |      80%      |      70%       |
| Validators          |      20%      |      30%       |

Staking rewards will:

* Take into account validator uptime, which can be publicly monitored, and existing validator security measures.
    Uptime must be above 80% to receive rewards.
* Take into account staked amount, including self-bond and delegations.
* Require that the FTSO data provider is constantly rewarded for providing good enough prices.
* Be manually calculated off-chain using a public script and then distributed on-chain.

## 3. Link to Code Repository

[GitHub Pull Request #18](https://github.com/flare-foundation/go-flare/pull/18) on the `flare-foundation/go-flare` repository.

## 4. Proposed Implementation Date Range

If the voting passes, deployment of the new code will start immediately.

## 5. Voting Details

To pass, the proposal requires a simple majority of votes in favor of it.

## 6. Deadline for Voting

* **Notice period**: 25-Aug-2023 to 31-Aug-2023
* **Voting period**: 1-Sep-2023 to 8-Sep-2023

## 7. Interaction with the FTSO Management Group

[FIP.02](./FIP_2.md), approved on March 2023, introduced the FTSO Management Group with the goal of "reporting possible infractions by FTSO data providers and collectively determine whether punitive actions should be taken".

[Section 5](./FIP_2.md#5-duration-of-the-proposed-changes) of FIP.02 states that:

> The FTSO management group will operate under the proposed conditions only until a staking mechanism for data providers is implemented.

However, the management group has proved to be such a useful tool that it is proposed to **continue its operation**, as an additional security measure.

## 8. Revision History

|  Revision | Changes                                                                                               |
| --------: | :---------------------------------------------------------------------------------------------------- |
| [1][rev1] | Initial document.                                                                                     |
|    [2](#) | Set minimum uptime to receive rewards to 80% (Clause 2.3).<br>Add link to code repository (Clause 3). |

[rev1]: https://github.com/flare-foundation/governance-proposals/blob/f62238/FIP/FIP_5.md
