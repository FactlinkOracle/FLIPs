### `FLIPs/flip-2.md`

## Headers

| FLIP-2     |                                                      |
| ---------- | ---------------------------------------------------- |
| FLIP Title | Add `GENERAL_YES_NO_QUERY` as a supported query type |
| Authors    | Founder (founder@factlink.xyz)                       |
| Status     | Approved                                             |
| Created    | 24th June, 2025                                      |

# Summary

This FLIP proposes whitelisting a new, foundational query type: `GENERAL_YES_NO_QUERY`. This type does not define a specific question but instead provides a standardized framework for asking any question that can be resolved by the Factlink Data Verification Mechanism (DVM).

Query creators will use this type and provide a `query_ipfs_cid` pointing to a structured JSON file that contains the full, unambiguous question and its resolution criteria. If disputed, the DVM will resolve the query to a single `u8` value according to a strict four-outcome model:

- **Vote `0` (NO):** The answer to the question is "No" or "False".
- **Vote `1` (YES):** The answer to the question is "Yes" or "True".
- **Vote `2` (INDETERMINATE):** The question is valid and has reached its resolution time, but the answer cannot be determined (e.g., due to ambiguity, data unavailability, or a canceled event, etc.).
- **Vote `3` (PREMATURE):** The request to resolve the query is invalid because it was made _before_ the event's resolution time, and the query was not marked for early expiration.

# Motivation

Approving this generic query type unlocks the creative potential of the Factlink ecosystem. It empowers developers to immediately create a vast range of financial products without the governance friction of whitelisting a new query type for every specific use case. This will enable permissionless innovation for:

- Prediction Markets
- Insurance Products
- Event-based Derivatives

# Query Data Specification

For a query of type `GENERAL_YES_NO_QUERY`, the `query_ipfs_cid` provided when calling `raise_query` on the Optimistic Oracle **must** point to a UTF-8 encoded JSON file adhering to the following structure:

```json
{
  "title": "A short, human-readable title for the query.",
  "question": "The full, unambiguous question to be answered. This MUST contain all details necessary for independent resolution.",
  "resolutionTime": "2025-12-25T12:00:00Z",
  "resolutionSources": [
    {
      "source": "https://primary-source-of-truth.com/api",
      "description": "Primary source of verification. This endpoint will be used to fetch publicly available confirmation data published by the official project, such as a tweet from their verified Twitter account (@ProjectHandle) or a post on their official website."
    },
    {
      "source": "https://secondary-source-of-truth.com/api",
      "description": "Secondary backup source for verification in case the primary source is unavailable or unclear. This may include mirrors of announcements, public API data, or additional statements from the official @ProjectHandle Twitter account or affiliated platforms."
    }
  ],
  "earlyExpiration": false
}
```

- `title` (string): A brief title for display in UIs.
- `question` (string): The precise, long-form question. This is the canonical text for resolution.
- `resolutionTime` (string, ISO 8601 format): The specific UTC timestamp at or after which the question is intended to be resolved.
- `resolutionSources` (array of resolution sources): A list of sources and detailed descriptions of those sources voters must use.
- `earlyExpiration` (boolean): A critical flag.
  - `false`: A resolution is valid only if it is made _after_ the `resolutionTime`.
  - `true`: A resolution may be valid _before_ `resolutionTime`.

# Technical Specification

To implement this, the Optimistic Oracle `owner` must call the `add_query_type` instruction with the following parameter:

- `query_type_name`: `"GENERAL_YES_NO_QUERY"`

This will whitelist the corresponding hash in the `OOQueryTypeWhitelistAccount`, making it available for use. No changes are needed to the DVM.

# Rationale

This design is a deliberate trade-off, prioritizing flexibility and richness over on-chain rigidity.

- **Off-Chain Richness (IPFS):** Moving the full specification to IPFS allows for detailed, human-readable questions that are impossible to store on-chain.
- **Four-Outcome Model:** The `0, 1, 2, 3` model is critical for deterministic resolution. It cleanly separates a **truly unresolvable query (`2`)** from a **procedurally invalid request (`3`)**. This prevents attackers from forcing premature settlements and gives voters a clear way to dismiss invalid requests without having to answer the underlying question.
- **Explicit Timestamps:** The mandatory `resolutionTime` field removes ambiguity for voters, making the check for prematurity (`vote 3`) a simple, deterministic timestamp comparison.

# Implementation Guide for DVM Voters

If a query of type `GENERAL_YES_NO_QUERY` is escalated to the DVM, voters **must** follow this precise algorithm to determine their vote:

1.  **Fetch & Validate IPFS Data:**

    - Retrieve the JSON file from the `query_ipfs_cid`.
    - If the file is missing, malformed, or does not contain all required fields (`title`, `question`, `resolutionTime`, `resolutionSources`, `earlyExpiration`), **vote `2` (Indeterminate)**.

2.  **Check for Premature Expiration (Vote `3`):**

    - Identify the DVM price request timestamp (the time the dispute was escalated).
    - Identify the `resolutionTime` and `earlyExpiration` flag from the IPFS JSON.
    - If `earlyExpiration` is `false` AND the price request timestamp is _before_ the `resolutionTime`, the request is premature. **Vote `3`**.
    - If this condition is not met, proceed to the next step.

3.  **Resolve the Question (Votes `0`, `1`, or `2`):**
    - You are now past the premature check. The question is considered valid for resolution.
    - Carefully read the `question` and consult _only_ the `resolutionSources`.
    - If the sources provide a definitive "Yes" or "True" answer: **Vote `1`**.
    - If the sources provide a definitive "No" or "False" answer: **Vote `0`**.
    - If the question is inherently ambiguous, the sources are unavailable, the sources conflict, or a definitive answer cannot be reached for any other reason: **Vote `2` (Indeterminate)**.

### Examples

**Scenario A: Standard Resolution**

- **Question:** "Did the price of SOL close above 250 USD on 2025-07-01 (UTC Timezone) according to the daily closing price on Binance?"
- `resolutionTime`: "2025-07-02T00:00:00Z"
- `earlyExpiration`: `false`
- An asserter has asserted 0 ("NO") at 2025-07-02T00:15:00Z
- A disputer has disputed the query with 1 ("YES") at 2025-07-02T00:30:00Z.
- Coinbase data shows the closing price was 260 USD.
- **Outcome:** The request is not premature and the answer is "YES". **Voters must vote `1`**.

**Scenario B: Premature Resolution Request**

- Same question as above.
- An asserter has asserted 0 ("NO") on 2025-06-25T10:00:00Z
- A disputer has disputed the query with 3 ("PREMATURE") at 2025-06-25T10:30:00Z.
- **Outcome:** `earlyExpiration` is `false` and the request is before `resolutionTime`. **Voters must vote `3`**. They do not need to check the price.

**Scenario C: Indeterminate Outcome**

- Same question as above.
- An asserter has asserted 0 ("NO") on 2025-07-02T00:15:00Z
- A disputer has disputed the query with 2 ("INDETERMINATE") at 2025-07-02T00:40:00Z.
- The Coinbase API was down all day on 2025-07-01 (UTC Timezone) and reported no data. No other sources are listed.
- **Outcome:** The request is not premature, but the answer cannot be determined from the specified sources. **Voters must vote `2`**.

# Security Considerations

This FLIP places significant responsibility on the creators and users of queries.

- **Question Ambiguity:** The primary attack vector is an ambiguously worded `question` designed to be interpreted in multiple ways. Malicious actors can use this to guarantee they win a dispute.
- **Source Reliability:** The integrity of the vote is entirely dependent on the integrity of the `resolutionSources`. If a source is manipulable or unreliable, the query is insecure.
- **Burden of Due Diligence:** The security model requires that users of any application built on this query type **MUST** personally inspect the IPFS JSON file before risking funds. User interfaces and dApps built on Factlink have a responsibility to prominently display the full, unabridged question, resolution time, and sources.
- **`earlyExpiration` Flag:** A malicious actor could set `earlyExpiration: true` on a long-term query and attempt to dispute it early with a temporary, misleading data point, hoping voters are not diligent.

By approving this FLIP, the Factlink DAO provides a powerful tool but does not endorse and is not responsible for any specific question asked with it. Security rests on the clear construction of questions and the diligence of its users and voters.
