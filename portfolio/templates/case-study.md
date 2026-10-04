# How to read the product cases

[← Catalogue](../README.md) · [Worked product artifacts](product-artifacts.md)

Each case has a short opening, a product narrative, and expandable technical detail. The pages are developed concept studies with explicit assumptions and validation plans.

| Case | Assumed setting | Central choice |
|---|---|---|
| [AI Operations Copilot](../case-studies/ai-operations-copilot.md) | B2B SaaS incident handoffs. | Draft follow-up work with source evidence and human approval. |
| [Clinical Workflow](../case-studies/clinical-workflow.md) | Outpatient diagnostic laboratory. | Preserve identity and specimen traceability across handoffs and corrections. |
| [Predictive Maintenance](../case-studies/predictive-maintenance.md) | Small industrial pump-motor fleet. | Establish useful condition monitoring before predictive models. |
| [Safe School Pickup](../case-studies/safe-school-pickup.md) | Staffed school pickup operation. | Preserve current authorization and staff-confirmed release. |

These scenarios make the analysis concrete. They do not represent named clients, completed deployments, or customer research.

## Start with the decision

The opening explains the problem, direction, trade-off, and status. For AI assistance, the important choice is which decisions a person must review. For maintenance, it is whether monitoring warrants the cost at all. The [decision snapshot](product-artifacts.md#decision-snapshot) makes that reasoning reviewable.

## Follow the product narrative

**Problem and evidence.** A laboratory quality handbook supports traceability practices; it does not prove that a particular laboratory loses specimens. That local hypothesis still needs observation.

**People and workflow.** An operator raises a concern, a technician investigates it, and a planner schedules work. The product boundary includes the handoffs and exceptions between those roles.

**Scope and alternatives.** Each proposal includes a simpler baseline and explains what is deferred. Configuring existing software may be preferable to building a clinical application. Scheduled maintenance remains a legitimate industrial baseline.

**Decision and trade-off.** The [trade study](product-artifacts.md#trade-study) makes compromises visible. A model may reduce drafting work while increasing review burden. A release confirmation takes time while improving accountability.

**Requirements and delivery.** “Handle duplicates” becomes a concurrent-request test with one accepted release. The [requirements matrix](product-artifacts.md#requirements-matrix) connects needs to observable behavior. Priorities follow dependencies and risk.

**Metrics and economics.** Measures have denominators and collection methods. Targets are proposed; results require executed evaluation. Hypothetical calculations expose break-even assumptions and unfavorable scenarios.

**Validation and rollout.** The plan states what permits expansion and what stops it. Synthetic journeys verify defined behaviors; supervised use tests whether people can complete the workflow.

## Inspect the engineering detail

The expandable sections connect technical choices to consequences:

- AI source references let reviewers check proposed tasks against the original note.
- Clinical quarantine keeps unresolved identity mismatches out of reporting.
- Maintenance data-quality states prevent stale readings appearing healthy.
- Atomic pickup release prevents competing handovers on two devices.

The [technical appendix example](product-artifacts.md#technical-appendix) covers boundaries, states, interfaces, and recovery. Each case adds domain requirements, failure analysis, screen walkthroughs, and evaluation.

## Use the interview versions

The 30-second version states the context, choice, and trade-off. The two-minute version adds evidence and alternatives. The five-minute version opens a requirement, failure scenario, or economic assumption for discussion.

The story stays consistent at every length: independent concept work, a reasoned design, and a concrete test plan. A proposed screen walkthrough is an interaction specification; it does not claim that a working prototype exists.

## What the evidence language means

| Label | Meaning |
|---|---|
| Sourced principle | Supported by a linked primary reference; applicability still needs local review. |
| Scenario assumption | Chosen operating condition that makes the concept analyzable. |
| Proposed requirement | Behavior specified for a future implementation and acceptance test. |
| Design target | Intended threshold needing validation against actual baseline and conditions. |
| Hypothetical calculation | Arithmetic with assumed inputs to expose sensitivity or break-even. |
| Validation plan | Proposed evidence collection that has not produced results yet. |
| Measured result | Reserved for documented executed evaluation. None is asserted for these concepts. |

The [assumption register](product-artifacts.md#assumption-register), [risk table](product-artifacts.md#risk-table), and [metrics tree](product-artifacts.md#metrics-tree) contain filled examples.
