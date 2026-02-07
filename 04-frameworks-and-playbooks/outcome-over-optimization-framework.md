# Outcome Over Optimization Framework

## Purpose

This framework guides product decisions in **complex, variable systems** where:
- outcomes matter more than process efficiency
- incorrect assumptions are costly
- recovery from failure is harder than preventing it

It helps decide **when to prioritize outcome reliability over optimization, efficiency, or reuse**.

---

## Core Principle

**A system that reliably reaches the desired outcome is more valuable than a system that is optimized but unpredictable.**

Efficiency gains that compromise outcome reliability create hidden cost and long-term erosion of trust.

---

## When to Use This Framework

Use this framework when:
- outcomes depend on multiple interacting systems
- conditions vary between executions or sessions
- inferred or cached assumptions are tempting but risky
- failures surface late and are hard to recover from
- user trust depends on predictability, not speed

Typical signals:
- frequent recovery logic or renegotiations
- difficulty explaining system behavior to stakeholders
- growing gap between “expected” and “actual” outcomes

---

## When NOT to Use This Framework

Do not use this framework when:
- the problem space is stable and repeatable
- inputs can be trusted across executions
- failures are cheap, visible, and reversible
- optimization directly improves user outcomes

Applying this framework indiscriminately leads to over-engineering.

---

## Decision Criteria

When evaluating options, ask:

1. Does this option increase the likelihood of consistently reaching the desired outcome?
2. Does it rely on inferred, cached, or reused assumptions?
3. If it fails, does it fail loudly or silently?
4. Does it reduce downstream recovery effort?
5. Does it improve explainability to users or partners?

If optimization weakens answers to these questions, it should be deferred.

---

## Trade-offs Explicitly Accepted

By applying this framework, you accept:
- higher upfront complexity
- repeated validation or execution steps
- slower short-term delivery

In exchange for:
- fewer deviations and recoveries
- higher predictability
- increased trust from users and partners
- lower long-term maintenance cost

---

## Common Failure Modes

This framework fails when:
- applied to low-risk, low-variability problems
- optimization opportunities are ignored without evidence
- correctness is pursued beyond what outcomes require
- stakeholders are not aligned on the accepted trade-offs

---

## Signals to Revisit the Decision

Re-evaluate this framework if:
- system behavior becomes significantly more predictable
- inputs can be reliably reused without degradation
- recovery costs become negligible
- optimization directly improves outcome reliability

---

## Related Case Studies

- **Charging Orchestration** — prioritizing reliable target outcomes over cached optimization data  
- **Pre-Charging Decision** — rejecting inferred location-based assumptions in favor of session-based accuracy  

This framework emerged directly from decisions documented in those cases.
