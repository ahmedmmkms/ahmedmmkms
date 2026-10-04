# Reusable product artifacts

[← Catalogue](../README.md) · [Case-study template](case-study.md)

These are reusable Markdown components for GitHub. Every input marker is deliberately unfinished; sample structures do not establish product evidence.

## Decision Snapshot

| Field | Fill with |
|---|---|
| Decision and status | [PROPOSED / VALIDATED: selected direction] |
| Why it matters | [INPUT REQUIRED: user / business / system consequence] |
| Alternatives considered | [INPUT REQUIRED: feasible options] |
| Decision drivers | [INPUT REQUIRED: value, feasibility, risk, cost, strategic fit, confidence] |
| Evidence | [INPUT REQUIRED: public-safe source or explicit assumption] |
| Trade-off accepted | [INPUT REQUIRED: cost or capability deliberately sacrificed] |
| Risk introduced | [INPUT REQUIRED: consequence of the choice] |
| Validation | [INPUT REQUIRED: method, metric, and pass / stop criteria] |
| Reconsider when | [INPUT REQUIRED: evidence that would invalidate the choice] |

## Assumption Register

| ID | Assumption | Confidence | Impact | Validation method | Status |
|---|---|---|---|---|---|
| A01 | [ASSUMPTION FOR CASE STUDY — REQUIRES VALIDATION] | [Low / Medium / High with rationale] | [Low / Medium / High] | [Interview / observation / test / data] | Open |

Prioritize high-impact assumptions with weak evidence. Update confidence only when new evidence warrants it.

## Trade Study

| Criterion | Weight and rationale | Option A | Option B | Option C | Evidence confidence |
|---|---|---|---|---|---|
| Customer value | [INPUT REQUIRED] | [INPUT REQUIRED] | [INPUT REQUIRED] | [INPUT REQUIRED] | [INPUT REQUIRED] |
| Engineering effort | [INPUT REQUIRED] | [INPUT REQUIRED] | [INPUT REQUIRED] | [INPUT REQUIRED] | [INPUT REQUIRED] |
| Risk | [INPUT REQUIRED] | [INPUT REQUIRED] | [INPUT REQUIRED] | [INPUT REQUIRED] | [INPUT REQUIRED] |
| Operating cost | [INPUT REQUIRED] | [INPUT REQUIRED] | [INPUT REQUIRED] | [INPUT REQUIRED] | [INPUT REQUIRED] |
| Time to value | [INPUT REQUIRED] | [INPUT REQUIRED] | [INPUT REQUIRED] | [INPUT REQUIRED] | [INPUT REQUIRED] |

If scoring is useful, define the scale, normalize weights, and check sensitivity to uncertain inputs. Never supply invented values to make a matrix look complete.

**The matrix informs the decision; it does not make the decision.**

Record feasibility gates, qualitative concerns, the accepted trade-off, and the reason a higher score may not determine the choice.

## Metrics Tree

**Business outcome → Product outcome → User behavior → Operational measure**

| Layer | Candidate measure | Definition / denominator | Baseline source | Target or result | Instrumentation |
|---|---|---|---|---|---|
| Business | [INPUT REQUIRED] | [INPUT REQUIRED] | Not supplied | Not set | [INPUT REQUIRED] |
| Product | [INPUT REQUIRED] | [INPUT REQUIRED] | Not supplied | Not set | [INPUT REQUIRED] |
| User behavior | [INPUT REQUIRED] | [INPUT REQUIRED] | Not supplied | Not set | [INPUT REQUIRED] |
| Operational | [INPUT REQUIRED] | [INPUT REQUIRED] | Not supplied | Not set | [INPUT REQUIRED] |
| Guardrail | [INPUT REQUIRED: potential harm] | [INPUT REQUIRED] | Not supplied | Not set | [INPUT REQUIRED] |

Identify a North Star candidate, adoption and quality measures, reliability where relevant, review cadence, and decision thresholds. Numeric design targets must be labeled **requires validation**.

## Risk Table

| ID | Failure / risk | User or business consequence | Severity rationale | Likelihood evidence | Mitigation | Verification | Residual risk / owner |
|---|---|---|---|---|---|---|---|
| R01 | [INPUT REQUIRED] | [INPUT REQUIRED] | [INPUT REQUIRED] | Unknown | [PROPOSED control] | [INPUT REQUIRED: failure scenario] | [INPUT REQUIRED] |

Use qualitative ratings with rationale until a numeric scale and evidence are available. Expand to FMEA in the appendix when it informs the product choice.

## Requirements Matrix

| Need ID | Stakeholder need | Requirement ID | Testable product requirement | Feature / dependency | Verification | Evidence status |
|---|---|---|---|---|---|---|
| N01 | [INPUT REQUIRED] | REQ-01 | [INPUT REQUIRED: measurable behavior under stated conditions] | [INPUT REQUIRED] | [INPUT REQUIRED: test and acceptance criteria] | Proposed |

Specify functional behavior, non-functional constraints, design load, interfaces, and exception conditions. “Fast” and “secure” alone are not verifiable requirements.

## Technical Appendix

<details>
<summary><strong>Expand the technical review</strong></summary>

### Context and system boundary

[INPUT REQUIRED: users, external systems, included components, excluded responsibilities.]

### Architecture and dependencies

[INPUT REQUIRED: components, data flows, operating constraints, and reasons for important choices.]

### Interfaces and data model

[INPUT REQUIRED: input / output contracts, identifiers, ownership, states, retention, and correction paths.]

### Verification and validation

[INPUT REQUIRED: requirement tests, user workflow validation, representative conditions, and limitations.]

### Failure analysis

[INPUT REQUIRED: failure modes, effects, mitigations, degraded behavior, and residual risk.]

### Domain-specific depth

- AI: dataset provenance, reference labels, error taxonomy, quality / cost / latency, and reviewer performance.
- Clinical workflow: identity, record completeness, permissions, audit events, and matching scenarios.
- Industrial systems: signal quality, sampling assumptions, missing data, false alarms, and maintenance response.
- Pickup operations: current authorization, staff release confirmation, duplicate prevention, exceptions, and recovery.

### Decision log

| Date | Decision | Evidence / assumption | Trade-off | Revisit trigger |
|---|---|---|---|---|
| [INPUT REQUIRED] | [INPUT REQUIRED] | [INPUT REQUIRED] | [INPUT REQUIRED] | [INPUT REQUIRED] |

</details>
