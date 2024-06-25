---
nav_order: 10009
title: FIP.09
---

# FIP.09: Introduce FLR Protocol Emissions

| Type               | Flare Improvement Proposal |
| :----------------- | :------------------------- |
| Author             | Flare Foundation           |
| Created            | 25-Jun-2024                |
| Document Status    | Draft                      |
| Majority Condition | 50% (required to reject)   |

## 1. Brief Description

Since its launch in July 2022, the Flare mainnet ecosystem has grown significantly.
Importantly, this growth has been achieved without using any of the original 20 billion `$FLR` tokens allocated to the incentive pool.
Decentralized exchanges (DEXs), lending protocols, and other dApps have launched and are anticipated to launch more frequently over the coming months.
Coupled with FAssets and the launch of other bridges, the Flare ecosystem is now considered suitably mature such that a targeted `$FLR` emissions program could help to further accelerate sustainable ecosystem growth and community expansion.
This proposal by the Flare Foundation seeks to introduce a `$FLR` emissions budget and a framework to deploy a small portion of the incentive pool to attract builders and support liquidity on Flare.

If this proposal is accepted, the Flare Foundation will allocate approximately 510,000,000 `$FLR` of the total 20,000,000,000 `$FLR` incentive pool to an [emissions schedule](#21-emissions-schedule) as detailed below.

## 2. Technical Description

The monthly maximum of `$FLR` emissions will be distributed according to the following schedule.
At all times, monthly `$FLR` emissions proposed will account for a notably small portion of the projected emissions modeled on the [Flare tokenomics page](https://flare.network/tokenomics-flr-updated/).
Given the design of the [incentive pool contract](https://flare.network/fip01/) and the coded constraint to allocate the lower of either 3% per annum (calculated by available supply) or 10% of the pool per annum, this FIP proposes two 1/12 tranches of 6.8% of the total circulating supply to be withdrawn over the first two months of the schedule, July and August 2024, to cover the projected emissions for the total schedule.
This amount has been calculated to equal a total of approximately 510,000,000 `$FLR` to be made available as part of the proposed emissions schedule.

### 2.1 Emissions Schedule

The following table shows the proposed emissions schedule.
The amounts are the most accurate estimations, given the dynamic circulating supply and coded withdrawal requirements of the incentive pool contract.
The actual amount may vary.

Each of the monthly emissions will be split among the beneficiaries.
To receive their allocations in full, beneficiaries must wait 12 months to claim them.
However, beneficiaries who want to claim before the 12-month period has elapsed can do so, but they will lose 50% of their allocation.
During this 12-month period, beneficiaries will still accrue FlareDrop rewards, which they can claim.

| Date           | `$FLR` emissions |
| :------------- | :--------------- |
| July 2024      | 15,000,000       |
| August 2024    | 20,000,000       |
| September 2024 | 25,000,000       |
| October 2024   | 30,000,000       |
| November 2024  | 35,000,000       |
| December 2024  | 40,000,000       |
| January 2025   | 45,000,000       |
| February 2025  | 50,000,000       |
| March 2025     | 55,000,000       |
| April 2025     | 60,000,000       |
| May 2025       | 65,000,000       |
| June 2025      | 70,000,000       |

### 2.1 The Emissions Committee

The responsibility for directing emissions toward dApps and protocols building on Flare will be vested in an Emissions Committee.
The Committee will initially launch with six members but may expand in size and change members upon majority approval from the Committee.

The Committee will have the discretion to allocate `$FLR` emissions to dApps and protocols, while adhering to the proposed rules and maximum monthly `$FLR` emissions limits, as detailed in the [emissions schedule](#2-technical-description).
The Committee will have the authority to decide the form and distribution of the emissions, such as whether they are available in `$FLR` or a wrapped version of `$FLR` with vesting periods.
If a vesting schedule is applied to the distribution mechanisms for `$FLR` emissions, measures will be taken for emissions beneficiaries to receive [FlareDrop](https://docs.flare.network/tech/the-flaredrop/) rewards after claiming from dApps directly or via the [Flare Portal](https://portal.flare.network/).

The Committee will have the discretion to allocate up to the maximum unique monthly `$FLR` emission amounts in the schedule.
If the full amount for any unique month is not allocated during that specified month, the Committee will have the discretion to either reallocate unspent funds to a future program or burn the `$FLR`.
After the `$FLR` emissions schedule and any associated vesting distribution mechanisms are complete, the Committee will have the discretion to either burn or reallocate the undistributed and unclaimed `$FLR` emissions to a future incentive program.

Beneficiaries of emissions are solely responsible for claiming their rewards on a monthly basis.
Rewards do not expire.
[FlareDrop](https://docs.flare.network/tech/the-flaredrop/) rewards begin to accrue to portions of `$FLR` rewards upon claiming.

### 2.2 Emissions Committee Reporting Requirements and Limitations

To ensure transparency and accountability, the Emissions Committee will be required to publicly report their emissions allocations every quarter.
If the Committee fails to act in the Flare community’s interest, a governance proposal can be initiated via Songbird and then Flare to defund and dissolve the Committee.
In this case, the emissions schedule will be terminated and the Committee will not receive the `$FLR` emissions tokens that were allocated for the remaining months in the schedule.

All Committee-directed `$FLR` emissions must support dApps on Flare mainnet, and the committee will establish monitoring mechanisms to verify that dApps are properly distributing emissions across their qualifying participating user base.
The Committee will be restricted from allocating more than 33% of the quarterly budget to any single protocol.
Furthermore, the Committee will require that all members disclose to the Committee their investments in projects under review for emissions to avoid conflicts of interest.

### 2.3 Emissions Committee Members

The Emissions Committee will be comprised of these members:

* **Hugo Phillion: Cofounder of Flare Network, CEO of Flare Labs**

    With prior experience as a derivatives trader managing portfolios for major funds, Hugo later pursued studies in machine learning at University College London, where he met Flare’s other co-founders.

* **Gabe Bassin: CIO of Flare Network**

    Seasoned finance professional with 20-plus years of experience including hedge funds, real estate, and venture capital investing.
    Gabe has been a Flare advisor since 2017 when he met the founding team through an NYC-based accelerator.
    He joined as full-time chief investment officer in 2023.

* **Dhruv Shah: DeFi Analyst at Flare Network**

    Prior to joining Flare Network, Dhruv worked at a Bitcoin custody firm that focused on helping Bitcoin miners understand the complexities of UTXOs and broadcasting transactions.
    Now at Flare, he is focused on growing the DeFi ecosystem by fostering strategic partnerships and building relationships with capital partners.

* **Toby Norfolk-Thompson: Founder of Trident Digital**

    Toby set up Trident Digital, a digital asset lender and DeFi firm in 2023 having previously served as the CIO of Matrixport and Coinbase.
    He spent 18 years in senior fixed income trading and structuring roles at Barclays and has been heavily involved in decentralized finance since 2016, investing in a range of fintech and decentralized finance firms besides Flare, such at Cleartoken (UK clearing house project), Singularity (Egyptian payments platform) and Preyus, a private banking platform.
    He is heavily involved in regulatory policy and serves as a fellow of the Digital Economy Initiative and the Adam Smith Institute.
    He is also on the global advisory board of AWR Lloyd, a Hong Kong-based investment bank.

* **Into the Block**

    The IntoTheBlock Institutional DeFi platform is a multi-product offering for crypto-native companies and investors to access DeFi with proper alpha intelligence, automation, security, and risk management.
    The platform is used by a large percentage of the largest crypto institutions and DeFi protocols in the market.

* **Quantdesign**

    Quantdesign is a leading tokenomics and mechanism design firm.
    It uses data to design more effective incentives for blockchain projects and ecosystems.
    It has helped Flare develop the protocol emissions strategy outlined in this proposal and will continue to remain involved in the process as a member of the Committee.

## 4. Proposed Implementation Date Range

The Committee would be funded immediately after the passage of the governance proposal with the first month’s emissions allocation process beginning July 6th 2024.

## 5. Voting Details

To pass, the proposal requires a simple majority of votes in favor of it.

## 6. Deadline for Voting

* **Notice period**: 25-June-2024 to 27-June-2024
* **Voting period**: 28-June-2024 to 4-July-2024

## 7. Appendix

### 7.1 Purpose of Emissions

Alongside grants, developer tools, guides, and documentation, token emissions are designed to support the engineers, builders, and dApps launching on a blockchain network through the incentivization of on-chain activities. Protocol emissions can be composed of both the native blockchain currency and a number of protocol tokens. They can help to ensure a more targeted distribution of rewards to enable application growth and to support an efficient ecosystem during the product-launch phase.

By providing liquidity to protocols and participating in other emission-incentivized activities on the blockchain, stakeholders can receive rewards.
These rewards may be used to provide additional security to the network via [wrapping](https://docs.flare.network/user/wrapping-tokens/), [delegation](https://docs.flare.network/user/delegation/managing-delegations/), and [staking](https://docs.flare.network/user/staking/), which can help ensure the expansion of a blockchain network and thereby, an ecosystem that functions more efficiently.

### 7.2 FAQs

1. _What distribution mechanisms are being considered and how would a vested distribution schedule for `$FLR` work?_

    Flare has worked with a tokenomics design team to determine the most efficient way to distribute `$FLR` emissions and maximize community participation.
    A smart contract-based `$FLR` distribution schedule will be deployed involving a linear vesting schedule whereby monthly emissions will be redeemable on a time-locked basis.

    According to this schedule, each of the unique monthly `$FLR` emissions distributions will be unlocked for users to redeem in equally sized tranches over a total period of 12 months.
    Therefore, the total length of the program, from the first emission to the final month of claiming, is projected to run for 24 months.

    Users may redeem the maximum amount of any portion of a month’s allocated `$FLR` distribution immediately after receipt, although doing so will incur a 50% redemption penalty for that month’s earned emissions.
    Portions of awarded emissions claimed from dApps will begin to accrue FlareDrop rewards proportional to claim amounts.
    The distribution mechanism is designed to smooth rewards, maintain user participation, and help to mitigate any adverse token dynamics that could result from monthly emissions distribution in full.

2. _When would I receive my `$FLR` emissions, and how can I claim them?_

     `$FLR` emissions will be made available for users on a monthly basis by the participating dApps.
    Each month, qualifying dApps can award `$FLR` emissions to users in proportion to their use of protocols according to qualifying actions and parameters.
    Parameters will be set and communicated at the discretion of the dApps.

    DApps will have a grace period of one extra month to distribute allocated rewards for any given month, at which point the rewards for the given month for that dApp may be revoked by the Committee.
    Users may redeem `$FLR` emissions by using new functionality for emissions to be added to the [Flare Portal](https://portal.flare.network/) after claiming and be sure to complete their final redemption before the end of the program, which is projected to finish 24 months after its launch on July 6, 2024.

3. _How will I know how much `$FLR` I have and how do I wrap and delegate it?_

    `$FLR` emissions eligibility and quantity will be visible via dApps and the [Flare Portal](https://portal.flare.network/).
    To check, connect to the Portal using the same wallet address you used to participate participate in dApps on Flare mainnet.

    The Portal may be used to claim emissions, at which point FlareDrop rewards will begin to accrue.
    The Portal may also be used to redeem emissions for `$FLR`, whereby the full amount of `$FLR` may be redeemed with a 50% penalty according to the elapsed time of the linear 12-month vesting schedule.

4. _Why are only 510,000,000 `$FLR` tokens being allocated from a total incentive pool of 20,000,000,000 tokens?_

    Flare is targeting 0.51% of the total token supply initially by way of incentivizing the use of community dApps and protocols launching on Flare mainnet.
    This allocation provides headroom to launch alternative emissions programs, including any FAsset incentive initiatives, and to expand any protocol incentives beyond the end of the FlareDrop schedules.

5. _Can we vote to launch another round of incentives to coincide with the end of the FlareDrop?_

    The bicameral Songbird and Flare governance system set in the [blog](https://medium.com/flarenetwork/flare-governance-and-the-role-of-songbird-4579c9891b47) proposes that the Flare community may submit proposals it deems appropriate to encourage builders and users to participate on Flare mainnet.

    The governance system is currently in development for deployment on Songbird over the coming weeks and will facilitate the proposal of alternative incentive programs in addition to any proposals to dissolve or amend the Emissions Committee.
