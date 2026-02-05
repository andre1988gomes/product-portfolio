# Example — Sliced Backlog (Outcome-Driven)

This example shows how a complex initiative can be sliced into deliverable, testable increments while preserving outcome intent.

The goal is to move from:
- **unreliable execution under dynamic conditions**
to:
- **predictable outcomes with controlled renegotiation**

All terminology is vendor-agnostic (no brand references).

---

## Epic: Reliable Charging Outcome Under Dynamic Conditions

**Intent:** Ensure that charging execution reaches user-defined targets despite changing conditions, by detecting deviations and enabling renegotiation with external energy providers.

---

### Slice 1 — Observability First: Detect When Reality Diverges

**Título**: Detect charging plan deviations during an active session

**User Story**:  
_As charging orchestration, I want to detect deviations between the agreed plan and the real charging state, so that we can protect the user outcome before it becomes unrecoverable._

**Why it needs to be done**:  
Without early detection, execution failures surface late and cannot be corrected, leading to missed target SoC and loss of trust.

**What needs to be done**:
- Define what “deviation” means in product terms (e.g., SoC trajectory, unexpected setting changes).
- Ensure deviation detection is consistent and explainable to downstream stakeholders.
- Ensure the product can distinguish “normal variance” vs “actionable deviation”.

**Acceptance Criteria (Free Form Editor checklist)**
- [ ] **AC1** — Deviation detection triggers when SoC trajectory diverges beyond an agreed threshold. (Status: Not Started | Assignee: PO/Team | Priority: P0)
- [ ] **AC2** — Deviation detection triggers when user settings relevant to the plan change during execution. (Status: Not Started | Assignee: PO/Team | Priority: P0)
- [ ] **AC3** — Deviations are recorded with a reason category usable for reporting and stakeholder explanation. (Status: Not Started | Assignee: PO/Team | Priority: P1)

---

### Slice 2 — Controlled Response: Trigger Renegotiation When Needed

**Título**: Request renegotiation when deviations are detected

**User Story**:  
_As charging orchestration, I want to request renegotiation from the energy provider when deviations occur, so that the plan can be adjusted to still reach the target outcome._

**Why it needs to be done**:  
Deviation detection without response only creates alerts. Renegotiation is the mechanism to recover the outcome.

**What needs to be done**:
- Ensure renegotiation requests are triggered only for actionable events.
- Ensure renegotiation is resilient to provider latency or partial failure.
- Ensure renegotiation preserves the primary user goal (target SoC by departure time).

**Acceptance Criteria (Free Form Editor checklist)**
- [ ] **AC1** — When an actionable deviation is detected, a renegotiation request is triggered to the provider. (Status: Not Started | Assignee: PO/Team | Priority: P0)
- [ ] **AC2** — If the provider does not respond within an agreed time window, the system degrades safely (e.g., fallback behavior defined). (Status: Not Started | Assignee: PO/Team | Priority: P0)
- [ ] **AC3** — Renegotiation requests include sufficient context for the provider to compute an updated plan. (Status: Not Started | Assignee: PO/Team | Priority: P1)

---

### Slice 3 — Trust & Predictability: Prevent Silent Assumption Reuse

**Título**: Ensure every charging session uses current, validated inputs

**User Story**:  
_As charging orchestration, I want each charging session to rely on current validated data instead of cached assumptions, so that execution remains predictable across variable conditions._

**Why it needs to be done**:  
Cached assumptions are a silent failure mode: they reduce renegotiations in the short term but increase deviations and trust erosion over time.

**What needs to be done**:
- Ensure inputs used for planning reflect current session conditions.
- Ensure the system can explain why a plan was accepted or rejected.
- Reduce renegotiations by increasing correctness upfront, not by hiding variance.

**Acceptance Criteria (Free Form Editor checklist)**
- [ ] **AC1** — Planning inputs used for a session are derived from current session state, not prior-location snapshots. (Status: Not Started | Assignee: PO/Team | Priority: P0)
- [ ] **AC2** — If critical inputs cannot be validated, the session does not proceed with a plan that assumes correctness. (Status: Not Started | Assignee: PO/Team | Priority: P0)
- [ ] **AC3** — The system exposes a reason when pre-validation could not be performed and what fallback was used. (Status: Not Started | Assignee: PO/Team | Priority: P1)

---

### Slice 4 — Stakeholder Clarity: Explainability and Reporting

**Título**: Provide explainable renegotiation and deviation insights

**User Story**:  
_As a stakeholder (internal or external), I want explainable insights about deviations and renegotiations, so that I can understand behavior, improve integrations, and reduce operational friction._

**Why it needs to be done**:  
A technically correct system still fails if stakeholders cannot understand or trust its behavior.

**What needs to be done**:
- Provide consistent categories for deviation causes.
- Provide visibility into renegotiation frequency and reasons.
- Enable data-driven discussions with external partners.

**Acceptance Criteria (Free Form Editor checklist)**
- [ ] **AC1** — Deviations are grouped by category with counts over time. (Status: Not Started | Assignee: PO/Team | Priority: P1)
- [ ] **AC2** — Renegotiations are traceable to initiating events and outcomes. (Status: Not Started | Assignee: PO/Team | Priority: P1)
- [ ] **AC3** — Stakeholders can distinguish provider-caused vs vehicle/system-caused renegotiations. (Status: Not Started | Assignee: PO/Team | Priority: P2)

---

## Notes on Slicing Strategy

- Start with observability, then add controlled response.
- Prefer slices that reduce silent failure modes early.
- Keep each slice independently testable and outcome-aligned.
- Avoid over-specifying implementation; focus on behavior and guarantees.
