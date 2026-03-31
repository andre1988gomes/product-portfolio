# Example - Using Data To Reframe The Product Problem

## Evidence Metadata

| Field | Value |
| --- | --- |
| Used in | [Controlled API / OCPX](../01-case-studies/case-study-controlled-api/smart-charging.md) |
| Decision supported | Shift focus from charging optimization depth toward execution reliability and recovery capability. |
| Evidence type | Data analysis, Operational, Proxy |
| Confidence level | Medium to High |
| Outcome influenced | Prioritization, problem framing, and investment in monitoring over optimization. |

## Decision To Be Informed

Should product effort focus on more charging-cost optimization and configuration depth, or on improving charging outcome reliability and predictability?

## Initial Assumption

The starting belief was that:

- users primarily valued cost optimization
- better optimization logic would create more perceived value
- configuration flexibility was a meaningful differentiator

## Data Signals Considered

Instead of focusing on broad usage metrics, the analysis examined signals connected to value loss:

- frequency of charging sessions that missed the expected outcome
- distribution of deviations over time during the session
- relationship between deviations and later recovery attempts
- support and operational events tied to failed or unexpected charging behavior

## What The Data Showed

The data suggested that:

- a small number of failure modes drove a disproportionate share of negative outcomes
- late-session deviations were significantly harder to recover from
- optimization improvements had limited value when execution itself was unstable
- recovery mechanisms were being used often enough to indicate systemic fragility

## Insight

The primary source of value loss was not weak optimization. It was unreliable execution under changing conditions.

## Decision

The product focus shifted toward:

- earlier detection of deviations
- better execution monitoring
- recovery and renegotiation mechanisms
- correctness and predictability before further optimization

## Why This Matters

This example belongs in the portfolio because the analysis did not merely support a known plan. It changed the problem framing itself.
