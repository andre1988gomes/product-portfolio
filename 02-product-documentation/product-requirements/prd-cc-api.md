# Product Requirements — Controlled API

---

## Context & Problem Statement

Smart charging for electric vehicles requires tight coordination between vehicles, backend systems, and external energy providers.

Prior to the Controlled API, energy providers were interacting with EV vehicles through uncontrolled channels, leading to:
- loss of governance over vehicle behavior
- lack of visibility and predictability in charging execution
- inability to enforce consistent charging outcomes
- missed monetization opportunities

At the same time, users expect a *plug & forget* charging experience:
they define preferences (energy provider, target SoC, departure time) and expect the vehicle to be ready at the planned time, regardless of underlying complexity.

The Controlled API exists to **govern charging execution** in an environment characterized by:
- heterogeneous vehicle behavior
- unreliable or variable backend systems
- physical and mechanical constraints
- multiple external actors with different incentives

---

## Desired Outcomes

The product should enable the following outcomes:

- **Reliable charging outcomes**  
  Vehicles consistently reach the user-defined target SoC at the planned departure time.

- **Controlled ecosystem interaction**  
  All third-party charging interactions are mediated, observable, and governed by other systems.

- **Reduced operational noise**  
  Fewer charging deviations and renegotiations caused by incorrect assumptions or inferred data.

- **Trust from energy providers**  
  Energy providers receive accurate, timely data that enables predictable charging plans.

- **Long-term platform autonomy**  
  Retains control over critical execution paths without excessive dependency on other teams or systems.

---

## Constraints & Assumptions

### Constraints

- Vehicle behavior varies significantly across models and generations.
- Backend systems cannot be assumed to be fully reliable or real-time.
- Physical constraints (battery behavior, charging hardware) can override digital intent.
- Charging execution depends on external energy providers with independent systems and priorities.
- Location accuracy and inferred data are inherently unreliable in certain scenarios.

### Assumptions

- Accurate, real-time data reduces charging deviations more effectively than cached or inferred data.
- Ownership of the execution loop enables faster reaction to deviations and safer trade-offs.
- Not all optimization opportunities are worth pursuing if they degrade correctness or trust.

Assumptions are expected to be revisited as the system evolves.

---

## Non-Goals

The Controlled API is **not** intended to:

- Optimize charging purely for speed or cost at the expense of predictability.
- Abstract away all vehicle-specific behavior or physical constraints.
- Eliminate the need for renegotiation entirely under exceptional conditions.
- Serve as a generic data aggregation platform for unrelated vehicle services.
- Replace energy providers’ pricing or tariff calculation logic.

Explicitly excluding these goals helps protect outcome reliability.

---

## Success Signals

Success will be evaluated using a combination of qualitative and operational indicators:

- Reduced frequency of charging deviations from planned SoC trajectories.
- Fewer renegotiation cycles triggered by incorrect or outdated data.
- Increased consistency of charging outcomes across vehicle models and generations.
- Positive feedback from energy providers regarding data quality and predictability.
- Improved stakeholder confidence in the Controlled API as a governance layer.

Perfect measurement is not required; shared understanding of progress is.

---

## Notes

- Product decisions prioritize **correctness, resilience, and trust** over execution shortcuts.
- Some delivery friction is acceptable if it prevents silent failure modes.
- This PRD intentionally leaves solution details open to allow engineering collaboration and learning during execution.
