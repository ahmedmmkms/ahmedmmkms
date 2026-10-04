<img src="../../assets/ai-handwritten-board.jpg" alt="Illustrated AI concept board: Show the source. Keep the reviewer in control. Is checking faster than writing?" width="100%" />

# AI Operations Copilot

[← All cases](../README.md) · [My approach](../approach.md)

*Concept product case · Proposed design and validation plan · October 2026*

The moment that interests me comes after an incident is resolved. Someone still has to read the notes, separate promises from suggestions, and turn the next steps into work another person can pick up. An assistant is useful only if checking its output takes less effort than doing that work manually.

I have scoped this case to that handoff: draft follow-up tasks from a resolved-incident note, with the evidence beside each suggestion. A reviewer decides what becomes work.

**Scenario assumption:** A 12-person operations team at a fictional B2B SaaS company processes 100 resolved-incident notes per month. Each note is English text, at most 4,000 tokens, containing a timeline and follow-up discussion. These are design inputs, not observations of an employer or customer. No interviews, prototype, deployment, or measured results are claimed.

| At a glance | Proposed direction |
|---|---|
| User and buyer | Operations engineer preparing follow-ups; engineering manager funding the workflow |
| My contribution | Product framing, requirements, system design, trade study, and evaluation plan |
| Core decision | Extract evidence-backed drafts; require review before export |
| Success target | Reduce median active handling time by at least 30%, without increasing missed commitments |
| Main trade-off | Extra review friction in exchange for accountable task creation |

## The decision I would make

I would start with one note, one reviewer, and CSV export. A smaller commercial model is the first candidate to evaluate alongside a rules-based baseline. A frontier model is a challenger; self-hosting becomes relevant if the data boundary demands it. None has earned selection through testing yet.

Automatic ticket creation is tempting. I would defer it. “We could increase the timeout” is not a commitment, and its author may not own the work. A polished task with the wrong owner is still extra work.

**Synthetic example:** “Mona will add a rollback checklist. We discussed increasing the timeout, but decided to measure first.” The desired draft is “Add a rollback checklist,” with Mona as the source-named owner and no invented deadline. The timeout discussion appears as an unresolved decision. A deadline entered during review is labeled a human addition.

**What would change my mind:** If a structured note template saves comparable time with fewer errors, I would ship the template. If review is slow because the source itself is unclear, a better model may not solve the problem.

<details>
<summary><strong>Read the product case: users, scope, priorities, and review experience</strong></summary>

## Evidence and problem boundary

Google’s SRE guidance treats postmortems as reviewed records with follow-up actions and emphasizes learning without blame. This supports choosing a reviewable workflow; it does not establish demand for this product. [Google SRE: Postmortem Culture](https://sre.google/sre-book/postmortem-culture/)

NIST identifies confabulation and data privacy among generative-AI risks. OWASP describes indirect prompt injection through source content and excessive permissions/autonomy. These inform the controls and tests here; they do not prove this design safe. [NIST AI 600-1](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf), [OWASP prompt injection](https://genai.owasp.org/llmrisk/llm01-prompt-injection/), [OWASP excessive agency](https://genai.owasp.org/llmrisk/llm062025-excessive-agency/)

The workflow, targets, effort, economics, and validation design below are proposed assumptions.

## People and service opportunity

| Stakeholder | Job to be done | Tension |
|---|---|---|
| Operations engineer | Prepare accurate follow-ups without repeatedly rereading the note | Speed versus checking subtle commitments |
| Receiving engineer | Understand the action, evidence, and ownership before accepting work | Useful context versus another noisy queue |
| Engineering manager | See which agreed actions entered the backlog | Visibility versus confusing creation with completion |
| Security administrator | Control which incident data leaves the organization | Assistance versus sensitive logs and customer identifiers |

The assumed current workflow is: close incident → read note → identify commitments → clarify ownership → copy tasks → manager reviews backlog. I would observe this sequence before treating any step as waste.

The opportunity has three branches: improve note structure, reduce copying, and clarify ambiguous commitments. The MVP addresses copying and makes ambiguity visible. It cannot determine what the team actually agreed. The manager remains accountable for follow-through after export.

## Goals and prioritization

The goal is a faster, equally accurate handoff. Scope includes pasted text, source-linked drafts, ambiguity flags, edits, review history, and approved CSV export. Non-goals: live incident response, root-cause diagnosis, automatic priority/deadline assignment, customer communication, email/chat ingestion, and autonomous execution.

I prioritize dependencies and risk reduction rather than inventing reach estimates.

| Priority | Deliverable and rationale | Relative effort assumption |
|---|---|---|
| P0 | Structured-note/manual baseline; establishes whether AI is necessary | Small |
| P0 | Input controls and workspace permissions; prerequisite for real notes | Medium |
| P0 | Evidence, extraction, ambiguity, and review; tests the central value | Large |
| P0 | Export and evaluation events; proves the complete handoff | Medium |
| P1 | Exact duplicate warning within one note; reduces review clutter | Small |
| P2 | Ticket connector; adds permissions and reconciliation before proving value | Large |

Effort labels compare scope; they are not team delivery estimates.

## Testable requirements

All acceptance criteria are proposed release gates.

| ID | Requirement | Acceptance criterion |
|---|---|---|
| F01 | Accept one permissioned note | Reject input over 4,000 tokens before submission; show processing destination |
| F02 | Extract explicit commitments | Each candidate links to an exact source-version span; absent fields say “Not stated.” Approval preserves a verified evidence excerpt with enough context to retain qualifications/negation |
| F03 | Preserve ambiguity | All curated owner/date conflicts and conditional-language examples trigger review flags |
| F04 | Review each candidate | Accept/edit/reject records actor, time, before/after values, and reason; pending items cannot export |
| F05 | Export approved tasks | CSV includes approved items, source IDs/version, retained evidence excerpts, and human additions without needing a live source link; formula-like cells are safely encoded |
| F06 | Detect stale approvals | Source edits invalidate approval; stale-version export returns a conflict |
| N01 | Enforce workspace access | Cross-workspace read, review, and export attempts in the authorization suite are denied |
| N02 | Bound processing time | P95 ≤10 seconds at five concurrent notes of ≤4,000 tokens; timeout offers manual processing |
| N03 | Minimize retention | Full notes and unapproved candidate content expire 7 days after import; approved records/evidence expire 90 days after approval. Day-8 tests show expired source links, retained approved excerpts, and blocked approval of expired drafts; deletion tests include derived records and backup expiry |
| N04 | Preserve integrity | Repeated submit/review requests create no duplicates; results identify model, prompt, schema, and source versions |

## Six-screen concept walkthrough

This is a proposed interaction sequence, not a built prototype.

1. **Review queue:** Note age, reviewer, and state. Opening an item never marks it reviewed.
2. **Import:** Paste sanitized text, confirm permission, and inspect detected sensitive strings. Rejection explains correction or manual processing.
3. **Source and suggestions:** Highlight the passage for the selected candidate. Distinguish commitments, questions, and no-action passages.
4. **Resolve ambiguity:** “Two owners are mentioned” appears above the conflicting passages. A person resolves it or leaves the candidate pending.
5. **Review export:** Show approved tasks separately from omissions/rejections. Newly entered owners and dates say “Added by reviewer.”
6. **Handoff receipt:** Download a versioned CSV. Corrections create a superseding export; a previously downloaded file cannot be recalled.

I would test keyboard navigation, long/no-action notes, timeout recovery, and whether reviewers notice uncertainty before polishing the interface.

</details>

<details>
<summary><strong>Open the engineering and evaluation appendix</strong></summary>

## Alternatives and decision log

This comparison contains hypotheses, not benchmark results. Data terms, region, and quality gates come first. Compare passing candidates using measured reviewer time, cost, operating effort, and latency. Prefer a candidate that is no worse on those dimensions and better on at least one; where trade-offs remain, document the additional reviewer time saved and its incremental cost. Avoid combining unlike measurements into an arbitrary weighted score.

| Option | Reason to consider | Main uncertainty or burden | Confidence |
|---|---|---|---|
| Structured template + rules | Deterministic capture; simple operation | Author habit change; informal prose | Medium on simplicity, low on adoption |
| Smaller commercial model | Bounded extraction may fit its capabilities | Ambiguity errors, data terms, vendor dependency | Low before evaluation |
| Frontier commercial model | Challenger for difficult negation/conflicts | Whether better quality offsets cost and latency | Low before evaluation |
| Self-hosted open-weight model | Control over deployment and data location | Quality, license, GPU utilization, patching, monitoring | Low without infrastructure evidence |

**D01 — Narrow the source.** One note avoids premature retrieval and permission complexity. Revisit when observations show essential commitments elsewhere.

**D02 — Review before integration.** CSV tests the handoff cheaply. Revisit a connector if export/copying exceeds 20% of assisted handling time.

**D03 — Show uncertainty.** Use “Not stated” and conflict flags instead of model-generated confidence percentages. Calibration needs labeled evidence.

**D04 — Gates before cost.** Keep the simplest passing option. If AI does not beat the baseline, improve the template.

## Architecture and interfaces

An authenticated API validates membership, size, and permitted content, then saves a versioned source. A worker sends only that note through a model adapter. Schema and exact-span validation run before drafts enter the review store. Human approval enables export. The model has no tools, credentials, browsing, or ticket-writing access.

`POST /notes` returns ID/version. `POST /notes/{id}/extract` accepts that version and an idempotency key. `PATCH /candidates/{id}` requires the current candidate version; conflicts return HTTP 409. `POST /notes/{id}/exports` rechecks membership and approvals.

Candidate fields include action, source offsets/hash, owner/date with supporting spans or null, ambiguity flags, review state, and reviewer changes. Approval additionally stores a source-verified evidence excerpt, including relevant qualifications/negation, under the same classification and access controls as the note. Null means absent from the source. Exact-span validation proves the citation exists, not that it supports the action.

Separate source/event storage, exclude raw text from operational logs, encrypt transport/storage, scope credentials, and cap retries at one. Invalid output falls back to manual review. Approve actual provider retention and training-use terms before real-data processing. Full notes and unapproved candidate content expire 7 days after import; approved records, their review history, and minimum supporting excerpts expire 90 days after approval. After day 7, show “Full source expired” and the retained excerpt; do not imply that the full context remains auditable. Expired drafts require a new permissioned import and fresh review. Exports contain relevant evidence and disclose both expiry dates; downloaded copies follow the receiving team's policy because service deletion cannot recall them. Deletion covers caches/indexes and documented backup expiry. These periods require the pilot team's policy approval.

## Evaluation dataset and protocol

**Planned, not collected:** 240 synthetic development notes, followed by 120 permissioned, de-identified notes for locked evaluation. Split by incident and author/template family to prevent near-duplicate leakage. Synthetic results alone cannot justify rollout.

The 120-note test comprises 60 routine, 20 ambiguous/conditional, 15 no-action, 15 long/contradictory, and 10 notes with synthetic adversarial insertions. Difficult cases are deliberately oversampled: report strata separately and reweight only after observing actual frequencies. Two operations reviewers independently label commitments, fields, and spans; adjudicate disagreements before scoring.

Freeze model, prompt, settings, and schema. Evaluate all four alternatives on the same set and run each AI configuration three times. Count abstentions/timeouts as incomplete processing. Report note-level bootstrap intervals; a small zero-error sample cannot prove safety.

Error taxonomy: **E1** unsupported action; **E2** missed commitment; **E3** wrong owner/date; **E4** suggestion or negation misread; **E5** incorrect citation; **E6** duplicate/split action; **E7** sensitive-content exposure or permission failure; **E8** schema failure/timeout. Score fields separately: a plausible title must not hide an incorrect owner.

| Measure | Proposed gate and denominator |
|---|---|
| Precision | ≥95% supported commitments among emitted action candidates |
| Recall | ≥90% recovered commitments among adjudicated explicit commitments; review E2 severity |
| Unsupported actions | ≤2% E1 among emitted candidates; no critical invented operational action |
| Field accuracy | ≥98% correct emitted owner/date values; report omission rate separately |
| Handling time | ≥30% lower median active minutes per note, including corrections/export |
| Review burden | ≤20% of candidates require substantive edits; acceptance alone is not quality |
| Runtime | P95 ≤10 seconds; variable compute/storage ≤$0.10 per attempted note |
| Security | Zero E7 events in the suite; any observed exposure stops the pilot |

For timing, recruit six consenting reviewers for a counterbalanced comparison on 12 matched note pairs each. Each person sees a note once; rotate manual/assisted conditions and blind final quality assessment. Include preparation, corrections, and export. Treat this as exploratory; report uncertainty and expand sampling before making a purchasing claim.

The KPI chain is **lower handoff cost → faster accurate review → fewer correction minutes → stable extraction and runtime**. Track eligible-note coverage and opt-outs to detect success limited to easy notes. Task completion and recurrence remain downstream outcomes with many confounders.

## Illustrative economics

These are hypothetical USD planning inputs, not vendor prices or measured savings. At 100 notes/month, assume manual handling takes 8 minutes and assisted handling 5. At $30/hour, 300 minutes saved × $0.50 yields **$150/month** of capacity value, not automatic cash savings.

Assume 4,000 input/600 output tokens at hypothetical rates of $1/$4 per million tokens: $0.0064 inference per note. Add $0.0036 variable processing/storage for **$0.01/note**, or $1/month. With $40 fixed infrastructure and two support hours at $30/hour, operating cost is **$101/month**, leaving $49 modeled capacity value before development, onboarding, procurement, and taxes.

Break-even is $100 ÷ ($1.50 − $0.01), rounded up: **68 notes/month**. With only one minute saved, it becomes **205 notes/month**. This is a fragile standalone business case. I would test an embedded feature or shared service before building a broader SaaS product.

</details>

<details>
<summary><strong>Review risks, validation roadmap, and interview story</strong></summary>

## Assumption register

All assumptions remain unvalidated; confidence is low.

| ID | Assumption and impact | Validation and decision rule |
|---|---|---|
| A01 | 100 notes/month; high economic impact | Inspect one month of permissioned counts; recompute economics |
| A02 | 8-minute baseline; high value impact | Time 20 manual notes; proceed only with material avoidable effort |
| A03 | Explicit commitments are common; high quality impact | Label 30 notes; prioritize note structure if clarification dominates |
| A04 | External processing is acceptable; blocking impact | Administrator reviews data classes/vendor terms; use synthetic data until approved |

## Risk register

Roles are proposed accountabilities, not an assembled team.

| Risk | Severity / owner | Control and stop signal |
|---|---|---|
| Invented commitments accepted under pressure | High / product lead | Source-adjacent review and independent audit; pause for critical accepted E1/E3/E4 |
| Injection or cross-workspace disclosure | Critical / security lead | Least privilege, tenant tests, adversarial suite; exposure disables processing |
| Empty results hide missed work | High / operations lead | Explicit no-action review; failed recall gate blocks expansion |
| Stale exports create conflicting work | Medium / engineering lead | Version checks/supersession notice; investigate duplicates before connector expansion |
| Model change degrades performance | Medium / platform lead | Versioned regression evaluation and budgets; revert on gate failure |

## Discovery and rollout

**Weeks 1–2: understand the handoff.** Recruit four operations engineers, two receiving engineers, one manager, and one administrator. Observe six handoffs and time 20 notes with consent. Try a structured template first. Stop AI work if copying is minor or template improvement is comparable.

**Weeks 3–4: evaluate offline.** Develop synthetic examples, label permissioned notes, compare alternatives, and test the six-screen concept. Require quality/security gates. Engineering and security approve the data-handling design before real-data processing.

**Weeks 5–6: bounded pilot.** Offer a passing candidate to one team for resolved, low-sensitivity incidents, capped at 25 notes/week. Audit every exported action initially. Keep the manual workflow and processing kill switch. Require the time target, no quality regression, and no critical errors.

**Expansion gate:** After two consecutive passing weekly reviews, the manager assesses value, security reviews incidents, and engineering confirms support capacity. Add a connector only if export friction is the remaining bottleneck. Weeks are planning assumptions, not delivery promises.

## What I would want to learn

My strongest concern is that the assistant could make vague notes look certain. That is why ambiguity is visible and missed commitments matter as much as accepted suggestions.

The economics also change the ambition: a small team may value this feature without supporting a separate business. The next useful evidence is a real handoff and its cost. I would count a simpler template as a good outcome if it solved the problem.

## Interview versions

**30 seconds:** “I designed a concept that turns resolved-incident notes into reviewed follow-up tasks. The key choice was evidence-linked drafts with approval. I compare it with a structured template using time, omission, security, and cost gates. No pilot results are claimed.”

**2 minutes:** Establish the assumed handoff; use the Mona example; explain deferred automation; compare alternatives; finish with the 30% time target and fragile economics.

**5 minutes:** Add F02/F06, versioned interfaces, the error taxonomy, locked evaluation, timing study, and rollout gates. Close with reversal conditions: template parity, poor recall, unacceptable data terms, or insufficient volume.

## Sources

Checked 4 October 2026. Sources ground workflow/risk principles; the targets, architecture, economics, and validation plan are my design choices.

- [Google SRE — Postmortem Culture](https://sre.google/sre-book/postmortem-culture/): reviewed incident records, follow-up actions, and blameless learning.
- [NIST AI 600-1 — Generative AI Profile](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf): confabulation and privacy risk framing.
- [OWASP — Prompt Injection](https://genai.owasp.org/llmrisk/llm01-prompt-injection/): untrusted content, bounded privileges, adversarial testing.
- [OWASP — Excessive Agency](https://genai.owasp.org/llmrisk/llm062025-excessive-agency/): limited functionality/permissions and approval for consequential actions.

</details>

[Case-study structure](../templates/case-study.md) · [Reusable product artifacts](../templates/product-artifacts.md) · [Technical appendix format](../templates/product-artifacts.md#technical-appendix)
