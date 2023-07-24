---
nav_order: 10002
title: FIP.02
---

# FIP.02: Add an FTSO Management Group

| Type               | Flare Improvement Proposal                  |
| :----------------- | :------------------------------------------ |
| Author             | Flare Foundation                            |
| Created            | 24-Jan-2022                                 |
| Status             | Final                                       |
| Majority Condition | 50% (required) 74.60% (obtained)            |
| Voting Outcome     | [**Accepted**][ProposalLink] on 06-Mar-2023 |
| Timeline           | 16-Mar-2023 Deployment started on Coston2.<br> 22-Mar-2023 Deployment started on Flare.<br>4-Apr-2023 Deployment complete. |

[ProposalLink]: https://portal.flare.network/proposal/view/69648011017705329136474870894283807954424628818699103324276605104614185715563

## 1. Brief Description

This proposal will create a self-policing FTSO committee, called the FTSO Management Group, to report possible infractions by FTSO data providers and collectively determine whether punitive actions should be taken.

Infractions are purposely undefined. The group will decide on a case-by-case basis whether an infraction has occurred, but this proposal aims to prevent behavior that impairs the FTSO ecosystem as a whole, including, but not limited to:

* **Collusion**, defined as multiple FTSO data providers showing a strong statistical correlation in their submissions or clearly submitting through the same node.
* **Duplication**, defined as multiple FTSO data providers on the same chain, controlled by the same entity, and running largely the same codebases, resulting in substantially similar submissions.

Any member of the management group can propose a provider to be _chilled_. A chilled provider will not be able to operate for some time, causing it economic loss. A second chilling will cause the provider to be banned forever.

Chill proposals will then be voted by all group members and, when approved, will be implemented by governance.

The management group will be formed by upstanding FTSO data providers, meaning that they need to be actively submitting data and earning rewards, and not have been punished recently, either by being chilled themselves or by failing to perform their duties in the management group.

## 2. Technical Description

### 2.1 Management Group Members

This section describes how the members of the FTSO management group are chosen.

1. Any address can request to be a member of the group, but only _upstanding FTSO data providers_ will be accepted.
    Requests from addresses that do not qualify as upstanding will be reverted.

    An address is considered upstanding if ALL the following conditions are met:
    1. It corresponds to an FTSO data provider that has been receiving FTSO rewards for the last 20 reward epochs.
        This condition implies it has been actively submitting good enough data to earn rewards.
        An initial [KYC](https://en.wikipedia.org/wiki/Know_your_customer) process will allow providers to skip this requirement, since not enough reward epochs will have elapsed since the time this proposal is implemented.
    2. It has not been chilled for at least 20 rewards epochs.
    3. It has not been removed from the management group in the last 7 days.

2. Any address can request a group member to be removed.
    The request will succeed if ANY of the following conditions is met:
    1. The member has not earned any FTSO rewards for the last 2 reward epochs.
    2. The member has not cast a vote in 2 out of the last 4 finished proposals that reached the quorum.
        These proposals are only counted since the last time the provider was added to the group.
        If less than 4 proposals have been voted since then, this condition does not apply.

    Providers that became group members using the KYC process can still be removed, and they cannot use the KYC to skip the wait described in clause `1.a` above.

3. Management group members can always be added or removed by the Flare Foundation.

4. The FTSO management groups on the Flare and Songbird networks are independent, since the data providers on each network might be different.

### 2.2 Process for Chill Proposals

The FTSO management group members propose FTSO data providers to be chilled and vote whether to accept or reject each proposal, according to the following process:

1. **Discussion**

    A public discussion should happen before a formal chill proposal is submitted.
    1. A group member (the proponent) creates a new thread on the [Flare FTSO Self-Policing Forum](https://forum.flare.network) proposing to chill a misbehaving FTSO data provider (the defendant).
        This forum is public, but only group members can post on it.
        Defendant data providers can also join by invitation from the group.
    2. The chill proposal must contain the reasons, the C-chain address of the accused data provider, and some kind of evidence.
        Evidence is purposely undefined because it can materialize in various ways, such as data from a collusion-analysis tool.
        There is no specific format for the chill proposal.
    3. Discussion is expected to happen about the chill proposal.
        Ideally the defendant provider should participate.
        The goal is to decide whether a formal chill proposal should be submitted for voting and to gain quorum for it.

2. **Chill proposal submission**

    After public discussion, the proposal must be submitted to the `PollingFtso` smart contract.
    1. This step can be omitted if, after public discussion, the proposal is dropped. If the proposal is dropped, the process stops.
    2. Submitting a proposal costs 100 FLR to limit abuse. This number might need to be adjusted as the system evolves.
    3. If the proponent submits the chill proposal too early or without enough public discussion, the proposal may not have gained enough traction nor meet the quorum condition required for the voting to pass (See 3.a below).
        Proponents should submit a proposal only when they are reasonably sure it will meet the quorum condition.
    4. The proposal must contain the forum thread URL where the discussion happened.
        Only this URL to the discussion is stored on-chain.
    5. Once the proposal is submitted on-chain, the proponent receives a _proposal ID_, which they must submit to the forum thread so all participants know which proposal to vote on.
    6. Voting starts immediately after proposal submission.
    7. Each group member has one vote.
    8. Group members can cast their votes on the `PollingFtso` voting contract, using the proposal ID provided by the proponent on the discussion thread.
       The address of the contract will be present in a prominent place in the forum.
    9. Voting lasts for 48h.
    10. Proposals can also be submitted by the Flare Foundation.

3. **Voting conditions**

    A vote only passes if both these conditions are met:
    1. Quorum condition (threshold): 66% of the current number of members of the management group must cast a vote.
    2. Majority condition: More than 50% of the current number of members of the management group vote for the proposal.

4. **Voting results**

    1. If the chill proposal passes, the Flare Foundation chills the proposed FTSO data provider.
    2. The Flare Foundation reserves the right to not act upon the results of the voting.

### 2.3 Chill Effects

1. When a provider is chilled for the first time, it is removed from the [FTSO data providers whitelist](https://docs.flare.network/infra/data/whitelisting/) and it cannot re-apply for 2 FTSO reward epochs.
    When providers are punished in this way, they cannot submit prices, get rewards or participate in the FTSO management group.

    After this chill period, the provider can re-apply for the whitelist.

2. When a provider is chilled for the second time, it is permanently banned from the whitelist.

## 3. Link to Code Repository

A web forum will be available at the address <https://forum.flare.network>, hosted by the Flare Foundation.

A new contract called [`PollingFtso`](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/698-polling-ftso-providers/contracts/governance/implementation/PollingFtso.sol) will be deployed to take care of voting on chill proposals.

Right after deploying it, a call will be made to the `setMaintainer` method to set the _maintainer_ of the contract.
This will be the only address authorized to add and remove management group members, and to change parameters.

## 4. Proposed Implementation Date Range

Implementation is expected to start shortly after the voting finishes and be completed quickly.

Because of the urgent need to add a regulatory mechanism to the FTSO data provider community, this proposal is being simultaneously submitted to the Flare ([FIP.02](../FIP/FIP_2.md)) and Songbird networks ([STP.03](../STP/STP_3.md)).

## 5. Duration of the Proposed Changes

The FTSO management group will operate under the proposed conditions only until a staking mechanism for data providers is implemented.

## 6. Voting Details

To pass, the proposal requires a simple majority of votes in favor of it.

## 7. Deadline for Voting

One week after the proposal is published in [the Flare Portal](https://portal.flare.network/).
