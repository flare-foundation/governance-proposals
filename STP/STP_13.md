---
nav_order: 30013
title: STP.13
---

# STP.13: Introduce Flare Confidential Compute

| Type                | Songbird Test Proposal                      |
| :------------------ | :------------------------------------------ |
| Author              | Flare Foundation                            |
| Created             | 29-Jun-2026                                 |
| Document Status     | Final                                       |
| Threshold Condition | 75% (required to reject) 3.68% (obtained)   |
| Majority Condition  | 50% (required to reject) 12.43% (obtained)  |
| Voting Outcome      | [**Accepted**][ProposalLink] on 12-Jul-2026 |

[ProposalLink]: https://portal.flare.network/proposal/view/99482489352199035902583237938299854023464286358415863587049119819700851628581?chainId=19

## 1. Brief Description

This proposal introduces Flare Confidential Compute (FCC) to Songbird, extending the network by providing its users with on-chain access to Trusted Execution Environments (TEEs), cloud-based confidential VMs that attest to their code and state.
The proposed deployment covers: the Songbird smart contracts facilitating the FCC and the system extension hosting initial applications.
There are two initial applications on the system extension:

* The Flare Data Connector v2 (FDC-V2), an upgrade to the pre-existing [FDC](https://proposals.flare.network/STP/STP_9.html) already deployed on Songbird.
* Protocol Managed Wallets (PMWs), applications on Songbird that allow users to create and manage addresses on other blockchains through interactions with FCC smart contracts on Songbird.

Both applications leverage the key design feature of the FCC: on-chain contracts receive instructions, with Songbird’s data providers handling off-chain relaying between these contracts and participating TEEs.
The FCC also grants Songbird users access to build with this infrastructure, designing their own applications leveraging on-chain access to TEEs.

The FCC architecture in this proposal launches without system or inflation-based rewarding.
Instead, participating providers will be compensated from a participation pool seeded by the Flare Foundation until a permanent rewarding system is developed.
The objective here is to reach a critical mass of data providers running the off-chain infrastructure necessary to support the FCC.
Similarly, at launch TEE machines registered to the FCC are deployed solely on Google Confidential Compute and by the Flare Foundation, and used only for system extension applications; mechanisms for user-deployed TEE machines, and user-defined applications, are available but not utilized at launch.

Consistent with Songbird's canary role, FCC is deployed without a final audit; users should size their usage accordingly.
A governance vote is required due to the new on-chain infrastructure and data provider responsibilities.

## 2. Technical Description

### 2.1 Overview

The FCC is organized around three main components:

1. Smart contracts on Songbird govern TEE machine registration, receive user instructions, and handle verification of results.
2. Data providers operate the off-chain infrastructure that carries instructions from Songbird to the TEE machines. Each participating data provider runs a relay client to ferry information from Songbird to the TEEs and FDC-V2 verifier servers to support attestations.
3. TEE machines execute the code corresponding to FCC extensions inside confidential virtual machines and return signed results.

**Extensions.** FCC instructions are managed via a system of extensions.
An FCC extension is a combination of a smart contract on Flare, a list of supported code versions, and a dedicated set of TEE machines that execute the code.
Users interact with an extension by calling its instruction-sender contract.
Data providers then relay the instruction to participating TEE machines.
Signed results, for example signed FDC-V2 attestation responses, are made available at the TEE machines for consumers to pick up, including for on-chain submission; the protocol itself does not publish them back to Songbird.

**System and Custom Extensions.** The FCC infrastructure introduced in this framework should be understood in two parts:

* The system extension and its applications provide Songbird users access to new commands that make use of TEEs to perform specific tasks.
* The FCC support for user-defined extensions provides a framework for users to build custom TEE-based applications on Songbird.
  To create an extension, the user must provide its components: a smart contract that receives user instructions from a list of supported commands, a set of registered TEE machines, and a set of supported code versions.
  Once these are provided, the extension is available for use.

**Role of the Data Providers.** Songbird’s data providers play a crucial role in securing and facilitating the FCC.
Users submit instructions to the TEE machines through the FCC smart contracts on Songbird, with the data providers responsible for relaying these commands to the TEE machines.
In turn, the TEE machines wait until they have received an instruction from a weighted majority of the data providers before execution, a process known as voting.
Data providers include their signature when relaying instructions: in this way, consensus among Songbird’s data providers ensures that only legitimate commands are executed by the TEEs.

**Augmentation and Off-chain Data.** For many instructions, the relay step performed by the data providers will also include augmentation: the user instruction issued on Songbird may specify additional information that is required by the TEE machines for execution.
The data providers are responsible for this augmentation, packaging the user instruction together with additional information before submitting the signed package to the TEE machines.
This allows FCC instructions to incorporate off-chain data: augmentation is not limited to on-chain information and can include arbitrary sources.
For example, FDC-V2 attestation requests require data providers to include proofs of validity of the request in their submissions to the TEEs.

**FDC-V2.** The [Flare Data Connector (FDC)](https://proposals.flare.network/STP/STP_9.html) is Songbird’s enshrined protocol for validating the existence of external events on Songbird.
The FDC functions through attestation requests, user requests for specified events to be confirmed on Songbird.
Requests are checked by data providers off-chain, in a sequence of rounds lasting approximately 90 seconds: the valid requests received each round are packaged together into a single Merkle root, signed by the data providers and published on-chain.

The FDC-V2 removes the latency induced by the system of rounds by handling each request individually using FCC infrastructure: attestation requests are submitted to the system extension smart contract, with data providers responsible for validating the request and submitting proof to the TEE machines.
Once a TEE machine receives enough signed confirmations, it signs the attestation response; users can then fetch the signed response from the TEE and submit it on Songbird.
The FDC-V2 is introduced with a set of attestation types that facilitate the off-chain parts of the FCC.

**Protocol Managed Wallets.** A PMW is an application running on Songbird that manages a wallet address on an external blockchain.
PMWs allow owners on Songbird to create addresses and submit transactions on external chains by interacting only with FCC smart contracts.
Keys for the address are generated and stored securely inside TEE machines registered to the system extension.
These keys are only accessed when the PMW owner on Songbird submits an instruction for the TEE machine to sign a transaction (or other account action): data providers relay the transaction instructions to the TEE machines, who sign only when they receive signatures from a weighted majority of data providers.

Thus, PMW accounts are secured by a combination of Songbird’s data providers and the secure properties of the TEE machines.
Safety conscious users have the option of adding cosigners to PMW accounts for an additional layer of security, with TEE machines then requiring signatures from both data providers and cosigners before signing a transaction.

### 2.2 Scope of the Initial Songbird Deployment

The on-chain deployment in this proposal includes the FCC smart contract system and the system extension hosting the FDC-V2 and PMW applications.
The FDC-V2 launches with four attestation types covering TEE availability and PMW payment verification.
The PMW architecture is initially configured to support only XRPL accounts.

Although no custom extensions are registered at launch, the infrastructure to build and register them are available from day one.
The full list of smart contracts and their commands are included in the `flare-smart-contracts-v2` repository referenced in [Section 3](#3-link-to-code-repositories-and-contracts).

### 2.3 Participants and Onboarding

FCC relies on two types of participants on Songbird at launch.

* **Data providers:** Participation in the FCC by Songbird data providers is atomic: it does not affect vote power or rewarding in other Songbird protocols.
  To take part, a data provider must deploy a single relay client and an FDC-V2 verifier server per supported attestation type, then publish the corresponding endpoint information through the existing provider metadata channel.
* **TEE operators:** In this initial phase, the Flare Foundation will be the only TEE operator for the FCC.
  Once the deployment is more mature, other entities will be able to register to operate TEE machines and receive rewards for doing so.

Only data providers are compensated in the initial phase, with rewards deriving from a participation pool provided by the Flare Foundation as explained in [Section 2.5](#25-participation-pool).
A detailed and sustainable rewarding structure that covers data providers and TEE operators is deferred to a follow-up proposal that will be informed by Songbird operational data.

### 2.4 FDC-V2 Attestation-Type Fees

The FDC-V2 supports dedicated attestation types that enable the FCC infrastructure.
This proposal sets the initial Songbird base fees for FDC-V2 attestation requests as follows:

| Attestation Type               | Proposed Base Cost (SGB)  |
| :----------------------------- | :------------------------ |
| `TeeAvailabilityCheck`         | 3                         |
| `PMWPaymentStatus`             | 20                        |
| `PMWMultisigAccountConfigured` | 20                        |
| `PMWFeeProof`                  | 50                        |

Values are proposed and may be adjusted prior to finalization; subsequent changes follow the standard governance path.
Fees accrued from FDC-V2 attestations will be added to the participation pool and distributed to participating data providers.

### 2.5 Participation Pool

Songbird’s data providers will be responsible for relaying information between FCC smart contracts on-chain and the TEE machines off-chain.
They are rewarded for fulfilling this duty directly from the participation pool.
This is a pool of SGB tokens funded from two sources: an initial Flare Foundation allocation, proposed at 40,000,000 SGB, and the fees accrued from successful FDC-V2 attestations during the bootstrap period.

During the initial months of the FCC, the Flare Foundation will check participation by data providers, monitoring how often they relay signed instructions to the TEE machines.
Each calendar month, a data provider whose participation rate meets or exceeds a predetermined threshold will receive a fixed grant, proposed at 100,000 SGB per data provider per month.
Providers below the threshold receive no grant that month.
The community is encouraged to operate independent monitoring of provider participation as a cross-check.

The pool is time-boxed.
The bootstrap period ends when either the pool is exhausted, the bootstrap duration elapses, or a follow-up SIP/STP establishes a permanent rewarding regime, whichever happens first.
Once the bootstrap period ends, a formal rewarding structure will be put in place in line with standard Songbird practices, including performance criteria for rewarding and minimal conditions for participation.

### 2.6 Associated Risks

* **Data Provider Participation:** FCC liveness depends on sufficient data provider coverage of relay clients and FDC-V2 verifier servers, infrastructure that data providers will need to manage.
* **Trust in the TEE platform:** At launch, TEEs registered to the FCC are hosted only by Google Confidential Compute.
  The FCC relies on the safety and liveness of these TEEs.
* **Application-specific Risk of PMW**: Certain PMW operations depends on correct functioning of the XRPL chain, which is outside the control of PMW participants.
* **FDC-V2 Fee Calibration:** Initial attestation type base fees are set without Songbird-specific empirical data, and may need to be revised through governance.
* **Deployment Without Final Audit:** The initial deployment of the FCC on Songbird proposed here will proceed while auditing is on-going.

## 3. Link to Code Repositories and Contracts

* [Smart contracts](https://github.com/flare-foundation/flare-smart-contracts-v2): branch tee-diamond-cut, paths `contracts/tee/` and `contracts/FDC-V2/`.
* [FCC specification](https://github.com/flare-foundation/flare-specs): path `src/FlareTEE/`.
* [TEE node](https://github.com/flare-foundation/tee-node)
* [TEE proxy](https://github.com/flare-foundation/tee-proxy)
* [Relay client](https://github.com/flare-foundation/tee-relay-client)
* [FDC-V2 verifier API](https://github.com/flare-foundation/go-verifier-api)
* [Shared libraries](https://github.com/flare-foundation/go-flare-common): TEE packages under `pkg/tee/`.

## 4. Auditing

If this proposal is accepted, the FCC will be deployed on Songbird without a final audit.
Interim audit reports produced during the bootstrap period will be published as they are completed.

## 5. Proposed Implementation Schedule

Implementation of the FCC is targeted for July 2026, shortly after acceptance, proceeding in two phases:

* **Trial:** Initial deployment of FCC contracts and activation of the system extension. The purpose of this phase is for data providers to validate their set up; thus, user instructions will be disabled and no rewarding will be in place.
* **Bootstrap:** In this phase, user instructions are enabled, the FDC-V2 fees are introduced, and the participation pool rewarding begins.

The bootstrap period will require participation from at least a weighted majority of Songbird’s data providers.

## 6. Voting Details

An STP is rejection-based, meaning that it is accepted unless certain conditions are met, namely, that the quorum threshold (75%) is reached and at least half of the votes are against it.
(See [Governance](https://dev.flare.network/#stps).)

## 7. Deadline for Voting

* **Notice period**: 29-June-2026 to 05-July-2026
* **Voting period**: 06-July-2026 to 12-July-2026
