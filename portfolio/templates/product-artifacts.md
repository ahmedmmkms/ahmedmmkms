# Product artifacts, with worked examples

[← Catalogue](../README.md) · [How to read the cases](case-study.md)

A useful artifact makes a choice easier to question and test. These completed examples use the school-pickup concept. They are **design proposals, not observed results**; each full case has its own domain-specific artifacts.

## Decision Snapshot

| Field | Worked example |
|---|---|
| Decision and status | Proposed: check current pickup authorization and require an authenticated staff member to confirm release. |
| Why it matters | Arrival in a queue does not establish permission to collect a child. |
| Alternatives considered | Manual procedure; digital coordination with staff confirmation; location-triggered release. |
| Decision drivers | Authorization, workload, revocation, connectivity, and traceability. |
| Evidence | Scenario assumptions and official principles cited in the school case; no school observation conducted. |
| Trade-off accepted | A confirmation adds work at the gate while preserving an accountable handover. |
| Risk introduced | Incorrect records, account misuse, and staff bypass remain possible. |
| Validation | Exercise authorized, revoked, duplicate, mismatched, and disconnected journeys; every accepted scripted release must reference current permission and a staff actor. |
| Reconsider when | The school cannot maintain authoritative permissions or staff cannot reliably complete the confirmation. |

## Assumption Register

| ID | Assumption | Confidence and impact | Validation | Consequence if false |
|---|---|---|---|---|
| EX-A01 | The school maintains reliable pickup permissions. | Low confidence; high impact. | Trace changes and revocation with the responsible administrator. | Establish record ownership before implementing release. |
| EX-A02 | Staff can confirm each handover during peak pickup. | Low confidence; high impact. | Rehearse normal and exception journeys at representative volume. | Redesign the station or reduce pilot volume. |
| EX-A03 | Connectivity may fail during handover. | Plausible; high impact; local frequency unknown. | Disconnect before and after acknowledgement. | Use an approved supervised fallback and reconcile records. |

Confidence changes with evidence, not enthusiasm for the solution.

## Trade Study

| Criterion | Manual procedure | Digital + staff confirmation | Location-triggered release |
|---|---|---|---|
| Permission changes | Depends on how updates reach staff. | Rechecks authoritative permission at release. | Location cannot establish permission. |
| Staff effort | Familiar; searching and recording may take time. | Adds confirmation; may reduce searching. | Leaves the authorization job unresolved. |
| Outage | Existing procedure remains available. | Requires supervised fallback and reconciliation. | Device or location failure undermines the trigger. |
| Auditability | Depends on record completeness. | Can retain actor, permission version, and outcome. | Arrival does not explain who authorized handover. |
| Delivery burden | Procedure review and training. | Identity, integration, transaction integrity, UI, and support. | Tracking burden without solving the core need. |
| Proposed use | Baseline and approved fallback. | Candidate for a bounded pilot. | Ineligible as release authority; optional arrival coordination only. |

These are qualitative design judgments, not customer scores. A feasibility gate can rule out an option before numerical scoring is useful.

## Metrics Tree

The intended outcome is dependable pickup coordination with manageable staff effort.

| Layer | Measure and denominator | Instrumentation | Proposed decision rule |
|---|---|---|---|
| Product | Complete records / digitally recorded releases. | Join release, staff actor, request, and permission version. | Block expansion if any scripted accepted release lacks provenance. |
| User | Exceptions resolved through designated workflow / observed exceptions. | Scenario logs and staff review. | Investigate every bypass before another rehearsal. |
| Operational | Eligibility-to-confirmation time, normal and exception journeys separately. | Events plus timed observation. | Establish manual baseline before choosing a time target. |
| Adoption | Independently completed staff tasks / attempted tasks. | Facilitator observation and help requests. | Revise confusing steps before live use. |
| Guardrail | Unauthorized or duplicate accepted transitions in the scripted suite. | State assertions and replay logs. | Proposed acceptance: zero; finite tests do not prove zero real-world risk. |
| Workload | Staff active minutes per pickup, including corrections. | Timed observation. | Include exception and reconciliation work in savings estimates. |

## Risk Table

| ID | Failure and consequence | Mitigation | Verification | Residual risk / proposed owner |
|---|---|---|---|---|
| EX-R01 | Revoked collector remains in queue. | Recheck permission in release transaction. | Revoke after queuing; release must fail. | Incorrect source records / school permission owner. |
| EX-R02 | Two devices release the same child. | Atomic transition and idempotent retries. | Concurrent requests create one release; retry returns original result. | Unrecorded physical handover / pickup supervisor. |
| EX-R03 | Lost acknowledgement creates uncertainty. | Show unresolved state; retrieve original result and reconcile. | Disconnect before and after server commit. | Fallback procedure errors / operations lead. |
| EX-R04 | Staff browse children outside assigned scope. | Role-limited views, session controls, audit. | Exercise allowed and denied paths. | Shared accounts and screen exposure / administrator. |

Likelihood remains unknown until the local workflow and incident history are examined.

## Requirements Matrix

| Need | ID | Proposed requirement | Acceptance check |
|---|---|---|---|
| Current authorization | EX-REQ01 | Check current permission when committing release. | Revocation after queue entry denies release. |
| Accountable handover | EX-REQ02 | Require authenticated staff confirmation. | Guardian sessions cannot submit accepted releases. |
| One release | EX-REQ03 | Commit atomically; repeated idempotency keys return the original result. | Concurrent devices and timeout retries create one transition. |
| Traceability | EX-REQ04 | Persist request, permission version, actor, time, and outcome with release. | Reconcile every accepted test release against these fields. |
| Visible exception | EX-REQ05 | Denied requests stay unreleased and show the escalation route. | Permission mismatch preserves the supervised workflow. |

The full school case expands these examples with privacy, accessibility, performance, and recovery requirements.

## Technical Appendix

<details>
<summary><strong>Inspect the worked system boundary and decision log</strong></summary>

### Context and system boundary

The school owns policy, permissions, exceptions, and physical handover. Software coordinates requests and records decisions. Location may support arrival; it cannot grant permission. Custody disputes go to authorized school staff under the established procedure.

### Architecture and dependencies

A guardian interface submits requests. A staff interface presents the queue and exceptions. The server validates identity and current permission, then commits release and audit together. Authoritative records and an approved outage procedure are dependencies.

### Interfaces and data model

A proposed request moves through requested, eligible, ready, and released states. Denied, cancelled, and exception states retain reasons. A release command includes request ID, authenticated staff identity, expected version, and an idempotency key. The server derives authorization from trusted records rather than a client permission flag.

Version conflicts return current state for review. Uncertain acknowledgement requires retrieving the original transaction. A second click cannot justify a second physical handover.

### Verification and validation

Unit and integration tests exercise state, roles, concurrency, and persistence. Supervised rehearsals establish whether staff notice revocation, understand exceptions, and recover from disconnection. A correct API alone does not establish a usable procedure.

### Decision log

| Proposed decision | Basis | Trade-off | Revisit trigger |
|---|---|---|---|
| Check permission at release. | Queue eligibility becomes stale. | Dependency on current data. | Updates cannot be delivered reliably. |
| Separate arrival from authority. | Proximity does not establish permission. | Retains staff confirmation. | Evidence supports an equally accountable authorized workflow. |
| Reconcile uncertain acknowledgements. | Timeout does not reveal transaction outcome. | Slower recovery than blind retry. | Rehearsal exposes ambiguity staff cannot resolve. |

</details>

Explore the full [AI](../case-studies/ai-operations-copilot.md), [clinical](../case-studies/clinical-workflow.md), [maintenance](../case-studies/predictive-maintenance.md), and [school-pickup](../case-studies/safe-school-pickup.md) cases.
