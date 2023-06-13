---
nav_order: 30000
title: STP.00
---

# STP.00: STP Purpose and Guidelines

| Type               | Songbird Test Proposal  |
| :----------------- | :---------------------- |
| Author             | Flare Foundation        |
| Created            | 21-Nov-2021             |
| Revision           | 2                       |
| Status             | Final                   |
| Songbird Timeline  | 4-Nov-2022 Deployment started on Coston.<br> 29-Nov-2022 Deployment started on Songbird.<br>29-Nov-2022 Deployment complete. |
| Flare Timeline     | 29-Nov-2022 Deployment started on Coston2.<br> 28-Dec-2022 Deployment started on Flare.<br>5-Jan-2023 Deployment complete. |

## 1. Brief Description

The Songbird network is the [canary network](https://docs.flare.network/tech/glossary/#canary_network) for the Flare network.
That is, upgrades to Flare that require testing will be tested on Songbird first.
Where upgrades to Flare are governed by the Flare Improvement Proposal (FIP) process, upgrades to Songbird are governed by the Songbird Improvement Proposal (SIP) process and the Songbird Test Proposal (STP) process.

## 2. Technical Description

Songbird is governed by two types of proposals:

* **Songbird Improvement Proposal (SIP)**: Requests an upgrade to Songbird only.
  If the requested change in the proposal will be incompatible with Flare's core protocols, rendering tests of FIPs on Songbird impossible, the proposal is automatically rejected.
  To make the type of change that is incompatible with Flare's core protocols, an SIP that specifically ends Songbird's status as Flare's canary network must be passed first.
  This system intentionally limits the set of potential changes that can be made only to Songbird.

* **Songbird Test Proposal (STP)**: Requests an upgrade to Songbird because new features need to be tested before they are implemented on Flare.
  An STP can be submitted without an FIP.

  STPs are accepted by default and rejected only if enough votes are cast against them.
  This system allows the community of Songbird token-holders to govern changes that are unpopular without limiting the utility of Songbird for testing.

### 2.1 STP Voting Process

1. The Flare Foundation uploads an STP to [the Songbird network test proposals repository](https://github.com/flare-foundation/private-governance-proposals).
2. Voters delegate their vote power before the deadline set in the STP.
Vote power to be calculated is determined by the randomly chosen [vote-count block](https://docs.flare.network/tech/governance/#the-vote-count-block).
3. The proposal is accepted or rejected.
Voting on STPs is rejection-based.
For an STP to be rejected, both of the following conditions must be true:

   * **Threshold condition**: The minimum quorum is at least 75% of all `$SGB` tokens in circulation (excluding the Flare Foundation's tokens) at the vote count block.

     Note that the quorum is specified as a fraction of the circulating native `$SGB` tokens instead of the wrapped tokens `$WSGB` used for voting.
     This measure tries to encourage users to wrap their tokens and use them in the network.

   * **Majority condition**: More than 50% of the votes cast, must be against the proposal.

   Therefore, an STP will be accepted if the quorum threshold is not reached or if less than half of the cast votes are against it.

## 3. Proposal Structure

A valid STP proposal will contain the elements in the following template.
To create a new STP, copy and paste the template below, provide details in the appropriate sections, and substitute values for these variables:

* *proposal-number*: The proposal's numerical indicator specified as the preceding proposal's number plus 1.
* *proposal-title*: The proposal's name.
* *dd-mm-yyyy*: The proposal's creation date.
* *revision-number*: The proposal's revision number. Do not include this row in the table for new proposals.
* *status-type*: The status of the proposal. Specify either `Draft` or `Final`.

```markdown
# STP.*proposal-number*: *proposal-title*

| Type         | Songbird Test Proposal |
| :----------- | :----------------------|
| Author       | Flare Foundation       |
| Created      | *dd-mm-yyyy*           |
| Revision     | *revision-number*      |
| Status       | *status-type*          |

## 1. Brief Description
## 2. Technical Description
### 2.1 Technical Subsection
### 2.2 Technical Subsection
## 3. Link to Code Repository
## 4. Link to Audit Report (if applicable)
## 5. Link to Bug-Bounty Information (if applicable)
## 6. Proposed Implementation Date Range
## 7. Voting Details
## 8. Deadline for Voting
## 9. Revision History
```

## 4. Revision History

|  Revision | Changes                                                                                       |
| --------: | :-------------------------------------------------------------------------------------------- |
| [1][rev1] | Initial document.                                                                             |
|         2 | This revision clarifies descriptions of Songbird proposals and the STP voting process.        |

[rev1]: https://github.com/flare-foundation/governance-proposals/blob/0b44501/STP/STP_0.md
