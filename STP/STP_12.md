---
nav_order: 30012
title: STP.12
---

# STP.12: Introduce Process to Delist FTSO Feeds

| Type                | Songbird Test Proposal                      |
| :------------------ | :------------------------------------------ |
| Author              | Flare Foundation                            |
| Created             | 20-Jan-2025                                 |
| Document Status     | Final                                       |
| Threshold Condition | 75% (required to reject) 0.71% (obtained)   |
| Majority Condition  | 50% (required to reject) 0.17% (obtained)   |
| Voting Outcome      | [**Accepted**][ProposalLink] on 08-Feb-2026 |

[ProposalLink]: https://portal.flare.network/proposal/view/103434924382280670445981380089547180511066060288836795792644521424838688791350?chainId=19

## 1. Brief Description

This proposal establishes a measurable, transparent process to retire underperforming or stale Flare Time Series Oracle (FTSO) price feeds.
Feeds are assessed against objective market-quality thresholds - liquidity shortfalls, isolated single-venue spikes, cross-exchange price divergence, and inadequate venue coverage - followed by a time-boxed Watchlist and, if warranted, a [Management Group](./STP_2.md) (MG) delisting vote.

The process safeguards FTSO reliability and market integrity, reduces downstream risk to dApps and users, and provides a clear path from detection to Watchlist to removal.

## 2. Technical Description

### 2.1 Definitions and Measurement

This section defines the market-quality metrics used to evaluate whether an FTSO feed remains reliable and economically meaningful.

Delisting criteria are introduced in [Section 2.2](#22-delisting-criteria), and rely solely on data from **eligible venues**. These are spot markets on reputable Tier-1/2 centralized exchanges, and other exchanges that are demonstrably primary venues for the relevant asset. Delisting criteria use the following definitions:

* **Cross-venue median price**: The median price value across multiple centralized exchanges.
* **Cross-venue price spread**: The maximum deviation of the mid-point price across multiple centralized exchanges.
* **±2% Depth (USD)**: The sum of executable notional within ±2% of the mid-point price on each eligible venue, aggregated.

Feeds that breach these criteria enter the Delisting process described in [Section 2.3](#23-delisting-flow).

* **Delisting Watchlist**: Temporary status for feeds under increased scrutiny by Flare Foundation (FF) for breaching the Delisting Criteria.
* **Delisting Vote**: Post Delisting Watchlist vote undertaken by the MG to delist a feed from FTSO.

### 2.2 Delisting Criteria

Each criterion below is designed to detect a distinct failure mode (illiquidity, dislocation, venue-specific anomalies, or degraded decentralization).
Breaches trigger Watchlist entry, and repeated or persistent breaches can escalate to a Delisting Vote, as described.

1. **Very low liquidity**
    * Condition: The 7-day average ±2% depth around the mid-point price aggregated across eligible venues is below USD 10,000.
    * Escalation: On first breach, the FF places the feed on the Delisting Watchlist.
    If the breach persists for 14 consecutive days, the FF may initiate a Delisting Vote.

2. **Sustained cross-exchange dislocation**
    * Condition: In any 24-hour period, there exists a continuous interval of at least 5 minutes during which the cross-venue price spread exceeds 2%.
    As before, this applies to eligible venues.
    * Escalation: On first breach, the FF places the feed on the Delisting Watchlist.
    If this occurs more than once within a 14-day window, FF may initiate a Delisting Vote.

3. **Isolated price spikes**
    * Condition: On any eligible venue, the venue price deviates by more than 5% relative to the median of other eligible venues.
    * Escalation: The feed is added to Delisting Watchlist on the first isolated spike.
    If a second isolated spike occurs within 14 days, the FF may initiate a Delisting Vote.

4. **Insufficient venue coverage**
    * Condition: Venue coverage for the asset drops to a level that materially reduces effective decentralized price sourcing, such that cross-venue aggregation is no longer robust.
    * Escalation: On first occurrence, FF places the feed on the Delisting Watchlist.
    If the breach persists for 14 consecutive days, FF may initiate a Delisting Vote.

Note that all Delisting Criteria are subject to the Exceptions & Special Cases described in [Section 2.4](#24-exceptions-and-special-cases).

### 2.3 Delisting Flow

The delisting process is designed to be transparent and predictable: a feed first enters a Watchlist period to allow for recovery (e.g., venue additions, improved liquidity), and only escalates to a vote when issues persist or recur.

1. **Delisting Watchlist (Soft breach)**
    * Upon the first breach of any criterion defined in [Section 2.2](#22-delisting-criteria), the FF will post a Watchlist notice on the Discourse Forum and begin enhanced monitoring for a period of 14 days.
    * If the criteria are cleared for 14 consecutive days, the feed is removed from the Delisting Watchlist, and a closure update is posted.

2. **Delisting Vote**
    * If the escalation thresholds in [Section 2.2](#22-delisting-criteria) are met (persistent breach or repeated events), the FF submits a proposal to the MG with a delisting justification and recommended effective removal date.
    * MG Vote Outcome:
        * If Accepted, the feed will be scheduled for removal, a final notice will be given on the Discourse Forum and feed emissions will be disabled.
        * If Rejected, the feed will remain on the Delisting Watchlist for another 14 days with continued monitoring.

### 2.4 Exceptions and Special Cases

Delisting Criteria are subject to the following exceptions:

* **Market-wide events**. If a broad market shock occurs (e.g., systemic exchange outages or broad cross-market price dislocations), measurement of dislocation triggers is paused.
* **Exchange issues or downtime**. If an exchange is down or experiencing degraded performance, that exchange and any dislocations caused by it are excluded.
* **Ecosystem-critical assets**. If a token is demonstrably important to the Flare ecosystem (e.g., core collateral, protocol dependency, major integrations), the FF may temporarily relax delisting criteria.

## 3. Link to Code Repository and Contracts

* [Flare Foundation Smart Contracts GitHub Repository](http://github.com/flare-foundation/flare-smart-contracts-v2)
* Contracts & Methods (no new contracts required, only governance calls):
  * FTSO Scaling: Method `replaceFtsoConfiguration` on `FtsoInflationConfigurations`.
  * FTSO Fast Updates: Method `removeFeeds` on `FastUpdatesConfiguration`.
  * Note: Both Fast Updates and Scaling removal methods will be called at the same time, near the end of a reward epoch, to minimize impact on FTSO rewards.

## 4. Proposed Implementation Schedule

Implementation is expected to start shortly after the voting finishes.

## 5. Voting Details

An STP is rejection-based, meaning that it is accepted unless certain conditions are met, namely, that the quorum threshold (75%) is reached and at least half of the votes are against it.
(See [Governance](https://dev.flare.network/#stps).)

## 6. Deadline for Voting

* **Notice period**: 26-January-2026 to 1-February-2026
* **Voting period**: 2-February-2026 to 8-February-2026
