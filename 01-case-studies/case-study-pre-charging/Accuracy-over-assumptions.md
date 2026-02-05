# Case Study — Pre-Charging  
**Choosing accuracy over assumptions in product design**

---

## Context

### Problem Space

As part of the smart charging ecosystem, there was a need to provide charging-relevant data early enough to enable planning and optimization before charging execution.

This led to discussions around introducing a *pre-charging* concept: collecting vehicle and grid data ahead of time to reduce uncertainty and enable accurate charging plans.

The open question was not whether pre-charging was needed, but **how it should be designed**.

### Why This Was Hard

The problem space appeared deceptively simple.

At first glance, pre-charging looked like a data optimization problem.  
In reality, it was a **modeling, correctness, and trust problem**, with direct impact on:

- charging accuracy  
- user perception  
- system reliability  
- long-term product credibility  

A wrong abstraction would not fail loudly — it would silently degrade outcomes.

### Stakeholders

**Internal**
- Functional Owner  
- IT Product Owner  
- Solutions Architect  
- Product and engineering teams  

**External**
- Vehicle owners (indirect impact)  
- Energy providers relying on charging data accuracy  

---

## Constraints

### Real-World Variability

Charging behavior is highly sensitive to real-time conditions, including:
- vehicle and battery state
- charging infrastructure characteristics
- grid conditions at the moment of charging

Any solution assuming stability across sessions introduced significant risk.

### User Perception

The initial pre-charging execution was **perceivable by the user**, as it involved charging the vehicle at maximum available power.

This meant:
- pre-charging could not be treated as an invisible background process
- incorrect assumptions would immediately affect user trust

### Location Accuracy

The proposed approach relied on GPS-based location detection with an approximate radius of 100 meters.

As a result:
- different wallboxes could fall under the same location
- electrical characteristics could differ significantly
- inferred charging data could easily be incorrect

This made location-based reuse of charging parameters inherently unreliable.

---

## Options Considered

### Option — Last Known Settings per Location (Proposed)

The proposed solution worked as follows:
- the user plugs in the vehicle and selects optimized charging
- the backend immediately starts charging at maximum available power
- this initial charging phase exposes all required vehicle and grid parameters
- the backend stores these parameters together with the vehicle location
- for subsequent sessions at the same location, the backend reuses the stored data and skips pre-charging

**Why it was attractive**
- pre-charging needed to happen only once per location
- reduced repeated data collection
- simpler execution model over time

**Why it was risky**
- the first pre-charging session was user-visible
- incorrect assumptions would persist silently
- GPS-based reuse could infer wrong data
- changes in infrastructure or conditions would not be detected
- errors would compound rather than self-correct

Despite its apparent simplicity, this approach optimized for convenience over correctness.

---

## Decision

**We chose to prioritize data accuracy over reuse and optimization shortcuts, even at the cost of repeated pre-charging on every plug-in.**

The guiding product principle behind this decision was clear:

Providing the **most accurate possible data** to the energy provider (“creator”) is essential to:
- avoid deviations during SoC monitoring
- reduce renegotiations during charging execution
- protect the end-to-end charging outcome

In practice, inaccurate inferred data creates more operational noise than it saves:
- more deviations
- more renegotiations
- reduced trust from energy providers
- degraded user experience

From a product perspective, delivering a **fundamentally better service** has more long-term value than a fragile optimization.

Executing a pre-charging step on **every plug-in** ensured that:
- charging plans were based on real, current conditions
- deviations were minimized rather than corrected reactively
- both users and energy providers benefited from more predictable behavior

This decision deliberately traded execution efficiency for correctness, stability, and trust.

---

## Outcome

### Improved Charging Accuracy

Charging plans were consistently based on:
- current vehicle state
- actual grid and infrastructure conditions
- real-time data rather than inferred assumptions

This significantly reduced SoC deviations during execution.

### Fewer Renegotiations

By avoiding cached or inferred data:
- renegotiation frequency decreased
- charging execution became more predictable
- operational complexity shifted from reactive correction to proactive correctness

### Better Experience for Users and Energy Providers

For users:
- fewer unexpected charging adjustments
- higher confidence that the target SoC would be reached at departure time

For energy providers:
- clearer, more reliable inputs
- fewer corrective actions during charging sessions

Overall system trust improved as behavior became more consistent and explainable.

---

## What I’d Do Differently

- I would frame the problem earlier as **accuracy versus assumption reuse**, rather than as a technical optimization discussion.
- I would make the long-term cost of silent inaccuracies more explicit upfront, especially to non-technical stakeholders.
- I would introduce concrete failure scenarios earlier to illustrate how inferred data could degrade outcomes over time.

---

## Key Takeaways

- Optimization based on assumptions is dangerous in highly variable systems.
- Cached data can silently undermine product reliability.
- In outcome-driven products, **correctness beats efficiency**.
- Reducing renegotiations is often more valuable than reducing execution steps.
