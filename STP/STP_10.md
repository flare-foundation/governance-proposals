---
nav_order: 30010
title: STP.10
---

# STP.10: Add Custom Feeds to FTSO

| Type                | Songbird Test Proposal                      |
| :------------------ | :------------------------------------------ |
| Author              | Flare Foundation                            |
| Created             | 04-February-2025                            |
| Document Status     | Final                                       |
| Threshold Condition | 75% (required to reject)                    |
| Majority Condition  | 50% (required to reject)                    |

## 1. Brief Description

The Flare Time Series Oracle (FTSO) provides decentralized price feeds for the Flare network through a system of data providers who submit price data for various crypto assets.
Currently, the FTSO only supports direct price feeds for assets that maintain significant trading volume on centralized exchanges.
These feeds are supplied by Flare's network of data providers.

However, the system has limited support for custom types of data feeds such as Liquid Staked Tokens (LSTs), Liquid Restaked Tokens (LRTs), and more generally, feeds of an arbitrary time-series nature.
This proposal and its counterpart [FIP.13](../FIP/FIP_13.md) seek to extend the FTSO by implementing *Custom Feeds* - a new category of feeds that derive their values through custom logic encoded in on-chain smart contracts.

Custom Feeds will be made available through the same interface as FTSO's block-latency feeds i.e. through the long-term support (LTS) `FtsoV2Interface` contract.
Their implementation does not require any modification to the FTSO data provider client logic, nor to dApps integrating the FTSO.
Furthermore, there is no token inflation allocated to Custom Feeds.

The implementation of Custom Feeds will significantly enhance DeFi capabilities across the Flare network.
Lending and borrowing protocols will gain the ability to accurately value custom assets as collateral, while risk management systems will benefit from reliable price data for liquidation mechanisms, all of which will be served by the familiar FTSO on-chain interface.
This represents a significant expansion to the FTSO's core functionality, with substantial expansion in the amount and type of data the FTSO can support.
As such, a governance vote is required due to the proposal's far-reaching implications for the Flare network.

## 2. Technical Description

### 2.1 Overview

The implementation of Custom Feeds requires several technical modifications to the FTSO smart-contract system.
These changes are designed to maintain backward compatibility while introducing new functionality.
All modifications are visible on the public [Flare Smart Contracts GitHub](https://github.com/flare-foundation/flare-smart-contracts-v2).

Feed categories have been updated to use a composite classification system that combines a super-category with the existing feed type.
The super-category differentiates between normal and custom feeds - normal feeds are assigned values in the range 0-31 (i.e. 0x00-0x1F), while custom feeds use values in the range 32-63 (i.e. 0x20-0x3F).
Meanwhile, the existing type codes remain unchanged: 0 - none, 1 - crypto, 2 - FX, 3 - commodity, and 4 - stock.

Under this updated system, a normal crypto feed is classified as 1 (i.e. 0x01), whereas a custom crypto feed is classified as 33 (i.e. 0x21).
To ensure a consistent user and developer experience, the legacy mechanism of querying feeds using their index will be deprecated.

### 2.2 Associated Risks

Custom Feeds will have a completely distinct risk profile compared to standard FTSO feeds.
Unlike standard FTSO feeds, where every feed is backed by the entire network of data providers, the security of a Custom Feed is conditional on the logic defined in its smart contract.

This change in risk profile requires users and developers integrating a Custom Feed in their application to carefully evaluate each Custom Feed on a per-feed basis, and assess how the security of the feed, in particular the smart-contract implementation of that feed, impacts their application's security model.

### 2.3 Smart Contract Changes

#### 2.3.1 `FtsoV2Interface.sol`

The interface contract has been updated to support the new Custom Feeds functionality while removing legacy index-based methods that could create inconsistencies.

The following methods have been added:

* `getFtsoProtocolId`: Returns anchor feeds protocol ID.
* `getSupportedFeedIds`: Returns a list of supported Feed IDs.
* `getFeedIdChanges`: Returns a list of Feed ID changes.
* `calculateFeeById`: Returns the fee for fetching a feed.
* `calculateFeeByIds`: Returns the fee for fetching multiple feeds.

Several legacy methods have been deprecated to prevent potential inconsistencies with the new category of feeds:

* `getFeedByIndex`.
* `getFeedsByIndex`.
* `getFeedByIndexInWei`.
* `getFeedsByIndexInWei`.
* `getFeedIndex`.
* `getFeedId`.

Moving forward the only way to query FTSO feeds will be using their Feed ID.
For a list of all FTSO Feed IDs refer to the [Flare Developer Hub](https://dev.flare.network/ftso/feeds).

#### 2.3.2 `FtsoV2.sol`

The core FTSO contract has been enhanced to support Custom Feeds, alongside the following Governance methods being added:

* `addCustomFeeds`: Method to add new custom feed contracts.
* `replaceCustomFeeds`: Method to replace custom feed contracts.
* `removeCustomFeeds`: Method to remove custom feed contracts.

#### 2.3.3 `IICustomFeed.sol`

A new internal interface has been introduced to standardize the implementation of new Custom Feeds.
An example of an implementation using this internal interface is given in the next section using Sceptre Staked Flare.

### 2.4 Example Implementation

An example implementation of a custom feed is available in the `SFlrCustomFeed.sol` contract for [Sceptre Staked Flare](https://www.sceptre.fi/) (sFLR).

### 2.5 Process to add new Custom Feeds

A request to add a new Custom Feeds can be made by raising a [New Feed Request Issue](https://github.com/flare-foundation/developer-hub/issues/new?template=feed_request.yml) on the [Flare Developer Hub GitHub](https://github.com/flare-foundation/developer-hub), explaining the business justification for the request alongside a link to the verified contract code of the Custom Feed on the Flare explorer.
Once a request is raised, the Flare Foundation will determine eligibility after reviewing input from the data providers' [Management Group](./STP_3.html) (MG).
If significant concerns are raised, the request will be submitted for a vote within the MG group.

## 3. Links to Code Repositories and Contracts

* [Flare Smart Contracts](https://github.com/flare-foundation/flare-smart-contracts-v2)
* [`FtsoV2.sol`](https://github.com/flare-foundation/flare-smart-contracts-v2/blob/v1.1.0-rc.0/contracts/protocol/implementation/FtsoV2.sol)
* [`FtsoV2Interface.sol`](https://github.com/flare-foundation/flare-smart-contracts-v2/blob/v1.1.0-rc.0/contracts/userInterfaces/LTS/FtsoV2Interface.sol)
* [`IICustomFeed.sol`](https://github.com/flare-foundation/flare-smart-contracts-v2/blob/v1.1.0-rc.0/contracts/customFeeds/interface/IICustomFeed.sol)
* [`SFlrCustomFeed.sol`](https://github.com/flare-foundation/flare-smart-contracts-v2/blob/v1.1.0-rc.0/contracts/customFeeds/implementation/SFlrCustomFeed.sol)

## 4. Proposed Implementation Date Range

Implementation is expected to start shortly after the voting finishes.

## 5. Voting Details

An STP is rejection-based, meaning that it is accepted unless certain conditions are met, namely, that the quorum threshold (75%) is reached and at least half of the votes are against it.
(See [Governance](https://docs.flare.network/tech/governance/#stps).)

## 6. Deadline for Voting

* **Notice period**: 11-February-2025 to 16-February-2025
* **Voting period**: 17-February-2025 to 23-February-2025
