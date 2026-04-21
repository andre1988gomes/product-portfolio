# Case Study - Controlled API / OCPX
**Owning the charging execution path in a partner ecosystem where reliability and control mattered as much as feature delivery**

## Snapshot

| Field | Value |
| --- | --- |
| Role | Product Owner / Product Manager |
| Domain | EV charging platform and partner ecosystem |
| Users / customers | Vehicle owners, energy providers, and internal charging teams |
| Scope / complexity | 10+ external partners across Europe, variable vehicle behavior, backend dependencies, and physical charging constraints |
| Timeframe | Multi-sprint product and platform build-out |
| Evidence type | Operational, Qualitative, Proxy |
| Key result | Shifted the product from passive integration to active orchestration, improving control, predictability, and long-term autonomy. |

## Problem And Why It Mattered

The product problem was bigger than "enable smart charging." Users expected a plug-and-forget experience that reliably reached their target state of charge by departure time. At the same time, the business had already lost control of part of the charging ecosystem: external energy providers were interacting with vehicles through existing channels without clear governance or monetization.

If we treated this as a thin integration problem, two things would continue to happen:

- users would experience unpredictable charging outcomes in a system they could not understand
- the company would remain dependent on other teams and partners for a business-critical outcome

This was a product decision about control, trust, and business leverage, not just about backend architecture.

## User / Customer Context

For vehicle owners, the real promise was simple: define a provider, a target state of charge, and a departure time, then trust the system to do the rest.

What mattered most to users was not configuration depth. It was confidence that:

- the vehicle would be ready when needed
- the behavior would remain predictable under changing conditions
- the system would not silently fail late in the charging window

Energy providers were also customers of the orchestration layer in practice. They needed accurate, timely information to create viable charging plans and adapt when conditions changed.

When the new API was introduced, partners complained that the structure differed significantly from the legacy interface they had built against. The biggest concrete change request was to include the relay cycle budget — a physical constraint that limited how many times the system could switch charging modes. Partners needed this data to build valid plans, but it was not initially exposed. This signaled that the API contract was not just a technical interface; it was a developer experience problem where consumer assumptions had to be respected.

## Business Context

Before the Controlled API, external partners could drive charging behavior through less-governed channels. That created three business problems:

- weak control over a core customer experience
- limited monetization of partner-driven charging activity
- slow response to incidents or product changes because key execution paths sat elsewhere

The business case for this initiative was therefore both defensive and strategic: regain governance over the charging loop, enable a monetization model, and create a platform position strong enough to evolve without constant dependency drag.

## Constraints

The main challenge was not one isolated limitation. It was the interaction of several unstable layers at once:

- vehicle behavior differed across models and generations
- backend systems could not be treated as fully reliable or real-time
- charging hardware and battery behavior could invalidate otherwise correct plans
- external providers had their own systems, priorities, and failure modes
- some dependencies, such as plug-in and plug-out events, were initially outside the product's direct control

This meant that every shortcut which reduced ownership also reduced the product's ability to protect the outcome.

## Options Considered

### Option A - Reuse existing data streams and services

This was attractive because it reduced initial implementation effort and accelerated the first release.

It was rejected because it would have left the product dependent on:

- other teams' priorities
- other teams' data semantics
- slower incident response
- limited ability to evolve monitoring and renegotiation logic

### Option B - Act as a thin relay between providers and vehicles

This would have minimized product ownership and made the system look simpler on paper.

It was rejected because the product would still have been accountable for the user outcome without owning the critical loop that produced it.

### Option C - Own the charging execution loop end to end

This option carried higher short-term complexity, but it was the only one aligned with the product goal. It allowed the team to own monitoring, deviations, renegotiation, and key data dependencies rather than treating them as someone else's problem.

## Decision

> **We chose to own the charging execution loop end to end, where feasible, and treat orchestration as a product capability rather than a passive integration layer.**

This meant accepting higher short-term delivery cost in exchange for:

- stronger control over outcome reliability
- faster reaction to deviations
- clearer ownership boundaries
- long-term autonomy to evolve the product without waiting on other teams

## How I Led It

I helped frame this as a product ownership decision, not an implementation preference.

My contribution was to:

- reframe the initiative around outcome reliability and business control rather than technical reuse
- push for explicit discussion of trade-offs instead of implicit acceptance of dependency risk
- challenge proposals that optimized for initial convenience while weakening long-term product credibility
- align stakeholders around why owning the critical path was worth the short-term cost

One of the clearest examples was the pre-charging discussion. A "last known settings per location" approach looked efficient, but I argued against it because it converted real-world variability into silent, compounding risk. That position was initially unpopular, but it was consistent with the actual customer outcome the product was accountable for.

Another significant moment was the default plan removal. The system automatically generated a default charging plan during pre-charging. I pushed to remove it because it consumed relay cycle budget unnecessarily, created unpredictable behavior when immediately superseded by a partner-submitted plan, and added orchestration complexity. The onboard software team resisted removal because their logic depended on receiving a plan within thirty seconds — a constraint imposed by charging hardware standards. I staged the deprecation: the default plan stayed temporarily while the consuming team developed fallback logic, and only after their migration was complete did the removal proceed. This cost multiple sprints of parallel work but preserved the simplification.

A third point of friction was the command interface decision. I advocated for switching from a complex charging profile interface to a simpler start/stop command model for remote charging control. Engineering disagreed — the profile-based approach had better backward compatibility across older vehicle generations and was battle-tested. I argued that start/stop was simpler to debug, easier to explain, and mapped directly to the actual control model. The simpler interface was selected, accepting additional integration work for older vehicles in exchange for long-term interface clarity.

## Execution And Trade-Offs

The decision changed both scope and sequencing.

We invested in:

- dedicated charging-state visibility
- monitoring logic for deviations
- renegotiation behavior when execution diverged from plan
- stronger ownership over data and execution dependencies

We explicitly did not optimize first for:

- minimum implementation effort
- reduced coordination in the short term
- reusing every existing dependency by default

The most visible cost was time. Building and stabilizing the dedicated stream and surrounding ownership model cost roughly one to two sprints that could otherwise have gone into narrower delivery items.

## Results And Evidence

### User / customer impact

The product moved closer to the outcome users actually cared about: predictable charging by departure time. The strongest evidence here was operational and qualitative rather than purely numeric. Earlier visibility into deviations improved the team's ability to protect the charging outcome before failures became unrecoverable.

### Business / operational impact

Owning the loop restored business control over a monetizable partner interaction and reduced the product's dependency on external teams for critical configuration changes. Operationally, the team could evolve monitoring logic and react to incidents faster because the product owned the relevant path instead of negotiating every change across team boundaries.

### Leadership / influence impact

The initiative made trade-offs more explicit across product, engineering, and functional leadership. The outcome was not just a more resilient system; it was clearer decision-making around what the product would own, what it would not delegate, and why those ownership calls mattered.

### Evidence notes

- Operational evidence: faster reconfiguration and incident response once the stream and monitoring logic were owned by the product path
- Proxy evidence: reduced reliance on downstream recovery as observability and renegotiation improved
- Qualitative evidence: stronger stakeholder clarity on why control, not just optimization, had become a product requirement

## What I Would Do Differently

- I would implement an end-to-end event streaming pipeline from the start. The lack of a unified observability stream meant debugging required manually stitching traces across four or more systems. Having this foundation would have enabled better monitoring, faster incident response, and fleet-wide observability instead of per-vehicle tracing.
- I would define explicit kill criteria for every external dependency: "if X is not ready by date Y, we execute fallback Z." Dependencies were identified as risks but not actively killed or mitigated early enough. A cross-boundary data overflow defect proved that interface assumptions should have been validated earlier.
- I would invest in a more complete testing strategy from the beginning. Pre-production testing caught issues that should have been identified earlier with systematic cross-version compatibility testing. A comprehensive test plan was eventually created, but the systematic approach came late.
- I would surface vehicle-specific constraints earlier so stakeholders understood sooner how much real-world variability shaped the solution space.
- I would escalate disagreements earlier when I believed a proposed shortcut created silent downstream risk.

## Linked Supporting Artifacts

- [PRD - Controlled API](../../02-product-documentation/product-requirements/prd-cc-api.md)
- [Decision record - Owning the charging execution data stream](../../02-product-documentation/decision-records/example-risk-based-decision.md)
- [Example - Sliced backlog](../../02-product-documentation/backlog-artifacts/example-sliced-backlog.md)
- [Example - Communicating a trade-off decision](../../02-product-documentation/backlog-artifacts/alignment-docs/example-tradeoff-communication.md)
- [Example - Using data to reframe the product problem](../../05-data-analysis/example-data-informed-decision.md)
- [Framework - Outcome over optimization](../../04-frameworks-and-playbooks/outcome-over-optimization-framework.md)
- [Case study - Platform Migration](../case-study-platform-migration/platform-migration.md)
- [Case study - Degradation Strategy](../case-study-degradation-strategy/degradation-strategy.md)
- [Case study - Monitoring Evolution](../case-study-monitoring-evolution/monitoring-evolution.md)
