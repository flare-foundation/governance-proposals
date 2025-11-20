---
nav_order: 30011
title: STP.11
---

# STP.11: Add Support for FDC Web2 Attestations

| Type                | Songbird Test Proposal                      |
| :------------------ | :------------------------------------------ |
| Author              | Flare Foundation                            |
| Created             | 20-November-2025                            |
| Document Status     | Draft                                       |
| Threshold Condition | 75% (required to reject)                    |
| Majority Condition  | 50% (required to reject)                    |

## 1. Brief Description

This proposal introduces a new FDC attestation type - `Web2Json` - and defines a standardized process for adding, updating, or removing new attestation sources (API endpoints).
While [STP.09](./STP_9.md) allows new attestation types to be added through approval from the [Management Group](./STP_3.md), this proposal specifies how Web2 attestation sources are governed under that framework.
It also establishes a default attestation fee mechanism, with any future adjustments to the fee subject to approval by the [Management Group](./STP_3.md).

As part of introducing this new attestation type, an initial set of Web2 attestation sources (API endpoints) will be included in this proposal.

## 2. Motivation

* Enable transparent and user-driven inclusion of new API endpoints into the FDC `Web2Json` attestation system.
* Ensure legal and technical vetting by the Flare Foundation for each request.
* Preserve decentralization and oversight by requiring the [Management Group](./STP_3.md) to vote on both endpoint inclusion/removal and attestation fee adjustments.

## 2. Technical Description

### 3.1 Process of Adding, Updating, or Removing an API Endpoint

Changes to FDC attestation sources will follow the process below:

1. **Pull Request Submission**
    * Users submit pull requests with desired endpoint changes according to instructions set by the Flare Foundation in the [verifier-indexer-api](https://github.com/flare-foundation/verifier-indexer-api/blob/main/CONTRIBUTING.md#proposing-web2json-attestation-type-source-changes) repository.

2. **Flare Foundation Review**
    * The foundation reviews submissions to ensure legality, relevance, and technical feasibility.
    * Requests failing legal or technical validation are rejected with clear feedback.
    * Approved requests proceed to the next stage.

3. **Management Group Voting**
    * This is similar to the workflow described in [STP.09](./STP_9.md), where new attestation types are submitted, reviewed by the Foundation, and then voted on by the [Management Group](./STP_3.md).
    * The Flare Foundation forwards vetted requests to the Management Group forum for formal voting.
    * **Voting conditions**: (Acceptance-based)
        * **Quorum**: at least 66% of Management Group members must cast a vote.
        * **Pass threshold**: >50% in favor.
    * Upon approval, the API endpoint is added, updated or removed per the user’s request.

### 3.2 Attestation Fee Structure

The attestation fee will be determined based on specific source requests.
The default fee is set to 1 SGB.
This fee is required from users to prioritize and process the attestation.

**Fee adjustments** (increases or decreases):

* May be proposed by users when a request for a new endpoint is made.
* May be proposed by any Management Group member to increase or decrease this fee by initiating a discussion on [Discourse](https://forum.flare.network/) which will then be put to a vote.
* Are subject to the same quorum and majority voting thresholds outlined above.

### 3.3  Inclusion of new Web2 attestation source

This proposal introduces a new Web2 attestation source requested by Ignite Markets, a prediction market utilizing Web2 API data for settling outcomes such as sporting events.
The addition of the FDC `Web2Json` attestation type significantly expands this capability, enabling countless additional data-rich use cases through the provision of decentralized consensus on the outcome of such events.

Details of this new attestation source can be found on the [verifier-indexer-api](https://github.com/flare-foundation/verifier-indexer-api/blob/main/src/config/web2/web2-json-sources.ts#L9) Github Repository.
The attestation fee for this source will be set to 100 SGB.

## 4. Proposed Implementation Date Range

This proposal includes the implementation of the following parts after the voting finishes.

1. FDC `Web2Json` Attestation Type: expected to start shortly after the voting finishes, providing sufficient time for data providers to update and redeploy their infrastructure.
2. Inclusion of a new `Web2Json` Attestation Source: implemented along with the new attestation type mentioned above.

New FDC attestation sources will be implemented in accordance with the steps mentioned in [Section 3.1](#31-process-of-adding-updating-or-removing-an-api-endpoint) above.

## 5. Voting Details

An STP is rejection-based, meaning that it is accepted unless certain conditions are met, namely, that the quorum threshold (75%) is reached and at least half of the votes are against it.
(See [Governance](https://docs.flare.network/tech/governance/#stps).)

## 6. Deadline for Voting

* **Notice period**: 20-November-2025 to 27-November-2025
* **Voting period**: 28-November-2025 to 5-December-2025
