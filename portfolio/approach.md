# How I make product decisions

[← Catalogue](README.md) · [Systems thinking](systems-thinking.md) · [Artifact templates](templates/product-artifacts.md)

**Product decisions are decisions under uncertainty.** I make the assumptions, constraints, and trade-offs visible so a team can challenge a choice and test it.

## From discovery to learning

| Step | Question | Useful output |
|---|---|---|
| Discover | Which user problem is worth solving? | Workflow, stakeholder map, evidence gaps |
| Model | What system are we changing? | Boundaries, interfaces, dependencies, failure modes |
| Prioritize | Which outcome or uncertainty matters first? | MVP scope and sequencing rationale |
| Decide | Which alternative best fits the constraints? | Decision snapshot and accepted trade-off |
| Deliver | What small slice can we verify? | Testable requirements and rollout gates |
| Measure | What would demonstrate value or harm? | Outcome metrics, guardrails, and baseline plan |
| Learn | What evidence would change the decision? | A decision to continue, adapt, or stop |

I consider **customer value, business value, technical feasibility, strategic fit, risk, and evidence confidence** together. This is a set of questions to reason through, rather than a literal multiplication formula.

## Three proposed decisions to examine

These are concept directions from the catalogue, not decisions validated with customers.

| Case | Proposed choice | Trade-off | Evidence that could change the choice |
|---|---|---|---|
| [AI Operations Copilot](case-studies/ai-operations-copilot.md) | Keep a reviewer between suggestion and action | More review effort in exchange for control and traceability | Task-level evaluations and reviewer time show whether assistance adds value |
| [Clinical Workflow](case-studies/clinical-workflow.md) | Establish identity and auditable handoffs first | Narrower initial scope in exchange for dependable workflow data | Workflow observation reveals a different bottleneck or existing traceability capability |
| [Predictive Maintenance](case-studies/predictive-maintenance.md) | Start with thresholds and anomaly detection | Less predictive capability initially, with lower data and integration demands | Representative failure histories and pilot results justify a predictive model |

## Prioritization with judgment

Reach, impact, confidence, and effort can structure a comparison when inputs are defensible. I avoid inventing RICE scores when user counts, evidence confidence, and implementation effort are unknown.

Safety controls, identity, and architectural dependencies may need to precede a feature with a larger apparent benefit. I record why that exception matters and what evidence would permit a different sequence.

## A decision is reviewable when…

- Its problem and intended user are clear.
- Evidence and assumptions are distinguishable.
- Alternatives and the trade-off are named.
- Its requirements can be verified.
- Its success metric and harm guardrail are defined.
- There is a practical way to invalidate the hypothesis.

[Use the Decision Snapshot template →](templates/product-artifacts.md#decision-snapshot)
