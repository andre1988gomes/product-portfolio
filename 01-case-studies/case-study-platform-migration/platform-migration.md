# Case Study - Platform Migration
**Migrating seven B2B partners from a legacy API to a new backend without breaking a single integration**

## Snapshot

| Field | Value |
| --- | --- |
| Role | Product Owner / Product Manager |
| Domain | EV charging platform and B2B partner ecosystem |
| Users / customers | Energy providers (B2B partners), vehicle owners, and internal charging teams |
| Scope / complexity | 7 B2B partners ranging from under 500 to approximately 40,000 connected vehicles, per-vehicle cutover, rollback safety, multi-system coordination |
| Timeframe | Strategy defined during a multi-month planning window; execution sequenced across approximately seven months |
| Evidence type | Operational, Qualitative, Proxy |
| Key result | Designed a partner-sequenced migration strategy with per-vehicle cutover, canary deployment logic, and systematic rollback coverage that reduced migration risk from "big bang gamble" to "controlled, reversible transition." |

## Problem And Why It Mattered

The legacy charging API was reaching end-of-life. Seven external B2B partners depended on it to manage charging sessions for tens of thousands of vehicles. The replacement backend had been built and validated, but the transition itself was the product problem: how do you move every partner and every vehicle from the old system to the new one without breaking active integrations or degrading the charging experience?

This mattered because:

- a failed migration would directly break partner operations and user charging sessions
- the old and new systems had fundamentally different data models, provisioning mechanisms, and notification flows
- partners had different integration maturities, fleet sizes, and risk tolerances
- the business needed the legacy system decommissioned to reduce operational cost and eliminate dual-maintenance

This was not a technical migration that could be delegated to engineering. Every decision about sequencing, rollback, communication, and coexistence required product judgment.

## User / Customer Context

Vehicle owners expected uninterrupted smart charging. They did not know or care which backend version was running. Any disruption — a missed charging session, a delayed notification, a plan that failed to execute — would erode trust in a system they were only beginning to adopt.

Energy providers (B2B partners) had built their operations around the legacy API's structure, authentication model, and data semantics. Migration meant they had to:

- adapt to a new API contract
- re-test their integration end-to-end
- accept a transition period where some of their vehicles were on the old system and others on the new one

Partners had already expressed frustration that the new API structure differed from the legacy one. The biggest concrete feedback: a critical data field (relay cycle budget) was not initially exposed in the new contract, and partners needed it to build valid charging plans. This signaled that migration was not just a backend swap — it required treating partners as first-class consumers whose integration assumptions had to be respected.

## Business Context

The legacy system could not be maintained indefinitely alongside the new backend. Dual operation meant:

- duplicated operational cost
- split monitoring and incident response
- slower feature evolution because changes had to be validated across both systems

The business needed a credible migration path that would hold up under scrutiny from functional leadership and partner management. A failed migration would not just be a technical incident — it would damage partner relationships that took years to build.

The strategic value of completing migration cleanly was high: it unlocked single-system operation, simplified the partner interface, and removed the largest source of operational complexity in the product.

## Constraints

- Partners ranged from under 500 vehicles to approximately 40,000, creating dramatically different risk profiles for the same migration playbook.
- Per-vehicle provisioning was required — the migration could not be done at the partner level alone; each vehicle had to be individually transitioned.
- Customer-facing notifications had prerequisites: the notification system had to support migration-specific messages before any partner cohort could begin.
- The old and new systems could not safely synchronize usage quotas, meaning the migration had to be a clean break rather than a gradual fade.
- Rollback had to be safe for every migration scenario, including edge cases like mid-session cutover.

## Options Considered

### Option A - Forced Migration (per-partner, per-vehicle cutover)

Each partner would be migrated individually. Within each partner, vehicles would be cut over one at a time using their unique identifier. The old system would stop receiving new vehicles; existing vehicles would be migrated on a defined schedule.

This was attractive because it offered maximum control and reversibility. Each partner could be treated as an independent cohort. The smallest partner could go first as a canary deployment.

Trade-off: higher operational complexity during the coexistence period. Required per-vehicle provisioning tooling and careful scheduling.

### Option B - Silent Migration (behind-the-scenes with quota synchronization)

The new backend would silently take over execution while the legacy API continued as the partner-facing interface. Quota and state synchronization between old and new systems would maintain consistency.

This was attractive because it minimized partner disruption — they would not need to change anything immediately.

It was rejected because quota synchronization introduced a new class of consistency risk. If the sync failed or drifted, the product would be in a worse state than either system alone. The engineering complexity of maintaining real-time consistency across two backends was disproportionate to the benefit.

### Option C - Big Bang (all partners on a fixed date)

All partners would be migrated simultaneously on a single cutover date.

This was attractive because it eliminated the coexistence period entirely.

It was rejected because it concentrated all risk into a single event. If something went wrong, every partner would be affected simultaneously. Rollback would require reverting the entire fleet, which was operationally impractical.

## Decision

> **We chose Forced Migration with a hybrid approach: only onboard new vehicles into the new backend, while migrating existing vehicles per-partner in a deliberate sequence from smallest to largest fleet.**

This explicitly prioritized:

- control over speed
- reversibility over simplicity
- learning from small cohorts before exposing large ones

The decision to not synchronize quotas between old and new systems was a conscious product choice: a clean break is safer than a leaky bridge. Partners would transition fully or not at all for each vehicle.

The partner sequence was designed as a risk gradient: start with the smallest partner (under 500 vehicles), validate the migration playbook, then progress through mid-size partners before reaching the largest (approximately 40,000 vehicles).

## How I Led It

I framed this as a product strategy problem, not a deployment task.

My contribution was to:

- evaluate the three migration options as product trade-offs with different risk, complexity, and partner experience profiles
- advocate for the canary deployment model against pressure to move faster with a big bang approach
- define the partner sequencing logic based on fleet size and integration maturity
- push for systematic rollback test coverage before any partner cohort could begin migration
- ensure customer notification prerequisites were treated as hard blockers rather than nice-to-haves
- frame the "no quota sync" decision as a product quality choice rather than an engineering limitation

The most contentious discussion was whether to synchronize quotas between old and new systems. The engineering argument for sync was that it would make the transition smoother for partners. I argued against it because sync introduced a new failure mode that neither system alone had, and the product would be accountable for consistency bugs that were inherently harder to debug across two backends.

## Execution And Trade-Offs

The migration strategy changed how the team operated:

- eleven end-to-end rollback test scenarios were defined and validated before the first partner cohort, covering corner cases like mid-session migration, notification delivery during cutover, and provisioning failures
- per-vehicle provisioning tooling was built to support individual vehicle transitions rather than bulk partner switches
- customer notification flows were implemented as a prerequisite gate: no partner cohort could begin until the notification system could inform users about the transition
- the coexistence period was accepted as an operational cost, with explicit monitoring for both systems running in parallel

What was deliberately not done:

- no quota synchronization between old and new systems
- no attempt to migrate all partners simultaneously
- no optimization for migration speed at the expense of rollback safety

The most visible cost was timeline. The sequenced approach stretched migration across approximately seven months instead of the weeks a big bang would have taken. That cost was accepted because the risk reduction was proportional.

## Results And Evidence

### User / customer impact

The migration strategy was designed to make the transition invisible to vehicle owners. No charging session disruption, no change in app behavior, no user action required. The strongest evidence is structural: per-vehicle cutover with rollback safety means that any individual migration failure affects one vehicle, not the entire fleet.

### Business / operational impact

The canary deployment approach allowed the team to validate the migration playbook at low risk before exposing larger partners. Systematic rollback coverage reduced the probability of an unrecoverable migration failure. The decision to avoid quota sync eliminated the most complex and failure-prone aspect of the original migration design.

### Leadership / influence impact

The migration strategy required alignment across product, engineering, partner management, and functional leadership. Framing the approach as a product quality decision — not just a deployment plan — created stronger organizational commitment to the sequenced timeline.

### Evidence notes

- Operational evidence: eleven rollback test scenarios validated before partner execution began
- Proxy evidence: canary deployment logic tested at smallest partner scale before larger cohorts
- Qualitative evidence: partner communication plan and notification prerequisites treated as hard gates

## What I Would Do Differently

- I would start migration planning earlier — specifically, I would define the migration strategy in parallel with the new backend design rather than sequentially after it.
- I would validate the per-vehicle provisioning tooling with synthetic data before relying on it for real partner cohorts.
- I would create a partner-facing migration dashboard earlier so partners could track their own vehicles' transition status rather than relying on periodic updates.
- I would define explicit "pause criteria" for the migration: under what conditions do we stop migrating the next cohort and investigate? Having these criteria documented upfront would reduce decision latency during execution.

## Linked Supporting Artifacts

- [Case study - Controlled API / OCPX](../case-study-controlled-api/smart-charging.md)
- [Framework - Outcome over optimization](../../04-frameworks-and-playbooks/outcome-over-optimization-framework.md)
