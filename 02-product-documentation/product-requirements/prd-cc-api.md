# Product Requirements - Controlled API

## Evidence Metadata

| Field | Value |
| --- | --- |
| Used in | [Controlled API / OCPX](../../01-case-studies/case-study-controlled-api/smart-charging.md) |
| Decision supported | Treat charging orchestration as a product capability focused on reliable outcomes, governance, and control. |
| Evidence type | Product documentation, Operational, Strategy |
| Confidence level | High |
| Outcome influenced | Scope, non-goals, ownership, and success signals for the Controlled API. |

## Context And Problem Statement

Smart charging for electric vehicles requires coordination between vehicles, backend systems, and external energy providers.

Before the Controlled API, partner interactions with vehicles happened through channels that reduced governance and made charging execution harder to control. At the same time, users expected a plug-and-forget experience: define preferences such as provider, target state of charge, and departure time, then trust the system to make the vehicle ready at the right moment.

This product exists to govern charging execution in a system shaped by:

- heterogeneous vehicle behavior
- unreliable or variable backend systems
- physical charging constraints
- external partners with different incentives and capabilities

## Desired Outcomes

- Vehicles reliably reach the target state of charge by the planned departure time.
- Partner-driven charging interactions are observable, governed, and commercially sustainable.
- Charging deviations are detected early enough to support recovery or renegotiation.
- Energy providers receive inputs accurate enough to create viable charging plans.
- The product retains ownership over the critical path needed to protect the outcome.

## Constraints And Assumptions

### Constraints

- Vehicle behavior varies across models and generations.
- Backend systems cannot be treated as fully reliable or fully real-time.
- Physical battery and hardware constraints can override digital intent.
- External providers have independent systems, priorities, and response behavior.
- Location-derived or cached assumptions are unsafe in some scenarios.

### Assumptions

- Accurate session-level data reduces deviations more effectively than inferred or cached data.
- Owning the execution path improves speed of response and product autonomy.
- Optimization should follow predictability, not precede it.

## Non-Goals

This product is not intended to:

- optimize charging purely for visible speed or cost while reducing predictability
- eliminate all renegotiation under exceptional conditions
- abstract away all vehicle-specific behavior
- serve as a generic data platform for unrelated vehicle services
- replace provider-side tariff logic

## Success Signals

Success should be evaluated using outcome-oriented signals such as:

- fewer deviations from the agreed charging trajectory
- lower dependence on avoidable renegotiation caused by weak inputs
- more predictable charging outcomes across models and conditions
- stronger partner confidence in the data they receive
- better internal confidence in the Controlled API as the governance layer

## Notes

This PRD intentionally keeps implementation open where engineering judgment should shape the solution. The key requirement is not architectural elegance. It is protecting the charging outcome under real-world variability.
