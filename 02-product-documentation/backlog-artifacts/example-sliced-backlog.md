# Example - Sliced Backlog (Outcome-Driven)

## Evidence Metadata

| Field | Value |
| --- | --- |
| Used in | [Controlled API / OCPX](../../01-case-studies/case-study-controlled-api/smart-charging.md) |
| Decision supported | Sequence delivery around observability, controlled response, validated inputs, and explainability. |
| Evidence type | Delivery artifact, Operational |
| Confidence level | Medium to High |
| Outcome influenced | Better execution sequencing and clearer outcome-oriented backlog structure. |

This example shows how a complex initiative can be sliced into deliverable increments while preserving the product outcome rather than fragmenting it into disconnected tickets.

## Epic - Reliable Charging Outcome Under Dynamic Conditions

**Intent:** Ensure charging execution reaches the user-defined target despite changing conditions by detecting deviations and enabling controlled renegotiation.

## Slice 1 - Observability First

**Title:** Detect charging plan deviations during an active session

**User story:**  
As charging orchestration, I want to detect deviations between the agreed plan and the real charging state, so that the target outcome can still be protected before recovery becomes difficult.

**Why it matters:**  
Without early detection, failures surface late and the product loses the ability to act.

**Acceptance criteria**

- [ ] Deviation detection triggers when the charging trajectory diverges beyond an agreed threshold.
- [ ] User-setting changes relevant to execution are detected during the session.
- [ ] Deviations are categorized in a way that supports reporting and explanation.

## Slice 2 - Controlled Response

**Title:** Request renegotiation when deviations are detected

**User story:**  
As charging orchestration, I want to trigger renegotiation with the provider when actionable deviations occur, so that the plan can still be adapted toward the target outcome.

**Why it matters:**  
Detection without response only creates alerts. The product needs a way to recover.

**Acceptance criteria**

- [ ] Actionable deviations trigger a renegotiation request.
- [ ] The system degrades safely if the provider does not respond in time.
- [ ] Renegotiation includes enough context for an updated plan.

## Slice 3 - Validated Inputs

**Title:** Ensure every charging session uses current validated inputs

**User story:**  
As charging orchestration, I want each session to rely on current validated data instead of cached assumptions, so that execution remains predictable across variable conditions.

**Why it matters:**  
Cached assumptions reduce visible effort upfront but create silent risk later.

**Acceptance criteria**

- [ ] Planning inputs come from current session state, not prior-location snapshots.
- [ ] The system does not proceed with a plan that assumes correctness when critical inputs cannot be validated.
- [ ] Fallback behavior is visible and explainable when validation cannot be completed.

## Slice 4 - Explainability

**Title:** Provide explainable deviation and renegotiation insights

**User story:**  
As an internal or external stakeholder, I want explainable insights about deviations and renegotiations, so that I can understand system behavior and improve decisions over time.

**Why it matters:**  
A technically correct system still fails organizationally if no one can understand or trust its behavior.

**Acceptance criteria**

- [ ] Deviations are grouped by category over time.
- [ ] Renegotiations are traceable to initiating events and outcomes.
- [ ] Stakeholders can distinguish provider-caused from vehicle- or system-caused renegotiations.

## Notes On Slicing Strategy

- Start with visibility, then add controlled response.
- Prefer slices that reduce silent failure modes early.
- Keep each slice independently testable and outcome-aligned.
- Avoid over-prescribing implementation when the behavior and guarantees are the real product requirement.
