---
nav_order: 20001
---

# SIP.01: Align Songbird and Flare Smart Contracts

| Type      | Songbird Improvement Proposal |
| :-------- | :---------------------------- |
| Author    | Flare Foundation              |
| Created   | 27-Feb-2023                   |
| Revision  | 2                             |
| Status    | Draft                         |
| Threshold | 0%                            |
| Majority  | 50%                           |

## 1. Brief Description

The purpose of the Songbird network is to act as a [canary network](https://docs.flare.network/tech/glossary/#canary_network) for the Flare network. To achieve this goal, the different smart contracts are first deployed on Songbird, and, once proven to operate well, they are deployed on Flare.

However, because the [FIP.01](https://proposals.flare.network/FIP/FIP_1.html) proposal required voting infrastructure to be available immediately after the token distribution event, some smart contracts were deployed on Flare before doing so on Songbird first.

This proposal will deploy on Songbird the most recent version of some smart contracts like the `FtsoRewardManager`. This update will align the Songbird network more closely with Flare. The effects of this deployment are described in the Technical Description section. No changes will be made to the Flare network.

## 2. Technical Description

The list of contracts to be deployed on Songbird can be found in the [next section](#3-link-to-code-repository). This deployment will have the following impact on the Songbird network:

1. **Smart contract alignment**: These smart contracts will have the same code on both networks, greatly simplifying their maintenance and freeing Flare Foundation development resources to work on other parts of the code. Application writers will be able to support both Songbird and Flare networks with less effort.

2. **Add autoclaiming facilities**: The ability to nominate executors that can claim rewards on the user's behalf is already deployed and working on Flare and is [documented here](https://docs.flare.network/tech/automatic-claiming/). This proposal will bring this functionality to Songbird too.

3. **Add Personal Delegation Accounts**: PDAs can temporarily receive and store rewards to track which funds are from rewards, for example, for personal or tax purposes. This functionality is already available on Flare and is [documented here](https://docs.flare.network/tech/personal-delegation-account/). This proposal will bring this functionality to Songbird too.

4. **Unclaimed FTSO reward burning**: FTSO delegation rewards that have not been claimed in 90 days are currently redistributed in the following reward epoch on Songbird, whereas on Flare these unclaimed rewards are burned. This proposal will align both networks and burn unclaimed FTSO rewards on Songbird too, which has the added benefit of reducing inflation.

   This change will not affect the length of the Songbird FTSO reward epoch, which continues to be 7 days.

   This change will not affect unclaimed rewards from previous epochs, which will still be claimable through the current `FtsoRewardManager` contract until they expire.

Please note that the above effects are already present on the Flare network.

## 3. Link to Code Repository

The smart contracts deployed on Songbird will use the same source code as the ones on Flare, namely, the [`flare_network_deployed_code`](https://gitlab.com/flarenetwork/flare-smart-contracts/-/tree/flare_network_deployed_code) branch.

The exact contracts deployed and their source code links are:

* [`FtsoRewardManager`](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/flare_network_deployed_code/contracts/tokenPools/implementation/FtsoRewardManager.sol)

    Applications should start using the new address once the new contract is deployed, since the current `FtsoRewardManager` will stop accruing rewards.
    Use the `FlareContractRegistry` contract to retrieve addresses [as explained in the documentation](https://docs.flare.network/dev/reference/contracts).

    Applications should also continue checking the current `FtsoRewardManager` for unclaimed rewards from previous epochs.

* [`ClaimSetupManager`](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/flare_network_deployed_code/contracts/claiming/implementation/ClaimSetupManager.sol)

* [`DelegationAccount`](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/flare_network_deployed_code/contracts/claiming/implementation/DelegationAccount.sol)

* [`CleanupBlockNumberManager`](https://gitlab.com/flarenetwork/flare-smart-contracts/-/blob/flare_network_deployed_code/contracts/token/implementation/CleanupBlockNumberManager.sol)

## 4. Proposed Implementation Date Range

Deployment of the new contracts on the [Coston network](https://docs.flare.network/dev/reference/network-configs/) is expected to start shortly after the voting finishes and be completed quickly.

Within a month, if no problems are detected the new contracts will be deployed on Songbird.

## 5. Voting Details

To pass, the proposal requires a simple majority of votes in favor of it.

## 6. Deadline for Voting

One week after the proposal is published in [the Flare Portal](https://portal.flare.network/).

## 7. Revision History

|  Revision | Changes                                              |
| --------: | :--------------------------------------------------- |
| [1][rev1] | Initial document.                                    |
|         2 | Clarify effects on the `FtsoRewardManager` contract. |

[rev1]: https://github.com/flare-foundation/governance-proposals/blob/e9e390/SIP/SIP_1.md
