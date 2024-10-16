---
nav_order: 30009
title: STP.09
---

# STP.09: Add Support for the Flare Data Connector

| Type                | Songbird Test Proposal                      |
| :------------------ | :------------------------------------------ |
| Author              | Flare Foundation                            |
| Created             | 16-October-2024                             |
| Document Status     | Draft                                       |
| Threshold Condition | 75% (required to reject)		    |
| Majority Condition  | 50% (required to reject) 		    |


## 1. Brief Description

This proposal introduces the Flare Data Connector (FDC), a protocol that will allow applications to securely import external events onto the Flare networks.
The introduction of the FDC will strengthen Flare’s position as the blockchain for data.
The goals of the proposal are to:

* Introduce the FDC protocol as an evolution and replacement of the State Connector.
* Add the FDC protocol contracts to the Songbird network.
* Assign part of the inflation to incentivise provider participation in the FDC.
* Initially have the FDC support attestation types required for the FAsset system, as well as EVM Transaction types.

The rewarding structure, minimum participation requirements, and implementation details are described in the next section.
The FDC protocol is also described in more technical terms in the whitepaper.

## 2. Technical Description

The goal of the FDC is to improve the range of available data in a trustless manner - trust will be derived from the underlying security assumptions of the Flare (or in this case, Songbird) network.
To this end, the FDC is a request based system that allows users to request attestations of external data be published on the Flare networks. 
This process is secured by Flare’s data providers, so that when the user requires this data for e.g. a smart contract, interested parties can trust that the data is genuine.

As such, the introduction of the FDC is needed on Flare to facilitate importing external data on to the network, solving two key problems: 

* Trusting the data - the FDC is secured by Flare’s providers, and thus can be trusted within the setting of the Flare (or Songbird) network.
* Filtering for useful data - the FDC is request based, so only data that is desired by a user is attested to, rather than wasteful monitoring or importing of all data from given sources.

The FDC protocol runs in voting epochs, synced with the FTSOv2 protocol.
Each round of the FDC is roughly split into:

* **Collect phase:** the period during which users submit requests for the round.
* **Choose phase:** during this period the data providers collate the requests, determine which ones are to be handled this round, and attest to the outcome of each of them.
* **Resolution phase:** the set of confirmed attestation requests is published on-chain as a Merkle root, if enough data providers support the outcome.

The events contained in a published Merkle root are then considered true according to consensus on the network.


### 2.1 Attestation Types

In order to be correctly handled by the FDC, each attestation request must conform to one of the predefined attestation types, which define the data source and response method for requests of the chosen type.
Well-defined response methods must specify the type of information that is sufficient to prove the veracity of the attested event, which will be included in the attestation response.

To begin with, the attestation types that will be supported by the FDC are in preparation to the FAsset system, namely:

* `AddressValidity`: for verifying the validity of an address on an external chain.
* `BalanceDecreasingTransaction`
* `ConfirmedBlockHeightExists`
* `Payment`
* `ReferencedPaymentNonexistence`
* `EVMTransaction`

These attestation types will use as source one of the following blockchains: `BTC`, `DOGE`, `XRP`, `ETH`, `FLR`, and `SGB`.


#### 2.1.1 Process to Add new Attestation Types


The community can propose new attestation types, which would be accepted if they gain the support of the majority of the data providers. The process is as follows:

1. A request to include a new attestation type can be made on Flare’s Discord server, in the `Discuss-Ideas` developer channel, which should include:
    1. A set of rules that precisely define the new attestation type.
    2. Motivation and potential software requirements (verifier server).
    3. A proposal for the base fee of such attestation requests. 
2. The Flare Foundation will forward the request to the [Management Group's forum](./STP_3.md#3-link-to-code-repository) and request a vote.
3. If the majority of data providers support the new type and the base fee, the requested attestation type will be added to the system.


### 2.2 Rewarding Structure

FDC rewards will be provided for voting on and attesting to requests, with providers being rewarded relative to their total active weight. Rewards for the FDC will come from two sources. First, the yearly inflation used to reward data providers for their participation in the FTSO protocol will be modified in the following manner:

| Protocol          		 | Proposed Inflationary Share    |
| :----------------------------- | :----------------------------- |
| FTSO        			 | 50%         		          |
| FDC 				 | 50%				  |

The FDC protocol will include an additional source for rewarding.
Each attestation request has an individual reward offer provided by the user in the form of a fee.
Requests with higher fees will be prioritized by the system, being thus more likely to be attested by the data providers.

### 2.3 Minimal Requirements

In line with [SIP.04](../SIP/SIP_4.md), the FDC protocol will introduce certain minimal participation requirements for the data providers.
If these requirements are not met in a given reward epoch, data providers may not be eligible for rewards in any of the network protocols, as described in the [SIP.04](../SIP/SIP_4.md) proposal.

Successful participation in a voting round implies getting rewards for that round (participating in correct Merkle root voting).
The minimal requirements for a reward epoch are defined as:

* Successful participation in 80% of all voting rounds in that reward epoch.


### 2.5 Deployment Strategy

Enabling the FDC protocol does not require a hard fork of the network.
Instead, the new contracts listed in the next section will be deployed, and the inflation allocation updated as previously described.

During the 90 days following the deployment, the Flare Foundation may change some of the system’s parameters to fine-tune it.


## 3. Links to Code Repositories and Contracts



## 4. Proposed Implementation Date Range

Implementation is expected to start shortly after the voting finishes. The exact date will be announced in time for FTSO data providers to prepare.

## 5. Voting Details

An STP is rejection-based, meaning that it is accepted unless certain conditions are met, namely, that the quorum threshold (75%) is reached and at least half of the votes are against it. (See [Governance](https://docs.flare.network/tech/governance/#stps).)

Before voting, data providers should be aware that due to the minimal requirements imposed above, as per [SIP.04](../SIP/SIP_4.md), they will have to handle more tasks as part of their duties on Songbird.
In particular, the FDC requires verification infrastructure (such as, but not limited to, nodes on external chains) not explicitly required in the past.
Nevertheless, the additional FDC rewarding mechanism is expected to cover these infrastructure costs.


## 6. Deadline for Voting

* **Notice period**: 17-October-2024 to 23-October-2024
* **Voting period**: 24-October-2024 to 30-October-2024
