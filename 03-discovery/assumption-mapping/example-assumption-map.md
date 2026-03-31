# Example - Assumption Mapping For Smart Charging Orchestration

## Evidence Metadata

| Field | Value |
| --- | --- |
| Used in | [Controlled API / OCPX](../../01-case-studies/case-study-controlled-api/smart-charging.md), [Pre-Charging](../../01-case-studies/case-study-pre-charging/Accuracy-over-assumptions.md) |
| Decision supported | Prioritize current validated data and reliable orchestration over assumption reuse and thin ownership. |
| Evidence type | Discovery artifact, Qualitative, Proxy |
| Confidence level | Medium to High |
| Outcome influenced | Focus of discovery, validation strategy, and prioritization of the riskiest assumptions. |

## Identified Assumptions

### User And Behavior

- Users value reliable automated outcomes over manual control.
- Users will trust the system to reach a target outcome without supervision.

### Value And Outcomes

- Reaching the target state of charge consistently matters more than minimizing cost variability.
- Users notice and care about failed or late charging outcomes.

### Feasibility And Constraints

- Vehicle and infrastructure behavior is predictable enough within a single session.
- Real-time data is more reliable than inferred or cached data.

### Ecosystem And Stakeholders

- External providers can respond to renegotiation within acceptable bounds.
- Better input quality reduces renegotiation frequency.

## Classification

| Assumption | Certainty | Impact | Classification |
| --- | --- | --- | --- |
| Users trust automated outcomes | Medium | High | Critical |
| Target state of charge matters more than cost variance | Medium | High | Critical |
| Real-time data reduces deviations | Medium | High | Critical |
| Providers can renegotiate in time | Low | High | Critical |
| Cached data is sufficient | Low | High | Critical and rejected |

## Highest-Risk Assumption

Providing accurate, current data to external partners will reduce deviations and renegotiations enough to justify the extra complexity.

## Disconfirming Signals

The assumption would weaken if:

- renegotiation frequency stayed unchanged
- deviations continued despite higher-quality inputs
- partners could not use the better data effectively

It would strengthen if:

- deviations decreased
- renegotiations became less frequent or more explainable
- partners reported better predictability

## Discovery Activities

- compare sessions using inferred data versus current validated data
- review renegotiation causes before and after stronger validation
- collect partner feedback on data usefulness and explainability

## Decision Outcome

The assumption was supported strongly enough to prioritize correctness over optimization shortcuts.
