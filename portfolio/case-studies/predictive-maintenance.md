<img src="../../assets/maintenance-handwritten-board.jpg" alt="Illustrated maintenance concept board: Start with useful signals. Every alert needs a reason. Does the value justify the cost?" width="100%" />

# Industrial Predictive Maintenance

[← All cases](../README.md) · [My approach](../approach.md)

*Concept Product Case Study · Product definition and validation plan · October 2026*

An alert earns its place when it helps someone make a maintenance decision. A worrying graph with no way to check it simply creates another job for the technician.

This is what interests me about the case: connecting imperfect measurements to a practical decision. I would start with a small fleet, understand its operating conditions, and make every alert explain itself.

## The decision in a minute

| Question | My proposed answer |
|---|---|
| Who is this for? | A maintenance team responsible for 20 fixed-speed induction motors driving non-safety-critical water circulation pumps at one fictional manufacturing site. |
| What problem am I targeting? | Changes between scheduled inspections may go unnoticed, while symptom notes and maintenance decisions remain disconnected. This is a workflow hypothesis. |
| What would I build first? | Condition visibility, engineer-approved thresholds, and an alert-to-inspection log. Evaluate simple anomaly detection silently alongside it. |
| What is the trade-off? | Less predictive ambition and continued inspection effort in exchange for understandable behavior and evidence for the next investment. |
| What is my role here? | Independent product definition, alternative analysis, requirements, and evaluation design. |
| What exists today? | This specification and screen walkthrough. No interviews, sensor analysis, prototype, pilot, deployment, or savings measurement has been completed. |

The first success criterion would be a technician saying, with evidence, “This alert warranted that inspection.” A remaining-life estimate would come later, if it improves that decision.

## The operating scenario

I assume one site operates 20 comparable pump motors across two shifts. Four enter an initial observational pilot. Existing protection, operating procedures, and scheduled maintenance remain authoritative. This advisory product has no control or shutdown interface.

The assumed workflow is: an operator notices noise or heat; a technician performs rounds and takes occasional readings; a planner combines notes with scheduled jobs; a reliability engineer reviews recurring problems. Work-order closure records the intervention but may omit its original symptom or operating conditions. These are case assumptions, not findings about an actual employer or customer.

The opportunity is to connect condition history, an accountable inspection decision, and its eventual finding. That could help a planner schedule justified work and stop the same unexplained alert returning every shift.

| Stakeholder | Job to be done | Product implication |
|---|---|---|
| Technician | Decide what to inspect and what evidence to bring. | Show signal, time, operating state, and alert reason. |
| Maintenance planner | Fit justified work into available windows. | Assign an owner, due shift, and work-order reference. |
| Reliability engineer | Separate degradation from operating changes and sensor problems. | Compare like operating regimes; retain configuration and inspection history. |
| Operator | Report symptoms without interpreting analytics. | Short symptom entry tied to the correct motor. |
| Operations and IT/OT owners | Understand value while keeping plant systems dependable. | Separate observation from control; expose availability, workload, and cost. |

## Why start with simpler monitoring?

DOE's maintenance guide describes vibration analysis for investigating rotating-equipment problems. That supports investigating motor signals; it does not establish universal alarm limits or demonstrate value for this fleet. [DOE guide, Chapter 6.5](https://www.energy.gov/sites/default/files/2020/04/f74/omguide_complete_w-eo-disclaimer.pdf)

NIST's PHM integration white paper emphasizes selecting a use case, establishing current maintenance capability, and assessing effectiveness. I use that principle to require a baseline and economic gate before expansion. [NIST-hosted integration paper](https://www.nist.gov/publications/white-paper-determining-when-and-where-phm-should-be-integrated-manufacturing)

| Option | Value and burden in this assumed setting | Confidence and decision |
|---|---|---|
| Scheduled maintenance | Familiar and predictable; cannot observe every change between rounds. Uses existing labor and records. | Established approach; local adequacy unknown. Retain as baseline. |
| Fixed thresholds | Explainable and inexpensive to run; inappropriate limits or changing loads cause nuisance alerts. Needs commissioned sensors and engineering review. | Rationale is strong; local effectiveness untested. Select for first advisory release. |
| Statistical anomaly detection | Detects deviations without failure labels; normal startup or changed duty can look abnormal. Adds baseline management and tuning. | Plausible, untested hypothesis. Evaluate in shadow mode. |
| Predictive ML | Could provide useful lead time for specific failures; needs representative events, labels, and ongoing monitoring. Highest data and maintenance burden. | Insufficient case evidence. Defer until it outperforms simpler methods on held-out assets and decisions. |

I would reverse the decision if inspections already cover the relevant degradation, sensing proves unreliable, or recoverable value cannot cover operating cost.

<details>
<summary><strong>Read the product brief, economics, and delivery scope</strong></summary>

## Goal and business case

**Goal:** Help technicians investigate persistent condition changes and let planners trace alerts to findings. **Non-goals:** autonomous control, certified protection, guaranteed failure prediction, pump hydraulic diagnosis, replacing the maintenance system, or immediate fleet-wide ML.

This arithmetic example uses assumed USD values, not quotations or a forecast. Eight relevant interruptions annually across the fleet, four hours each, and $500 per hour of recoverable downtime cost create `8 × 4 × $500 = $16,000/year` of exposure. A hypothetical 30% reduction gives $4,800 gross annual benefit. That reduction is the central unvalidated assumption.

Assume $8,000 initial installation/integration and $3,000 recurring annual cost covering software, sensor upkeep, support, and routine alert review. Net annual benefit would be $1,800; simple payback about 4.4 years. First-year cash effect would be negative $6,200. Extra intervention costs would reduce benefit further and must enter the pilot ledger.

At 10%, 30%, and 50% exposure reduction, annual net benefit becomes negative $1,400, positive $1,800, and positive $5,000. Covering recurring cost requires 18.75% reduction; two-year payback requires about 43.75%, before financing and extra interventions. This does not justify an immediate fleet purchase. Operations and finance would first validate downtime attribution, redundancy, recoverable margin, and quotations.

## MVP and ordered backlog

| Priority | Deliverable | Why and release condition |
|---|---|---|
| P0 | Asset registry, commissioned sensing, stale/missing status | Identity and trustworthy readings precede analysis; pass commissioning checks. |
| P0 | Versioned thresholds, persistence, deduplicated alerts | Explain behavior; reliability engineer approves each asset configuration. |
| P0 | Owner, inspection finding, closure reason, audit export | Connect measurement to work and create evaluation labels. |
| P0 | Read-only OT boundary and recovery | Pass IT/OT review before connection. |
| P1 | Regime-specific anomaly detector in shadow mode | Assess additional detection against additional review burden after baseline collection. |
| P1 | Maintenance-system integration | Add after manual reference entry establishes the actual workflow. |
| P2 | Prognostics and other asset classes | Require representative labels and demonstrable decision advantage. |

Delivery needs product ownership, reliability engineering, technician participation, data/backend engineering, and IT/OT review. Installation access and an agreed response process are dependencies. The MVP includes one site and asset class, three signal types, advisory review, and manual maintenance references.

</details>

<details>
<summary><strong>Inspect requirements, data behavior, and failure controls</strong></summary>

## Requirements and verification

All numerical limits are proposed, unvalidated commissioning targets.

| ID | Need and requirement | Acceptance test |
|---|---|---|
| FR01 | Technician: bind readings to approved asset, sensor, mounting position, and configuration. | Swap sensor identities; quarantine unmatched records and prevent cross-asset trends. |
| FR02 | Engineer: explain alerts with signal, limit, regime, and persistence. | Three consecutive valid one-minute exceedances create one alert on the third; invalid/stale samples reset the count. |
| FR03 | Technician: show “Monitoring unavailable” after three minutes without a valid expected reading. | Disconnect the sensor; clear any current healthy indication within that interval. |
| FR04 | Planner: require owner, inspection outcome, and maintenance reference or documented no-work reason for closure. | Exercise assignment and closure; incomplete closure fails. |
| FR05 | Engineer: threshold changes need reason, approval, and retained versions. | Replaying an event under its original version reproduces the decision. |
| NFR01 | Operations: buffer at least 24 hours of summaries offline. | Disconnect at configured fleet load, restore service, and reconcile sequence numbers. |
| NFR02 | Technician: p95 visible-alert delay below two minutes from the end-of-window event time of the third qualifying summary. | With clocks synchronized within one second, measure through gateway queuing, uplink, ingestion, cloud rule evaluation, and dashboard display for 20 motors at 250 ms network round-trip time and 1% packet loss. Report each stage's delay; test disconnected recovery separately under NFR01. |
| NFR03 | IT/OT: outbound authenticated transport, role restrictions, and no control commands. | Inspect network boundary; attempt unauthorized reads, edits, and command submission. |
| NFR04 | Engineer: corrections preserve actor, reason, time, and prior value. | Edit a finding and verify both versions after export and restore. |

## Architecture and data contract

Sensors feed a gateway that validates identity, time, and quality, computes summaries, and writes a durable queue. Authenticated ingestion deduplicates records into time-series storage. Versioned rules execute in the cloud after ingestion, evaluate persistence using event times, and generate advisory events; the dashboard joins them to findings and maintenance references. The gateway does not issue condition advisories. Device health has a separate stream so silence cannot masquerade as healthy equipment.

Proposed inputs are vibration velocity RMS in mm/s over a documented band, bearing-housing temperature in °C, and read-only run-state. The vibration instrument performs high-rate acquisition and filtering; this application receives one-minute summaries. Actual bandwidth, mounting, calibration, and waveform capture require commissioning review. Summaries alone do not justify diagnosing a bearing defect.

Every record carries asset and sensor IDs, UTC event and receipt times, sequence number, units, aggregation window, feature/configuration version, run-state, quality flag, and measured values. Missing values remain null with a reason, never zero. Unknown units, implausible clocks, and conflicting duplicates enter quarantine. Late readings repair history without sending yesterday's alert as a current event.

Stopped equipment appears stopped. Startup and unknown regimes suppress condition scoring with a visible explanation; stabilization intervals are commissioned and versioned. Stale signals suppress condition claims and raise monitoring issues. Approved advisory limits remain separate from learned baselines; existing machine protection remains independent.

The gateway reserves 24-hour summary capacity and reports queue usage. Longer outages create explicit data-loss markers if retention is exceeded. Recovery reconciles sequence numbers and restores history before current scoring resumes. Initial retention assumptions are 90 days of summaries and 12 months of inspection/audit records, subject to site review.

## Lightweight failure analysis

Likelihood is unknown. Severity labels prioritize investigation rather than imply measured risk.

| Failure | Consequence and severity | Control and verification |
|---|---|---|
| Degradation missed | Avoidable interruption; high | Continue scheduled inspection; review every relevant maintenance event against prior signals. |
| Load change causes alarms | Unnecessary work; medium | Regime gating, persistence, and nuisance-event review. |
| Detached or frozen sensor | False reassurance; high | Quality/flatline checks, physical inspection, disconnected/frozen replay tests. |
| Wrong asset mapping | Wrong inspection; high | Commissioning sign-off and FR01 identity-swap test. |
| Alert ignored | Useful warning never becomes action; high | Owner, overdue queue, daily pilot review. |
| Gateway outage | Lost visibility; medium | Queue, unavailable status, recovery reconciliation, existing manual rounds. |

## Assumptions to resolve

All are low-confidence design assumptions, currently unvalidated.

| ID | Assumption | Validation method |
|---|---|---|
| A01 | Comparable motors form a useful cohort. | Inspect asset register, duty cycles, mounting, and access. |
| A02 | Useful degradation precedes relevant interruptions. | Review failure narratives and representative historical signals. |
| A03 | Staff can review advisories during their shift. | Observe rounds; time scenarios with both shifts. |
| A04 | Downtime has recoverable cost despite redundancy. | Reconcile event duration, production effects, and finance assumptions. |
| A05 | A 24-hour buffer covers ordinary outages. | Examine network incidents and storage sizing. |

</details>

<details>
<summary><strong>Review measurement, pilot gates, and proposed screens</strong></summary>

## Measurement plan

The measurement chain is **recoverable interruption cost → useful inspection decisions → reviewed alerts → valid signal coverage**. These are unvalidated targets, not results.

| Measure | Definition | Pilot target |
|---|---|---|
| Useful-alert rate | Reviewed episodes justifying inspection or a documented monitoring change / reviewed episodes; show unresolved episodes separately. | At least 60%, with engineer review of labels. |
| False-alarm burden | Episodes without corroborating issue or justified monitoring change per observed asset-week. | At most one per asset-week; also report review minutes. |
| Missed-event rate | Independently confirmed in-scope degradation events without a preceding alert in the agreed seven-day window / all such events. | No unexplained miss before expansion; too few events means effectiveness remains unknown. |
| Signal coverage | Valid / expected summaries during scheduled running minutes; exclude only predeclared downtime. | At least 98%, by asset and shift. |
| Response and adoption | Next-staffed-shift acknowledgement rate; complete findings / closed alerts. | At least 90% acknowledgement and 100% closure completeness. |
| Economic guardrail | Review, inspection, support, and incremental maintenance costs against defensible avoided-cost scenarios. | Credible agreed payback case; no savings claim from a short pilot. |

An episode combines recurring signals for the same asset and condition until reviewed or cleared under an approved rule. Counting sensor samples would inflate performance statistics.

## Evaluation and rollout

First examine maintenance history and observe handoffs. Commission four motors spanning age, duty intensity, and both shifts. Allow two weeks for observation and at least four further weeks in shadow mode. These periods assess usability and collection; rare failures may require much longer observation.

Replay startup, steady running, shutdown, load changes, sensor loss, clock errors, and seeded abnormal patterns. Synthetic replay verifies software only, not field sensitivity. If permissioned historical events exist, split by asset and time to prevent adjacent-window leakage; freeze settings before evaluation.

Review every alert and relevant work order, including events without alerts. Sample normal periods across regimes and shifts. Have a reliability engineer adjudicate uncertain outcomes without seeing detector identity where practical. Report counts, uncertainty intervals, exclusions, and unresolved labels. Zero observed failures cannot demonstrate detection recall.

NIST emphasizes verifying sensing and analytics under changing manufacturing conditions. That informs my evaluation approach; this proposed protocol is not a validated NIST procedure. [NIST monitoring, diagnostics, and prognostics program](https://www.nist.gov/programs-projects/monitoring-diagnostics-and-prognostics-manufacturing-operations)

**Now:** Establish identity, data quality, context, and ownership. Gate: requirements tests pass and every pilot motor meets coverage targets. **Next:** Enable threshold advisories and compare shadow anomalies using the same reviewed episodes. Gate: acceptable workload, investigated misses, and credible economics. **Later:** Expand in small cohorts; consider predictive ML only with representative labels, useful lead time, and better held-out decision performance.

Pause advisories after wrong mapping, uninterpretable data, or a missed event revealing unsafe reliance. A separate short-term workload stop applies when one motor generates at least three confirmed nuisance episodes across two consecutive fully observed staffed shifts, each with at least 98% signal coverage; evaluate at each shift end after those two shifts have elapsed. This stop threshold does not replace the stricter weekly target used to judge expansion. The maintenance lead returns to existing rounds. Preserve evidence, correct the cause, rerun affected tests, and require engineering sign-off before resuming. Connectivity recovery cannot clear a commissioning fault.

## Six proposed screens

These are interaction specifications, not a built prototype.

1. **Fleet overview:** Pilot motors show running, stopped, attention, or unavailable, plus last valid reading.
2. **Asset condition:** Compare vibration, temperature, operating state, sensor quality, and recent work. Gaps stay visible.
3. **Alert explanation:** Open the qualifying samples, approved limit, configuration version, and asset-specific inspection guidance.
4. **Inspection record:** Record finding, supporting note, and further-work decision. “No corroborating issue” is valid.
5. **Planner queue:** Show owner, due shift, and maintenance reference. A second related alert joins the open episode.
6. **Pilot review:** Review useful episodes, nuisance burden, missed-event investigations, coverage, and costs before expansion.

</details>

## What this case changed in my thinking

The economic example is deliberately uncomfortable: modest avoided downtime may not pay for a platform. Asset selection and the inspection workflow therefore become central product decisions. Better model accuracy cannot rescue a deployment with little recoverable value.

My next learning priority would be to follow a technician and planner through a real alert-to-work decision, then inspect the corresponding data. Until then, this is a testable proposal with clear reasons to stop or change direction.

<details>
<summary><strong>Interview walkthrough: 30 seconds, two minutes, or five minutes</strong></summary>

**30 seconds:** “I designed a concept for a small pump-motor fleet. I chose explainable threshold advisories and an inspection feedback loop, with anomaly detection in shadow mode. The trade-off is less prediction in exchange for trustworthy operation and useful evidence. The hypothetical economics show why I would validate the use case before a fleet-wide purchase.”

**Two minutes:** Add the assumed workflow, four alternatives, and distinction between missing data and healthy equipment. Explain the $16,000 exposure scenario, slow payback at 30% reduction, and useful-alert/workload measures. Close with the evidence needed to proceed.

**Five minutes:** Walk through the six screens, one identity or outage failure, requirements, and event-based evaluation. Explain independent review of missed events, normal-period sampling, rollback, and the evidence that could justify predictive ML.

**“Why predictive maintenance without a predictive model?”** This explores a path toward condition-based and potentially predictive decisions. Its first release is condition monitoring; remaining-life prediction is a gated research option.

**“What did you achieve?”** A concept specification and evaluation plan. I can explain the decisions and limitations; there is no measured operational improvement to report.

</details>

[Explore the other cases →](../README.md)
