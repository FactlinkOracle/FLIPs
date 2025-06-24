# FLIPs

FLIPs (“Factlink Improvement Proposals”) are the design documents used to propose changes to the Factlink ecosystem.
They provide information to the Factlink community that describes a new feature for the Factlink protocol, or its ecosystem.
The FLIP should provide a concise technical specification of the feature and a rationale for the feature.
They are modeled after UMA's [UMIPs](https://github.com/UMAprotocol/UMIPs/blob/master/README.md) and Solana's [SIMDs](https://github.com/solana-foundation/solana-improvement-documents) (Solana Improvement Documents).

We intend FLIPs to be the primary mechanism for proposing new features, collecting community technical input on an issue, and for documenting the design decisions that have gone into the Factlink protocol.
FLIPs are a convenient way to track the progress of an implementation.

# What is the lifecycle of a FLIP?

A successful FLIP will move along the following stages: Draft ⟶ Last Call ⟶ Final ⟶ Approved.
Unsuccessful states are also possible: Abandoned and Rejected.

## Draft

A FLIP that is open for consideration and is undergoing rapid iteration and changes.
In order to proceed to “Last Call,” the implementation must be complete.
Every FLIP author is responsible for facilitating conversations and building community consensus for the proposal.

## Last Call

A FLIP that is done with its initial iteration and ready for review by a wide audience.
If upon review, there is a material change or substantial unaddressed complaints, the FLIP will revert to "Draft".
If the FLIP requires code changes, the core devs must review the FLIP.
A successful FLIP will be in "Last Call" status for a reasonable period of time for comments and be merged (if necessary) before moving to a tokenholder vote.

## Final

A FLIP that successfully passes the "Last Call" will move to the "Final" state and be put to a Factlink tokenholder vote.

## Approved

If tokenholders approve the proposal, the live protocol will be updated to reflect it.
At this time, any code changes (if relevant) should be merged into the core protocol repository so that the core protocol repository always reflects the live code that is running.
The FLIP is then considered to be in the "Approved" state.

## Abandoned

If at any point during the FLIP Finalization Process, a FLIP is abandoned, it will be labeled as such.
Grounds for abandonment include a lack of interest by the original author(s), or it may not be a preferred option anymore.

## Rejected

If tokenholders do not approve a proposal, or the FLIP is fundamentally broken or rejected by the core team, it will not be implemented.
