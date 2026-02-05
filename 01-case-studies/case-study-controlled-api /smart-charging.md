# Case Study — Controlled API / OCPX  
**Owning End-to-End Charging Execution in an Unpredictable System**

---

## Context

The Controlled API was created to address multiple, interrelated problems across user experience, business control, and ecosystem governance.

### 1. User Problem — Smart Charging as a “Plug & Forget” Experience

EV users expect smart charging to be effortless and cost-efficient.  
The goal was to enable a *plug & forget* experience where users can:

- select **Optimized Charging Mode**
- choose an **energy provider**
- define **target State of Charge (SoC)** and **departure time**

At the planned departure time, the vehicle should be charged to the target SoC, while minimizing charging costs based on energy tariffs.

Delivering this experience requires reliable coordination between vehicles, backend systems, and external energy providers.

### 2. Business Problem — Loss of Control and Monetization

Before the Controlled API, energy providers were directly controlling vehicles via the Vehicle App API, without governance or monetization.

This resulted in:
- lack of control over third-party vehicle operations
- direct financial loss for the company

The Controlled API reintroduced control and enabled a monetization model, allowing BMW to charge energy providers per charging session.

### 3. Platform Role — Controlled API as Charging Orchestrator

The Controlled API acts as the backend orchestrator for smart charging, owning the execution flow rather than acting as a passive integration layer.

Its responsibilities include:
- detecting when a vehicle is plugged in (plug-in as the trigger)
- notifying the selected energy provider that the vehicle is ready for charging
- providing accurate charging and calculation data
- receiving and validating charging plans aligned with tariffs and constraints
- continuously monitoring the vehicle State of Charge (SoC) during the charging session
- detecting deviations from the agreed charging plan, including:
  - SoC deviations  
  - changes in user settings or external conditions
- triggering renegotiations with energy providers whenever deviations occur, regardless of root cause

The primary objective is to **guarantee that the vehicle reaches the user-defined target SoC at the planned departure time**, even under changing conditions.

This positions the Controlled API as an **active control and governance layer**, not merely a command relay.

### 4. Strategic Extension — Internal Charging Plan Creation

To avoid full dependency on external energy providers, the platform can also generate its own charging plans.

For specific use cases:
- Internal systems access market tariff data
- charging plans are generated internally
- plans aim to deliver the best cost–benefit outcome for the user

This increases resilience and long-term strategic flexibility.

### Stakeholders

**External**
- Vehicle owners  
- Energy providers (10+ across Europe)

**Internal**
- Functional Owner  
- Other components in the functional chain  
- Other internal teams

---

## Constraints

The most critical constraint was not any single limitation, but the **unpredictable combination and interdependence of all constraints across the system**.

Charging plans could only be executed successfully if **all layers behaved within acceptable bounds simultaneously**, which was rarely guaranteed.

### Systemic Interdependence

Successful execution depended on:
- vehicle-specific behavior (model and generation differences)
- backend reliability (Vehicle state, Remote calls, other APIs)
- physical and mechanical constraints (battery behavior, charging hardware)

A failure in any layer could invalidate an otherwise correct plan.

### Inability to Isolate Dependencies

It was not possible to abstract away problematic dependencies:
- vehicle behavior could not be standardized quickly
- backend systems could not be treated as consistently reliable
- physical constraints could override digital intent at any time

A simplified execution model was therefore not viable.

### Risk-Driven Decision Making

This context required:
- cutting non-essential dependencies
- reducing coupling wherever possible
- making explicit, risk-based trade-offs instead of aiming for theoretical completeness

Decisions prioritized:
- outcome reliability (reaching target SoC)
- system resilience under partial failure
- controlled degradation over full failure

---

## Options Considered

### Option — Reuse Existing Data Streams and Services

One obvious alternative was to:
- rely on charging data from existing backend services
- reuse Kinesis streams owned by other teams instead of creating a dedicated one

**Short-term benefits**
- faster development
- lower initial complexity
- avoidance of uncertainty related to building and operating a new stream

**Why it was rejected**
This option introduced unacceptable risks:
- dependency on other teams for availability and prioritization
- loss of control over data semantics, configuration, and evolution
- slower reaction to incidents and product-driven changes
- increased coordination overhead with misaligned incentives

Given the importance of real-time charging state, delegating ownership of this dependency would have compromised the product’s ability to guarantee its primary outcome.

---

## Decision

**We chose to own the charging execution loop end-to-end, where possible, even at the cost of higher initial complexity, in order to preserve outcome reliability and long-term autonomy.**

This included owning critical data streams, monitoring logic, and renegotiation mechanisms.

### Decision Under Stakeholder Pressure — Pre-Charging

One of the most controversial decisions concerned *pre-charging*.

The Functional Owner proposed a solution based on **“last known settings at a specific location”**, where:
- vehicle and grid data would be collected during the first charging session
- the same data would be reused for subsequent sessions at that location

I opposed this approach.

Together with the team, we argued that:
- vehicle behavior and charging conditions are too variable to infer safely from historical snapshots
- relying on “last known settings” would introduce silent inaccuracies
- deviations would surface late, undermining trust in the system

We proposed an alternative *pre-charging* approach focused on:
- collecting and validating charging-relevant data closer to execution time
- prioritizing accuracy over convenience
- accepting higher complexity to protect outcomes

This decision was initially unpopular, but aligned with the core product goal: **guaranteeing target SoC under real-world conditions**.

---

## Outcome

### Increased Stability and Predictability

System stability improved significantly.

Owning the execution loop allowed faster reactions to deviations and proactive adjustments, which was critical in an environment dominated by unpredictability.

### Stronger Operational Control

Full ownership enabled:
- frequent Kinesis reconfiguration
- evolution of monitoring and renegotiation logic
- faster product-driven iteration

Relying on another team for these changes would have introduced friction and delays.

### Limited Impact on Stakeholder Clarity

Technical autonomy did not automatically improve stakeholder alignment or communication, highlighting that governance and communication require explicit effort beyond architecture.

### Trade-offs and Cost

The main cost was time:
- one to two sprints invested in building and stabilizing the dedicated stream
- slower progress on other tasks in the short term

This cost was considered acceptable given the long-term risks avoided.

### Risks Avoided

The decision avoided:
- long-term dependency on external teams
- slower incident response
- loss of flexibility to adapt the system to product needs

---

## What I’d Do Differently

### Make Vehicle Constraints Explicit Earlier

I would surface vehicle technical limitations earlier, especially:
- behavioral differences between models and generations
- the difficulty of validating assumptions without sufficient access to real vehicles

### Explicit Risk Framing

I would invest more upfront in:
- documenting accepted vs non-negotiable risks
- making trade-offs explicit earlier to stakeholders

### Earlier Alignment on Responsibilities and Disagreements

Although stakeholder relationships were well managed, I would:
- surface disagreements earlier
- give feedback sooner on decisions I did not fully agree with

### Cut Certain Dependencies Earlier

One dependency should have been removed sooner:
- reliance on a third team for plug-in and plug-out events

Although it was cut before production, doing so earlier would have reduced uncertainty sooner.

### Signals Recognized Too Late

Early signals pointing to the dominance of **real-world vehicle behavior** were initially underestimated.

Recognizing this earlier would have shifted focus sooner from optimization to resilience and tolerance.
