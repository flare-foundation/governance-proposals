---
nav_order: 10012
title: FIP.12
---

# FIP.12: Add Support for the Flare Data Connector

| Type               | Flare Improvement Proposal                  |
| :----------------- | :------------------------------------------ |
| Author             | Flare Foundation                            |
| Created            | 22-Jan-2025                                 |
| Document Status    | Final                                       |
| Majority Condition | 50% (required)                              |



## 1. Brief Description


This proposal introduces the Flare Data Connector (FDC), a protocol that will allow applications to securely import external events onto the Flare networks.
The introduction of the FDC will strengthen Flare's position as the blockchain for data.
The goals of the proposal are to:

* Introduce the FDC protocol as an evolution and replacement of the State Connector.
* Add the FDC protocol contracts to the Flare network.
* Assign part of the network's yearly inflation to rewarding the FDC protocol to incentivise provider participation.
* In its initial deployment, the FDC protocol will include support for attestation types required for the FAsset system, as well as EVM Transaction type.

The rewarding structure, minimum participation requirements, and implementation details are described in the next section.
The FDC protocol is also described in more technical terms in the [whitepaper](https://flare.network/whitepapers/).

## 2. Technical Description

The goal of the FDC is to improve the range of available data in a trustless manner - trust will be derived from the underlying security assumptions of the Flare network.
To this end, the FDC is a request based system that allows users to request attestations of external data be published on the Flare networks.
This process is secured by Flare's data providers, so that when the user requires this data for e.g. a smart contract, interested parties can trust that the data is genuine. 

The introduction of the FDC is needed on Flare to facilitate importing external data on to the network, solving two key problems:

* Trusting the data - the FDC is secured by Flare's providers, and thus can be trusted within the setting of the Flare network.
* Filtering for useful data - the FDC is request based, so only data that is desired by a user is attested to, rather than wasteful monitoring or importing of all data from given sources.

The FDC protocol runs in voting epochs, synced with the FTSOv2 protocol.
Each round of the FDC is roughly split into:

* **Collect phase:** the period during which users submit requests for the round.
Note that a new collect phase starts immediately after the previous round's collect phase ends, ensuring that requests can be sent at all times.
* **Choose phase:** during this period the data providers collate the requests and determine which  ones can be confirmed in the current round. 
* **Resolution phase:** the set of confirmed attestation requests is published on-chain as a Merkle root, if enough data providers support the outcome.

The events represented by a published Merkle root are then considered true according to consensus on the network.

### 2.1 Attestation Types

In order to be correctly handled by the FDC, each attestation request must conform to one of the predefined attestation types, which specify the data source and response method for requests of the chosen type.
Well-defined response methods must specify the type of information that is sufficient to prove the veracity of the attested event, which will be included in the attestation response. 


To begin with, the attestation types that will be supported by the FDC are chosen in preparation for the FAsset system, namely:

* `AddressValidity`: verify the validity of an address format on an external chain.
* `BalanceDecreasingTransaction`: verify a transaction that either decreases the balance of a given address, or is signed by the source address. 
This attestation could prove a violation of an agreement.
* `ConfirmedBlockHeightExists`: an assertion that a given block at specified height is confirmed on a specified chain.
* `Payment`: a relay of a transaction on an external chain that is considered a payment in a native currency.
* `ReferencedPaymentNonexistence`: an assertion that an agreed-upon payment has not been made by a certain deadline.
* `EVMTransaction`: a relay of a transaction from an EVM chain together with a part of its receipt.

The `EVMTransaction` attestation type will support `ETH`, `FLR`, and `SGB` source chains, while the remaining attestation types will be supported on `BTC`, `DOGE`, and `XRP`. 


#### 2.1.1 Process to Add new Attestation Types


The community can propose new attestation types, which would be accepted if they gain the support of the majority of the data providers.
The process is as follows:

1. A request to include a new attestation type can be made on Flare's Discord server, in the `Discuss-Ideas` developer channel, which should include:
    1. A set of rules that precisely define the new attestation type.
    2. Motivation and potential software requirements (verifier server).
    3. A proposal for the base fee of such attestation requests. 
2. The Flare Foundation will forward the request to the [Management Group's forum](https://proposals.flare.network/FIP/FIP_2.html) and request a vote.
3. If the majority of data providers support the new type and the base fee, the requested attestation type will be added to the system.

A more formal process for adding new attestation types will be introduced in a future proposal.

### 2.2 Rewarding Structure

FDC rewards will be provided for voting on and attesting to requests, with providers being rewarded relative to their total active weight.
Rewards for the FDC will come from two sources. 
The first of those is the network's yearly inflation, previously used to reward data providers for their participation in the FTSO protocol. 
This will be modified in the following manner:

| Protocol          		     | Proposed Inflationary Share    |
| :----------------------------- | :----------------------------- |
| Staking       			     | 30%         		              |
| FTSOv2       			         | 35%         		              |
| FDC 				             | 35%				              |

The secondary source of rewards comes directly from the users of the FDC protocol.
Each attestation request has an individual reward offer provided by the users in the form of a fee.
Requests with higher fees will be prioritized by the system, being thus more likely to be attested by the data providers.
These fees will be distributed among the data providers that responded to the requests.

### 2.3 Minimal Requirements

In line with [FIP.10](../FIP/FIP_10.md), the FDC protocol will introduce certain minimal participation requirements for the data providers.
If these requirements are not met in a given reward epoch, a data provider may not be eligible for rewards in any of the network protocols, as described in the [FIP.10](../FIP/FIP_10.md) proposal.

Successful participation in a voting round requires getting rewards for that round (participating in correct Merkle root voting).
Initially, the minimal requirements for a reward epoch will be defined as:

* Successful participation in 60% of all voting rounds in that reward epoch.

This percentage will gradually increase in the following months up to 80%, unless the requirement is deemed too restrictive.

### 2.3 Parameter Adjustments

In line with [FIP.11](../FIP/FIP_11.md), this proposal will also allow for future adjustments to the FDC protocol parameters.
The Flare Foundation will directly communicate these changes to the data providers in a timely manner, before they are submitted for a vote within the [Management Group](https://proposals.flare.network/FIP/FIP_2.html).
These changes include:

* Share of inflationary rewards.
* Optimizations in reward distribution calculation, including reward split between signing and finalization, as well as the fraction of rewards burnt for providers who did not bit vote but did sign the correct Merkle root.
* The minimal fee required for an attestation request.
* Improvements to the bitvoting process.
* Duration of grace periods.



### 2.4 Wider Ecosystem Implications

The FDC protocol offers a versatile framework for applications that require external data from both on-chain and off-chain sources, with far reaching implications for the Flare ecosystem.
The FDC protocol will be a key part of the FAssets system, which will allow non-smart contract tokens to be used trustlessly with smart contracts within the Flare networks.

The FDC protocol recently demonstrated its capabilities in a [prediction market dApp](https://oipredict.xyz/about/) for verifying real-world events during the 2024 Summer Olympics.
Note that while the necessary attestation type for this type of applications will not be initially supported, they could be added in the future as previously described.


### 2.5 Deployment Strategy

Enabling the FDC protocol does not require a hard fork of the network.
Instead, the new contracts listed in the next section will be deployed, and the inflation allocation updated as previously described.

During the 90 days following the deployment, the Flare Foundation may change some of the system's parameters to fine-tune it.


## 3. Links to Code Repositories and Contracts

* [FDC client](https://github.com/flare-foundation/fdc-client)
* [Flare system client](https://github.com/flare-foundation/flare-system-client)
* [Smart contracts](https://github.com/flare-foundation/flare-smart-contracts-v2)


## 4. Proposed Implementation Date Range

Implementation is expected to start shortly after the voting finishes.
The exact date will be announced in time for FTSO data providers to prepare.


## 5. Voting Details

To pass, the proposal requires a simple majority of votes in favor of it.


## 6. Deadline for Voting

* **Notice period**: 24-January-2025 to 27-January-2025
* **Voting period**: 28-January-2025 to 03-February-2025
