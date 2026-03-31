# Decision Record - Owning The Charging Execution Data Stream

## Evidence Metadata

| Field | Value |
| --- | --- |
| Used in | [Controlled API / OCPX](../../01-case-studies/case-study-controlled-api/smart-charging.md) |
| Decision supported | Own the charging execution data stream instead of relying on another team for a critical dependency. |
| Evidence type | Decision record, Operational, Strategy |
| Confidence level | High |
| Outcome influenced | Faster incident response, stronger autonomy, and clearer ownership of the charging loop. |

**Status:** Accepted  
**Date:** Redacted  
**Decision Owner:** Product Owner / Product Manager

## Context

Reliable visibility into charging state was required to:

- monitor execution
- detect deviations from the planned charging behavior
- trigger renegotiation when needed

An early decision had to be made between reusing existing streams owned by other teams or creating a dedicated stream shaped around charging execution needs.

## Decision Drivers

- outcome reliability
- response speed when the charging session diverges
- control over data semantics and configuration
- lower dependency drag
- long-term platform autonomy

## Options Considered

### Option A - Reuse existing OEM data streams

Why it was viable:

- lower initial implementation cost
- faster first delivery
- less operational ownership in the short term

Key risks:

- dependence on other teams' priorities
- limited control over change cadence and semantics
- slower response to product-driven changes and incidents

### Option B - Own a dedicated charging data stream

Why it was viable:

- stronger control over data behavior
- faster evolution of monitoring and renegotiation logic
- direct ownership of a product-critical path

Key risks:

- more implementation work upfront
- more operational responsibility
- short-term delivery delay elsewhere

## Decision

We chose to own a dedicated charging execution stream and accept higher short-term complexity to preserve reliability and product autonomy.

## Consequences

### Benefits gained

- stronger control over a critical dependency
- faster response to incidents and product change
- less coordination overhead for data-related evolution

### Costs accepted

- roughly one to two sprints of additional effort
- ongoing responsibility for operation and evolution

### Risks introduced

- the product team must maintain the ownership discipline needed to justify this investment

## Notes

This decision should be revisited only if alternative data sources reach similar reliability and control standards without recreating the same dependency risk.
