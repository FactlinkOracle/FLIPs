_A FLIP number will be assigned by an editor. When opening a pull request, please use an abbreviated title in the filename, e.g., "flip_add_querytypename.md". Please remove this and all italicized instructions before submitting. All bolded fields should be filled in._

## Headers

| FLIP           |                                                   |
| -------------- | ------------------------------------------------- |
| FLIP Title     | Add **Query Type Name** as a supported query type |
| Authors        | **Name**                                          |
| Status         | Draft                                             |
| Created        | **Today's Date**                                  |
| Discourse Link | **[Link to FLIP Discourse Channel in Discord]**   |

# Summary

This FLIP proposes whitelisting the **Query Type Name**, which will enable the Optimistic Oracle to handle requests about **[Summarize the purpose of this query type]**. The `query_type_identifier` will be the Keccak-256 hash of "**Query Type Name**".

# Motivation

_Please explain why you want to add this query type. What types of products or use cases does this enable? Why is it valuable to the Factlink ecosystem?_

# Implementation for Voters

_This section should guide DVM voters on how to resolve a price request of this type if it gets disputed and escalated to the DVM._

When a query of this type is escalated to the Factum DVM, voters will receive a request containing the `query_ipfs_cid` from the original Optimistic Oracle query. Voters must:

1.  Retrieve the full query details from the IPFS CID.
2.  Analyze the query and determine the correct answer based on the specification below.
3.  The DVM expects a `u8` value as the final answer. Define the mapping from real-world outcomes to the `u8` value. For example:
    - `0` for "No" or "False".
    - `1` for "Yes" or "True".
    - `2` for "Outcome A".
    - `3` for "Outcome B".
    - ...etc.
4.  Commit and reveal this `u8` value during the DVM voting rounds.

## Data Specifications

_How should voters access the data necessary to resolve this query type? What specific data sources, APIs, or methodologies should be referenced to ensure a deterministic outcome?_

- **Primary Data Source(s):** [e.g., official API from a specific project, a specific sports league's website, a government data portal]
- **Fallback Data Source(s):** [If the primary is unavailable]
- **Resolution Logic:** [Describe the exact steps to take. Example: "Query the API at `timestamp`. Check the `status` field. If `SUCCESS`, vote 1. If `FAILURE`, vote 0."]

# Security Considerations

_What are the risks associated with this query type?_

- _Data Source Reliability:_ Is the source robust? Can it be manipulated?
- _Ambiguity:_ Is the query type defined clearly enough to prevent ambiguity? Could multiple valid interpretations exist?
- _Oracle Manipulation:_ How could this query type be used to attack a system that relies on it?
