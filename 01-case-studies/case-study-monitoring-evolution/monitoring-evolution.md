# Case Study - Monitoring Evolution
**Replacing a polling-based monitoring system with event-driven observability after operational evidence proved the original design was simultaneously too sensitive and too blind**

## Snapshot

| Field | Value |
| --- | --- |
| Role | Product Owner / Product Manager |
| Domain | EV charging platform monitoring and orchestration |
| Users / customers | Vehicle owners, energy providers, and internal operations teams |
| Scope / complexity | Backend monitoring architecture, cross-team ownership boundaries, dynamic deviation logic, multi-generation vehicle support |
| Timeframe | Decision emerged from operational evidence over multiple sprints; redesign executed across a focused initiative |
| Evidence type | Operational, Qualitative, Proxy |
| Key result | Replaced a static polling-based monitoring system with event-driven, context-aware deviation detection that reduced false renegotiations and improved late-session accuracy. |

## Problem And Why It Mattered

The original monitoring system checked vehicle state every fifteen minutes using a fixed five percent deviation threshold. If the actual state of charge deviated from the plan by more than five percent, the system triggered a renegotiation — requesting the energy provider to create a new charging plan.

Operational evidence revealed this design was broken in two directions simultaneously:

- **early in charging:** the five percent threshold was too tight. Small, expected fluctuations triggered unnecessary renegotiations, consuming the limited renegotiation budget and creating plan churn without meaningful benefit.
- **late in charging:** the five percent threshold was too loose. When the vehicle was near its target, a five percent deviation was significant enough to miss the target — but the system would not react until the next fifteen-minute poll.
- **during system events:** monitoring timers were not suspended during backend maintenance or system restarts, causing false renegotiations when the system came back online and processed stale deviation data.

This mattered because renegotiation was not free. Each renegotiation consumed relay cycle budget (a finite physical resource), required energy provider computation time, and introduced a window where the vehicle was executing an outdated plan. Unnecessary renegotiations degraded the very outcome the monitoring system was supposed to protect.

## User / Customer Context

Vehicle owners expected their vehicle to reach the target state of charge by departure time. They did not know that a monitoring system existed, and they certainly did not know it was triggering unnecessary plan changes.

The user impact was indirect but real:

- unnecessary renegotiations could consume the relay cycle budget, leaving the system unable to make corrections when they actually mattered
- late-session blindness meant the vehicle could miss the target because the system did not react to deviations that occurred between polling intervals
- plan churn created unpredictable charging behavior that was difficult to explain if a user noticed their vehicle's charging pattern changing without apparent reason

Energy providers experienced the problem as excessive renegotiation requests. Each request required their systems to compute and deliver a new plan. High-frequency renegotiation from the polling model created unnecessary load and reduced the signal-to-noise ratio: providers could not distinguish between genuine deviations and system noise.

## Business Context

Monitoring was a core product capability, not an operational afterthought. The product was accountable for charging outcomes, and monitoring was the mechanism that detected and corrected deviations during execution.

A monitoring system that generated false positives and missed real deviations simultaneously was a credibility risk:

- internally, it made the product look unreliable during reviews and testing
- operationally, it created unnecessary load on partner systems
- strategically, it consumed the renegotiation budget on noise rather than preserving it for genuine corrections

The business case for redesigning monitoring was defensive: without the change, the product would gradually lose the ability to protect charging outcomes as fleet scale increased.

## Constraints

- The renegotiation budget was finite and physically constrained — each renegotiation cycled a relay, and relays had a limited lifecycle.
- Energy providers had their own response time and computation constraints — renegotiation requests could not be fired at arbitrary frequency.
- The monitoring system had to work across multiple vehicle generations with different data update frequencies and reliability characteristics.
- Cross-team ownership meant the backend team owned deviation detection but the onboard software team owned the execution response — monitoring boundary changes required alignment.
- The replacement had to be backward-compatible with existing partner integrations and vehicle behavior.

## Options Considered

### Option A - Adjust the polling interval and threshold

This meant keeping the same architecture but tuning the parameters: shorter polling interval, adjusted threshold.

It was rejected because the fundamental problem was architectural, not parametric. No single fixed threshold could simultaneously avoid early-session false positives and catch late-session real deviations. Shorter polling intervals would increase system load without solving the directional mismatch.

### Option B - Event-driven monitoring with dynamic deviation ranges

This replaced polling with event-driven listening: the system would react to state changes as they occurred rather than checking on a fixed schedule. The deviation threshold would vary based on charging context: wider tolerance early in the session (when fluctuations are expected), narrowing progressively as the vehicle approached its target.

This was selected because it addressed both failure modes simultaneously and aligned the monitoring sensitivity with the actual risk profile of the charging session.

### Option C - Delegate monitoring to the energy provider

This would have shifted deviation detection to the partner side, making the provider responsible for detecting when their plan was no longer being followed.

It was rejected because:

- the product was accountable for the charging outcome, not the provider
- providers had inconsistent monitoring capabilities
- this would have weakened the product's ability to protect the user outcome

## Decision

> **We chose to replace the polling-based monitoring system with event-driven, context-aware deviation detection that adjusts sensitivity based on charging session progress.**

This decision explicitly prioritized:

- responsiveness over simplicity
- context-appropriate sensitivity over fixed thresholds
- real-time reaction over periodic checking

The new system used a dynamic deviation range: approximately five percent or wider tolerance at the start of charging, narrowing to approximately one percent as the vehicle approached the target state of charge. Renegotiation decisions included context about the deviation's magnitude, direction, and time remaining — not just whether a threshold was crossed.

## How I Led It

I framed this as a product quality decision driven by operational evidence, not an engineering improvement request.

My contribution was to:

- surface the operational evidence that the existing monitoring was simultaneously too sensitive and too blind, using specific defect patterns and renegotiation data
- frame the redesign around outcome protection rather than system optimization: "we are not making monitoring better; we are fixing the mechanism that protects the charging promise"
- define the monitoring boundary with the onboard software team: which deviations trigger backend renegotiation versus onboard correction?
- push for monitoring timer suspension during system events, after a specific defect revealed that unsuspended timers were causing false renegotiations on restart
- ensure the new monitoring logic was tested against multi-generation vehicle data to validate that dynamic thresholds worked across different data update frequencies

The hardest part was the cross-team boundary. The backend team wanted to own all deviation detection. The onboard team had their own renegotiation logic. I pushed for an explicit interface definition: the backend detects state-of-energy deviations; the onboard software handles power-level adjustments. This prevented duplicate or conflicting reactions to the same underlying deviation.

## Execution And Trade-Offs

The monitoring evolution changed the system's operating model:

- polling was replaced with event listeners that reacted to vehicle state changes in near-real-time
- the fixed five percent threshold was replaced with a dynamic range that narrowed as charging progressed
- renegotiation triggers now included contextual information: deviation magnitude, direction, session progress, and time to departure
- monitoring timers were updated to suspend during system maintenance and restart cleanly

What was deliberately not done:

- no attempt to optimize for minimum renegotiation count — the goal was appropriate renegotiation, not fewer renegotiations
- no delegation of monitoring responsibility to partners
- no attempt to build the event-driven system as an add-on to polling — the architectural change was a replacement, not a supplement

The most significant delivery cost was validation effort. The dynamic threshold logic had to be tested against real charging session data from multiple vehicle generations to ensure it did not introduce new failure modes. Edge cases at transition points between threshold levels required careful tuning.

## Results And Evidence

### User / customer impact

Charging sessions became more predictable in the critical final phase. The system could now detect and correct deviations as they happened rather than waiting up to fifteen minutes. Early-session plan churn was reduced because expected fluctuations no longer triggered unnecessary renegotiations.

### Business / operational impact

Renegotiation budget consumption became more purposeful — corrections happened when they mattered rather than when a timer fired. Partner systems experienced lower request noise, improving the signal quality of renegotiation requests. Operationally, monitoring failures became easier to diagnose because event-driven triggers carried contextual information that polling-based anomalies did not.

### Leadership / influence impact

The initiative demonstrated that monitoring design is a product decision, not an engineering implementation detail. The explicit cross-team monitoring boundary became a template for other interface ownership discussions. The use of operational evidence to justify architectural change — rather than theoretical arguments about system design — set a precedent for data-informed technical decisions.

### Evidence notes

- Operational evidence: specific defect where the static threshold was simultaneously too loose and too tight, validated the need for dynamic sensitivity
- Operational evidence: monitoring timers not suspending during system events, causing false renegotiations — fixed as part of the redesign
- Proxy evidence: reduced unnecessary renegotiations preserved relay cycle budget for genuine corrections
- Qualitative evidence: cross-team monitoring boundary formally documented and agreed

## What I Would Do Differently

- I would implement event-driven monitoring from the start rather than beginning with a polling model and evolving it later. The polling model was a reasonable starting point, but the transition cost was higher than building event-driven logic from day one.
- I would invest in a unified event streaming pipeline early. The monitoring evolution was constrained by the lack of a single stream that captured all relevant vehicle and backend events. Having that pipeline would have made the redesign significantly faster.
- I would define monitoring success criteria upfront: what is the acceptable false renegotiation rate? What is the maximum acceptable detection latency for a deviation near the target? These criteria would have provided clearer validation targets during testing.
- I would surface the renegotiation budget as a product metric earlier. Making the relay cycle budget visible to stakeholders would have created urgency for the monitoring redesign sooner than waiting for defect evidence to accumulate.

## Linked Supporting Artifacts

- [Case study - Controlled API / OCPX](../case-study-controlled-api/smart-charging.md)
- [Case study - Pre-Charging / Accuracy over assumptions](../case-study-pre-charging/Accuracy-over-assumptions.md)
- [Framework - Outcome over optimization](../../04-frameworks-and-playbooks/outcome-over-optimization-framework.md)
