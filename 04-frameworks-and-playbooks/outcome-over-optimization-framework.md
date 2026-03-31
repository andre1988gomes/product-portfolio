# Outcome Over Optimization Framework

## Evidence Metadata

| Field | Value |
| --- | --- |
| Used in | [Controlled API / OCPX](../01-case-studies/case-study-controlled-api/smart-charging.md), [Pre-Charging](../01-case-studies/case-study-pre-charging/Accuracy-over-assumptions.md) |
| Decision supported | Prioritize outcome reliability and explainability before optimization when the system is highly variable. |
| Evidence type | Framework, Strategy, Operational |
| Confidence level | High |
| Outcome influenced | Better sequencing, clearer trade-offs, and stronger protection against silent failure modes. |

## Purpose

This framework helps decide when to prioritize outcome reliability over optimization, efficiency, or reuse in complex systems.

## Core Principle

A system that reliably reaches the desired outcome is usually more valuable than a system that looks optimized but behaves unpredictably.

## When To Use It

Use this framework when:

- outcomes depend on multiple interacting systems
- conditions vary between executions or sessions
- cached or inferred assumptions are tempting but risky
- failures surface late and are hard to recover from
- user trust depends on predictability, not just speed

## Decision Criteria

Ask:

1. Does this option increase the likelihood of reliably reaching the outcome?
2. Does it depend on assumptions being reused across variable conditions?
3. If it fails, will the failure be visible and recoverable?
4. Does it reduce downstream recovery effort?
5. Does it improve explainability for users or partners?

If optimization weakens these answers, it is probably too early.

## Trade-Offs Explicitly Accepted

Applying this framework means accepting:

- higher upfront complexity
- repeated validation or execution steps
- slower short-term delivery

In exchange for:

- fewer deviations and recoveries
- better predictability
- more trust from users and partners
- lower long-term hidden cost

## Signals To Revisit The Decision

Revisit the choice if:

- the system becomes much more predictable
- inputs can be safely reused without degradation
- recovery costs become low and visible
- optimization directly improves reliability
