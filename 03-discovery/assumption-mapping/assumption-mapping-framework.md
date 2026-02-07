# Assumption Mapping Framework

**Purpose:**  
Identify, prioritize, and test assumptions that materially impact product direction and outcome reliability.

---

## Core Principle

**The riskiest assumptions are those that feel most obvious and remain untested.**

---

## Step 1 — Identify Assumptions

List assumptions across four dimensions:

### 1. User & Behavior
- Users experience the problem as painful.
- Users are willing to change behavior.
- Users will trust automated or delegated decisions.

### 2. Value & Outcomes
- Solving this problem creates meaningful value.
- The outcome is noticeable and worth the effort.
- Value persists beyond the first use.

### 3. Feasibility & Constraints
- The system can support the behavior reliably.
- Dependencies behave within acceptable bounds.
- Physical or operational constraints are manageable.

### 4. Ecosystem & Stakeholders
- External partners can support the required behavior.
- Incentives are sufficiently aligned.
- Governance and ownership boundaries are clear.

---

## Step 2 — Classify Assumptions

For each assumption, assess:

- **Certainty**  
  How confident are we that this assumption holds?

- **Impact**  
  If this assumption is wrong, how damaging is it?

Use these two dimensions to classify assumptions:

- **High Impact / Low Certainty** → Critical assumptions  
- **High Impact / High Certainty** → Monitor  
- **Low Impact / Low Certainty** → Defer  
- **Low Impact / High Certainty** → Ignore for now

Discovery should focus primarily on **Critical assumptions**.

---

## Step 3 — Select the Highest-Risk Assumption

Choose **one assumption** that:
- would invalidate the initiative if wrong
- cannot be mitigated through design alone
- requires evidence, not opinion

Avoid testing multiple critical assumptions at once.

---

## Step 4 — Define Disconfirming Signals

For the selected assumption, define:
- what evidence would **support** the assumption
- what evidence would **invalidate** it

Disconfirming signals are more valuable than confirming ones.

---

## Step 5 — Design Discovery Activities

Select the **minimum set of activities** required to test the assumption.

Examples:
- targeted interviews
- behavioral data analysis
- real-world observation
- prototype usage in realistic conditions

Avoid activities that do not reduce uncertainty.

---

## Step 6 — Decide

Based on evidence, explicitly choose to:
- proceed
- pivot
- stop

Document the decision and rationale.

Discovery is complete when a decision is made.

---

## Common Failure Modes

- Treating assumption mapping as a workshop output.
- Testing assumptions that are easy instead of risky.
- Looking only for confirming evidence.
- Continuing discovery after the decision is clear.

---

## Signals to Revisit Assumptions

- System behavior changes significantly.
- New constraints or dependencies emerge.
- User behavior evolves.
- The cost of being wrong increases.
