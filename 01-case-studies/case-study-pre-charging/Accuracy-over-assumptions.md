# Case Study - Pre-Charging
**Choosing accuracy over assumption reuse in a system where optimization errors would show up as silent customer harm**

## Snapshot

| Field | Value |
| --- | --- |
| Role | Product Owner / Product Manager |
| Domain | Smart charging product design and orchestration |
| Users / customers | Vehicle owners, energy providers, and internal charging stakeholders |
| Scope / complexity | Location ambiguity, variable charging conditions, visible user impact, and cross-functional disagreement on the right abstraction |
| Timeframe | Decision made during product design before scaling the approach |
| Evidence type | Operational, Qualitative, Proxy |
| Key result | Rejected location-based assumption reuse in favor of session-based validation, improving correctness and reducing silent degradation risk. |

## Problem And Why It Mattered

The team needed a way to gather charging-relevant information early enough to support planning before charging execution. At first glance, this looked like an optimization problem: capture data once, reuse it later, and reduce repeated work.

In reality, it was a product correctness problem. If the product reused outdated or inferred charging conditions, it would not fail loudly. It would fail by producing worse outcomes over time while still looking efficient on the surface.

That mattered because the user promise was outcome-based, not process-based. If the wrong assumptions fed the plan, the product would gradually lose trust with both drivers and energy partners.

## User / Customer Context

Vehicle owners cared about whether the charging outcome was correct and predictable. They did not care whether the system looked efficient internally if it occasionally delivered the wrong result.

Energy providers depended on the accuracy of the inputs they received. Poor upstream assumptions increased the chance of:

- incorrect charging plans
- late renegotiation
- harder-to-explain execution behavior

The user-facing wrinkle was important: the initial pre-charging behavior was visible to the driver because it involved charging at maximum available power. That made trust especially sensitive.

## Business Context

The product needed a design that could scale without slowly degrading reliability. A "save the last known settings by location" model looked attractive because it reduced repeated work and created a cleaner execution story.

The business risk was that convenience would mask correctness issues:

- the product would appear more optimized
- teams would spend less time validating inputs upfront
- but more time would later be spent explaining deviations, renegotiations, and trust erosion

This was therefore a product design decision with operational and partner implications, not just an implementation preference.

## Constraints

Several realities made assumption reuse risky:

- charging behavior depends on real-time vehicle, battery, and infrastructure conditions
- the visible pre-charging step directly affects user perception
- location detection was approximate, which meant multiple wallboxes could be treated as the same place
- charging characteristics could vary even at the same nominal location

The core constraint was that stability across sessions could not be assumed safely enough to become the default design.

## Options Considered

### Option A - Reuse last known settings by location

This option looked efficient because it reduced repeated data collection and made the system appear smoother after the first session.

It was rejected because:

- the initial assumptions could be wrong
- errors would persist silently
- GPS-level location reuse was too coarse for reliable charging behavior
- infrastructure or real-time conditions could change between sessions

### Option B - Validate conditions on every session

This required more repeated work and a less elegant short-term story, but it protected correctness and reduced the chance of hidden degradation. It was selected.

## Decision

> **We chose to validate charging-relevant conditions on every plug-in session rather than reuse inferred or location-based assumptions by default.**

This explicitly prioritized:

- accuracy over convenience
- outcome reliability over execution efficiency
- explainability over optimization theater

## How I Led It

I worked to reframe the discussion from "how do we optimize pre-charging?" to "what design best protects the product outcome?"

That involved:

- challenging the idea that fewer execution steps automatically meant a better product
- surfacing the long-term cost of silent inaccuracy, especially for non-technical stakeholders
- tying the design choice back to the user promise and partner trust, not just technical elegance
- supporting an unpopular but safer path when the more convenient option had intuitive appeal

## Execution And Trade-Offs

Choosing session-based validation changed the design philosophy:

- current session conditions became the default source of truth
- assumption reuse became something to earn, not something to assume
- optimization work was deferred until correctness was stronger

The decision accepted:

- more implementation effort
- repeated validation steps
- a less optimized short-term interaction model

In exchange, it reduced the need for reactive correction later in the charging flow.

## Results And Evidence

### User / customer impact

The charging plan was more consistently based on current conditions, which improved predictability for vehicle owners. The user impact is best described through proxy and operational evidence: fewer situations depended on stale assumptions, and charging behavior became easier to explain.

### Business / operational impact

The decision reduced the likelihood of avoidable renegotiations and protected partner trust by improving input quality. Operationally, it shifted effort from reactive correction toward proactive correctness.

### Leadership / influence impact

This case made trade-offs explicit across product, architecture, and functional leadership. It also demonstrated the importance of resisting locally attractive optimization when the system is too variable for those assumptions to hold.

### Evidence notes

- Operational evidence: reduced dependence on downstream correction when current inputs were validated
- Proxy evidence: stronger predictability and lower silent-risk exposure compared with location-based assumption reuse
- Qualitative evidence: better alignment on why correctness was a product requirement rather than an engineering preference

## What I Would Do Differently

- I would frame the choice earlier as accuracy versus assumption reuse so the real trade-off was visible from the start.
- I would use concrete failure scenarios sooner to show how cached assumptions could silently degrade outcomes over time.
- I would make the user-visible impact of the first pre-charging behavior more explicit earlier in stakeholder discussions.

## Linked Supporting Artifacts

- [Example - Assumption mapping for smart charging orchestration](../../03-discovery/assumption-mapping/example-assumption-map.md)
- [Example - Using a prototype to reduce decision risk](../../06-prototypes-and-validations/example-prototype-to-reduce-decision-risk.md)
- [Example - Communicating a trade-off decision](../../02-product-documentation/backlog-artifacts/alignment-docs/example-tradeoff-communication.md)
- [Framework - Outcome over optimization](../../04-frameworks-and-playbooks/outcome-over-optimization-framework.md)
- [Example - Applying outcome over optimization](../../04-frameworks-and-playbooks/example-outcome-over-optimization.md)
- [KPI and success signals](../../07-strategy-and-prioritization/kpi-and-success-signals.md)
