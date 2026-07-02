# Business Requirements Document

**BRD-EA-001 — Employee AI Assistant Dashboard**
*Internal staff-facing search and task hub, Income Insurance*

| Field | Detail |
|---|---|
| Document Code | BRD-EA-001 |
| Related Document | BRD-CP-001 — Insurance Customer Portal (customer-facing; this document covers internal staff instead) |
| Audience | Internal employees (claims handlers, policy administrators, underwriters, customer service staff) — **not** policyholders |
| Author | Business Analysis Team |
| Version | 0.1 — Draft for Review |
| Status | Draft |

---

## 1. Purpose & Background

Employees currently have to know *which internal system* handles a given task — the claims system for claim status, the policy admin system to register a policy, a separate tool for rates — and navigate to each one individually. This document proposes a single internal dashboard where an employee can search or type what they need in plain language (e.g. "travel insurance," "claim status"), and an AI assistant interprets the request and surfaces the right actions, documents, and links to existing internal tools.

The model for this is an internal employee-portal pattern already common in large enterprises: one search bar and AI assistant at the front, with the assistant routing the employee to the right tool rather than replacing those tools.

## 2. Business Objectives

- Reduce time employees spend figuring out *which* internal system to open for a given task.
- Give employees one place to search across products, tasks, and documents instead of memorising multiple tool locations.
- Reuse existing internal systems (claims, policy admin, rating) rather than rebuilding them — the assistant routes and pre-fills, it doesn't replace them.
- Make the assistant's suggestions specific to the employee's role, not a generic list.

## 3. Target Users & Role Domains

- Claims handlers
- Policy administration staff
- Underwriters / rating staff
- Customer service / support staff
- IT engineers
- Data engineers
- AI agent / platform engineers

Each employee belongs to one primary role/domain. Before the assistant returns any result, the employee declares which of the above they are — this declaration is what scopes every subsequent search, not just the order suggestions appear in (see Section 6).

Out of scope: policyholders/customers — they are covered separately by BRD-CP-001.

## 4. In Scope

- A mandatory role/domain declaration step before the assistant returns any result, used to scope every search that follows (see Section 6).
- A search bar where an employee types a product name, task, or free-text query.
- An AI assistant that identifies the relevant insurance product (e.g. travel, motor, health, home, domestic helper) from the query.
- A set of suggested actions per product: register a new policy, check claim status, view premium rates.
- Surfacing the relevant policy document (e.g. policy wording PDF) for the identified product.
- Deep-links from each suggested action into the existing internal system that already handles it (claims dashboard, policy admin console, rating engine), carrying the employee's session so they don't have to log in again.
- A sidebar showing the employee's own workload stats, frequently used internal apps, and their team lead/escalation contact.
- An audit trail of what the assistant suggested and which action the employee took.

## 5. Out of Scope

- Rebuilding the claims system, policy admin system, or rating engine — this dashboard is a routing and search layer in front of them, not a replacement.
- Customer-facing features (covered in BRD-CP-001).
- Approving or processing any transaction itself — the assistant routes the employee to the system where that already happens.

## 6. Functional Requirements

| ID | Requirement | Priority | Acceptance Test |
|---|---|---|---|
| FR-01 | The employee shall declare their role/domain (support, claims handler, policy administrator, underwriter, IT engineer, data engineer, or AI agent engineer) before the assistant returns any result — at first login, and re-confirmed at the start of each session. | MUST | A new employee cannot search until a role is declared; a returning employee sees their last declared role pre-selected and can confirm or change it before searching. |
| FR-02 | The dashboard shall provide a single search bar where an employee can type a product name or task in free text. | MUST | Typing "travel" or "travel insurance" both resolve to the same product. |
| FR-03 | The AI assistant shall identify the relevant insurance product from the employee's query and confirm it back to them. | MUST | Assistant responds "Looks like you need help with Travel insurance" for a travel-related query. |
| FR-04 | The actions and tools the assistant returns for a given product shall be filtered to only those relevant to the employee's declared role/domain — a restricted list, not a re-ordered version of the same list. | MUST | A support-role employee searching "travel insurance" sees only support tools; a data-role employee searching the same term sees only data tools; neither sees the other's tools (see Section 6.1). |
| FR-05 | The assistant shall surface the relevant policy document for the identified product, where the employee's declared role has a legitimate reason to see it. | MUST | Document surfaced still respects existing document-access rules on top of role filtering. |
| FR-06 | Each surfaced action shall deep-link into the existing internal system that handles it, carrying the employee's active session. | MUST | Clicking a suggested action opens the correct internal tool already logged in, filtered to the relevant product where applicable. |
| FR-07 | An employee shall be able to change their declared role/domain if their responsibilities change. | SHOULD | Role change is possible without contacting IT support, and is itself logged (see Section 9 on verification). |
| FR-08 | The dashboard shall display a quick-access row of the employee's most common products/tasks, scoped to their declared role. | SHOULD | Row only ever shows items valid for the employee's current role. |
| FR-09 | The sidebar shall show the employee's current workload stats, frequently used internal apps for their role, and their team lead/escalation contact. | SHOULD | Sidebar renders correctly and only shows role-appropriate apps for a sample account. |
| FR-10 | Every AI suggestion, the employee's declared role at the time, and the action they took shall be logged for audit. | MUST | Audit log entry includes the declared role alongside the query and outcome for a sample session. |

### 6.1 Role Domains — Example Tools Surfaced for "Travel Insurance"

Worked example of how the same search returns a different, role-scoped result:

| Role/domain | What the assistant surfaces |
|---|---|
| Support | Customer ticket/CRM tool, FAQ and response scripts, escalation contact, read-only claim-status lookup |
| Claims handler | Claims dashboard (travel), document intake queue, claim status update tool |
| Policy administrator | Policy admin console (travel), policy registration/amendment tool |
| Underwriter / rating | Rating engine (travel), premium calculation tool, risk guideline reference |
| Data engineer | Travel insurance data pipeline/reporting tool, data schema/dictionary reference |
| IT engineer | System health/monitoring for travel insurance services, incident and configuration console |
| AI agent engineer | AI agent configuration console, model/prompt logs for the travel insurance assistant, Neuro® AI governance panel |

## 7. Non-Functional Requirements

| ID | Requirement | Target |
|---|---|---|
| NFR-01 | Assistant response time. | Under 3 seconds median for a first response. |
| NFR-02 | Access control. | Employees only see products/tools relevant to their role; least-privilege on internal data. |
| NFR-03 | Session handling. | Deep-links carry an active, secure session — no separate login per internal tool. |
| NFR-04 | Audit log completeness. | 100% of AI suggestions and employee actions captured. |
| NFR-05 | Availability. | Available during business hours at a minimum; uptime target to be confirmed with IT. |
| NFR-06 | Role declaration integrity. | The list of roles offered to an employee is limited to what's consistent with their HR/Active Directory record, where that data is available; unusual role declarations are flagged for review. |

## 8. Integration Notes

| System | Role | Notes |
|---|---|---|
| Claims system | Existing | Assistant deep-links here for "check claim status"; not rebuilt. |
| Policy administration system | Existing | Assistant deep-links here for "register new policy." |
| Rating engine | Existing | Assistant deep-links here for "view premium rates." |
| Document/knowledge repository | Existing or new | Source of the policy-wording documents the assistant surfaces. |
| AI/intent layer | New | Interprets the employee's query and maps it to a product and role-appropriate action set (e.g. via Cognizant Neuro® AI Decisioning, consistent with BRD-CP-001 Section 6). |

## 9. Risks & Mitigations

| Risk | Mitigation |
|---|---|
| Assistant misidentifies the product from an ambiguous query. | Assistant asks a clarifying question rather than guessing when confidence is low. |
| Deep-links break if an internal system changes its URL structure or login flow. | Maintain a central routing/config layer for internal links rather than hardcoding them per feature. |
| Employees over-trust the assistant's suggestions and skip checking the underlying system. | Assistant routes to the system of record for the actual transaction — it never completes a transaction itself. |
| Role-based suggestions are wrong for an employee with a mixed or unusual role. | Manual override — employee can always browse the full action list, not just the suggested order. |
| An employee could self-declare a broader role than their actual job, to see tools beyond what they're meant to access. | Tie the selectable role list to HR/Active Directory group membership rather than open free-text self-declaration; log every role declaration and change for periodic review. |

## 10. Open Questions

- Which internal systems (claims, policy admin, rating) already expose a way to deep-link with session carried over, and which would need that built?
- Should this run as its own dashboard, or as a new module inside an existing internal employee portal?
- Which employee roles should be modelled first for role-based filtering (FR-04)?
- Is the declared role pulled automatically from HR/Active Directory/IT role records, or is it self-declared by the employee? If self-declared, what stops someone from picking a broader role than their actual job needs?
- Can an employee hold more than one role/domain at once (e.g. a hybrid support-and-data role), and if so, how are the two roles' results combined or switched between?
- Does this reuse the same AI/intent layer being proposed for the customer-facing portal (BRD-CP-001), or does it need a separate instance for internal-only data?

## 11. Glossary

| Term | Definition |
|---|---|
| Deep-link | A link that opens a specific screen or filtered view inside another system, rather than just its homepage. |
| Intent | What the employee is actually trying to do, inferred by the AI assistant from their free-text query. |
| Role/domain | The functional category an employee declares before searching (e.g. support, data engineer, IT engineer, AI agent engineer) — used to restrict which tools and suggestions the assistant returns, not just their order. |
| Role-based | Content or suggestions that change depending on the employee's job role. |
| System of record | The system that actually owns and processes a given transaction (e.g. the claims system owns claims, not the dashboard). |
