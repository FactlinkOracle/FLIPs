**FLIP #** - tbd

- **FLIP title:** Add Oracle Authority to DVM Whitelist
- **Author:** Name (email)
- **Status:** Draft
- **Created:** Date
- **Discourse Link:** [Link to FLIP Discourse Channel in Discord]

## Summary _(2-5 sentences)_

This FLIP proposes adding a new trusted Oracle Authority to the Factum DVM's whitelist. This will grant the specified `Pubkey` the permission to call `raise_query_resolution_request` on the DVM, allowing it to forward disputed Optimistic Oracle queries for a full vote.

## Motivation

_Explain why a new Oracle Authority is necessary. For example: "A new partner, 'Project X', is integrating with Factlink and requires its own authority to manage disputes for its users."_

## Technical Specification

To whitelist this new authority, the `owner` of the Factum DVM must call the `add_oracle_authority_to_whitelist` instruction with the following parameter:

- `oracle_authority`: _Solana Pubkey of the new authority_

This will add the `Pubkey` to the `DVMOracleAuthorityWhitelistAccount`. The Optimistic Oracle instance running at this new authority's address can then successfully escalate disputes to this DVM.

## Rationale

The proposed `Pubkey` represents the on-chain address of the **\[program name]**, which is operated and maintained by **\[project name]**.

- **Authority Address:** `[The Pubkey to be added]`
- **Controlling Entity:** `[Name of the organization or group]`
- **Rationale:** _This entity has been selected due to its alignment with the Factlink ecosystem, demonstrated technical competence, and its operational readiness to escalate disputes responsibly. Granting it authority to forward queries to the DVM ensures smoother integration with the Factlink infrastructure while maintaining the DVM’s decentralization and scalability goals._

## Security considerations

The `OracleAuthorityWhitelist` is a critical component of the DVM's security. While the authority cannot resolve disputes itself, it acts as a gatekeeper for what disputes reach the DVM voters.

Key considerations for tokenholders:

- **Trust Assumption:** An Oracle Authority is a highly privileged role. Approving this FLIP means placing trust in the entity controlling the new key to act honestly and maintain high operational security.
- **Spam/Denial-of-Service Risk:** A compromised or malicious authority could attempt to spam the DVM with invalid or frivolous requests. This risk is mitigated by the DVM's own rate-limiting (`max_requests_per_round`) and the fact that each escalated dispute has an economic cost (the bond).
- **Program Security:** The single most important risk is the compromise of the new authority program. The security audits of the proposed authority must be robust enough to warrant this level of trust from the Factlink DAO.
