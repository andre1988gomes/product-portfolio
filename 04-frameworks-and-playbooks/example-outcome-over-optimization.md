# Example - Applying Outcome Over Optimization

## Evidence Metadata

| Field | Value |
| --- | --- |
| Used in | [Pre-Charging](../01-case-studies/case-study-pre-charging/Accuracy-over-assumptions.md), [Controlled API / OCPX](../01-case-studies/case-study-controlled-api/smart-charging.md) |
| Decision supported | Reject assumption reuse when it improves apparent efficiency but weakens outcome reliability. |
| Evidence type | Framework application, Operational, Qualitative |
| Confidence level | High |
| Outcome influenced | Better trade-off framing and a stronger case for per-session validation. |

## Situation

A proposal suggested reusing previously observed charging parameters instead of validating conditions on every session.

The optimization appeared attractive because it would:

- reduce execution steps
- lower visible system complexity
- improve short-term delivery speed

## Risk Identified

Applying the outcome-over-optimization lens exposed the real problem: cached assumptions would create a silent failure mode.

If those assumptions became wrong:

- errors would appear late
- recovery options would narrow
- failures would be harder to explain to users and partners

## Decision Using The Framework

The decision was to:

- validate charging conditions on every session
- reject assumption reuse as the default strategy
- accept additional execution effort upfront
- postpone optimization until outcome reliability improved

## Trade-Off Accepted

The team accepted:

- higher short-term complexity
- slower visible optimization
- more interaction with dependent systems

In exchange for:

- earlier detection of deviations
- fewer late-stage failures
- better explainability and trust

## Principle Reinforced

In variable systems, optimization that weakens outcome reliability creates hidden cost that outweighs its short-term benefit.
