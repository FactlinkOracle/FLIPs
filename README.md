# Factlink Governance Proposals

This is the central repository for Factlink Improvement Proposals (FLIPs) and Factlink Parameter Proposals (FPPs). These documents are the primary way for the Factlink community to propose, discuss, and ratify changes to the Factlink protocol.

To contribute, first review [FLIP-1](FLIPs/flip-1.md) for the process overview. Then, clone this repository and add your proposal using the appropriate template. Finally, submit a Pull Request for community review.

## What's the difference between a FLIP and an FPP?

While both proposal types require tokenholder approval, they are designed for different kinds of changes.

### FPPs (Factlink Parameter Proposals)

FPPs are for parameter changes to the configurable parameters within the `Optimistic Oracle` and `Factum DVM` contracts.

**Use FPPs for changes like:**

- Adjusting DVM staking rewards
- Modifying DVM phase lengths or unstaking cooldown time
- Updating DVM voting thresholds or slashing penalties
- Changing Optimistic Oracle fees

### FLIPs (Factlink Improvement Proposals)

FLIPs are for more significant changes that add new functionality, change whitelists' values, alter trust assumptions, or introduce new assets to the ecosystem.

**Use FLIPs for changes like:**

- Adding or removing a new **Query Type** to the Optimistic Oracle whitelist, enabling new use cases.
- Adding or removing a new **SPL Token** to the Optimistic Oracle's mint whitelist for use as a bond or reward.
- Modifying already whitelisted mints' fee structure or status.
- Adding or removing a new trusted **Escalation Manager** (e.g., a new DVM instance) to the Optimistic Oracle.
- Adding or removing a new trusted **Oracle Authority** to the Factum DVM's core whitelist.
- Proposing entirely new contracts or major architectural upgrades.
