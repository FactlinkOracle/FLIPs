**FLIP #** - tbd

- **FLIP title:** Add **Token Name (Token Address)** to Mint Whitelist
- **Author:** Name (email)
- **Status:** Draft
- **Created:** Date
- **Discourse Link:** **[Link to FLIP Discourse Channel in Discord]**

## Summary _(2-5 sentences)_

This FLIP proposes adding **Token Name (Token Address)** SPL token to the Optimistic Oracle's mint whitelist. This will allow it to be used for posting bonds and/or rewards for queries, enhancing the economic security and flexibility of the oracle.

## Motivation

_Explain why this token is a valuable addition to the Factlink ecosystem. What problems does it solve or what new opportunities does it create? E.g., "Adding USDC as a whitelisted bond token provides a stable, low-volatility option for securing queries, making the economic guarantees more predictable for all participants._

## Technical Specification

To accomplish this, the Optimistic Oracle `owner` must call the `add_mint_to_whitelist` instruction with the following parameters:

- **Mint Address:** `[Solana address of the token mint]`
- `whitelist_status`: _Choose 1, 2, or 3_
  - `1`: Bond only
  - `2`: Reward only
  - `3`: Both Bond and Reward
- `min_fee_for_bond`: _Minimum bond fee in the token's smallest unit (lamports)_
- `min_fee_for_reward`: _Minimum reward fee in the token's smallest unit (lamports)_

This will add a `MintDetails` entry to the `OOMintWhitelistAccount`.

## Rationale

This section evaluates the proposed token's suitability for its intended role(s).

### Suitability as a Security Bond

A security bond must be a reliable economic deterrent. It is posted by both the asserter and the disputer, with the loser forfeiting their bond. The ideal bond token is stable and highly liquid.

- **Stability Analysis**: _Analyze the token's price stability. Is it a stablecoin? If not, what is its historical volatility? A stable token ensures the economic security of a query doesn't degrade during the dispute period._
- **Liquidity Analysis**: _Analyze the token's liquidity on Solana DEXs. Is there enough deep liquidity on platforms like Orca or Raydium for participants to easily acquire and sell the token without significant price impact?_
- **Recommended Bond Amount**: _e.g., "For queries using USDC, we recommend a standard bond amount of 500 USDC to provide a meaningful economic guarantee."_

### Suitability as an Asserter Reward

An asserter reward is paid by the query creator to incentivize a correct and timely assertion.

- **Incentive Analysis**: _Why would an asserter want to receive this token? Is it a desirable asset?_
- **Liquidity for Asserters**: _Is it easy for an asserter to sell this reward on the open market if they choose to? Poor liquidity might make the reward less attractive._

### Proposed Fee Parameters

The proposed min_fee_for_bond is [amount], approximately [$X.XX]. This is to [e.g., provide a meaningful economic guarantee that asserter or disputer acts with honesty].
The proposed min_fee_for_reward is [amount], approximately [$Y.YY]. This is to [e.g., provide a meaningful reward for asserters and to ensure that a protocol captures a nominal fee from reward flows].

## Security considerations

Adding a new token for bonds and rewards introduces economic risk. The primary risks are:

- **Volatility Risk:** If used for bonds, a highly volatile token could see its value drop mid-dispute, reducing the economic guarantee.
- **Liquidity Risk:** If the token is illiquid, it may be difficult for participants to acquire it for bonding or to sell it after receiving it as a reward.
- **Centralization/Censorship Risk:** If the token can be frozen or censored (e.g., a centralized stablecoin), it could disrupt the oracle's function.

_This token should be monitored for these risks. Projects using the Optimistic Oracle should carefully consider which bond and reward tokens they are willing to accept for their queries._
