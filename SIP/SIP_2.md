---
nav_order: 20002
title: SIP.02
---

# SIP.02: Calibrate the State Connector's performance and security

| Type               | Songbird Improvement Proposal |
| :----------------- | :---------------------------- |
| Author             | Flare Foundation              |
| Created            | 14-Feb-2024                   |
| Document Status    | Draft                         |
| Majority Condition | 50% (required)                |

## 1. Brief Description

The Flare Foundation will implement code changes in the State Connector attestation provider code base that:

* Strengthen the security and stability of the State Connector protocol
* Simplify the on-chain and off-chain interactions with the State Connector by using request and response encodings that align with the [Solidity Contract ABI Specification](https://docs.soliditylang.org/en/latest/abi-spec.html)
* Simplify the existing attestation types, which in turn will result in simplified interaction with the FAssets system
* Add two new attestation types: `AddressValidity` and `EVMTransaction`

If this proposal is accepted, the above changes will be executed, which are described in more detail in the [Songbird State Connector protocol repository](https://github.com/flare-foundation/songbird-state-connector-protocol).

## 2. Link to Code Repository

These are the repository branches containing the code changes to be deployed:

* [Multichain client](https://github.com/flare-foundation/multi-chain-client/tree/SIP.02)
* [EVM verifier](https://github.com/flare-foundation/evm-verifier/tree/SIP.02)
* [Doge indexer](https://github.com/flare-foundation/doge-indexer/tree/SIP.02)
* [Attestation client](https://github.com/flare-foundation/attestation-client/tree/SIP.02)
* [Songbird State Connector protocol reference](https://github.com/flare-foundation/songbird-state-connector-protocol)

## 3. Proposed Implementation Date Range

Deployment of the new attestation provider code on the Songbird network is expected to start shortly after the voting finishes and be completed quickly.

## 4. Voting Details

To be accepted, the proposal requires a simple majority of votes in favor of it.

## 5. Deadline for Voting

* **Notice period**: 20-Feb-2024 to 24-Feb-2024
* **Voting period**: 27-Feb-2024 to 5-Mar-2024
