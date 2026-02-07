## Example — Applying Outcome Over Optimization

### Situation

A proposal suggested optimizing the charging execution flow by **reusing previously observed charging parameters** instead of validating conditions on every session.

The optimization aimed to:
- reduce execution steps
- shorten perceived response time
- lower short-term implementation complexity
- minimize interaction with external dependencies

At first glance, the approach appeared efficient and aligned with performance goals.

---

### Why Optimization Was Tempting

The optimization was attractive because:
- charging conditions often looked stable across sessions
- failures were relatively infrequent in happy-path scenarios
- skipping validation reduced visible system complexity
- short-term delivery would be faster

From a local perspective, the system appeared “good enough”.

---

### Risk Identified

Applying the **Outcome Over Optimization** lens revealed a critical risk:

Reusing inferred or cached parameters introduced a **silent failure mode**.

If assumptions became invalid:
- errors would surface late in the charging session
- recovery options would be limited
- renegotiation would be harder or impossible
- failures would be difficult to explain to users and partners

The optimization reduced visibility into correctness precisely where predictability mattered most.

---

### Decision Using the Framework

Using the framework, the decision was to:
- validate charging conditions on **every session**
- reject assumption reuse as a default strategy
- accept additional execution steps upfront
- postpone optimization until outcome reliability was proven

The decision explicitly prioritized **predictable outcomes** over execution efficiency.

---

### Trade-off Accepted

This choice accepted:
- higher initial implementation complexity
- slower short-term delivery
- increased interaction with dependent systems

In exchange for:
- earlier detection of deviations
- fewer late-stage failures
- simpler recovery paths
- improved explainability and trust

---

### Resulting Principle Reinforced

This example reinforced the core principle of the framework:

> **In variable systems, optimization that weakens outcome reliability creates hidden cost that outweighs its short-term benefits.**
