---
nav_order: 30005
title: STP.05
---

# STP.05: Inflation Updates

| Type                | Songbird Test Proposal                      |
| :------------------ | :------------------------------------------ |
| Author              | Flare Foundation                            |
| Created             | 28-Apr-2023                                 |
| Document Status     | Final                                       |
| Threshold Condition | 75% (required to reject) 1.66% (obtained)   |
| Majority Condition  | 50% (required to reject) 0.35% (obtained)   |
| Voting Outcome      | [**Accepted**][ProposalLink] on 03-Jul-2023 |
| Timeline            | 12-Jul-2023 Deployed on Coston.<br>14-Sep-2023 Deployed on Songbird.|

[ProposalLink]: https://portal.flare.network/proposal/view/86986707598029556313330355529395772870755361418487059291222730976661664120502?chainId=19

## 1. Brief Description

Here is a quick explanation of the inflation behaviors that STP.05 proposes to improve:

* **Accrue Unclaimed Rewards:** Currently, when each inflation time slot concludes, reward funds are no longer accessible, potentially causing some payments to be pushed to the next slot. STP.05 proposes allowing unclaimed rewards to accrue across time slots to ensure timely payments. (See [2.1 Accrue Unclaimed Rewards](#21-accrue-unclaimed-rewards), below.)
* **Align Time Slots:** Using 1-year time slots on Songbird is out of sync with the Flare code and requires a third-party date-time library and unnecessary exposure to bugs. STP.05 proposes aligning Songbird and Flare inflation time slots by changing Songbird's time slot from 1 year to 30 days. (See [2.2 Align Time Slots](#22-align-time-slots-with-flare), below.)
* **Calculate Inflatable Balance:** Currently, if some authorized inflation is not claimed, it becomes part of the inflatable balance on Songbird, adding to inflation. STP.05 proposes changing the formula so that only inflation that is minted and actually claimed is included. (See [2.3 Calculate Inflatable Balance](#23-calculate-inflatable-balance), below.)
* **Songbird Inflation Cap:** From year 3 onwards, Flare's inflation is set to 5% of inflatable balance with a cap of 5B (5,000,000,000), while Songbird's inflation is set to 6% with no cap. STP.05 proposes setting Songbird's inflation to 5% of inflatable balance from year 3 onwards and adding a cap of 750M. (See [2.4 Songbird Inflation Cap](#24-songbird-inflatable-balance-cap), below.)
* **Two Burn Addresses:** Songbird uses two burn addresses, but only one is being monitored when calculating the circulating supply. STP.05 proposes to properly monitor both burn addresses, resulting in more accurate circulating supply calculation. (See [2.5 Two Burn Addresses](#25-two-burn-addresses), below.)
* **Flare Foundation Updates Process:** Flare-Foundation-held `$SGB` amounts are updated through a governance multi-signature account. For efficiency and a more precise `$SGB` balance bookkeeping, STP.05 proposes making all updates for the Flare Foundation through the [Supply contract](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/735-songbird-update-supply-and-inflation-contracts/contracts/inflation/implementation/Supply.sol). (See [2.6 Flare Foundation Updates Process](#26-flare-foundation-updates-process), below.)

As it can be seen, some of the proposed changes for Songbird are already present on Flare, but some others, like the ones proposed in sections 2.1 and 2.6, are not. After testing on Songbird these changes will be added to Flare through the corresponding FIP.

## 2. Technical Description

The technical descriptions provide more detailed reasoning behind the proposals and their implications.

### 2.1 Accrue Unclaimed Rewards

Inflation is recognized in time slots of 1 year on Songbird and 30 days on Flare.
During each time slot, recognized inflation is authorized and minted daily to the different reward contracts.

Currently, tracking of authorized and distributed inflation is done separately on each time slot for each reward service. When time slots are switched (once per year for Songbird), this process results in unclaimed rewards becoming inaccessible on the old time slot, so late claims are made in the new time slot instead, which was not expecting them.

If late claimants claim in the new time slot, the new time slot may run out of funds before all user claims are met, even for claims that are on time.

Note that since token holders usually claim rewards before inflation switches to the next time slot, and this only happens once a year, meeting all claims is not usually an issue.

#### Proposal

Update inflation logic to apply across all time slots, instead of separately per time slot. With this change, unclaimed rewards from previous time slots accrue and no rewards become inaccessible.

### 2.2 Align Time Slots with Flare

Currently, the Songbird inflation time slots are set to 1 year and the Flare time slots are set to 30 days. The Songbird time slots are out of sync with Flare, and they require a third-party date-time library to simplify the yearly inflation calculation, exposing more code to the potential for bugs.

#### Proposals

The proposals include the main proposal to switch Songbird to a 30-day time slot and the supporting proposals to rename the time slots and process the switch-over.

* Move to a 30-day (bank month) inflation time slot, aligning the Songbird and Flare inflation logic, removing the dependency on the date-time library, and reducing the potential for bugs. This change causes higher inflation, which is mitigated by the change made in [2.3 Calculate Inflatable Balance](#23-calculate-inflatable-balance).

* Rename the code for inflation time slots from `annum`, for yearly inflation, to `timeSlot`, for 30-day time slots.

* Time-Slot Switch-Over Process:

  Switching Songbird from 1-year to 30-day time slots requires a new contract.
  STP.05 proposes adding code to enable this and any future switch to new inflation-manipulating contracts with minimal Flare intervention.

  The process for switching contracts involves reading all inflation data from the existing inflation contract:

  1. **Total [authorized](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/master/docs/specs/Inflation.md#authorization)** and **total distributed** amounts are copied from all existing yearly time slots for all reward services to global values.
  2. **Global total authorized** and **global total distribution** are calculated by adding up all amounts from the previous step.
  3. **Total [recognized](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/master/docs/specs/Inflation.md#recognition) inflation** is set as the sum of all authorized inflation until this point.

  Once all the inflation data is copied, a new inflation time slot is created, starting at the day of the switch-over.

### 2.3 Calculate Inflatable Balance

Currently, if some [authorized inflation](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/master/docs/specs/Inflation.md#authorization) is not claimed by users, it still becomes part of the inflatable balance on Songbird, resulting in higher inflation than necessary. The current formula for the **inflatable balance** on Songbird is:

`inflatable balance = genesis amount + all recognized inflation from previous time slots`.

The main difference from the Flare calculation is that NOT `all recognized inflation` is actually minted and claimed.

#### Proposal

STP.05 proposes changing the Songbird formula to:

`inflatable balance = genesis amount + all claimed inflation`

so that only inflation that is minted and actually claimed remains.
Updating the calculation for Songbird reduces inflation, mitigating the higher inflation caused by [2.2 Align Time Slots with Flare](#22-align-time-slots-with-flare).

### 2.4 Songbird Inflatable Balance Cap

The Flare inflation contract adds 10% of inflatable balance the first year (July 2022-2023), 7% the second year, and 5% from year 3 onwards, with a cap of 5B (5,000,000,000) for any year.

The Songbird inflation contract added 10% of inflatable balance the first year (September 2021-2022), which resulted in 1.5B recognized tokens and 8% of inflatable balance the second year, which resulted in 1.32B recognized tokens. It is set to 6% of inflatable balance for the third year and onwards. There is no cap.

#### Proposal

STP.05 proposes changing the Songbird inflation for year 3 (starting September 2023) from 6% of the inflatable balance to 5%, to align with Flare. Additionally, from year 3 onwards, STP.05 proposes a cap on Songbird inflation of 750M (750,000,000) tokens.

### 2.5 Two Burn Addresses

The Songbird Supply contract initially kept track of burned tokens that were sent to the original Avalanche burn address `0x0100000000000000000000000000000000000000`.
However, Avalanche's pre-compiled contract prevents sending unwrapped `$SGB` to it, so unwrapped `$SGB` that needs to be burned is sent instead to address `0x000000000000000000000000000000000000dEaD`. This is the same address used on the Flare network.

When [SIP.01](../SIP/SIP_1.md) was implemented, the Supply contract started monitoring the new address and stopped monitoring the Avalanche address.

However, accurate calculation of the circulating supply requires monitoring both burn addresses.

#### Proposal

Modify the [Supply contract](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/735-songbird-update-supply-and-inflation-contracts/contracts/inflation/implementation/Supply.sol) so that it takes both addresses into account when calculating the total amount of burned tokens.

### 2.6 Flare Foundation Updates Process

Currently, Flare-Foundation-held `$SGB` is updated through a governance multi-signature account.
Although these accounts are considered more secure, they rely on human intervention (via their electronic signatures) and create a burden on the signers every time `$SGB` is sent out from the Foundation.
Dependence on human intervention results in random and fewer readings of the updated amounts, and therefore less precision on the reported Flare-Foundation-held `$SGB` amount.
Because this amount is part of the circulating supply of `$SGB`, the reported circulating supply is not as precise as it could be.

#### Proposal

STP.05 proposes automating all Flare Foundation `$SGB` updates to the circulating supply through the [Supply contract](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/735-songbird-update-supply-and-inflation-contracts/contracts/inflation/implementation/Supply.sol).
The Supply contract will read the Flare Foundation `$SGB` balances daily at the same time the new inflation is authorized, getting a more frequent and regular view of the balance.
An automated reading of the Flare-Foundation balance results in a more precise count of circulating supply.

## 3. Link to Code Repository

Three smart contracts, [`Inflation`](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/735-songbird-update-supply-and-inflation-contracts/contracts/inflation/implementation/Inflation.sol), [`InflationAllocation`](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/735-songbird-update-supply-and-inflation-contracts/contracts/inflation/implementation/InflationAllocation.sol), and [`Supply`](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/735-songbird-update-supply-and-inflation-contracts/contracts/inflation/implementation/Supply.sol), will be redeployed. The new code can be found in the [Songbird Inflation Contracts](https://gitlab.com/flarenetwork/flare-smart-contracts/-/tree/735-songbird-update-supply-and-inflation-contracts/contracts/inflation) repository branch.

## 4. Proposed Implementation Date Range

If this proposal passes, new contracts will be deployed on the [Coston test network](https://docs.flare.network/tech/flare/#the-flare-networks) within a month.

Once the new inflation is observed to work correctly on Coston for a time slot (30 days), the new contracts will be deployed on Songbird.

## 5. Voting Details

An STP is rejection-based, meaning that it is accepted unless certain conditions are met, namely, that the quorum threshold (75%) is reached and at least half of the votes are against it. (See [Governance](https://docs.flare.network/tech/governance/#stps).)

## 6. Deadline for Voting

One week after the proposal is published in [the Flare Portal](https://portal.flare.network/).
