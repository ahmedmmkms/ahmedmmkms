<img src="../../assets/clinical-workflow-cover.jpg" alt="Editorial illustration of a clinical folder, sample tubes and an identification tag." width="100%" />

# Clinical Workflow Platform

[← All cases](../README.md) · [My approach](../approach.md)

*Concept Product Case Study · Product specification · October 2026*

A specimen has been collected, and someone at the next desk is trying to work out what happens next. That is the handoff I wanted to make easier to trust.

I chose **one outpatient diagnostic laboratory, routine blood specimens, and registration through report availability**. This setting is a design assumption, not a laboratory I observed. My contribution is the product definition, requirements, system boundaries, and evaluation plan.

## The decision in a minute

**Problem hypothesis:** Re-entering information and chasing handoffs consume staff time while making identity and specimen exceptions harder to resolve.

**My choice:** A small workflow layer around the existing laboratory information system (LIS): verified identity, complete intake, unique specimen references, and an accountable next step. Result interpretation and report authorization stay in the LIS.

**The trade-off:** Explicit checks add friction, and unresolved mismatches stop progression. I accept that cost because a fast-looking queue is a poor outcome if its records cannot be trusted.

**Proposed value:** Less administrative chasing and a reconstructable history. The initial design target is 20% less median administrative handling time, conditional on establishing a baseline and passing identity, access, and audit gates.

**Evidence status:** Public reference material and explicit assumptions. No interviews, prototype testing, implementation, clinical effectiveness, or measured improvement is claimed.

<details>
<summary><strong>People, workflow, and the opportunity</strong></summary>

### Stakeholders and jobs to be done

| Stakeholder | Job to be done | Product responsibility |
|---|---|---|
| Patient | Give accurate details and understand the next step. | Clear confirmation and privacy at shared desks. |
| Registration clerk | Link an authorized order to the correct record. | Search, duplicate review, structured intake. |
| Collector | Match person, order, and labeled container. | Explicit verification and label accountability. |
| Laboratory receiver | Accept traceable material or route an exception. | Receipt scan and an owned exception queue. |
| Authorized laboratory professional | Control released and corrected reports in the LIS. | Status synchronization without interpretation. |
| Quality lead / manager | Reconstruct problems and assess dependability. | Audit review and rollout authority. |
| IT / privacy owner | Operate a bounded, recoverable integration. | Access, monitoring, recovery, and retention policy. |

The manager is the assumed buyer. Patient benefit needs investigation beyond staff efficiency.

### Assumed current state and service blueprint

| Stage | Assumed current handoff | Proposed frontstage | Backstage owner / evidence |
|---|---|---|---|
| Registration | Demographics are transcribed. | Review existing identity and ambiguity. | Clerk; verification event. |
| Intake | Missing order information triggers calls. | Explain and resolve missing fields. | Registration lead; intake version. |
| Collection | Request and tube travel together. | Verify, print, and scan the label. | Collector; collection event. |
| Receipt | Receiver reconciles paperwork. | Accept or quarantine with a reason. | Receiver; receipt event. |
| Reporting | Staff call for status. | Show source, status, and freshness. | LIS; report version reference. |

WHO covers labeling, acceptance/rejection, validation, and nonconforming events; it does not establish failures at this hypothetical laboratory. [WHO quality manual resources](https://www.who.int/publications/m/item/laboratory-quality-manual)

The opportunities are fewer missing inputs, clearer ownership, and less status chasing. Identity and ownership come first.

### Assumptions and discovery commitments

| ID | Unvalidated assumption | Validation / consequence |
|---|---|---|
| A1 | One site handles 100 eligible visits/day, 22 days/month. | Obtain de-identified counts; resize load and economics. |
| A2 | An existing LIS owns orders/reports and has a supported interface. | Demonstrate sandbox round trips; otherwise evaluate LIS configuration first. |
| A3 | Two staff from each of registration, collection, and receiving can review the design. | Conduct six individual walkthroughs; revise handoffs around findings. |
| A4 | A controlled downtime procedure can support outages. | Quality lead rehearses reconciliation; block pilot if it fails. |
| A5 | Administrative handling averages four minutes/visit. | Observe 100 permissioned journeys across busy and quiet sessions, recording timings without identifiers. |

</details>

<details>
<summary><strong>PRD, requirements, and traceability</strong></summary>

### Goal and scope

The goal is attributable handoffs from order to report availability. Every active item has an owner, timestamp, and next action.

The MVP covers one site, one configured routine specimen pathway, supported barcode hardware, and one LIS adapter. Non-goals are diagnostic recommendations, interpretation, a new patient master, billing, patient messaging, emergency workflows, analyzer control, and replacement of existing critical-result communication.

### Functional requirements

These are proposed acceptance criteria, not completed tests.

| ID | Requirement and testable acceptance criterion |
|---|---|
| F1 | **Identity:** Record two site-approved identifiers and verification actor before collection. Missing identifiers or unresolved duplicate candidates block confirmation. Similar names never trigger automatic merging. |
| F2 | **Intake:** Require authoritative patient/order references, test code, requester, and configured specimen type. Removing any required field blocks progression and identifies the field. |
| F3 | **Collection:** Allocate a unique container ID and capture collector/time plus matching scan. Another order's container creates a hold; collection cannot complete. |
| F4 | **Receipt:** Accept or quarantine each container with a reason. Repeated receipt submission changes state once; rejected material cannot enter the accepted queue. |
| F5 | **Exceptions:** Every hold has an owner and next action. Only a permitted resolution closes it; the original mismatch remains in history. |
| F6 | **Reports:** Display availability only from a mapped LIS event with matching references. Corrected versions supersede older ones without erasing history. |
| F7 | **Audit:** Every mutation captures actor, role, event/receipt times, before/after values, reason, and correlation ID. Failed audit writes abort the associated mutation. |

CDC's mycology submission guidance illustrates matching two identifiers across documentation and labels. Site identification policy governs this proposal. [CDC specimen labeling guidance](https://www.cdc.gov/fungal/hcp/laboratories/specimen-submission.html)

### Nonfunctional requirements

| ID | Requirement and verification |
|---|---|
| N1 | **Access:** Server checks role/site on every request. Negative tests demonstrate that clerks cannot fetch reports and cross-site requests disclose no patient data. Review encrypted transport/storage and managed credentials before pilot. |
| N2 | **Integrity:** Commit state, audit, and outgoing event atomically. Crash/retry produces one effective transition; stale versions produce conflicts. |
| N3 | **Response:** Target p95 under two seconds for local search/confirmation with 20 concurrent users and 10,000 synthetic active records. Show LIS delay separately. |
| N4 | **Recovery:** Target restoration within 60 minutes and at most five minutes of recoverable database loss. Reconcile the possible loss window before resuming. |
| N5 | **Usability:** Complete all six screen flows by keyboard; status survives grayscale; assistive technology announces field errors. |

Before live data, the privacy owner must approve a jurisdiction-specific retention schedule. Synthetic evaluation records use a proposed 30-day deletion policy.

### Traceability matrix

| Need | Requirement / feature | Verification family |
|---|---|---|
| Correct identity | F1, F3 / verification and scan | T1 similar names; T2 swapped container |
| Complete information | F2 / intake validation | T3 missing order metadata |
| Accountable exceptions | F4, F5 / hold queue | T2 mismatch; T4 rejection/recollection |
| Trustworthy report status | F6, N2 / versioned synchronization | T5 corrections; T6 reordered events |
| Reconstructable history | F7, N4 / audit and recovery | T7 crash/restore |
| Responsive handoffs | N3 / search and confirmation | Load test with 20 users and 10,000 records |
| Appropriate access and usability | N1, N5 / scoped screens | T8 permission and keyboard walkthrough |

</details>

<details>
<summary><strong>Choices, MVP, and proposed screens</strong></summary>

### Alternatives and decision log

| Option | Strength | Limitation | Decision |
|---|---|---|---|
| Improve paper procedures | Low technology dependency | Limited shared status/history | Retain as downtime foundation. |
| Configure existing LIS | One authoritative system | Vendor capability and workflow fit | First discovery test; preferable if sufficient. |
| Add workflow layer | Focused handoffs and exceptions | Interface/reconciliation burden | Conditional concept choice. |
| Replace LIS | Broad control | Migration, training, continuity burden | Outside MVP. |

**D1 — Conditional build:** Abandon the separate application if LIS configuration satisfies critical requirements at lower lifecycle cost.

**D2 — Identity resolution:** Search flags candidates; the patient master owns merges. Accept less convenience for clearer ownership.

**D3 — Online confirmation:** Use approved downtime procedures; defer offline confirmation until reconciliation is proven.

### Prioritized backlog

1. **P0 prerequisites:** Confirm interface, identity rules, ownership, downtime, and access. Failure changes the build decision.
2. **P0 first slice:** F1–F5 and F7 with N1–N2, covering registration through receipt and holds.
3. **P0 complete MVP:** F6, accessible screens, recovery rehearsal, and measurement events; demonstrate the whole synthetic journey.
4. **P1 after evidence:** Aging queues, saved filters, and exception dashboards, ordered by observed chasing time.
5. **P2 expansion:** Additional sites/pathways after stable reconciliation. AI interpretation remains outside scope.

### Six-screen walkthrough

Proposed screens; no built or tested prototype is claimed.

1. **Find the visit:** Search an exact identifier, inspect bounded candidate details, and compare the second identifier. Similar records prompt review.
2. **Review intake:** Confirm source order and required fields. Readiness appears only after validation.
3. **Confirm collection:** Show person/order context, label print history, and scan result. Mismatches keep confirmation blocked.
4. **Receive specimen:** Scan the container; accept or quarantine. Explain the reason and responsible role.
5. **Resolve exception:** Show event history and permitted actions. Recollection links a new container to the rejected attempt.
6. **Check report:** Show source time, version, and correction banner. Authorized users open the source report through existing access controls.

</details>

<details>
<summary><strong>System design and failure paths</strong></summary>

### Architecture and ownership

The architecture uses a browser, authenticated API, relational database, transactional outbox, LIS adapter, and restricted audit destination. Retries preserve visible status freshness.

Registration owns demographics/merges. The LIS owns orders, acceptance policy, results, authorization, and corrections. This layer owns handoff tasks/history and minimum references.

A sandbox contract would evaluate FHIR R4 [ServiceRequest](https://hl7.org/fhir/R4/servicerequest.html) for orders, [Specimen](https://hl7.org/fhir/R4/specimen.html) for identity/collection, and [DiagnosticReport](https://hl7.org/fhir/R4/diagnosticreport.html) for report context and result references. Versions, profiles, terminology, and update semantics require contract tests; LIS support is assumed.

### State, data, and API sketch

Local work progresses through registered, intake-complete, collected, received, accepted, and report-available. A hold blocks progression independently; rejection/cancellation are explicit dispositions. These are local workflow states, not FHIR Specimen status codes.

Core fields: patient_ref, order_ref, container_id, source_version, workflow_version, owner_role, state, hold_reason, and append-only events. Store UTC; display site time.

A proposed request is POST /work-items/SYN-W042/receipt with Idempotency-Key SYN-E107 and If-Match version 7. The synthetic payload identifies container SYN-C042, disposition quarantine, and reason identity-mismatch.

The server derives actor/site from authentication, validates transition and version, then commits state/event together. Replays return the prior result; conflicting versions return 409; unauthorized access returns 403. Authenticated LIS callbacks are deduplicated by source event/version. Unknown references enter an integration exception queue.

### Exception paths

| Failure | Required behavior |
|---|---|
| Possible duplicate patient | Block collection; registration lead resolves in the patient master. Synchronize the authoritative decision; never silently merge. |
| Wrong/unreadable label | Hold progression; authorized staff apply specimen policy. Reprinting cannot establish identity. |
| Rejected specimen | Preserve rejection; link authorized recollection to a new container ID. |
| Demographic correction after collection | Preserve old values and flag affected work for identity review. |
| Corrected report | Supersede the previous version and assign an acknowledgment task to the responsible role. |
| LIS outage/reordered events | Show freshness, retry idempotently, and prevent older source versions regressing state. |

### Risk register

| Risk | Severity / uncertainty | Control and owner |
|---|---|---|
| Wrong association survives checks | High; likelihood unmeasured | Hard stops and adversarial cases; quality lead. |
| Staff bypass blocked queues | High; behavior unknown | Observe completion and exception burden; operations lead. |
| Cross-site disclosure | High; configuration dependent | Negative access tests; privacy/IT owners. |
| Lost/stale integration event | High; interface dependent | Reconciliation and freshness labels; integration owner. |
| Availability mistaken for clinical review | High; interpretation unknown | Distinct status wording; laboratory manager. |

</details>

<details>
<summary><strong>Measurement, validation, rollout, and economics</strong></summary>

### KPI tree and definitions

KPI tree: administrative effort → complete handoffs → verified intake/receipt → versioned events. All numbers are **unvalidated design targets**.

Use weekly registration cohorts and evaluate each visit seven calendar days after registration. This fixed observation window is a measurement assumption, not a clinical turnaround promise. Valid terminal dispositions are report available, authorized cancellation, or final rejection with a documented closure reason and no pending recollection. A rejected container awaiting recollection remains an open visit. Report cohort size and terminal-disposition counts alongside rates; a zero denominator is reported as not applicable.

| Measure | Definition / target |
|---|---|
| North Star candidate: attributable closure | Terminal visits with every handoff required for their disposition attributable and no unresolved hold ÷ all visits reaching a valid terminal disposition within the observation window; target ≥95%. Valid cancellation/rejection can qualify without a report. |
| Visit completion rate | Visits reaching any valid terminal disposition within seven days ÷ all eligible visits in the registration cohort. Report open visits and their age separately; establish a baseline before setting a target. |
| Report availability rate | Visits reaching report availability within seven days ÷ all eligible visits in the registration cohort. Show cancellation and final-rejection rates separately using the same denominator; no improvement target until case mix is understood. |
| Administrative handling | Active staff minutes registering, reconciling, and chasing per eligible visit; median 20% below baseline, matched by session/complexity. |
| Completeness | Intakes complete on first submission ÷ all submitted intakes; ≥98%. Include blocked submissions. |
| Adoption | Eligible visits completing the digital path ÷ all eligible visits; ≥90%; record downtime/bypass reasons. |
| Identity guardrail | Confirmed wrong associations reaching acceptance; any event pauses expansion. Zero scripted failures does not establish zero real-world risk. |
| Audit/integration | 100% scripted transitions reconstructable; source commit to display p95 ≤60 seconds under agreed sandbox load. |

During pilot, quality reviews exceptions daily; the PM reviews timing/adoption weekly. Analytics omit identifiers/report contents. Clinical turnaround is contextual because processing sits outside this product.

### Synthetic validation and staged gates

The proposed pack has 80 ordinary journeys and 40 exceptions: ten each for identity, specimen rejection, corrections/reordering, and outage/access. T1–T8 identify families. Define expected states, blocked actions, owners, and audit records before execution. Tests remain unexecuted.

**Now — establish feasibility:** Six role walkthroughs, baseline study, LIS configuration comparison, and contract proof. Proceed only with named owners and a justified capability gap.

**Next — prove behavior:** Build the sandbox and six-screen prototype; execute all 120 journeys, access/concurrency tests, and recovery rehearsal. Every critical criterion must pass with no unresolved high-severity defect.

**Then — bounded pilot:** Subject to site approval, one routine pathway, trained staff, daily reconciliation, and established downtime procedures. Pause for wrong association, disclosure, unreconstructable transitions, or lost correction visibility. Quality lead authorizes recovery after cause review and targeted retesting.

**Later — expand:** After four stable pilot weeks, assess guardrails, adoption, and timing. Reconsider if work merely shifts to another role or handling time worsens.

### Illustrative business case

Assume 2,200 visits/month, four administrative minutes/visit, 20% reduction, and USD 10/hour loaded labor. Released capacity is 2,200 × 4 × 20% ÷ 60 = **29.3 hours/month**, valued at approximately **USD 293/month**.

At assumed recurring support/hosting of USD 150/month, the difference is USD 143 before implementation, hardware, training, and change costs. An illustrative USD 3,000 setup cost gives roughly 21-month simple payback **only if released capacity has realizable value**.

At 10% reduction the recurring difference is approximately negative USD 3/month; at 30% it is USD 290/month. These are scenario inputs, not quotes or savings. Compare them with LIS configuration costs before building.

</details>

<details>
<summary><strong>Reflection, interview story, and sources</strong></summary>

### What I take from this design

The difficult question is who may declare a record correct. A barcode does not answer it; ownership of corrections clarifies the boundary.

I would challenge the modest modeled benefits early. Existing LIS configuration might be the better decision; real evidence could reverse this choice.

### Interview versions

**30 seconds:** “I developed a concept for outpatient lab handoffs. I prioritized identity, specimen traceability, and exception ownership while keeping clinical authorization in the LIS. The trade-off is explicit checking. I defined requirements and rollout gates, but have not claimed a pilot or results.”

**Two minutes:** Explain the assumed workflow, why identity precedes automation, the LIS configuration alternative, and a mismatched specimen. Finish with handling time as the value measure and identity/audit failures as stop conditions.

**Five minutes:** Add stakeholder needs, F1–F7 traceability, integration ownership, correction paths, synthetic tests, staged pilot, and economic sensitivity. Close with evidence that would reverse the build decision.

**“Is this compliant?”** No compliance assessment is claimed. Technical criteria still require site-specific jurisdictional, laboratory-policy, and governance review.

**“What did users say?”** No interviews occurred. Six role walkthroughs are proposed; workflow claims remain explicit assumptions.

### Sources

Reviewed 4 October 2026. These inform design, not claims about a particular laboratory.

- [WHO Laboratory quality management system handbook](https://www.who.int/publications-detail-redirect/9789241548274): general quality-management context.
- [WHO laboratory quality manual resources](https://www.who.int/publications/m/item/laboratory-quality-manual): labeling, acceptance/rejection, validation, and nonconforming events.
- [CDC specimen collection and labeling guidance](https://www.cdc.gov/fungal/hcp/laboratories/specimen-submission.html): contextual identifier guidance; site policy must be established locally.
- [HL7 FHIR R4 ServiceRequest](https://hl7.org/fhir/R4/servicerequest.html), [Specimen](https://hl7.org/fhir/R4/specimen.html), and [DiagnosticReport](https://hl7.org/fhir/R4/diagnosticreport.html): versioned resource boundaries for the proposed adapter.

</details>

[Case-study structure](../templates/case-study.md) · [Reusable product artifacts](../templates/product-artifacts.md)
