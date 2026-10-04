<img src="../../assets/school-handwritten-board.jpg" alt="Illustrated school pickup concept board: Arrival is not permission. A staff member confirms release. Plan for the awkward moments." width="100%" />

# Safe School Pickup

[← All cases](../README.md) · [My approach](../approach.md)

*Concept Product Case Study · Product specification · October 2026*

A parent is waiting at the gate. A teacher is looking after a group of children. Someone needs to establish that this adult can collect this child today. I started with that moment because a smoother queue only helps if the handover remains accountable.

I chose an **assumed primary school with 300 pupils, one staffed pickup gate, and 200 guardian pickups in a 40-minute dismissal window**. These are planning inputs, not observations of a real school. My contribution is the product definition, system boundaries, requirements, and evaluation plan.

## The decision in a minute

**Problem hypothesis:** Staff spend time reconciling requests, changing permissions, and handover status across disconnected records.

**My choice:** A guardian request, a current school-managed authorization, and an explicit release by an authenticated staff member. Arrival information helps organize work; it never grants permission.

**The trade-off:** Verification adds effort at a busy moment. I would simplify that effort while keeping the decision visible and attributable.

**Proposed value:** Less coordination work and a reconstructable release record. A 15% reduction in median coordination time is a pilot hypothesis, subordinate to authorization and recovery checks.

**Evidence status:** Secondary research and explicit assumptions. No school interviews, built prototype, pilot, measured improvement, or demonstrated safety outcome is claimed.

<details>
<summary><strong>People, workflow, and evidence</strong></summary>

### Who needs what

| Stakeholder | Job to be done | Product implication |
|---|---|---|
| Guardian / authorized collector | Request pickup and understand delays. | Clear status and a staffed route without a smartphone. |
| Child | Remain supervised through a clear handover. | No child account or self-service release. |
| Teacher | Prepare the correct child without abandoning supervision. | Class-limited queue and readiness acknowledgment. |
| Gate staff | Verify collector, permission, and handover. | Focused release view with explicit exceptions. |
| School administrator | Maintain who may collect whom and when. | Versioned permissions and immediate revocation. |
| Safeguarding lead | Resolve sensitive exceptions using school policy. | Restricted escalation; no automated custody decisions. |
| Headteacher / operations manager | Resource dismissal and own adoption. | Assumed buyer and accountable rollout sponsor. |

The assumed current workflow combines an office permission list, verbal arrival messages, teacher coordination, and a manual handover record. That hypothesis must be observed before its inefficiencies can be quantified.

The proposed flow adds permission screening, teacher readiness, staff identity checks, current-permission recheck, and confirmed release. Exceptions route to a named school role while the child remains supervised.

### Research and its limits

England's Department for Education requires staff to follow school safeguarding procedures. This supports explicit ownership and escalation. England is a reference context; local duties require review. [DfE: Keeping children safe in education](https://www.gov.uk/government/publications/keeping-children-safe-in-education--2)

The UK ICO Children's Code emphasizes limited data, privacy defaults, and constrained geolocation. These inform the design; applicability to this adult-facing service needs assessment. [ICO: Code standards](https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/childrens-information/childrens-code-guidance-and-resources/age-appropriate-design-a-code-of-practice-for-online-services/code-standards/)

NIST distinguishes identity proofing, authentication, and authorization. My inference: sign-in cannot replace current pickup permission or a physical identity check. [NIST SP 800-63-4](https://pages.nist.gov/800-63-4/sp800-63.html)

### Assumption register

| ID | Assumption / confidence | Validation and decision consequence |
|---|---|---|
| A1 | 200 daily pickups and three gate staff; low. | Count five permissioned dismissal sessions; revise staffing and load assumptions. |
| A2 | The office can maintain one authoritative permission register; low, critical. | Rehearse changes and revocations with the administrator; no pilot without an accountable owner. |
| A3 | Most guardians can use a mobile web service; low. | Recruit six guardians with varied access needs; retain equivalent staffed pickup. |
| A4 | Added confirmation fits gate work; low. | Four staff complete normal and exception rehearsals; redesign if supervision suffers. |
| A5 | Connectivity can fail; plausible, impact high. | Interrupt devices and server access; require a workable supervised fallback. |

These sources support design principles. They do not establish demand, incident frequency, willingness to pay, or usability at the assumed school.

</details>

<details>
<summary><strong>Alternatives, MVP, and testable requirements</strong></summary>

### Options and scope

| Option | Strength | Cost or limitation | Decision |
|---|---|---|---|
| Improve paper process | Low technical dependence; familiar. | Version distribution and reconstruction depend on disciplined staff work. | Baseline and potential final answer. |
| Shared digital queue | Easier coordination. | Queue visibility alone cannot verify current permission or enforce one release. | Insufficient as the complete product. |
| Authorization register plus staffed release | Makes permission changes and decisions attributable. | Integration, training, connectivity, and recovery overhead. | Proposed MVP. |
| GPS / proximity release | Can signal arrival with little input. | Device location proves neither collector identity nor pickup authority. | Excluded from release decisions. |

I would choose improved paper procedures if they solve the problem at lower operational cost.

**Must:** One school, one gate, school-managed collector enrollment, permission validity/revocation, guardian requests, class queues, staff release, exception ownership, audit, and supervised fallback. These are dependency-ordered, not scored with invented reach data.

**Should:** Accessible messages, office-assisted requests with equivalent checks, queue filters, and reconciliation export.

**Later:** Notifications and supported school-information-system adapters, only after the core process works.

**Non-goals:** Facial recognition, continuous tracking, automated custody interpretation, unattended release, transport routing, emergency evacuation, and permission changes approved solely by a guardian. GPS is omitted from the MVP.

### Requirements and acceptance criteria

Every row is a proposed test, not a passed test.

| ID | Requirement and acceptance test |
|---|---|
| F1 | **Enrollment:** Only authorized school administrators link a verified collector to a child and time-bounded permission. A guardian's attempted self-authorization is denied and logged. |
| F2 | **Current authority:** At release, check active permission, its version, validity period, and restrictions. Revocation after queue entry blocks release. A concurrent revocation and release serialize against the same permission record. |
| F3 | **Staff decision:** Require an individually authenticated, gate-authorized staff actor and recorded physical identity check. Guardian credentials, readiness, arrival, or a queue entry alone cannot release. |
| F4 | **One release:** Enforce one final release per child/dismissal session. Two simultaneous confirmations produce one committed transition; a replay returns its original outcome. |
| F5 | **Exceptions:** Identity mismatch, disputed authority, or ambiguous status creates an assigned exception. Gate staff cannot bypass it with a generic override. The designated school role resolves it under policy. |
| F6 | **Traceability:** Commit request, child/session reference, collector, permission version, staff actor, server time, and outcome together. Audit-write failure prevents a release commit. |
| F7 | **Recovery:** Lost acknowledgment shows “Status uncertain—ask the coordinator.” Query the original command before retrying; never turn a timeout into a success indication. |
| F8 | **Access:** Guardians see only their permitted children; teachers only assigned classes. Cross-child, cross-class, and cross-school requests are denied server-side. |

| ID | Nonfunctional requirement and proposed check |
|---|---|
| N1 | **Responsiveness:** At 20 concurrent staff sessions and 10 requests/second, proposed p95 server response below two seconds; test a 15-minute burst. Timeouts retain an explicit unresolved state. |
| N2 | **Privacy:** No location, document images, or safeguarding narrative in routine queue logs. Inspect payloads, telemetry, exports, and role views for excluded fields. |
| N3 | **Usability:** Keyboard access, readable contrast, plain-language errors, and text labels alongside color; four staff complete a scripted session without facilitator rescue. |
| N4 | **Recovery:** In a rehearsal, coordinator establishes controlled fallback within five minutes and reconciles every affected child before digital release resumes. This is a proposed operational target. |

Traceability is direct: revoked permission maps to F2; duplicate handover to F4; ambiguous network response to F7/N4; unnecessary disclosure to F8/N2. Passing a software test does not prove that people follow the physical process.

</details>

<details>
<summary><strong>System boundary and six proposed screens</strong></summary>

### Data and interfaces

Proposed components: guardian/staff web interfaces, administration console, identity provider, authorization service, transactional database, and restricted audit viewer.

Core records: school, child, collector, versioned permission, dismissal session, request, staff assignment, release command, and exception. Sensitive documents stay in the school's controlled process; queues only direct staff to the designated lead.

The school-managed register is authoritative for the pilot. Bulk imports cannot silently replace a newer revocation. A future external register needs demonstrable freshness and conflict handling before it can authorize release.

A request submits child/session references; the server derives the collector from the authenticated session. A release submits request reference, expected permission version, and idempotency key; the server derives the staff actor, checks role and latest authority, and commits release plus audit atomically. A uniqueness constraint prevents multiple release records even when different keys arrive.

States are requested, eligible, ready, released, cancelled, and exception. Eligibility is provisional. Release is a recorded staff action immediately before physical handover, after identity checks and child readiness. Staff require a confirmed outcome before proceeding. If the handover then cannot occur, an exception records what happened; nobody erases the release or silently resets it.

### Outage boundary

The coordinator pauses digital release for the affected gate and establishes one supervised fallback ledger under the school's procedure. A cached permission list is not treated as current authority. If identity or permission cannot be established through that procedure, the designated school lead manages the situation while maintaining supervision.

Recovery reconciles the server record, command identifiers, physical handovers, and fallback ledger child by child. Digital release stays paused until inconsistencies are resolved. This avoids two competing sources of truth during a network failure.

### Screen walkthrough

These six screens are a written prototype specification; no functioning prototype is claimed.

1. **Guardian pickup:** Select an authorized child and request today's collection. Show “Request received” and the staffed help option. An unavailable permission produces a private office-contact message.
2. **Class queue:** Teacher sees only assigned children and marks readiness. “Ready” means prepared for the gate process, with no implication of release approval.
3. **Gate verification:** Staff select the request, compare the collector through the school's identity procedure, and view current permission status. A changed permission removes the confirmation action.
4. **Release confirmation:** A focused child/collector summary asks for one explicit staff decision. Success displays the recorded outcome; repeated taps return that same record.
5. **Exception desk:** Coordinator sees owner, reason category, and next permitted action. Custody-related concerns route privately to the safeguarding lead; other staff see no sensitive narrative.
6. **Reconciliation:** After disruption, coordinator compares unresolved commands and the fallback ledger, records discrepancies, and signs off restoration. Ambiguous records remain exceptions.

Staff must distinguish “ready,” “eligible,” and “released” in rehearsal; confusion requires interface revision.

</details>

<details>
<summary><strong>Metrics, economics, and risk</strong></summary>

### Measurement plan

Baselines are unmeasured. Observe five authorized sessions first, recording anonymous timing and workload samples, then compare equivalent dismissal conditions.

| Measure | Definition | Proposed decision rule |
|---|---|---|
| Coordination time | Median minutes from staffed arrival check-in to confirmed handover; normal and exception journeys reported separately. | Explore 15% reduction; reject gains that increase checking failures or staff burden. |
| Staff effort | Active coordination seconds across staff divided by completed pickups, including corrections. | No increase over baseline; supervision work must not be displaced. |
| Traceability | Releases with every F6 field / digitally recorded releases. | 100% in scripted testing; any missing record blocks expansion. |
| Authorization guardrail | Accepted unauthorized transitions / attempted unauthorized transitions in the adversarial set. | Zero accepted attempts in that finite set. This is not a population safety estimate. |
| Duplicate guardrail | Child/session pairs with multiple release commits. | Zero in concurrency tests and any monitored pilot. |
| Adoption | Staff completing normal and exception rehearsal tasks unaided / participating staff. | All four staff pass before participation; retrain or redesign otherwise. |

### Hypothetical economics

For illustration, 200 pickups × 20 days × 15 seconds less **aggregate staff work** equals 60,000 seconds, or 16.7 hours/month. At an assumed £20/hour, that is approximately £333/month of capacity, not necessarily cash savings.

Assume £120 monthly software/support and 12 onboarding hours at £20/hour amortized over 12 months: £140/month before hardware, integration, ongoing administration, and disruption costs. The illustrative capacity margin is £193/month. At five seconds saved, capacity value falls to £111 and the margin becomes negative. Break-even is 6.3 seconds saved per pickup on these limited assumptions.

This illustrates why effort needs measuring; purchase requires a fuller cost case.

### Risk register

| Risk / priority | Control and owner | Residual exposure / test |
|---|---|---|
| Stale or incorrect permission / critical | Versioned authority and release recheck; administrator. | Wrong source data remains possible; test revoke-before-confirm and concurrent change. |
| Account misuse or staff bypass / critical | Individual accounts, appropriate authentication, physical check, audit; school lead. | Credentials and judgment can fail; rehearse mismatched collector and shared-account attempts. |
| Concurrent release / critical | Atomic transition and child/session uniqueness; engineering owner. | Unrecorded physical actions remain outside software; test two devices and repeated commands. |
| Lost acknowledgment / high | Status inquiry and controlled fallback; coordinator. | Reconciliation creates workload; disconnect before and after commit. |
| Excess disclosure / high | Role limits, minimal fields, restricted audit; privacy owner. | Authorized misuse remains possible; test forbidden reads and review access records. |

Likelihood is unestimated. Severity and test priority express design judgment rather than fabricated incident statistics.

</details>

<details>
<summary><strong>Validation, delivery, and decisions</strong></summary>

Delivery order: agreed procedure/owner, register/access controls, request/queue, release, exceptions, then recovery rehearsals. Product coordinates; the school lead approves operations; engineering verifies implementation; the privacy owner approves data handling and retention before real records are introduced.

**Synthetic protocol:** Create 40 fictional children, 60 collector accounts, four staff roles, and 80 scripted journeys: 30 authorized, 10 revoked/expired, 10 mismatched or unlinked collectors, 10 concurrent/replayed commands, 10 network interruptions, and 10 access/exception cases. Each fixture specifies the expected state, permitted actor, and audit outcome. Repeat race and interruption cases at different transaction boundaries; preserve failures for regression.

The pass gate is every expected authorization/state outcome correct, zero duplicate commits, complete audit records, and a reconciled fallback rehearsal. Separately, four staff and six guardians review usability with fictional records. Neither exercise validates real-world safety.

**Rollout:** Start with process discovery, then synthetic rehearsal, then supervised shadow use that cannot authorize handover. A small live pilot requires school approval, privacy review, training, documented fallback, and successful reconciliation. Start with 20 consenting households at one gate while retaining an equivalent staffed route.

**Stop conditions:** Any unauthorized accepted transition, duplicate release, missing audit, unresolved permission freshness, privacy exposure, or staff confusion affecting supervision pauses the affected digital process. Resume only after the owner resolves the cause and reruns the relevant scenarios. After ten comparable pilot sessions, review workload, access barriers, exceptions, and traceability before expanding.

### Decision log and reflection

| Decision | Reason | Revisit when |
|---|---|---|
| Keep staff confirmation | Physical handover needs an accountable person. | Workflow evidence changes how confirmation fits, not who owns release. |
| Exclude GPS initially | Adds data and complexity without establishing authority. | Arrival coordination shows a measured need and privacy review supports limited use. |
| Start with one gate | Recovery ownership stays understandable. | One-gate reconciliation and staffing are dependable. |
| Treat unknown status explicitly | A timeout conceals whether a transaction committed. | A different implementation demonstrably preserves the same guarantees. |

A short queue can hide unresolved exceptions. I would judge the product by whether staff can explain and recover every handover, alongside whether families and staff find it easier to use.

</details>

<details>
<summary><strong>Interview versions</strong></summary>

### 30 seconds

“I developed a concept for staffed school pickup around one decision: arrival is not authorization. The design combines current school-managed permission with an authenticated staff release. I specified revocation, duplicate prevention, and outage recovery before proposing a small pilot. The contribution is a testable product specification; I have not run a school pilot or claimed safety improvements.”

### Two minutes

“I assumed 200 pickups at one school gate and compared improved paper procedures, a shared queue, and permission-aware release. I chose the last provisionally: staff verify the collector while the server checks current permission and records one release.

The MVP includes exceptions and fallback. An unknown network outcome requires reconciliation, not a second handover. I defined 80 synthetic journeys, usability rehearsals, and staged rollout gates. The economics become unattractive at five seconds saved per pickup, so evidence could still favor paper. This is a design contribution with untested assumptions, not an implemented school system.”

### Five minutes

Expand the two-minute version with three examples:

- **Revocation:** Eligibility changes after queue entry. Explain F2's transaction check and the administrator's source-data responsibility.
- **Lost acknowledgment:** Distinguish server commit from visible success, query the command, and explain fallback/reconciliation through F4, F6, and F7.
- **Learning:** Show the five-second economics sensitivity and separate routine journeys from exceptions. Explain what evidence would favor paper.

Close with the actual outcome: a complete specification and validation plan; implementation and field evaluation remain unperformed.

</details>

[How to read the cases](../templates/case-study.md) · [Worked product artifacts](../templates/product-artifacts.md) · [← All cases](../README.md)
