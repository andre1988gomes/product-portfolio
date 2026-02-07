# Example — Using Data to Reframe the Product Problem

---

## Decision to Be Informed

Should product effort focus on:
- further optimizing charging cost and configuration options  
or
- improving charging outcome reliability and predictability?

This decision directly impacted:
- product scope
- system complexity
- stakeholder expectations
- long-term maintenance cost

---

## Initial Assumption

The working assumption was that:
- users primarily value cost optimization
- improvements in optimization logic would meaningfully improve perceived value
- configuration flexibility was a key differentiator

This assumption was widely accepted but weakly evidenced.

---

## Data Signals Considered

Rather than focusing on aggregate usage metrics, data analysis focused on **signals related to failure and friction**, including:

- frequency of charging sessions not reaching the expected outcome
- distribution of deviations over time during charging sessions
- correlation between deviations and subsequent recovery actions
- support cases and operational events tied to failed or unexpected charging behavior

The goal was not to optimize performance, but to understand **where value was actually breaking down**.

---

## What the Data Showed

Data analysis revealed that:
- a small number of failure modes accounted for a disproportionate share of negative outcomes
- deviations occurring late in the charging session were significantly harder to recover from
- optimization improvements had limited impact when execution became unpredictable
- recovery mechanisms were triggered more often than expected, indicating systemic fragility

Importantly, these issues were **not visible** through high-level success metrics alone.

---

## Insight

The primary source of value loss was **not suboptimal optimization**, but **unreliable execution under changing conditions**.

This reframed the product problem from:
> “How do we optimize charging better?”  
to:
> “How do we ensure the charging outcome is consistently reached?”

---

## Decision

Based on these signals, product focus shifted toward:
- earlier detection of deviations
- improving execution monitoring
- enabling recovery and renegotiation mechanisms
- prioritizing correctness and predictability over further optimization

Optimization work was explicitly deprioritized until outcome reliability improved.

---

## Trade-offs Accepted

This decision accepted:
- slower visible feature delivery
- less emphasis on cost-based differentiation
- increased investment in non-user-facing capabilities

In exchange for:
- higher likelihood of reaching the desired outcome
- reduced operational friction
- improved trust from users and external partners
- a more stable foundation for future optimization

---

## Why This Belongs in Data Analysis

This example demonstrates how data was used:
- not to justify a predefined solution
- not to produce dashboards
- but to **reframe the product problem itself**

Data served as an input to judgment, not a substitute for it.
