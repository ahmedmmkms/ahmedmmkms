# Industrial Predictive Maintenance

[← Catalogue](../README.md) · [Decision approach](../approach.md) · [Case template](../templates/case-study.md)

**Concept Product Case Study · Foundation stage**

Industrial / IoT · Reliability

This is an independent portfolio concept. The workflow, users, and proposed product direction below require validation. No customer research, implementation, deployment, or measured outcome is claimed.

## 30-second snapshot

| Question | Concept direction |
|---|---|
| Problem hypothesis | Asset monitoring signals may not translate into timely, understandable maintenance decisions. |
| Intended users — assumed | Maintenance technicians, reliability engineers, and asset owners; asset class and operating environment require input. |
| My role in this portfolio entry | Concept framing and proposed decision structure; discovery and delivery have not been established |
| Proposed key decision | Begin with visibility, fixed thresholds, and anomaly detection; consider predictive ML after data and operating value are established. |
| Intended outcome | Make monitored signals useful for maintenance action; asset availability and maintenance economics require a baseline. |
| Primary trade-off | Defer predictive capability in exchange for lower initial data demands and more explainable alerts. |

## Decision snapshot

**Proposed decision:** Begin with visibility, fixed thresholds, and anomaly detection; consider predictive ML after data and operating value are established.

**Why it matters:** Make monitored signals useful for maintenance action; asset availability and maintenance economics require a baseline.

**Alternatives to compare:** Scheduled maintenance; fixed-threshold monitoring; statistical anomaly detection; ML predictive maintenance.

**Decision drivers:** Time to value, data readiness, explainability, false alarms, integration effort, infrastructure cost, and reliability.

**Trade-off accepted in the concept:** Defer predictive capability in exchange for lower initial data demands and more explainable alerts.

**Risk introduced or remaining:** Missed events or frequent false alarms could create misplaced confidence or excessive technician workload.

**Proposed validation:** Inspect permissioned asset data, maintenance logs, failure histories, and the current alert-to-action workflow.

## Evidence and assumptions

The supplied portfolio brief provides the concept direction. It does not provide domain-specific observations or results.

| ID | Assumption | Confidence | Impact | Validation method | Status |
|---|---|---|---|---|---|
| A01 | Useful operating signals exist, but representative labeled failure histories may be insufficient for predictive ML. | Low | High | Inspect permissioned asset data, maintenance logs, failure histories, and the current alert-to-action workflow. | Open |

[INPUT REQUIRED: provide permissioned observations, workflow examples, or datasets before describing real user pain or outcomes.]

## Proposed MVP boundary

**In scope:** One asset class, agreed signals, condition visibility, explainable alerts, and a logged maintenance response.

**Non-goals:** Guaranteed failure prediction, autonomous shutdown or control, and fleet-wide deployment before a representative pilot.

This scope is a starting hypothesis. A detailed PRD, prioritized backlog, delivery plan, and acceptance criteria are still pending.

## Illustrative system context

```mermaid
flowchart LR
    Asset[Selected asset] --> Sensors[Sensor signals]
    Sensors --> Edge[Gateway and buffering]
    Edge --> Ingest[Ingestion and storage]
    Ingest --> Rules[Rules and anomaly detection]
    Rules --> Review[Technician review]
    Review --> Action[Recorded maintenance action]
```

This diagram is a concept workflow, not an implemented or deployed architecture.

## Measurement plan

All entries are proposed measurements. Baselines, numeric targets, and results have not been supplied.

| Candidate measure | How it would be assessed |
|---|---|
| Actionable-alert rate | Label reviewed alerts and track whether they support a justified maintenance action. |
| Missed events and false alarms | Replay representative normal and abnormal signals with explicit reference labels. |
| Signal availability | Record missing readings, stale data, and gateway interruptions. |
| Maintenance value | Establish downtime, response effort, and maintenance cost baselines before claiming improvement. |

## Primary risk

**Failure to examine:** Missed events or frequent false alarms could create misplaced confidence or excessive technician workload.

**Proposed controls to verify:** Retain existing maintenance practices, expose data availability, record alert handling, and evaluate missed events alongside false alarms.

[Use the risk and requirements templates →](../templates/product-artifacts.md)

<details>
<summary><strong>Technical appendix — planned depth</strong></summary>

Signal definitions, sampling assumptions, edge buffering, threshold rationale, alternative comparison, lightweight FMEA, and pilot gates.

[INPUT REQUIRED: supply system constraints and evidence before completing the appendix.]

[Reusable technical appendix template](../templates/product-artifacts.md#technical-appendix)

</details>

## Next learning step

Asset class; available sensors and sampling rates; historical failures; maintenance process; integration constraints; operational and economic baselines.

The [full case-study template](../templates/case-study.md) defines the remaining discovery, requirements, prioritization, roadmap, validation, rollout, and reflection sections.
