# Case Study - Degradation Strategy
**Shipping a reliability trade-off across twenty tickets to keep older vehicles steerable when the primary communication channel failed**

## Snapshot

| Field | Value |
| --- | --- |
| Role | Product Owner / Product Manager |
| Domain | EV charging platform with multi-generation vehicle support |
| Users / customers | Vehicle owners with older-generation vehicles, energy providers, and internal charging teams |
| Scope / complexity | Cross-team coordination, multiple vehicle generations, alternative communication channels with different reliability profiles, phased model-range rollout |
| Timeframe | Multi-sprint execution with phased vehicle model additions |
| Evidence type | Operational, Qualitative, Proxy |
| Key result | Enabled charging control for an otherwise unsteerable vehicle generation by implementing a fallback communication path, accepting a known reliability trade-off rather than leaving users without service. |

## Problem And Why It Mattered

A newer generation of vehicles could not be steered through the primary communication channel due to vehicle software limitations. These vehicles had the hardware capability for smart charging, but the software path that the backend relied on to send charging commands did not work for them.

Without intervention, these vehicles would join the fleet as "registered but unsteerable" — visible in the system, connected to energy providers, but unable to receive or execute optimized charging plans. Users would see the system promise smart charging, but the vehicle would charge on its default behavior.

This mattered because:

- leaving a vehicle generation unsupported would undermine user trust across the entire fleet
- the vehicle software fix was not in the product team's control and had no committed timeline
- the fleet was growing — every month without a solution meant more vehicles in this state
- operational monitoring had revealed that a significant portion of connected vehicles were already not being steered, and this generation would make that gap worse

This was not a feature request. It was a product credibility problem: the system was accountable for an outcome it could not deliver for this vehicle generation.

## User / Customer Context

Vehicle owners expected smart charging to work when they enrolled. They did not know which communication channel the backend used. They only cared about the result: was the vehicle charged to the target level by departure time?

For energy providers, these vehicles were part of the addressable fleet. Plans would be created for them, but without a working control channel, those plans would fail silently. Partners would see unexplained failures without understanding the root cause.

The hardest part of the user context was invisibility. Unless the system explicitly told the user that their vehicle was not being steered, they would assume it was working — and trust would erode gradually rather than through a single visible failure.

## Business Context

The business had committed to fleet-wide smart charging coverage as a product capability. A vehicle generation that could not be steered was a gap in that promise that would be visible to partners and to leadership.

The business context also included:

- upcoming expansion into new markets that would use these vehicle generations
- partner negotiations where fleet coverage was a key metric
- Go-Live timelines where the product would be accountable for fleet-wide steering capability

Doing nothing was not a neutral choice. It was a decision to accept a growing coverage gap.

## Constraints

- The primary communication channel could not be fixed from the backend; it required a vehicle software update with no committed delivery date.
- An alternative communication channel existed but was known to be slower and less reliable.
- The alternative channel had different latency characteristics, different failure modes, and different monitoring requirements.
- Cross-team coordination was required because the monitoring boundary between backend and onboard software needed to be explicitly redefined for this path.
- Each vehicle model range within the generation had slightly different capabilities, requiring phased rollout rather than a single switch.

## Options Considered

### Option A - Wait for the vehicle software fix

This was attractive because it would have solved the root cause and avoided the complexity of supporting an alternative path.

It was rejected because:

- the fix had no committed timeline
- the fleet was growing every month
- the product would be accountable for the gap in the meantime
- user trust was being quietly eroded with every unsteered session

### Option B - Implement the alternative communication channel as a fallback

This meant accepting a known reliability trade-off: the alternative channel was slower and less reliable, but it worked for this vehicle generation. Charging commands would reach the vehicle through a different path, with different failure characteristics.

This option required significant investment: payload mapping, new interfaces, notification integration, monitoring logic, and phased model-range rollout. Approximately twenty tickets across bug fixes, new interfaces, and operational infrastructure.

### Option C - Accept the gap and communicate it to users

This would have meant explicitly telling users with this vehicle generation that smart charging was not available for them.

It was rejected because:

- the alternative channel existed and was technically viable
- accepting the gap when a workaround was available would signal that the product team was not willing to invest in coverage
- the coverage gap would grow over time as more vehicles of this generation joined the fleet

## Decision

> **We chose to implement the alternative communication channel as a fallback for this vehicle generation, accepting the known reliability trade-off in exchange for fleet coverage.**

This decision explicitly prioritized:

- coverage over perfection
- degraded but functional service over no service
- investment in a known-imperfect path rather than waiting indefinitely for the ideal one

The reliability trade-off was documented and communicated: the fallback path was slower, had different failure modes, and required dedicated monitoring. The team accepted that some commands would fail or arrive late. The product decision was that a partially reliable path was better than no path.

## How I Led It

I framed this as a coverage and trust decision, not a technical workaround.

My contribution was to:

- quantify the coverage gap using operational data — showing that a significant and growing portion of the fleet was unsteerable
- frame the reliability trade-off explicitly: "this path is X% less reliable than the primary channel, but 100% more reliable than no path"
- define the phased model-range rollout to manage risk incrementally rather than enabling all models simultaneously
- negotiate the cross-team monitoring boundary with the onboard software team, ensuring both teams had clear ownership of their respective failure modes
- build monitoring and observability into the fallback from day one, treating it as a product requirement rather than an afterthought

The hardest leadership moment was the monitoring ownership discussion. The backend team owned the command dispatch, but the onboard software team owned the execution. When the fallback path failed, whose failure was it? I pushed for explicit ownership boundaries defined as product requirements, not left to engineering to figure out ad hoc.

## Execution And Trade-Offs

The execution required approximately twenty tickets spanning:

- payload mapping between the primary and alternative channels
- new backend interfaces for the fallback path
- notification integration to inform the system when commands were dispatched via the alternative channel
- monitoring logic specific to the fallback path's failure characteristics
- phased model-range additions, with each model validated before enabling the next

What was deliberately not done:

- no attempt to make the fallback path invisible — it was tracked and monitored as a distinct execution path
- no attempt to match the primary channel's reliability profile — the trade-off was accepted and communicated
- no optimization of the fallback path before validating that it worked end-to-end

The most significant delivery cost was cross-team coordination. The monitoring boundary negotiation alone took multiple sprint cycles to resolve, because the interface between "command dispatched" and "command executed" crossed team ownership lines.

## Results And Evidence

### User / customer impact

Vehicle owners with this generation gained access to smart charging that would otherwise have been unavailable. The experience was functionally equivalent — the user did not know which communication channel was being used. The reliability difference was absorbed by the system's renegotiation and retry logic rather than exposed to the user.

### Business / operational impact

Fleet coverage expanded to include a generation that would otherwise have been a persistent gap. Operationally, the dedicated monitoring for the fallback path provided better visibility into this generation's behavior than the team had before the fallback was implemented — a secondary benefit of treating observability as a product requirement.

### Leadership / influence impact

The initiative demonstrated that coverage decisions were product decisions, not engineering preferences. The explicit monitoring boundary negotiation established a template for cross-team interface ownership that was reused in subsequent initiatives.

### Evidence notes

- Operational evidence: approximately twenty tickets delivered across multiple sprints, with each model range validated before the next was enabled
- Proxy evidence: fleet coverage for the affected generation moved from zero to functional through the fallback path
- Qualitative evidence: cross-team monitoring boundary documented and agreed, reducing ambiguity for future interface decisions

## What I Would Do Differently

- I would start the monitoring boundary discussion earlier — it was the longest lead-time item and blocked other work.
- I would define explicit success criteria for the fallback before starting implementation: what failure rate is acceptable? At what point do we decide the fallback path is not viable?
- I would invest in end-to-end event streaming from the start rather than stitching together per-vehicle traces across multiple systems. The lack of a unified observability pipeline made debugging fallback failures significantly harder than it should have been.
- I would create a decision framework for "when to implement a degraded path vs. when to wait for the root cause fix" — this decision pattern will recur as the fleet adds more vehicle generations.

## Linked Supporting Artifacts

- [Case study - Controlled API / OCPX](../case-study-controlled-api/smart-charging.md)
- [Case study - Pre-Charging / Accuracy over assumptions](../case-study-pre-charging/Accuracy-over-assumptions.md)
