---
nav_order: 10001
title: FIP.01
---

# FIP.01: Widen FLR Distribution and Reduce Inflation

| Type               | Flare Improvement Proposal                  |
| :----------------- | :------------------------------------------ |
| Author             | Flare Foundation                            |
| Created            | 18-Jan-2023                                 |
| Document Status    | Final                                       |
| Majority Condition | 50% (required) 93.78% (obtained)            |
| Voting Outcome     | [**Accepted**][ProposalLink] on 27-Jan-2023 |
| Timeline           | 22-Feb-2023 Deployment started on Coston2.<br> 23-Feb-2023 Deployment started on Flare.<br>16-Mar-2023 Deployment complete. |

[ProposalLink]: https://portal.flare.network/proposal/view/31313223399225215467886651717691514407244212421129328237186875197531473335173

## Brief Description

When Flare Networks began, it offered a single utility to a single community. Since then, Flare has expanded to deliver robust protocols that enable developers to build applications that can use data from other blockchains and the internet. Recently during this time of expansion, Flare successfully delivered 15% of the original FLR airdrop to eligible recipients to reward them for their support and incentivize them to continue participating with the network. Now, Flare wants to continue developing the ecosystem with the help from more communities, while still rewarding in equal measure the original airdrop recipients.

Additionally, Flare wants to grant airdrop recipients more independence from exchanges and other token custodians, since this has proven a delicate topic in the past.

To achieve these objectives, this proposal changes the method of supplying the remaining 85% of the public token distribution and the inflation rate.

## Technical Description

On January 9, 2023, 15% of the planned airdrop tokens was sent to eligible recipients.
If this proposal passes with a simple majority, the Flare Foundation will take the following actions regarding the distribution of the remaining 85% of the tokens:

* Distribute the remaining 24,246,183,166 FLR tokens as part of the FTSO delegation incentive pool in 36 monthly increments to addresses that contain wrapped FLR tokens (WFLR).
  Wrapping is a proxy for delegation.
* Reduce the original 10%-per-annum inflation rate according to the following schedule:

  | Year                | Inflation Rate |
  | ------------------- | -------------: |
  | 1                   |            10% |
  | 2                   |             7% |
  | 3 and in perpetuity |             5% |

  The original 10% inflation rate is based on the total fully diluted supply of FLR. The proposed inflation rate is based on the available supply of FLR, not the fully diluted supply, and it is capped at 5B FLR per annum.

### Key Objectives

* [Improve long-term token economics](https://flare.network/fip01/#improve-long-term-token-economics)
* [Alleviate the risk of exchanges and centralized-finance platforms for FLR token holders](https://flare.network/fip01/#alleviate-the-risk-of-exchanges-and-cefi-platforms-for-token-holders)
* [Reward existing participants and incentivize new participants](https://flare.network/fip01/#reward-existing-participants-and-incentivise-new-participants)
* [Manage liquidity in better ways](https://flare.network/fip01/#better-manage-liquidity)
* [Improve the tokenomics for new ecosystem entrants](https://flare.network/fip01/#improve-the-tokenomics-for-new-ecosystem-entrants)
* [Enable the distribution of FLR to be transferable](https://flare.network/fip01/#enable-the-distribution-of-flr-to-be-transferable)
* [Reward infrastructure providers and entities that generate value for the Flare network by enabling them to vest FLR while constraining inflation](https://flare.network/fip01/#reward-infrastructure-providers-and-those-that-generate-value-for-the-network-by-enabling-them-to-vest-flr-whilst-constraining-inflation)

## Changes in the Smart Contracts After the Vote

The [`DistributionTreasury`](https://flare-explorer.flare.network/address/0x1000000000000000000000000000000000000004) contract contains all distribution funds. Depending on the outcome of the vote, the following actions from this contract will occur:

* **If the proposal passes**:

  1. The `selectDistributionContract` method will be called to select the [`DistributionToDelegators`](https://flare-explorer.flare.network/address/0x32c6379B2978A9aB75993cA82e3ADc77dd50010C) contract. This selection cannot be undone.
  The address for the `DistributionToDelegators` contract will be shown in the `selectedDistribution` read-only field of the `DistributionTreasury` contract.
  2. The timestamp of the distribution will be written to the `setEntitlementStart` method in the `DistributionToDelegators` contract.

* **If the proposal does not pass**:

  1. The `selectDistributionContract` method will be called to select the [`Distribution`](https://flare-explorer.flare.network/address/0xd7b8D88603De93dD9b430ea5f81dC32C117Df250) contract. This selection cannot be undone.
  The address for the `Distribution` contract will be shown in the `selectedDistribution` read-only field of the  `DistributionTreasury` contract.
  2. The timestamp of the distribution will be written to the `setEntitlementStart` method in the `Distribution` contract.

> **NOTE:**
> Both the `Distribution` and `DistributionToDelegators` contracts are currently being improved to add autoclaiming functionality.
> Therefore, their final addresses might not be the ones provided above.
> No other changes in functionality are expected.

## Proposed Implementation Date Range

The changes will be implemented at least 3 weeks after the voting ends, due to the aforementioned addition of the autoclaiming functionality.

## Voting Information

* **Interim period**: 10-Jan-2023 to 28-Jan-2023.
  During this period, network inflation will be calculated as per this proposal, and no further token distributions will occur.
* **Notice period**: 13-Jan-2023 to 19-Jan-2023
* **Voting period**: 21-Jan-2023 to 28-Jan-2023
* **Voting eligibility**: Except for the Flare Foundation and the Flare VC Fund, because this entity derives some of its token balance from the Flare Foundation, all FLR token holders can vote.
* **Voting metrics**: To pass, the proposal requires a simple majority of votes in favor of it.

## Rules for Fairness

* All Flare-related entities which are eligible to vote have escrowed 85% of their tokens, to be distributed over the next 36 months. This portion cannot be used to vote.
  With 85% of their tokens in escrow, Flare-related entities can use 15% of their final balance to vote on this proposal.
* No Flare-related entity, employee, or founder may use the tokens derived from their position to earn any part of the token distribution.
  To ensure they do not earn a portion of the token distribution, legal agreements are in place, which are contingent on the proposal passing, with severe sanctions that amount to forfeiture of their entire FLR stake.
  However, they can be network participants outside their official positions with Flare, and, as such, they can use tokens that they did not obtain through their position to earn part of the token distribution.

## Formulas for Calculating Distribution and Available FLR Supply

* [Distribution](https://flare.network/fip01/#ftso-distribution)
* [Available FLR supply](https://flare.network/fip01/#definition-of-available-supply)
