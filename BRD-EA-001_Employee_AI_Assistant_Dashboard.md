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

## 3. Target Users

- Claims handlers
- Policy administration staff
- Underwriters / rating staff
- Customer service / contact-centre staff

Out of scope: policyholders/customers — they are covered separately by BRD-CP-001.

## 4. In Scope

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
| FR-01 | The dashboard shall provide a single search bar where an employee can type a product name or task in free text. | MUST | Typing "travel" or "travel insurance" both resolve to the same product. |
| FR-02 | The AI assistant shall identify the relevant insurance product from the employee's query and confirm it back to them. | MUST | Assistant responds "Looks like you need help with Travel insurance" for a travel-related query. |
| FR-03 | The assistant shall present the correct set of actions for the identified product: register new policy, check claim status, view premium rates. | MUST | All three actions appear for each of the five supported product lines. |
| FR-04 | The assistant shall surface the relevant policy document for the identified product. | MUST | Correct policy-wording document is linked for each product. |
| FR-05 | Each suggested action shall deep-link into the existing internal system that handles it, carrying the employee's active session. | MUST | Clicking "check claim status" opens the claims dashboard already filtered/logged in, with no separate login prompt. |
| FR-06 | Suggested actions shall be tailored to the employee's role (e.g. a claims handler sees claim status first; an underwriter sees rates first). | SHOULD | Two test accounts with different roles see a different default action order for the same query. |
| FR-07 | The dashboard shall display a quick-access row of the employee's most common products/tasks. | SHOULD | Row reflects the employee's actual usage pattern after a defined period. |
| FR-08 | The sidebar shall show the employee's current workload stats, frequently used internal apps, and their team lead/escalation contact. | SHOULD | Sidebar renders correctly for a sample account. |
| FR-09 | Every AI suggestion and the action the employee actually took shall be logged for audit. | MUST | Audit log contains a complete, timestamped trace for a sample session. |

## 7. Non-Functional Requirements

| ID | Requirement | Target |
|---|---|---|
| NFR-01 | Assistant response time. | Under 3 seconds median for a first response. |
| NFR-02 | Access control. | Employees only see products/tools relevant to their role; least-privilege on internal data. |
| NFR-03 | Session handling. | Deep-links carry an active, secure session — no separate login per internal tool. |
| NFR-04 | Audit log completeness. | 100% of AI suggestions and employee actions captured. |
| NFR-05 | Availability. | Available during business hours at a minimum; uptime target to be confirmed with IT. |

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

## 10. Open Questions

- Which internal systems (claims, policy admin, rating) already expose a way to deep-link with session carried over, and which would need that built?
- Should this run as its own dashboard, or as a new module inside an existing internal employee portal?
- Which employee roles should be modelled first for the role-based suggestion ordering (FR-06)?
- Does this reuse the same AI/intent layer being proposed for the customer-facing portal (BRD-CP-001), or does it need a separate instance for internal-only data?

## 11. Glossary

| Term | Definition |
|---|---|
| Deep-link | A link that opens a specific screen or filtered view inside another system, rather than just its homepage. |
| Intent | What the employee is actually trying to do, inferred by the AI assistant from their free-text query. |
| Role-based | Content or suggestions that change depending on the employee's job role. |
| System of record | The system that actually owns and processes a given transaction (e.g. the claims system owns claims, not the dashboard). |
