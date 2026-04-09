---
nav_order: 10016
title: FIP.16
---

# FIP.16: Restructure FLR Tokenomics for Long-Term Network Sustainability

| Type               | Flare Improvement Proposal                  |
| :----------------- | :------------------------------------------ |
| Author             | Flare Foundation                            |
| Created            | 27-Mar-2026                                 |
| Document Status    | Draft                                       |
| Majority Condition | 50% (required)                              |

## 1. Brief Description

Making FAssets safer through stablecoin collateral and more capital-efficient via the Core Vault unintentionally reduced `FLR`’s role in the ecosystem.
This Governance proposal aims to fundamentally relink `FLR` token economics to existing and growing ecosystem activity.

If accepted, this proposal will immediately reduce `FLR` inflation by 40% and establish mechanisms to collect and distribute the organic earnings of the network.
The design ensures that as ecosystem activity expands, `FLR` will first transition to being non inflationary, and then to being deflationary.
Furthermore, the system will distribute parts of the earnings collected in FAssets, `FLR`, stablecoins, `WETH` and others, to applications and asset issuers, to incentivize further ecosystem expansion.
This creates a feedback loop driving higher network earnings.
Additionally, the proposal seeks to accelerate `FLR` supply reduction and further incentivize staking over delegation, a shift designed not only to improve network security, but also to increase locked supply and thus ensure economic stability for validators.

### 1.1 Current Token Role

As things stand, the `FLR` token dynamics are governed by a balance of inflationary and disinflationary mechanisms, as follows.

Inflationary mechanisms -- actions that introduce new tokens to incentivize participation:

* Provide and support network security through staking rewards.
* Strengthen oracle security through staking and delegation rewards.
* Encourage ecosystem growth through emissions from the incentive pool (initially set at 20Bn `FLR`).

Disinflationary mechanisms -- actions designed to reduce the growth rate of the token supply:

* Regulate network usage and capacity through transaction fees.
* Reduce circulating supply by burning unclaimed inflation rewards after 3 months.
* Enforce certain performance standards among infrastructure providers by burning rewards for those who fail to meet network requirements.
* Burn payments for FDC oracle proofs when attestation requests fail.

Network and Oracle security rely on infrastructure providers who operate validators and contribute to Flare’s protocols (FTSO and FDC).
Providing an economic incentive for these resource-intensive roles is the primary reason `FLR` inflation exists.
Under this proposal, inflation will fall immediately and over time the economic incentives for infrastructure providers -- and their delegators -- will transition from `FLR` inflation to **organic yield** driven by on-chain activity.

### 1.2 Structure of the Proposal

This governance proposal splits into four areas:

* [Inflation reduction](#2-inflation-reduction)
* [Ongoing supply reduction](#3-ongoing-supply-reduction)
* [Network earnings](#4-network-earnings) (which will be used for further inflation reduction & supply reduction)
* [Network safety and validator sustainability](#5-network-safety-and-validator-sustainability)

## 2. Inflation Reduction

### 2.1 Immediate Inflation Rate Reduction

If this proposal passes annual inflation will reduce to 3% from the current rate of 5%.
The existing inflation system also includes a maximum inflation cap, which limits yearly inflation to a maximum of 5 billion `FLR`,  irrespective of inflation percentage and base inflatable supply.
This proposal also reduces the hard cap for yearly inflation from 5 billion to 3 billion, to keep the cap in line with the decreased inflation rate.

### 2.2 Revised Inflation Calculation Criteria

This proposal seeks to exclude certain pools of tokens from the calculation of inflation rate, effectively reducing the total amount of inflation generated.
The excluded pools are where `FLR` is either temporarily or permanently unavailable.
The additional excluded pools would be:

* **Burnt address** (`0x000000000000000000000000000000000000dEaD`): the `FLR` burned in deflationary actions is permanently removed from circulation and should not count towards the total network supply.
* **Temporary pools of unearned rewards:** Consisting of tokens captured from protocol-level penalties, these pools hold unearned rewards in a temporary state.
  While a fraction is reallocated to incentivize core protocols, such as Block-Latency Feeds and FDC requests, the rest is permanently burned.

* **[Flare Income Reinvestment Pools (FIRE):](#45-flare-income-reinvestment-entity)** `FLR` accrued in any such pools from e.g. fees until they are either burnt or redistributed.
  Since `FLR` in the pool is unavailable until distributed and tokens paid in are taken from the circulation, it is therefore appropriate to exclude them from the inflation calculation prior to reallocation.

## 3. Ongoing Supply Reduction

All `FLR` used for transaction fees is burned, serving as an important supply sink.
At present, Flare Network's transaction fees are remarkably low, both in `FLR` and fiat terms: the current minimal base gas fee per gas is set to 25 gwei (`10^-9 FLR`), and will be temporarily lowered further with the v1.13.0 [node upgrade](https://github.com/flare-foundation/go-flare).
The data below shows a comparison for average transaction costs across multiple popular blockchains.
For Flare, a 60 gwei value is used to estimate the *average* gas fee for a transaction.

| Chain | Average Base Fee | Approximate $ Cost for Simple Transfer |
| :--- | :--- | :--- |
| Flare | 60 gwei | $0.000004 |
| Flare (after) | 1200 gwei | $0.00008 |
| Ethereum | 0.089 gwei | $0.004093 |
| Base | 0.005 gwei | $0.000228 |
| Avalanche | 0.028 gwei | $0.0000055 |
| BNB | 0.1 gwei | $0.001361 |

Every transaction requires computational resources, disk space, and network bandwidth from block producers.
To better reflect these infrastructure costs and align Flare with industry standards, this proposal suggests increasing the base transaction fee.
More precisely, this proposal seeks to increase the base gas fee by 20X -- from 25 gwei to 500 gwei -- with the necessary modifications introduced by the ACP-176 mechanism after the v1.13.0 [node upgrade](https://github.com/flare-foundation/go-flare).
Even with this adjustment, Flare’s transaction costs will remain orders of magnitude lower than those of comparable networks in both native and fiat terms.
This change strengthens the token’s deflationary trajectory without compromising affordability.

To understand the potential impact of this change on token supply: over the past six months, approximately 7.5M `FLR` was burned.
Under the proposed fee structure, and assuming a conservative scenario of zero ecosystem growth, this figure would rise to roughly 300M `FLR`.
With inflation being reduced to 3%, on an inflatable balance of 86B `FLR`, the gross inflation would be 2.58B `FLR`.
Subtracting the 300M burnt `FLR` from this number brings gross inflation to 2.28B and thus the net inflation percentage to 2.66% rather than 3.0%.
Given the current rapid expansion of the ecosystem, these projections are overly conservative.
The Flare governance will closely monitor network activity and readjust the fee structure as needed to balance ecosystem growth with overall transaction costs.

## 4. Network Earnings

Network earnings will accrue to a dedicated entity named FIRE, whose structure, operations, and governance are detailed in [Section 4.5](#45-flare-income-reinvestment-entity).
By design, FIRE’s resources will expand in tandem with the network’s economic activity.

### 4.1 FDC Data Request Fees

To request an FDC attestation, users must pay a fee in `FLR` to compensate data providers (and their delegators) for the associated workload.
The fee also creates a prioritization mechanism for request processing while not congesting the request queue.
Timely and accurate FDC attestations are critical for on-chain protocols, with two core drivers being FAssets and Flare Smart Accounts, each requiring at least one or two attestation per action (minting, redemption, Flare Smart Account Action invocation).

FDC requests are available to all network projects, with growing adoption observed in recent months.
Current use cases range from prediction markets to cross-chain payment metadata attestations for compliance.
Notably, Flare Confidential Compute (FCC) will heavily leverage FDC to attest and verify deployments and configurations of all applications built within FCC environment.

Currently, the fee is fixed at 1 `FLR` for all attestation types except the `Web2Json`-type requests, where a request has a minimal fee of 100 `FLR`.
All fees paid for FDC requests are collected and distributed to infrastructure providers and their delegators as part of the rewarding system, on top of inflationary rewards.
Fees for any unconfirmed requests due to malformed requests, invalid or unconfirmable requests are permanently burned.

This proposal seeks to adjust the base request cost in the following manner:

| Request Type | Current Base Cost (`FLR`) | Proposed Base Cost (`FLR`)|
| :--- | :--- | :--- |
| `AddressValidity` | 1 | 20 |
| `Payment` | 1 | 20 |
| `ReferencedPaymentNonExistence` | 1 | 20 |
| `ConfirmedBlockHeightExists` | 1 | 3 |
| `BalanceDecreasingTransaction` | 1 | 20 |
| `EVMTransaction` | 1 | 20 |
| `Web2Json` | 100 | 100 |

Furthermore the fee redistribution mechanism will change as follows.
For each request, 10% of the fees will be distributed alongside inflation -- consistent with the current model -- while the remaining 90% will be directed to FIRE.
Even with this new split, the absolute amount of fees going towards protocol rewards will be higher than before.
Flare Governance will monitor the impact of these changes and may propose further changes in accordance to the methodology established in [FIP.11](https://proposals.flare.network/FIP/FIP_11.html) and [FIP.14](https://proposals.flare.network/FIP/FIP_14.html).

The fee increase makes sure that applications using the underlying Flare Data Infrastructure are paying sufficient fees to support the crucial maintenance done by their data providers and by `FLR` stakers who provide a crucial security layer.
The FDC system attested upwards of 25 thousand attestations before the launch of FAssets alone.
With a large (2 to 5 fold) increase in Payment attestations since the launch of FAssets and additional 10 thousand attestations in the first two weeks since the launch of Flare Smart Accounts keeping the system sustainable and secure is more crucial than ever.

### 4.2 FAssets and Flare Smart Accounts Fee Switch

The upcoming FAssets upgrade simplifies the minting process by removing the need for two transactions and allowing minting directly to the core vault.
As well as decreasing the minting time, this upgrade greatly increases the bridging capability.
Additionally, it adds a layer of extendability, allowing users to compose bridging together with immediate transaction execution within the context of the Flare Smart Accounts.
The change in minting decreases the reliance on FAsset agents and changes their role to performing redemptions only.
As the new deposit mechanism does not depend on the capital lockup from the agent side, its cost decreases.
Instead of removing the minting fee, the existing fee will be reallocated to FIRE.

With the core vault that was already introduced in the FAssets v1.2, the capital requirements on agents was greatly decreased.
Additionally, a large volume of `FXRP` bridging transactions were introduced to support the agents and collateral pool expected from expansion.
To fund support for this FAsset infrastructure a portion (10%) of redemption fee will also be redirected to the FIRE incentive pool, with the majority still being kept by the agent and participants in the collateral pool to reward their participation.

Additionally `FXRP` will allow a "shortcut" version of minting with a destination tag.
With this shortcut, the minter will be able to permanently acquire a specific destination tag on the XRPL and core vault address as the entrypoint to mint directly to their designated Flare `0x` address.
The destination tags will be available for registration directly from the FAssets system for a fixed governance controlled fee and then transferred and modified by their owners.
The registration fee will be denominated in `FLR` and will start at 1000 `FLR`.
Flare governance will be able to increase the fee as a certain range of tags is depleted, per the methodology outlined in [FIP.11](https://proposals.flare.network/FIP/FIP_11.html).
The `FLR` acquired from tag purchase will be sent to FIRE for further protocol support.

As FAssets and Flare Smart Accounts heavily depend on all the underlying Flare infrastructure and services -- FTSO, FDC, and the whole network -- the security and functionality is linked directly to the security of the network protocols.
Redirecting fees to FIRE provides a natural mechanism for these funds to be reinvested where they are most needed, ensuring the long-term security and growth of the protocols.

### 4.3 Flare Confidential Compute

The upcoming addition of FCC on the network will heavily rely on the infrastructure providers to provide administration, registration, setup, machine maintenance, and validation, alongside with continuous oversight across the whole FCC stack.
All communication between on-chain users, deployed code, and hardware will be fully automated; infrastructure providers will perform message signing and machine deposits as core components of their role.

To ensure long-term sustainability, each request, setup, and maintenance instruction issued to the FCC will incur a fee.
As with other protocols, part of the fee will be distributed directly to entities and stakers to supplement inflation, while part of it will be diverted to FIRE.

### 4.4 Capturing Maximal Extractable Value for Ecosystem's Benefit

Maximal Extractable Value (MEV) represents the potential profit available to actors who control the sequencing of transactions.
MEV is traditionally harvested by validators and their collaborators, effectively benefiting at the expense of other network participants.

Numerous initiatives have sought to mitigate MEV, or redirect its value back to users.
Largely, these efforts have had small effects.
Although there are some arguments to its benefits in decentralized systems, MEV remains an inherently suboptimal feature that, in an perfectly efficient system, would not exist.

In the current landscape, MEV revenues flow to a handful of entities with no mandate to support the underlying infrastructure.
There is little evidence that this captured value is redirected towards initiatives that support the growth of the blockchain or the ecosystem.
**Flare will turn this on its head.**

On blockchains with high economic activity, MEV is a vast source of revenue for those engaged in extracting it.
Although perfectly reliable data is difficult to source, available market indicators allow us to establish reasonable estimates for the annual value of MEV capture.

| Chain | MEV per Annum (USD) | TVL (USD) | Activity (tx/day) |
| :--- | :--- | :--- | :--- |
| Ethereum | 200M – 500M | 55B | 1.4M |
| Solana | 500M – 1B | 6.7B | 70M |
| Arbitrum | 10M – 30M | 2B | 3.5M |
| Base | 200M – 300M | 4B | 10M |

Today’s inflation rate of 5% currently has a value of about $43mm USD.
It is therefore clear from the data that of all the elements in this proposal, capturing MEV has the potential to be one of the largest, if not the largest, sources of value accrual available to the Flare network.

#### 4.4.1 Flare's Proposal for Capturing MEV

This proposal establishes a roadmap that modifies the consensus to a verifiable single builder system.
The main goal of the single builder system migration is to designate an Entity (selected and monitored by the FIRE) to be the main builder of the blocks for the Flare Network.
Initially this entity will be the Flare Foundation, but later multiple entities can compete for the designated builder spot by providing proof that their blocks are built in accordance with the network mandate.
The validator network and Flare System Protocol (FSP) entities will evolve through the transition.
The contribution and requirement of the `FLR` token will stay the same: staked and delegated `FLR` provide votepower for entities across the ecosystem both as block validators and data providers, where the role of data providers will expand and become more and more prominent.

#### 4.4.2 Block Building Roadmap

The block production upgrade will proceed in multiple stages.
Each stage will ensure that parts are thoroughly tested, stable, secure, and that the integrity as well as liveness of the network is maintained at every point.
MEV accrual and capture will already start in the initial phase, and each successive phase will allow for more extensive and faster capture as well as increase in the transparency of the block building process.

##### Stage 1: Single FIRE Builder

Stage 1 moves to a simple setup where block production still relies on validator nodes, but block building is moved to the entity capturing the MEV.

In the current consensus and block production, a designated block producer has a fixed time window to propose a new block, after which other validators can step in.
Once proposed, a block enters the gossip stage of the consensus protocol.
Notably, the validation process does not mandate a specific block-building algorithm; as such, the proposer has full discretion over transaction ordering, provided all included transactions are valid.

This proposal modifies the consensus and block proposal algorithms, as follows:

* Blocks are built by a FIRE designated builder.
* During eligible windows, validator nodes fetch blocks built by this designated entity, validate them, and gossip them to the rest of the network.
* Consensus and validation mechanisms will be modified to ensure that the block was indeed built in accordance to the new system.

The new validation process will allow a block proposer to utilize a block provided by the designated builder, while also maintaining a fallback to the existing system should the builder fail to produce a block in a timely manner.
This approach ensures that any block producer can recover and maintain network liveness even if a builder is unavailable.
To further guarantee the stability and performance of the network, block production will be directly linked to the minimal performance requirements introduced within [FIP.10](https://proposals.flare.network/FIP/FIP_10.html).

FIRE will periodically review mempool data and block building data to ensure transactions are not being censored, new proposed transactions are non malicious and extracted MEV is positive for the network.
In case of discrepancy, security concerns or unavailability, FIRE will be able to execute a temporary stop to the second cosigning allowing block producers to bypass this and return back to the old style blocks.

##### Stage 2: Decentralized Block Building via FCC Deployment

Stage 2 upgrades the single builder model from Stage 1 to a fully transparent and decentralized method of MEV capture where block building is controlled by an algorithm running inside FCC deployment, or other similar secure deployments.

In the second stage the block building algorithm will be made (partially) public, constantly audited and reviewed to ensure captured MEV is positive to the network.
Additionally, blocks will be built within FCC to ensure compliance with the FIRE MEV mandate and prevent both censoring and malicious MEV.
The existing validators will still cycle and propose blocks, while the network will move to a single builder system where the whole block building process will run inside FCC to ensure correctness.

##### Stage 3: Unified Builder-Proposer Mechanism

Stage 3 upgrades the fully transparent system to a much more efficient version, where existing validators role becomes obsolete and fully migrates to the entity part already required by the FSP.

In this stage, the validator system will move to a single builder and proposer system where the FIRE assigned entity will be both building and proposing blocks.
This will require further changes to the consensus algorithm to allow for a single block proposer designated by network governance.
The designated proposer will make sure to provide sufficient uptime and availability as this is crucial for the ecosystem and blockchain performance, with multiple geographically distributed builder and proposer servers.

The role of the existing validator nodes will change: they will no longer be producing blocks, but still remain the core security layer which ensures the correctness of the produced blocks.
Importantly, the crucial part of `FLR` tokens which provide security to the system via staking and delegation will not change.
Staked and delegated stake will continue to count towards the vote power of FSP entities, which will continue to have the same impact on the FDC, FTSO, and FCC, irrespective of changes in the block builder and proposer mechanism.

With changes implemented in this proposal, `FLR` staking will become much more prominent in its impact on security of the core Flare protocols.
Meanwhile, the impact of `WFLR` delegations (not time locked) on votepower will decrease.
As the role of validator nodes changes, the technical implementation of how exactly staking is handled, where the funds reside, and concrete staking processes will change in accordance with the consensus implementation changes.

#### 4.4.3 MEV Mandate

The core requirement of the block builder will be to insert and reorder transactions into the blocks to support network growth and resilience -- network positive MEV -- and divert the proceeds from such transactions to the FIRE Incentive Pool.
FIRE will continuously monitor the avenues producing MEV and possible MEV opportunities and require the builder to implement the capture mechanisms.
To ensure fairness and transparency, the designated builder will only engage in the following network positive forms of MEV:

* Lending protocol liquidations using atomic, cross chain, and CEX/DEX settlement.
* Atomic DEX arbitrage and cross chain CEX/DEX arbitrage.
* Just in time liquidity provisioning for large trades.
* Post trade arbitrages on DEXes deployed on the Flare Network.

Any changes related to which family of MEV capture shall be implemented will be put to vote with the [FTSO Management Group](https://proposals.flare.network/FIP/FIP_2.html) as a rejection based vote.
The MEV capture and reallocation mechanism will improve network efficiency, while also supporting network growth by directly reinvesting the captured value to token holders and protocol and capital deployment.
This is done in a way that is unique and better suited to long term sustainability compared to any existing operations in the wild.

### 4.5 Flare Income Reinvestment Entity

FIRE is an umbrella entity that will control network revenues accrued via any of the methods proposed in the current FIP and any further network revenue redirected to network growth and sustainability.

This proposal designates the following network revenues as the main contributors to the Incentive Pool:

* FDC request protocol fee: a portion of fees for all FDC requests.
* FAssets and Flare Smart Accounts protocol fees.
* Fees collected from attestations and system level messages in FCC.
* Network wide value acquired through capturing MEV.

**It is important for the reader to understand that the accrued funds will be in a mix of `FLR` and other tokens such as stablecoins, FAssets, RWA’s and others.**

The primary mandate of FIRE established by this governance vote is to reduce the supply of the Flare token as far as possible.
The secondary mandates are to encourage economic activity on the network in order to generate expanded revenues and provide long term support to the work of the Flare Foundation.

The FIRE will periodically update the usage of funds within the pool with the mandate, and will only be able to reallocate the funds within the mandate.
Depending on asset liquidity and amount, accrued funds might either be reallocated directly or firstly converted to `FLR` and then reallocated as `FLR`.
The initial use mandate of the collected funds will be:

* Used to provide rewards to `FLR` validators and stakers, replacing inflation based rewards and moving inflation below the 3% stipulated above.
  The FIRE will work closely with network governance to use pool accrued funds to reduce inflation to the maximum extent possible.
  Depending on the amount, the inflation substitution can either be provided in the accrued tokens (`USDT0`, `FXRP`, `USDC.e`) or converted in `FLR`, depending on the amount and usability.
* Used to buy `FLR` tokens on the open market and be either burned or used as funds for other purposes within the mandate of the FIRE.
* Used to reward asset issuers in relation to the activity and MEV earned on the economic activity of their issued asset.
  This gives asset issuers an inbuilt incentive to deploy economic value on Flare which does not exist elsewhere and which further promotes ecosystem activity upon which further fees and MEV can be collected.
* Used to increase the yield or liquidity available through DApps to support expanded economic activity on the network.
* Used for Flare Foundation long term sustainability in the development and support of the network as whole: supporting security, engineering, application development and deployment, ecosystem development and user initiatives that directly strengthen the Flare Network.

#### 4.5.1 Governance of FIRE

The establishment and mandate of FIRE is authorized by this FIP.
The administration of FIRE will be undertaken by the Flare Foundation who will over time put together a committee of members both internal and external to the Foundation to manage the entity.

The decentralized governance of entities that hold (or in the case of FIRE potentially hold) a large amount of funds has historically been subject to many pitfalls and many forms of egregious exploitation.
This is largely due to governance procedures generally being an activity that has very low community participation, leaving the system open to exploitation by small groups of large holders who may not have the wider communities interests or the long term interests of the project in mind.
As such the passing of this FIP will establish a negative governance structure with a high participation threshold.
This will allow the community to establish the ability to stop, this being the negative aspect, the actions of the Flare Foundation should it find those actions to be against the BROAD wishes of the community, this being the high participation aspect.
This will work as follows.

If the community at the end of the first year of operation of FIRE and each subsequent year thereafter find that the actions of the entity are to its displeasure a vote will be held to move to a joint governance structure.

This initial vote will be structured with the question: "Does the community wish to move FIRE to a joint governance structure?".
The vote will be open to all `FLR` token holders and it must pass with at least 50% of the total inflatable supply of `FLR` voting "yes".

If such a vote passes in the affirmative the community will then have the ability to elect representatives drawn from the set of infrastructure providers that jointly operate on both Flare and Songbird.
The purpose of picking only from those infrastructure providers that operate on both networks is that it is assumed that such infrastructure providers have the best interests of both the project and the community in mind.

First a vote on Songbird will take place using `SGB` to pick the top eight vote winning infrastructure providers.
These top eight infrastructure joint Flare and Songbird infrastructure providers selected using `SGB` will then be put to vote on Flare using `FLR` from which four representatives will be chosen by selecting those with the highest votes.

Once these four representatives are chosen they will then be required to approve the suggested actions of FIRE such that neither the Flare Foundation nor any committee chosen to manage FIRE can take an action without also having the consent of the community representatives.

If joint governance of FIRE is established a new vote to select community representatives will be held each year.

### 4.6 `RNat` Unvested Funds Redistribution

This proposal changes the unvested Cross Chain incentive mechanism.
Instead of being burnt, the unvested claimed fees are sent back to the Cross Chain Incentive Pool.

## 5. Network Safety and Validator Sustainability

### 5.1 Rebalance Reward Distribution to P-chain Staking

The current voting power calculation gives equal importance to P-chain staked tokens and C-chain (`WFLR`) delegations, with the exception of FTSO anchor feeds, which rely solely on C-chain delegations.
This proposal firstly aims to unify votepower calculation across all the FSP protocols on the C-chain: FDC, FTSO Anchor feeds, and FTSO block latency feeds, to rely on what is currently known as the **signing weight** -- a combination of staked and delegated tokens, as described in the [FTSOv2 whitepaper](https://dev.flare.network/support/whitepapers).
In addition to this, the proposal aims to adjust **signing weight** calculation, by assigning a higher weight to P-chain stake, initially set at **5×** relative to C-chain power.

This change would also impact the rewards distribution according to participation rate: users staking to P-chain would get much more of the rewards accrued to C-chain FSP protocols, while users delegating `WFLR` on C-chain would get a lower rate.
The higher reward rate for P-chain staking would in turn drive more `FLR` to be staked on P-chain.
As P-chain stake is much more stable and provides greater security to both FSP functions and block building, the result will strengthen the network and increase the resiliency of data protocols necessary for all products.

To allow for an increased amount of stake, the max validator node size will be increased from 200 million to 300 million `FLR`.
This proposal would not change anything else related to the P-chain part of the protocols such as: staking on P-chain, block production rules, rewards distributed for block validations which still rely only on the P-chain stake.

Flare Governance will observe the changes and impact and will prepare further proposals to change the parameters to ensure a sustainable balance between staking and delegation, in accordance to the methodology introduced in [FIP.11](https://proposals.flare.network/FIP/FIP_11.html).

### 5.2 Minimum Entity Fee Share

Sustainability of entities providing both data and block validation is crucial for the long term network growth and stability.
Entities have to run independent infrastructure and data acquisition services, provide regular updates, and actively participate in network governance.
This requires a level of professionalism and sufficient funding.
To prevent a race to the bottom and dumping strategies, a minimum 20% entity fee will be applied network wide.
This means that entities will always take 20% of the rewards for anything that was delegated or staked to them.
This is consistent with current average value and will provide sufficient incentive for entities to continue running all necessary network services and also be able to expand services as Flare Network grows and new requirements arise.

As before, this value may be subject to future changes, in accordance to [FIP.11](https://proposals.flare.network/FIP/FIP_11.html) methodology.

## 6. Link to Code Repository and Contracts

* Inflation reduction will be implemented through the [`setTimeSlotInflation`](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/master/contracts/inflation/implementation/InflationAllocation.sol?ref_type=heads#L100) method of the [`InflationAllocation`](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/master/contracts/inflation/implementation/InflationAllocation.sol) contract.
* Changes in FDC fees will be done through the [`setTypeAndSourceFees`](https://github.com/flare-foundation/flare-smart-contracts-v2/blob/main/contracts/fdc/implementation/FdcRequestFeeConfigurations.sol#L57) method of the [`FdcRequestFeeConfigurations`](https://github.com/flare-foundation/flare-smart-contracts-v2/blob/main/contracts/fdc/implementation/FdcRequestFeeConfigurations.sol) contract.
* The [`Supply`](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/master/contracts/inflation/implementation/Supply.sol) contract will be redeployed to change the calculation for the inflatable balance and add support for the FIRE pools.
  Incentive pools will be added via the [`addTokenPool`](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/master/contracts/inflation/implementation/Supply.sol?ref_type=heads#L147) method.
* The [`Inflation`](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/master/contracts/inflation/implementation/Inflation.sol) contract will be redeployed to change the annual cap.
* The [`FlareSystemsCalculator`](https://github.com/flare-foundation/flare-smart-contracts-v2/blob/main/contracts/protocol/implementation/FlareSystemsCalculator.sol) contract will need redeployment due to signing weight changes.
* The [`PChainStakeMirrorVerifier`](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/master/contracts/staking/implementation/PChainStakeMirrorVerifier.sol) contract will also require redeployment for changing the maximum stake amount for a validator node.
* In order to enforce a minimum 20% delegation fee, a redeployment of [`WNatDelegationFee`](https://github.com/flare-foundation/flare-smart-contracts-v2/blob/main/contracts/protocol/implementation/WNatDelegationFee.sol) will be needed.
* The [`rNat`](https://github.com/flare-foundation/flare-smart-contracts-v2/blob/main/contracts/rNat/implementation/RNat.sol) contract will be updated and redeployed according to the proposed changes.
* Base fee adjustments will require changes to the [ACP-176](https://github.com/flare-foundation/go-flare/blob/main/coreth/plugin/evm/upgrade/acp176/acp176.go) gas calculation parameters [Flare validator node base code](https://github.com/flare-foundation/go-flare).
* Changes in the block production mechanism will require node-level changes to the [`proposerVM`](https://github.com/flare-foundation/go-flare/tree/main/avalanchego/vms/proposervm), as well as changes in the validation logic.

## 7. Proposed Implementation Schedule

Changes will be implemented in multiple phases.
Implementation for inflation reduction, signing weight calculation changes, and minimum entity fee, as well as the creation of FIRE and redistribution of protocol fees are expected to commence shortly after the voting finishes.
Changes in transaction fee structure, validator node size increase and addition of minimum validator fee, as well as the changes in the block production mechanism will require a network hard-fork, and will be implemented in due course.
Any changes requiring direct action from infrastructure providers will be communicated with sufficient notice.

## 8. Voting Details

To pass, the proposal requires a simple majority of votes in favor of it.

## 9. Deadline for Voting

* **Notice period**: 9-April-2026 to 16-April-2026
* **Voting period**: 17-April-2026 to 24-April-2026
