# Reusable case-study template

[← Catalogue](../README.md) · [Product artifacts](product-artifacts.md)

**Template, not evidence.** Replace every input marker with permissioned evidence or an explicitly labeled assumption. Do not mark a section complete until its evidence and limits are clear.

## Layer 1 — 30-second view

| Field | Content |
|---|---|
| Product / domain | [INPUT REQUIRED: name and domain] |
| Case type | [Concept / Strategy / Redesign / Sanitized Professional / Academic] |
| Status | [Concept / prototype / pilot / verified deployment] |
| My role | [INPUT REQUIRED: actual contribution and boundaries] |
| Problem and users | [INPUT REQUIRED: user, workflow, and problem] |
| Key decision | [PROPOSED or EVIDENCED: decision] |
| Outcome / target | [MEASURED result with source, or DESIGN TARGET — requires validation] |
| Primary trade-off | [INPUT REQUIRED: benefit and cost accepted] |

## Layer 2 — product narrative

### Context, problem, and evidence

[INPUT REQUIRED: business context, current workflow, problem, and evidence sources.]

Separate observations, inferences, assumptions, and proposed choices. Keep raw evidence and confidential details outside the public repository.

### Users, stakeholders, and jobs to be done

[INPUT REQUIRED: primary and secondary users, buyer, operating roles, and their needs.]

When [situation], [user] needs to [job], so that [desired outcome].

### Current workflow and opportunity

[INPUT REQUIRED: map real handoffs, exceptions, pain points, and the opportunity.]

### Goal and non-goals

[INPUT REQUIRED: intended user and business outcomes; scope explicitly excluded.]

### Requirements

[INPUT REQUIRED: functional and non-functional requirements with testable acceptance criteria.]

Use the [requirements matrix](product-artifacts.md#requirements-matrix) to connect needs, features, and verification.

### Alternatives and trade-offs

[INPUT REQUIRED: feasible options, decision criteria, evidence confidence, and qualitative considerations.]

Use the [trade-study template](product-artifacts.md#trade-study). The matrix informs the decision; it does not make the decision.

### Decision snapshot

[Complete the signature decision artifact](product-artifacts.md#decision-snapshot).

### MVP and prioritization

[INPUT REQUIRED: minimum testable slice, dependencies, why each capability comes first, and what is deferred.]

Avoid numeric prioritization scores unless reach, impact, confidence, and effort have a defensible basis.

### Product and system architecture

[INPUT REQUIRED: boundaries, components, interfaces, data flow, and operational dependencies.]

Explain how each important engineering choice supports value, feasibility, risk reduction, or learning.

### Roadmap and delivery plan

| Horizon | Outcome to establish | Themes | Evidence / exit gate |
|---|---|---|---|
| Now | [INPUT REQUIRED] | [INPUT REQUIRED] | [INPUT REQUIRED] |
| Next | [INPUT REQUIRED] | [INPUT REQUIRED] | [INPUT REQUIRED] |
| Later | [INPUT REQUIRED] | [INPUT REQUIRED] | [INPUT REQUIRED] |

### Metrics and guardrails

[INPUT REQUIRED: baseline source, outcome metric, input metrics, harm guardrails, instrumentation, and review interval.]

Use the [metrics tree](product-artifacts.md#metrics-tree). Distinguish an intended outcome from a measured result.

### Assumptions, risks, and validation

[INPUT REQUIRED: highest-impact unknowns, failure modes, mitigations, and how the hypothesis could be invalidated.]

Use the [assumption register](product-artifacts.md#assumption-register) and [risk table](product-artifacts.md#risk-table).

### Rollout and adoption

[INPUT REQUIRED: intended pilot boundary, operating owner, training, stop conditions, support, and recovery path.]

### Learning and reflection

[INPUT REQUIRED: what to test next and what would change the decision.]

If no work has been tested, write expected learning questions rather than retrospective lessons.

## Layer 3 — technical appendix

Use the [technical appendix](product-artifacts.md#technical-appendix) for context, interfaces, traceability, FMEA, data models, evaluation methods, and API sketches.

## Interview version

Prepare an evidence-aware 30-second, 2-minute, and 5-minute story:

**Context → Problem → Evidence → Options → Decision → Trade-off → Execution plan → Metric → Learning**

Clearly identify concept work and proposed decisions.

## Completion gate

- [ ] Case type, status, and actual role are clear.
- [ ] Problem, users, workflow, and evidence are documented.
- [ ] Assumptions are separate from observations.
- [ ] Goal, non-goals, requirements, and alternatives are defined.
- [ ] Decision, trade-off, MVP, and prioritization are explicit.
- [ ] Architecture supports the product reasoning.
- [ ] Measurement, guardrails, risks, and validation are defined.
- [ ] Roadmap, rollout, next learning step, and technical appendix exist.
- [ ] No unsubstantiated ownership, research, deployment, or outcome claims appear.
- [ ] Links, diagrams, mobile reading, and interview versions have been reviewed.
