**BUSINESS REQUIREMENTS DOCUMENT — MVP ITERATION 1**

**BRD-001**

**Wealth Individual Onboarding — MVP1 Consolidated Scope**

*AI-augmented, MAS-aligned source-of-wealth establishment · Proof of Concept*

*This is a single consolidated document assembled from: BRD-001 Wealth Individual Onboarding (master BRD), BRD-001 MVP Functional Requirements v1.0, BRD-001 Functional Components Overview, and the APJ_BFS_BRD001 Plan & MVP Feature List. It reflects **MVP Iteration 1 scope only** — later iterations (2, 3, 4) and unscheduled features are called out separately so they are not mistaken for MVP1 scope.*

| **Field** | **Detail** |
|---|---|
| Document Code | BRD-001 (MVP1 Consolidated) |
| Initiative | AI Platform · Wealth Onboarding Acceleration |
| Regulatory Anchor | MAS Circular AMLD 08/2024 (26 Jul 2024) — Establishing the Sources of Wealth of Customers |
| Programme Owner | Delivery Partner — Cognizant |
| Status | Draft — for review |
| Classification | Internal · Confidential |

---

# 1. Purpose of this Document

This document consolidates the Wealth Individual Onboarding BRD, its MVP functional requirements, its functional components overview, and the delivery plan/feature list into a single scope definition for **MVP Iteration 1**. It exists so that build, review, and sign-off can proceed against one source of truth rather than four separate documents.

# 2. Regulatory Anchor

> **MAS Circular AMLD 08/2024**
> Issued by the Monetary Authority of Singapore on 26 July 2024. Requires financial institutions to use appropriate and reasonable means to establish and independently corroborate the source of wealth (SoW) of customers, using documentary evidence or reliable public information. Three risk principles apply: **Materiality** (focus on more material or higher-risk SoW components), **Prudence** (prefer reliable third-party corroboration over self-attestation), and **Relevance** (fit-for-purpose evidence for each SoW claim). Procedures must be risk-proportionate and must not impose undue delay on legitimate customers. Senior management must exercise close oversight of higher-risk accounts. Reinforced by the ACIP Best Practices on Source of Wealth Due Diligence (May 2025).

The end-state target (all iterations) is to compress onboarding TAT from 10–20 business days to 1–3 days for clean cases and 3–5 days for cases needing deeper corroboration, while keeping the KYC analyst and Compliance Officer as decision-makers throughout.

# 3. MVP Iteration 1 — Scope

## 3.1 In Scope for MVP1

**Single profile, single happy/unhappy path only:**

- **Profile A — Business sale** (clear primary SoW; Sale & Purchase Agreement + tax filings available). No other wealth profile (B–H) is built in MVP1.

**Showcase scenarios for Profile A:**

1. **Positive case** — RM application upload through to account opening, with all checks matching as expected.
2. **Negative cases:**
   - Application sent back to RM for insufficient documentation.
   - SoW with **Red** risk rating — rejected by Compliance Officer (e.g. inconsistency in wealth evidence).
   - SoW with **Yellow** risk rating — e.g. inconsistency in current address (apartment-name misspelling in the application form but consistent elsewhere) — Compliance Officer approves with justification remarks.

**Intake mechanism:** RM uploads a single zip file with predefined naming conventions to classify artifacts:

| Artifact | Format |
|---|---|
| Application form from customer | PDF |
| Identity proof | — |
| Address proof | — |
| TIN — CRS self-attestation | — |
| Source of wealth declaration (customer statement) | — |
| Business sale agreement | — |

## 3.2 Out of Scope for MVP1

- **Profiles B through H** — ongoing business income, inheritance, mixed sources, prominent public profile, adverse-media false positive, PEP relative, uncorroborated component. (Deferred to later iterations / Phase 2.)
- **Public-source corroboration** — the MVP Feature List does not assign this to any MVP iteration (Feature #25); treat as **not built in MVP1** even though the base BRD and process narrative describe it as part of the target end-state. *(Flagged as an open item in Section 10 — the workflow narrative and the feature list currently disagree on this point and need reconciliation with the business.)*
- Date-format variants beyond a single assumed format (Gregorian vs other calendars) — explicitly out of scope.
- Re-upload / replace of artifacts after submission (MVP Iteration 2).
- Freeze/approve-upload gating logic as a distinct step — MVP1 assumes the application is frozen automatically upon upload (Iteration 2 formalizes an explicit freeze action).
- Configurable similarity-match thresholds for photo/signature comparison — MVP1 uses an inbuilt fixed threshold (configurability is Iteration 3+).
- Configurable/pluggable regulatory regime (swapping MAS for HKMA, etc.) — MVP1 is hard-coded to MAS; configurability is a later iteration.
- Analyst/compliance workload dashboards showing case status to RM/CO/Analyst (Iteration 2).
- System learning from compliance officer justification/rejection comments (not iteration-tagged — future).
- Live vendor/core-banking integrations — MVP1 uses stubs throughout.

# 4. Stakeholders / Roles (MVP1)

For MVP1, assume a **1:1:1 cast** — one Relationship Manager, one KYC/Operations Analyst, one Compliance Officer — so case-assignment/routing logic between multiple people in the same role is not required.

| **Role** | **MVP1 Responsibility** |
|---|---|
| Relationship Manager (RM) | Uploads the zipped application pack; receives consolidated information requests and rejection notices. |
| KYC / Operations Analyst | Views the application after RM upload/freeze; reviews the system assessment report; approves, rejects, or justifies each observation; escalates to Compliance Officer. |
| Compliance Officer (CO) | Receives analyst's actions; approves or rejects the application with mandatory justification comments; once decided, has no further action on that case. |
| MLRO / Senior Management | Oversight role per MAS Circular for higher-risk cases (workflow narrative describes this; confirm against MVP1 feature list — see Section 10). |
| System / AI Platform | Ingests, classifies, extracts, validates, cross-checks, summarizes mismatches, and routes the case between roles. |

# 5. MVP1 Functional Feature List

*Source: APJ_BFS_BRD001 Plan — "MVP FeatureList" worksheet, filtered to Iteration 1 items only. "Must-have" reflects the sheet's own flag; items marked TBD/Pending were still tagged Iteration 1 but have open questions attached (see Section 10).*

| # | Feature | Roles | MVP1 Must-Have | Timeline | Notes / Assumptions |
|---|---|---|---|---|---|
| 1 | RM can upload wealth management customer application and supporting documents. | RM, System | Yes | Week 1 | Needs realistic sample templates; upload size/format limits TBD. |
| 4 | System automatically triggers application analysis once RM has approved the upload. | System | Yes | Week 2 | Processing-time NFR compliance can be deferred to Iteration 2; base BRD targets ~5 min for a clean case. |
| 5 | System detects uploaded documents, classifies them, and reports format/size errors upfront. | System | TBD | Week 2 | Acceptance/exception criteria and real-time vs. offline error criteria still being defined. |
| 6 | System reads text from scanned documents (PDF, images) and URLs. | System | Pending info from BAs | Week 3–5 | MVP1 uses stubs for background/ID verification and adverse-media screening instead of live crawling. |
| 7 | System classifies extracted text/images into entities (name, dates, address, signature, etc.). | System | Yes | Week 3–4 | Start with a basic entity set in MVP1; extend in Iteration 2. |
| 9 | System reports mismatches in summary and detailed format with citations to source. | System | Yes | Week 3 | Lets the Analyst quickly see deviations. |
| 10 | System summarizes all mismatch observations into a single report with recommended actions. | System | Yes | Week 3–4 | Single consolidated report reduces TAT, consistent with the MAS Circular's intent. |
| 12 | KYC/Operations Analyst can view the application only after RM has approved/frozen the upload. | Analyst, System | Yes | Week 4 | MVP1 assumes 1 RM / 1 Analyst / 1 CO, so no assignment-pool logic is needed. |
| 13 | KYC/Operations Analyst can view the system assessment for the selected application. | Analyst, System | Yes | Week 4 | — |
| 16 | KYC/Operations Analyst can approve, reject, and/or justify each observation in the system-generated assessment report. | Analyst, System | Yes | Week 4 | MVP1 assumes RM opens a new application if the current one is rejected. |
| 18 | System sends KYC/Operations Analyst actions to the Compliance Officer for further action. | System, CO | Yes | Week 5 | MVP1 assumes a linear sequence — no bypassing the Compliance Officer for simple cases. |
| 19 | Compliance Officer can approve or reject the application. | System, CO | Yes | Week 5 | — |
| 20 | System enforces justification comments from the Compliance Officer for both approval and rejection. | System | Yes | Week 5 | — |
| 22 | Once the Compliance Officer approves/rejects, they have no further action on that application. | System, CO | Yes | Week 5 | — |

**Deferred beyond MVP1 (for reference — not built in Iteration 1):**

| Feature | Deferred To |
|---|---|
| RM re-upload / replace of artifacts | Iteration 2 |
| Explicit approve/freeze-upload action (MVP1 assumes auto-freeze on upload) | Iteration 2 |
| Configurable similarity thresholds for entity matching | Iteration 3 (inbuilt fixed threshold in Iter 1–2) |
| Configurable regulatory-regime injection (MAS → HKMA, etc.) | Iteration 3+ (MVP1 hard-coded to MAS) |
| Analyst view of recommended actions / accept-override of recommended actions | Not yet planned ("No") |
| Accumulation of justification/rejection comments for feedback/learning | Not yet planned ("No") |
| Application status dashboard visible to RM/CO/Analyst | Iteration 2 |
| Quality assessment of rejection justifications | Iteration 3 |
| System learning from Compliance Officer comments | Not yet planned ("No") |
| Public-source corroboration | Not yet planned ("No") — see Section 10 |

# 6. Functional Requirements — Validation Rules (MVP1, Profile A)

## 6.1 Match: ID proof vs. application form

- ID proof is under its expiry date (current date < expiry date).
- Customer name (first, last, middle) matches 100%.
- Date of birth matches 100%.
- Gender matches 100%.
- Photograph on the ID proof matches the customer's photograph (similarity match).

## 6.2 Match: address proof vs. application form

- Address proof is under its expiry/validity date.
- Customer name (first, last, middle) matches 100%.
- Address matches, pincode matches 100%, city matches 100%.
- Address matching allows for common abbreviations (e.g. Street/St, Apartment/Apt, first/1st).

## 6.3 Match: business sale agreement, self-declaration, and application form

- Customer declaration and business sale agreement match on: business name (100%), sale location in Singapore (business registered address, buyer and seller addresses all in Singapore), business purchase value (100%), buyer and seller names (100%).
- Sale date is in the past (sale date < current date and < application date).
- Signature matches between the sale agreement and the wealth onboarding application form.

## 6.4 Screening

- Customer application is checked against PEP (Politically Exposed Person), Sanctions, and adverse-media related questions.

## 6.5 Generic date validity

- All dates must be valid calendar dates.
- All dates are assumed to be in the same format for comparison (cross-format/calendar matching is explicitly out of scope for MVP1).
- All dates must be ≤ the application submission date.
- All "expiry" dates (ID, address proof) must be ≥ the current date (proof not expired).
- Dates must match across application, ID proof, address proof, and sale agreement wherever applicable (e.g. date-of-birth match).

# 7. Stubs for MVP1 (instead of real-time APIs)

- **Background (BG) verification** — third-party stub validating ID proof, address proof, and business sale agreement authenticity.
- **TIN validity check** — stub for Tax Identification Number validation.
- Sanctions / PEP / adverse-media screening are also treated as stub inputs for MVP1 rather than live vendor calls.

# 8. Workflow — Step-by-Step (Profile A / MVP1)

1. **RM submits the onboarding pack** — government photo ID, SoW declaration, and supporting documents (sale agreement, etc.), submitted as a single zip through the origination channel.
2. **System ingests and classifies documents** — each document is classified into a wealth-specific category and structured fields are extracted (names, dates, amounts, jurisdictions, signatories), requiring ≥90% classification confidence.
3. **System runs cross-document consistency checks** — verifies name/DOB/address consistency between the ID, application form, and SoW declaration/supporting documents. This is a consistency check, not document authentication.
   - **Decision 1 — Evidence sufficient?** If no, the system generates a single consolidated Request for Information back to the RM (all gaps listed once, not iteratively); the RM resubmits and the flow re-enters at Step 2. If yes, continue.
4. **System performs SoW analysis using MAS principles** — Materiality (rank SoW components, lead with the largest), Prudence (prefer audited/official evidence over self-attestation; flag self-attestation-only components), Relevance (match evidence type to claim — e.g. SPA for a business sale).
5. **System runs screening (stubbed) and, per the base BRD, public-source corroboration** — sanctions, PEP (incl. relatives/associates), adverse media, and ID verification stubs run in parallel and are logged. *(Public-source corroboration is described here in the base BRD's process narrative, but is not currently assigned to an MVP iteration in the feature list — see Section 10.)*
   - **Decision 2 — Sanctions / PEP hit?** If yes, the Analyst reviews and flags the case HIGH risk; the system auto-triggers an MLRO notification (read-only oversight access) per MAS Circular. If no, continue.
6. **System drafts the SoW narrative** — MAS-aligned narrative with line-level citations to source documents/vendor results, leading with the most material component and flagging gaps.
7. **System proposes a risk rating** — LOW / MEDIUM / HIGH with a reasoning trail (≥3 policy citations). *Risk-rating logic still requires clarification from domain SMEs (see Section 10).* This is a recommendation only.
8. **KYC Analyst reviews the case** on a single screen — extracted fields, screening results, narrative, and proposed rating.
   - **Decision 3 — Analyst accepts?** If no, the Analyst edits or overrides, with mandatory rationale captured in the audit log, then proceeds to Compliance. If yes, proceeds directly to Compliance.
9. **Compliance Officer reviews** the full case — narrative, rating, and any analyst overrides. This is the authorised HNW sign-off step.
   - **Decision 4 — CO approves?** If yes, proceeds to account-open (Step 10). If no, the rejection and rationale are logged and the case is routed back to the RM as a formal rejection; RM communicates the outcome to the customer; the application does not re-enter the workflow.
10. **System pushes the account-open request** to the core banking stub, which returns a `customer_id` and `account_id`.
11. **Audit log closed and RM notified** — a complete, append-only trace of every system event, AI output, citation, analyst/compliance action, and override is written; the RM is notified the account is live.

**Summary of outcomes**

| Path | Outcome |
|---|---|
| Clean case, no hits, accepted | Account open in 1–3 business days |
| Gaps found, RFI issued | Re-enters flow after RM resubmission |
| Sanctions / PEP hit | HIGH risk flag, MLRO notified, case continues under oversight |
| Analyst override | Rationale captured, case escalated to Compliance |
| Compliance rejection | Application closed, RM informs customer |

# 9. Functional Components Overview

The architecture has three layers:

**Layer 1 — Actors and channels.** RM (submits), customer (source of SoW declaration and documents), origination channel (carries both) feed in; KYC Analyst (primary reviewer) and Compliance Officer (sign-off authority) receive; MLRO sits separately as an oversight-only consumer, engaged only once a case is flagged HIGH.

**Layer 2 — AI Platform internals.** Five functional modules, each mapped to functional requirements:
- **Document Ingestion** — receive, classify, extract (FR01–03).
- **SoW Analysis** — applies Materiality, Prudence, Relevance (FR04–07).
- **Screening** — orchestrates the vendor checks in parallel (FR10).
- **Public Corroboration** — handles the prominent-profile case (FR08) — *status in MVP1 to be confirmed, see Section 10.*
- **Narrative & Rating** — produces the MAS-aligned draft with citations (FR11–12).
- **Case Review UI** — what the Analyst sees (FR13–15).
- The **audit log** is a cross-cutting concern every module writes to.

**Layer 3 — Data and integration.** Live/real for the POC: document repository, policy library, curated public-corroboration set, audit log store. Stubbed: sanctions, PEP/adverse media, ID verification, core banking. This live/stub boundary is what production would later replace with real vendor integrations.

# 10. Open Questions / Items Needing Clarification

Consolidated from the delivery plan and feature-list remarks (reference: email thread *"Re: AI Platforms for Industry Studios | APJ | BFS - BRD001 queries"*):

1. **Public-source corroboration in MVP1** — the base BRD's process narrative (Step 5) and Functional Components Overview both describe public-source corroboration as part of the flow, but the MVP Feature List does not assign it to any iteration (treated as "No"/not planned). Needs reconciliation: is it in MVP1, deferred, or dropped entirely for the POC?
2. **Risk-rating logic** — requires clarification from domain SMEs on how LOW/MEDIUM/HIGH is derived and weighted.
3. **Upload constraints (NFR)** — file size limits and accepted formats for the zip upload are still TBD.
4. **Processing-time NFR** — target is ~5 minutes for a clean case (per base BRD §12); full NFR compliance may be deferred to Iteration 2.
5. **Document error/exception criteria** — real-time criteria (format, size, naming, missing artifacts) vs. offline criteria (consistency mismatches, assumed 3–10 min) need to be finalized.
6. **Entity list for extraction** — full list of entities to classify (beyond the basic MVP1 set) is not yet finalized.
7. **Similarity-match threshold** — acceptable match percentage for photo/signature comparison (100% vs. 90–95%) needs a decision; MVP1 assumes an inbuilt fixed threshold.
8. **MAS compliance detail** — specifics needed to build the RAG/knowledge base grounding for the three MAS risk principles.
9. **Case-assignment logic** — how applications are assigned to a specific KYC Analyst / Compliance Officer from a pool is not yet defined (MVP1 sidesteps this by assuming 1 RM / 1 Analyst / 1 CO).
10. **Rejection routing** — on Analyst/CO rejection, MVP1 assumes the RM simply opens a new application; whether the same application can be resubmitted needs confirmation.
11. **Sample data / mock templates** — samples for application forms, supporting documents, and adverse-media search results are pending from the business team.
12. **URL sources for adverse-media/BG verification crawling** — needed once MVP1 moves beyond stubs.

# 11. Delivery Plan (MVP1-Relevant Milestones)

*Source: APJ_BFS_BRD001 Plan — "Plan" worksheet, items relevant to MVP1 definition and build.*

| Seq | Activity | Stakeholders | Status | Notes |
|---|---|---|---|---|
| 1 | Scope clarification | Madhu, Abhijeet | WIP | Discussion with Abhijeet/Jeevitha complete; open items on risk rating, sample documents, adverse-media rejection rules. |
| 2 | MVP Definition | Amit, Madhu | Completed | Refer to the MVP Feature List (Section 5 of this document). |
| 3 | Define functional architecture | Amit, Madhu | WIP | Draft pending Amit's review. |
| 4 | Define SDD + NAIE components (app front end, backend, API/stubs, agents, tools, guardrails) | Suganya | Planned | — |
| 5 | Enable dev team with SDD/NAIE demo environment access | TK, Suganya, Amit | WIP | Enabled post-training completion. |
| 6 | Provision project infra for use case development | Suganya, Amit | Planned | Tech stack, infra requirements, access enablement to be planned. |
| 7 | Define project plan, timelines | Suganya | Planned | — |
| 8 | Prepare mock data, templates | Abhijeet, Jeevitha | Pending | Samples for application form, supporting docs, adverse-media results. |
| 9 | Prepare synthetic data using NAIE | NAIE team | Planned | — |
| 10 | Upload BRD for Wealth onboarding platform | SDD team | Planned | — |
| 11 | SDD-led development | SDD team | Planned | — |
| 12 | Define guardrails, ground truth, RAG for agentic components | NAIE team | Planned | — |
| 13 | Define agents | NAIE team | Planned | — |
| 14 | Define stubs (instead of real-time APIs) | SDD team | Planned | — |
| 15 | Publish Agents as APIs | NAIE team | Planned | — |
| 16 | Integrate Wealth Onboarding platform with NAIE components | TBD | Planned | — |
| 17 | Demo of working prototype | — | — | — |

**MVP1 feature build timeline (tentative, per feature list):**

| Week | Activities |
|---|---|
| Week 1 | RM upload capability (Feature #1). |
| Week 2 | Auto-trigger analysis on approval; document detection/classification/error reporting (Features #4, #5). |
| Week 3 | Read text from scanned docs/URLs (stubbed); entity classification begins; mismatch reporting with citations (Features #6, #7, #9). |
| Week 3–4 | Entity classification continues; consolidated mismatch summary report (Features #7, #10). |
| Week 4 | Analyst views application, system assessment, and approves/rejects/justifies observations (Features #12, #13, #16). |
| Week 5 | Analyst actions routed to Compliance Officer; CO approve/reject with mandatory justification; CO actions finalized (Features #18, #19, #20, #22). |

# 12. Glossary

| **Term** | **Definition** |
|---|---|
| BRD | Business Requirements Document. |
| CDD | Customer Due Diligence — baseline KYC. |
| CO | Compliance Officer. |
| EDD | Enhanced Due Diligence — extra checks for higher-risk profiles. |
| FR / NFR | Functional / Non-Functional Requirement. |
| HITL | Human-in-the-Loop — analyst and compliance remain decision-makers. |
| HNW | High Net Worth. |
| KYC | Know Your Customer. |
| Materiality | MAS principle: focus diligence on more material or higher-risk SoW components. |
| MAS | Monetary Authority of Singapore. |
| MLRO | Money Laundering Reporting Officer. |
| MVP | Minimum Viable Product. |
| NAIE | (Platform team) responsible for agentic components, guardrails, and RAG. |
| PEP | Politically Exposed Person. |
| POC | Proof of Concept. |
| Prudence | MAS principle: prefer reliable third-party corroboration over self-attestation. |
| RCA | Relative or Close Associate (of a PEP). |
| Relevance | MAS principle: corroborative evidence must be fit for purpose for the declared SoW source. |
| RFI | Request for Information. |
| RM | Relationship Manager. |
| SDD | (Delivery team) responsible for spec-driven development / build. |
| SoW | Source of Wealth — origin of the customer's overall wealth. |
| SoF | Source of Funds — origin of the specific funds being deposited. |
| SPA | Sale and Purchase Agreement. |
| TAT | Turnaround Time. |
| TIN / CRS | Tax Identification Number / Common Reporting Standard self-certification. |
