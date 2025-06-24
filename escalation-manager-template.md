**FLIP #** - tbd

- **FLIP title:** Add **Escalation Manager Name** to Whitelist
- **Author:** Name (email)
- **Status:** Draft
- **Created:** Date
- **Discourse Link:** [Link to FLIP Discourse Channel in Discord]

## Summary (2-5 sentences)

This FLIP proposes adding a new Escalation Manager to the Optimistic Oracle's whitelist. This will allow query creators to designate this new contract as the destination for dispute resolution, enabling alternative or specialized arbitration mechanisms beyond the default Factum DVM.

## Motivation

**[Explain why this new Escalation Manager is needed. For example: "The proposed 'Social Consensus DVM' is a new Factum DVM instance with fundamentally different objectives (e.g., each tokenholder irrespective of stake just gets one vote) specifically designed for non-financial, subjective queries. Whitelisting it allows projects to use the Optimistic Oracle for opinion markets without requiring the high economic security of the main DVM."]**

## Technical Specification

To whitelist this Escalation Manager, the `owner` of the Optimistic Oracle must call the `add_escalation_manager_to_whitelist` instruction with the following parameter:

- `escalation_manager`: **[Solana address of the new Escalation Manager program/contract]**

This will add the program's `Pubkey` to the `OOEscalationManagerWhitelistAccount`, making it selectable by index when a new query is raised.

## Rationale

The proposed Escalation Manager is a **[describe the contract]**.

- **Contract Address:** `[Link to the contract on a block explorer like Solscan]`
- **Source Code:** `[Link to the verified source code repository]`
- **Audits:** `[Link to any security audits]`
- **Documentation:** `[Link to its technical documentation]`

This manager was chosen because **[explain its benefits and why it's a trustworthy and effective choice for resolving specific types of disputes]**.

## Security considerations

**This is the most critical section for this type of FLIP.**

Whitelisting a new Escalation Manager introduces a new trusted arbitration path. The security and integrity of any query that uses this new manager will depend entirely on the manager's own mechanism, not the default Factum DVM.

Key considerations for tokenholders:

- **Trust Assumption:** By approving this FLIP, the governance is endorsing this new contract as a legitimate and secure arbitration venue.
- **Economic Security:** What is the economic model of this new manager? How does it prevent attacks? If it's a DVM, what are its staking parameters (`GAT`, `SPAT`, `cumulative_stake`)?
- **Liveness and Correctness:** What are the guarantees that this manager will resolve disputes correctly and in a timely manner? What happens if it fails?
- **Centralization Risk:** Is the new manager controlled by a single key, a multisig, or a decentralized set of tokenholders? What are the risks of compromise or censorship?

Query creators and users must be aware that when a query specifies a custom escalation manager, they are implicitly trusting that manager's specific security model.
