**BUSINESS REQUIREMENTS DOCUMENT**

**BRD-001B**

**Wealth Individual Onboarding — Application Layer**

*AI-augmented, MAS-aligned source-of-wealth establishment · Proof of Concept*

*Split from master BRD-001. This document covers the Application layer only — case submission, case-management UI, human review/override, MLRO view, audit log, and downstream integrations. See BRD-001A for the AI Agent layer (classification, extraction, reasoning, narrative drafting).*

| **Field** | **Detail** |
|---|---|
| Document Code | BRD-001B |
| Initiative | AI Platform · Wealth Onboarding Acceleration — Application Layer |
| Regulatory Anchor | MAS Circular AMLD 08/2024 (26 Jul 2024) — Establishing the Sources of Wealth of Customers |
| Programme Owner | Delivery Partner — Cognizant |
| Author | Delivery / Business Analysis Team |
| Version | 0.1 — Draft for Review |
| Status | Draft |
| Classification | Internal · Confidential |

# 1. Document Control

## 1.1 Version History

| **Version** | **Date** | **Author** | **Summary of Changes** |
|---|---|---|---|
| 0.1 | DD-MMM-YYYY | Delivery / BA | Initial draft — split from master BRD-001 (Application layer) |

## 1.2 Distribution & Approvals

| **Name / Role** | **Function** | **Action** | **Signature / Date** |
|---|---|---|---|
| Sponsor — Head of Private Banking / Wealth | Business | Approve | |
| Head of Wealth KYC / CDD | Operations | Approve | |
| MLRO / Compliance Lead | Risk & Compliance | Approve | |
| Platform Owner | Technology | Approve | |
| Delivery Partner | Programme | Review | |

## 1.3 Purpose of this Document

This document defines the scope, functional requirements, and acceptance criteria for the **Application layer** of the Wealth Individual Onboarding proof of concept — the case submission channel, case-management UI, human review and override workflow, senior-management oversight view, audit logging, and downstream/vendor integrations. It is anchored in MAS Circular AMLD 08/2024, which requires senior-management oversight of higher-risk cases and a defensible audit trail. AI-generated outputs consumed by this layer (extraction, narrative, risk rating) are produced by the companion Agent BRD (BRD-001A); the KYC analyst and compliance officer remain the decision-makers throughout.

# 2. Executive Summary

Wealth onboarding in Singapore is under sustained regulatory and commercial pressure. MAS Circular AMLD 08/2024 raised expectations on senior-management oversight of higher-risk cases and requires a defensible, auditable process for source-of-wealth (SoW) establishment, while cautioning FIs not to impose undue delay on legitimate customers.

This document scopes the **Application layer** that makes the AI Agent's outputs usable, reviewable, and auditable by real people: the intake channel that receives the onboarding pack, the single-screen case-management UI where the analyst and compliance officer review extraction, screening, corroboration, narrative, and rating; the override capture mechanism; the MLRO/senior-management oversight view for higher-risk cases; the append-only audit log; and the integrations that push the final decision into core banking. This layer keeps the human as decision-maker throughout, while ensuring the process is fast, consistent, and defensible under MAS scrutiny.

> **Regulatory Anchor — MAS Circular AMLD 08/2024**
> Issued by the Monetary Authority of Singapore on 26 July 2024, addressed to CEOs of financial institutions in the wealth management sector. Requires FIs to use appropriate and reasonable means to establish and independently corroborate the source of wealth of customers. Senior management must exercise close oversight of higher-risk accounts; ongoing monitoring must reflect the customer's risk profile. Procedures must be risk-proportionate and must not impose undue delay on the onboarding of legitimate customers. Reinforced by the ACIP Best Practices on Source of Wealth Due Diligence (May 2025).

# 3. Business Context

## 3.1 The Current Pain

- **Time.** Sequential information requests and manual case handling drive 10–20 business day onboarding.
- **Customer experience.** HNW customers are repeatedly asked for documents, narratives, and clarifications — the experience at the most sensitive point of the relationship is the opposite of premium.
- **Fragmented review.** Analysts and compliance officers today review screening, documents, and narrative across multiple disconnected tools and emails, with no single audit trail.

## 3.2 The Regulatory Opportunity

MAS Circular AMLD 08/2024 requires senior-management oversight of higher-risk cases and a defensible audit trail. The Application layer operationalizes this: a single review screen with full traceability, a structured override-capture mechanism, and an append-only audit log give compliance and MLRO a documented, defensible process — while a consolidated (not iterative) information-request mechanism protects the customer experience the Circular explicitly cares about.

## 3.3 Business Objectives (Application Layer)

- Reduce wealth onboarding TAT from 10–20 business days to 1–3 days for clean cases, 3–5 days for cases requiring deeper corroboration, by giving analysts a single review screen instead of fragmented tools.
- Eliminate redundant information requests to customers — ask once, ask the right question, with the right justification.
- Provide compliance and audit with a complete, line-level traceable record of every decision and override.
- Give MLRO / senior management a dedicated, read-only oversight view of higher-risk cases per MAS Circular requirements.
- Establish a reusable case-management and audit pattern that downstream use cases (BRD-002 and beyond) inherit.

## 3.4 In Scope

- Intake of the onboarding payload and uploaded documents via a defined API/channel.
- Orchestration of sanctions, PEP, adverse-media, and ID verification stub calls.
- Single-screen case-management UI presenting Agent-produced extraction, screening, corroboration, narrative, and rating.
- Analyst and Compliance Officer accept / edit / override workflow, with rationale capture.
- MLRO / senior-management read-only oversight view for higher-risk cases.
- Append-only audit log covering system events, AI outputs, and human actions.
- Push of account-open request to core banking stub on Compliance sign-off.
- Consolidated RFI issuance to the RM channel.

## 3.5 Out of Scope

- Document classification, field extraction, narrative drafting, and risk-rating logic (see BRD-001A).
- Live production integration with screening vendors, core banking, or regulator portals — POC uses stubs.
- Highly complex structures (multi-layer offshore vehicles, discretionary trusts) — deferred to Phase 2.
- Corporate or institutional onboarding.

## 3.6 Assumptions

- A small set of synthetic / anonymized HNW customer profiles will be made available for the POC.
- Stubbed vendor responses (sanctions, PEP, adverse media, ID verification, core account-open) are acceptable for the POC.
- Analyst and compliance officer remain the decision-makers; Agent output (from BRD-001A) is available as structured input to the UI.

## 3.7 Dependencies

- Document repository for uploaded customer documents (real, lightweight).
- Agent-layer outputs (extraction, screening summary, narrative, rating) per BRD-001A.
- Stubs for sanctions, PEP, adverse-media, ID verification, and core account-open APIs.
- Lightweight case-management UI infrastructure and audit-log store.

# 4. Stakeholders

| **Stakeholder** | **Role in POC** | **Engagement** |
|---|---|---|
| Sponsor — Head of Private Banking | Owns the business case; approves go/no-go for scaling. | Steering / approval |
| Private Banker / Relationship Manager | Originates the relationship; submits the onboarding pack on the customer's behalf; receives consolidated RFI. | Daily during demo cycles |
| Wealth KYC Analyst (Maker) | Reviews AI extraction, screening, SoW narrative, and risk recommendation on the case-management UI. Primary user. | Primary user |
| Wealth KYC Checker / QC | Second-line review; confirms accuracy before compliance sign-off. | Secondary user |
| Compliance Officer / EDD Reviewer | Reviews and signs off the SoW narrative and risk rating via the UI. Authorised approver for HNW. | Primary reviewer |
| MLRO / Senior Management | Uses the oversight view for higher-risk cases; reviews override telemetry, per MAS Circular. | Periodic / end of pilot |
| Platform Owner / IT | Owns UI, audit log, and integration stubs. | Throughout |
| Delivery Partner | Programme management, build, testing, showcase orchestration. | Throughout |

# 5. Process Flow

## 5.1 AS-IS — Today's Wealth Onboarding

1. RM submits the customer pack including identification, address, and a free-form SoW declaration.
2. KYC operations requests further documentation iteratively — business sale agreements, audited accounts, inheritance documents, etc.
3. Analyst manually triggers sanctions, PEP, adverse-media, and ID verification checks. Reviews each individually.
4. Analyst manually searches public sources for corroboration of prominent profile elements.
5. Analyst writes a free-form SoW narrative — quality variable by analyst seniority.
6. Risk rating proposed; QC reviews; Compliance Officer signs off.
7. Account opened in core banking; relationship onboarded.

*Typical end-to-end TAT: 10–20 business days.*

## 5.2 TO-BE — Application-Layer Steps

1. RM submits an initial pack via the channel; Application receives the payload and uploaded documents (**FR-B01**).
2. Application orchestrates sanctions, PEP, adverse-media, and ID verification stubs in parallel and passes documents + stub results to the Agent layer (**FR-B10**).
3. Application receives from the Agent layer: extraction, corroboration findings, narrative, risk rating, and any consolidated information-request content.
4. Where the Agent flags insufficient evidence, Application issues the single, consolidated information request to the RM — not iterative (**FR-B09**).
5. Case-management UI presents extraction, screening, corroboration findings, narrative, and rating on a single review screen (**FR-B13**).
6. KYC Analyst reviews the package; accepts, edits, or overrides. Override rationale captured (**FR-B14**).
7. Compliance Officer signs off the SoW narrative and risk rating via the UI. For higher-risk cases (per Agent flag), MLRO / senior management is notified via the oversight view (**FR-B15**).
8. Application pushes account-open request to core stub; full reasoning trail and decisions written to the audit log (**FR-B16, FR-B17**).

*Target TAT: 1–3 business days for clean cases; 3–5 days where deeper corroboration is required. The KYC analyst and Compliance Officer remain the decision-makers.*

# 6. Risk & Screening Stubs Orchestrated by the Application Layer

| **Document / Information** | **Mandatory (Production)** | **Alternate / Simplification for POC** | **Notes** |
|---|---|---|---|
| Sanctions screening | Yes (mandatory) | Stub returning seeded results | OFAC, UN, EU, MAS list |
| PEP screening | Yes (mandatory) | Stub with prominent-profile cases | Includes RCAs |
| Adverse media screening | Yes (mandatory) | Stub with seeded false positives | Material adverse news |
| ID verification | Yes (mandatory) | Stub PASS / FAIL / MANUAL | Liveness optional for POC |

# 7. Functional Requirements — Application Layer

| **ID** | **Requirement** | **Priority** | **Acceptance Test** |
|---|---|---|---|
| FR-B01 | Application shall accept a wealth onboarding payload (JSON) and uploaded documents via a defined API. | MUST | Sample payload + 5+ documents accepted; case_id returned. |
| FR-B09 | Application shall issue a single, consolidated information request to the RM (not iterative) when the Agent layer flags insufficient evidence. | MUST | Sample case with gaps: one RFI issued, all gaps listed. |
| FR-B10 | Application shall orchestrate sanctions, PEP, adverse-media, and ID verification stubs in parallel. | MUST | All four stub calls visible in audit log per case. |
| FR-B13 | Case-management UI shall present extraction, screening, corroboration findings, narrative, and rating on a single review screen. | MUST | Analyst and Compliance can review without leaving the screen. |
| FR-B14 | Analyst and Compliance shall accept, edit, or override AI output. Override rationale captured. | MUST | Override path tested for both roles. |
| FR-B15 | Application shall surface Agent-flagged higher-risk cases to the MLRO / senior management view. | MUST | Seeded higher-risk case appears in MLRO view. |
| FR-B16 | Application shall write an append-only audit record for every step (system event, AI output, citations, analyst / compliance action, override). | MUST | Audit log contains complete trace for sample cases. |
| FR-B17 | Application shall push an account-open request to the core stub on Compliance sign-off. | MUST | Core stub returns customer_id and account_id. |
| FR-B18 | Application shall capture a customer-experience signal (time-to-first-RFI, document re-request count) for each case. | SHOULD | Metrics visible on a pilot dashboard. |

# 8. Non-Functional Requirements — Application Layer

| **ID** | **Requirement** | **Target for POC** |
|---|---|---|
| NFR-B02 | Concurrent cases supported during showcase. | Up to 5 in parallel. |
| NFR-B04 | Audit log completeness — including override rationale and senior-management notification trail. | 100% of system + analyst + compliance events captured. |
| NFR-B05 | Access control on case-management UI. | Role-based (Analyst, Checker, Compliance, MLRO read-only). |
| NFR-B06 | PII handling. | POC uses anonymised / synthetic data only. |

# 9. Data Requirements — Application Layer

## 9.1 Inputs

| **Input** | **Source** | **Mode in POC** |
|---|---|---|
| Wealth onboarding payload (JSON) | Origination channel (simulated) | Live |
| Document uploads (PDF / image) | Document repository | Live |
| Sanctions / PEP / adverse-media results | Vendor APIs | Stub |
| ID verification result | Vendor | Stub |
| Extraction, narrative, rating, RFI content | Agent layer (BRD-001A) | Live |

## 9.2 Outputs

| **Output** | **Consumer** | **Format** |
|---|---|---|
| Consolidated screening result | Case-management UI | Structured JSON + narrative |
| Consolidated RFI to RM (if needed) | RM / channel | Structured list |
| Higher-risk notification | MLRO / senior management view | Event + reason |
| Account-open request | Core banking stub | Structured JSON |
| Audit record | Audit log store | Append-only JSON |

## 9.3 Sample Data Set for POC

Shared with BRD-001A — 8 synthetic HNW customer profiles (A–H) spanning business sale, ongoing income, inheritance, mixed sources, prominent profile, adverse-media false positive, PEP relative, and uncorroborated component scenarios. Profiles G and H specifically exercise this layer's MLRO-surfacing and RFI-issuance requirements.

# 10. Stub & Integration Plan — Application-Layer Ownership

The POC keeps integration deliberately minimal. Total build effort approximately 25 dev-days (combined with Agent-layer stub consumption in BRD-001A).

| **System** | **Mode** | **Stub Contract / Behaviour** | **Effort** |
|---|---|---|---|
| Sanctions API | Stub | POST /screen — deterministic responses for seeded names. | 1.5 d |
| PEP / Adverse-media | Stub | Returns scored hits with summaries; includes seeded false positives and PEP-relative case. | 1.5 d |
| ID Verification | Stub | POST /verify — returns PASS / FAIL / MANUAL with confidence. | 1.0 d |
| Core Banking — Account Open | Stub | POST /accounts — returns customer_id, account_id. | 1.0 d |
| Document Repository | Live | S3-compatible bucket with metadata table. | 2.0 d |
| Audit Log Store | Live | Append-only JSON / Postgres store. | 2.0 d |
| Case-management UI | Live (minimal) | Single review screen for analyst + compliance; override capture; senior-management view. | 6.0 d |
| MLRO / Senior Management view | Live (minimal) | Read-only view of higher-risk cases with reason and audit trail. | 2.0 d |

# 11. Risks & Mitigations — Application Layer

| **Risk** | **Likelihood** | **Impact** | **Mitigation** |
|---|---|---|---|
| Analyst trust in AI narrative initially low. | Medium | Medium | Line-level citations on every claim (from BRD-001A); override always available. |
| Audit / model risk does not engage early. | Medium | High | Dedicated MLRO walkthrough in Week 5; audit log opened in showcase. |
| Customer experience improvements not visible if RM does not consolidate RFI. | Medium | Medium | RM playbook updated; consolidated RFI demonstrated in showcase. |

# 12. Acceptance Criteria for POC — Application Layer

1. Clean wealth case (Profile A): end-to-end completes in under 5 minutes from upload to account-open call; Compliance Officer accepts after minor edits via the UI.
2. Consolidated RFI: sample case with gaps generates a single, consolidated information request — not iterative.
3. Senior-management oversight: higher-risk case (Profile G) surfaces in MLRO view with reason and audit trail.
4. Override scenario: Compliance Officer override captured with rationale; visible to MLRO.
5. Audit log contains complete trace, including citations, for every showcase case.
6. Sponsor, Head of Wealth KYC, and Compliance lead sign off on continued scaling.

# 13. Indicative Timeline (6–8 weeks) — Application-Layer Milestones

| **Week** | **Phase** | **Key Activities** |
|---|---|---|
| 0 | Mobilisation | Sponsor confirm; environment provisioned. |
| 1 | Build — Foundation | Case-management UI shell; MLRO view shell; document repository live. |
| 2–3 | Build | Stub orchestration for sanctions/PEP/adverse-media/ID verification; RFI issuance mechanism; audit log wiring. |
| 4 | Internal UAT | Walkthrough with wealth KYC analysts + Compliance on the review screen and override flow. |
| 5 | Stakeholder UAT | MLRO walkthrough; audit trail review; senior-management notification path tested. |
| 6 | Showcase | Eight scripted profile scenarios run live; KPIs captured; sponsor sign-off. |
| 7 | Stabilisation (optional) | Fixes, second showcase, scale plan. |

# 14. Open Questions

- Will the POC environment be provisioned within the bank tenancy or in a Cognizant-hosted sandbox?
- Is the MLRO senior-management oversight notification preferred via email, dashboard, or both?
- How will the bank's existing case management system relate to the POC UI — co-exist, replace, or embed?

# 15. Glossary

| **Term** | **Definition** |
|---|---|
| ACIP | AML/CFT Industry Partnership — Singapore industry forum; issued the Best Practices on Source of Wealth Due Diligence (May 2025). |
| AI Platform | The reusable AI orchestration platform under pilot; produces recommendations, not decisions. |
| AMLD 08/2024 | MAS Circular 'Establishing the Sources of Wealth of Customers', 26 July 2024. |
| BRD | Business Requirements Document. |
| CDD | Customer Due Diligence — baseline KYC. |
| EDD | Enhanced Due Diligence — extra checks for higher-risk profiles, including wealth. |
| FR / NFR | Functional / Non-Functional Requirement. |
| HITL | Human-in-the-Loop — analyst and compliance remain decision-makers. |
| HNW | High Net Worth. |
| KYC | Know Your Customer. |
| MAS | Monetary Authority of Singapore. |
| MLRO | Money Laundering Reporting Officer. |
| PEP | Politically Exposed Person. |
| POC | Proof of Concept. |
| RCA | Relative or Close Associate (of a PEP). |
| SoW | Source of Wealth — origin of the customer's overall wealth (cumulative). |
| SoF | Source of Funds — origin of the specific funds being deposited. |
| TAT | Turnaround Time. |
