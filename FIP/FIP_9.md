---
nav_order: 10009
title: FIP.09
---

# FIP.09: Introduce FLR Protocol Emissions

| Type               | Flare Improvement Proposal |
| :----------------- | :------------------------- |
| Author             | Flare Foundation           |
| Created            | 11-Jun-2024                |
| Document Status    | Draft                      |
| Majority Condition | 50% (required to reject)   |

## 1. Brief Description

Since its launch in July 2022, the Flare mainnet ecosystem has grown significantly.
The original 20 billion `$FLR` tokens allocated to the [incentive pool](https://flare.network/tokenomics-flr-updated/) for bringing sustainable value to Flare remains fully intact.
With the advent of the decentralized exchange (DEX), lending protocols, and other dapps that might launch in the coming months, the Flare ecosystem has been deemed suitably mature such that a targeted `$FLR` emission program using a modest portion of the incentive pool could have a markedly positive impact on further ecosystem growth and broader Flare community expansion.
Therefore, this proposal by the Flare Foundation seeks to introduce a `$FLR` emissions budget and a framework to deploy a small portion of the incentive pool to attract and support builders and liquidity on Flare mainnet.

If this proposal is accepted, the Flare Foundation will:

* Allocate 450,000,000 `$FLR` of the total incentive pool to an emissions schedule as detailed below.
* Specify the distribution process and parameters of a proposed `$FLR` emission distribution program.

## 2. Technical Description

The monthly maximum of `$FLR` emissions will be distributed according to the following schedule.
At all times, monthly `$FLR` emissions proposed will account for a notably small portion of the projected emissions modeled on the [Flare tokenomics page](https://flare.network/tokenomics-flr-updated/).

| Date           | `$FLR` emissions |
| :------------- | :--------------- |
| July 2024      | 15,000,000       |
| August 2024    | 20,000,000       |
| September 2024 | 25,000,000       |
| October 2024   | 30,000,000       |
| November 2024  | 35,000,000       |
| December 2024  | 40,000,000       |
| January 2025   | 40,000,000       |
| February 2025  | 45,000,000       |
| March 2025     | 45,000,000       |
| April 2025     | 50,000,000       |
| May 2025       | 50,000,000       |
| June 2025      | 50,000,000       |

### 2.1 The Emissions Committee

The responsibility for directing emissions toward dapps and protocols building on Flare will be vested in an Emissions Committee.

The Committee will allocate `$FLR` emissions to dapps and protocols at its discretion, while adhering to the proposed rules and maximum monthly `$FLR` emissions limits, as detailed in the [emissions schedule](#2-technical-description).
The Committee will have the authority to decide the form and distribution of the emissions, such as whether they are available in `$FLR` or a wrapped version of `$FLR` with vesting periods.
If a vesting schedule is applied to the distribution mechanisms for `$FLR` emissions, measures will be taken for emissions beneficiaries to wrap and delegate awarded `$FLR` in order to benefit from the [FlareDrop](https://docs.flare.network/dev/reference/the-flaredrop/) and [FTSO delegation rewards](https://docs.flare.network/tech/ftso/#rewards).

The Committee will have the discretion to allocate up to the maximum unique monthly `$FLR` emission amounts in the schedule.
If the full amount for any unique month is not allocated during that specified month, the Committee will have the discretion to either reallocate unspent funds to a future program or burn the `$FLR`.
After the `$FLR` emissions schedule and any associated vesting distribution mechanisms are complete, the Committee will have the discretion to either burn or reallocate the undistributed and unclaimed `$FLR` emissions to a future incentive program.

Beneficiaries of emissions are solely responsible for claiming their rewards on a monthly basis and for wrapping and delegating the `$FLR` accordingly to benefit from the FlareDrop and FTSO delegation rewards that might be available as part of any distribution mechanisms established.

### 2.2 Emissions Committee Reporting Requirements and Limitations

To ensure transparency and accountability, the Emissions Committee will be required to publicly report their emissions allocations every quarter.
If the Committee fails to act in the Flare community's interest, a governance proposal can be initiated to defund and dissolve the Committee.
In this case, the emissions schedule will be terminated and the Committee will not receive the `$FLR` emissions tokens that were allocated for the remaining months in the schedule.

All Committee-directed `$FLR` emissions must support dapps on the Flare mainnet, and monitoring mechanisms will be established to verify that dapps are properly distributing emissions across their qualifying participating user base.

The Committee will be restricted from allocating more than 33% of the quarterly budget to any single protocol.
Additionally, the Committee will operate within a framework of governance principles, including the quarterly reporting, adherence to compliance standards, and the implementation of a conflict-of-interest policy to avoid potential biases in decision-making.
This conflict-of-interest policy will require the disclosure of relevant investments to the Committee.

### 2.3 Emissions Committee Members

The Emissions Committee will be comprised of these members:

* Hugo Phillion:
* Gabe:
* Dhruv:
* Maelstrom:
* FTSO Management Group or Quantdesign:

## 4. Proposed Implementation Date Range

The Committee would be funded immediately after the passage of the governance proposal with the first month’s emissions allocation process beginning July 1st, 2024.

## 5. Voting Details

To pass, the proposal requires a simple majority of votes in favor of it.

## 6. Deadline for Voting

* **Notice period**: 3-June-2024 to 9-June-2024
* **Voting period**: 12-June-2024 to 18-June-2024

## 7. Appendix

### 7.1 Purpose of Emissions

Alongside grants, developer tools, guides, and documentation, token emissions are designed to support the engineers, builders, and dapps launching on a blockchain network through the incentivization of on-chain activities.
Protocol emissions can be composed of both the native blockchain currency and a number of protocol tokens.
They can help to ensure a more targeted distribution of rewards to enable application growth and to support an efficient ecosystem during the product-launch phase.

By providing liquidity to protocols and participating in other emission-incentivized activities on the blockchain, stakeholders can receive rewards.
These rewards may be used to provide additional security to the network via [wrapping](https://docs.flare.network/user/wrapping-tokens/), [delegation](https://docs.flare.network/user/delegation/managing-delegations/), and [staking](https://docs.flare.network/user/staking/), which can help ensure the expansion of a blockchain network and thereby, an ecosystem that functions more efficiently.

### 7.2 FAQs

* _What distribution mechanisms are being considered and how would a vested distribution schedule for `$FLR` work?_

  Flare is working with a tokenomics-design protocol to determine the most efficient way to distribute `$FLR` emissions to maximize community participation.
  A distribution schedule could involve a linear vesting schedule whereby monthly emissions could be claimable on a time-locked basis spread over a number of months.
  You could claim the maximum amount two weeks after receipt, although doing so would incur a 50% claim penalty.
  Alternatively, you would be able to wrap and delegate the vesting tokens to enable accrual of the FlareDrop and FTSO delegation rewards to their `$FLR` emission positions, which would smooth any reward claims and help to mitigate any adverse token dynamics that could result from monthly emissions.

* _When would I receive my `$FLR` emissions, and how can I claim them?_

  `$FLR` emissions will be made available for users on a monthly basis by the participating dapps.
  At the end of each month, you will be awarded `$FLR` emissions in proportion to their use of protocols according to qualifying actions, which are at the discretion of the dapps to determine and communicate.
  You can claim your `$FLR` emissions by using the [Flare Portal](https://portal.flare.network/).

* _How will I know how much `$FLR` I have and how do I wrap and delegate it?_

  `$FLR` emissions eligibility and quantity will be visible in the [Flare Portal](https://portal.flare.network/).
  To check, connect to the Portal using the same wallet address you used to participate participate in dapps on Flare mainnet.
  You can also use the Portal to wrap and delegate any received emissions to earn FlareDrop and FTSO delegation rewards. Alternatively, you can claim the full amount of `$FLR` but pay a 50% penalty according to the vesting schedule.

* _Why is only 450,000,000 `$FLR` being allocated from a total incentive pool of 20,000,000,000 tokens?_

  Flare is targeting 0.45% of the total token supply initially by way of incentivizing the use of community dapps and protocols launching on Flare mainnet.
  This allocation provides headroom to launch alternative emissions programs and expand any incentives over time beyond the end of the FlareDrop schedules.

* _Can we vote to launch another round of incentives to coincide with the end of the FlareDrop?_

  The Flare community is welcome to submit any proposals it deems appropriate to encourage builders and users to participate on Flare mainnet.
