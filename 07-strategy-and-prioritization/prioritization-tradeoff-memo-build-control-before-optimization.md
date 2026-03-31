# Prioritization Trade-Off Memo - Build Control Before Optimization

## Evidence Metadata

| Field | Value |
| --- | --- |
| Used in | [Controlled API / OCPX](../01-case-studies/case-study-controlled-api/smart-charging.md), [Pre-Charging](../01-case-studies/case-study-pre-charging/Accuracy-over-assumptions.md) |
| Decision supported | Prioritize reliability, ownership, and explainability before optimization or convenience. |
| Evidence type | Strategy, Operational, Proxy |
| Confidence level | High |
| Outcome influenced | Better sequencing decisions, clearer non-goals, and lower silent-risk exposure. |

## Decision

In variable systems, the product should usually invest first in control, observability, and correctness before investing in optimization.

## Context

Optimization often looks attractive because it is visible:

- fewer steps
- faster execution
- less obvious complexity

But if the system remains difficult to predict, those gains often sit on top of weak foundations.

## Prioritization Logic

I use the following sequencing rule:

1. Protect the outcome.
2. Own the critical path where feasible.
3. Improve observability and explainability.
4. Optimize once the system is predictable enough that optimization does not create hidden cost.

## What This Prevents

Without this rule, teams tend to prioritize what is easiest to explain in a roadmap review:

- visible convenience
- lower initial delivery cost
- thinner ownership boundaries

Those choices often defer the harder but more valuable work of reducing silent failure modes.

## What This Enables

Prioritizing control before optimization creates better conditions for:

- stronger customer trust
- safer partner interactions
- fewer reactive corrections
- more credible optimization later

## Practical Test

Before prioritizing an optimization-heavy item, ask:

- Does this improve the reliability of the outcome?
- Does it reduce or increase assumption risk?
- If it fails, will the failure be visible and recoverable?
- Are we optimizing a stable system or hiding an unstable one?

If the answers are weak, the investment is probably sequenced too early.
