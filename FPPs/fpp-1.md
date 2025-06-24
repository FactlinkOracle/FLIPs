# FPPs

FPPs (“Factlink Parameter Proposals”) are a lighter-weight form of a FLIP meant for simple parameter changes to the live `Factum DVM` and `Optimistic Oracle` contracts. The difference between an FPP and a FLIP is semantic and process-based. Both still need tokenholder approval to be implemented and both can still pose a risk to the system.

The FPP should explain what parameter will be changed, why, and if there are any potential hazards to making this change.

_Note: despite the shorter process and simpler changes, these proposals are still quite powerful and should be considered just as carefully._

### What parameters can be changed via an FPP?

FPPs are used to call the `update_dvm_properties` function on the Factum DVM or the `update_admin_properties` function on the Optimistic Oracle. Examples include:

- **DVM Parameters:**
  - `emission_rate_per_second`
  - `unstake_cooldown_time`
  - `phase_lengths` (Commit, Reveal, Claim, Configure)
  - `slashing_params`
  - `voting_thresholds` (GAT and SPAT)
- **Optimistic Oracle Parameters:**
  - `burned_bond_basis_points`
  - `reward_fees_basis_points`
  - `factum_dvm_address`

### What is the lifecycle of an FPP?

A successful FPP will move along the following stages: Draft ⟶ Final ⟶ Approved.

#### Draft

An FPP that is open for consideration and is undergoing changes.

#### Final

An FPP that is done with changing due to feedback and is ready to be proposed for a vote.

#### Approved

If tokenholders approve the proposal, the live protocol will be updated to reflect the parameter change.

#### Abandoned

If at any point during the FPP Finalization Process, an FPP is abandoned, it will be labeled as such.

#### Rejected

If tokenholders do not approve a proposal, it will be labeled Rejected.
