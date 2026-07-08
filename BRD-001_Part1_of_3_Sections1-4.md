**BUSINESS REQUIREMENTS DOCUMENT**

**BRD-001**

**Wealth Individual Onboarding**

*AI-augmented, MAS-aligned source-of-wealth establishment · Proof of
Concept*

  -----------------------------------------------------------------------
  **Field**             **Detail**
  --------------------- -------------------------------------------------
  Document Code         BRD-001

  Initiative            AI Platform · Wealth Onboarding Acceleration

  Regulatory Anchor     MAS Circular AMLD 08/2024 (26 Jul 2024) ---
                        Establishing the Sources of Wealth of Customers

  Programme Owner       Delivery Partner --- Cognizant

  Author                Delivery / Business Analysis Team

  Version               0.1 --- Draft for Review

  Status                Draft

  Classification        Internal · Confidential
  -----------------------------------------------------------------------

# 1. Document Control

## 1.1 Version History

  ----------------------------------------------------------------------------
  **Version**   **Date**      **Author**           **Summary of Changes**
  ------------- ------------- -------------------- ---------------------------
  0.1           DD-MMM-YYYY   Delivery / BA        Initial draft for review

  ----------------------------------------------------------------------------

## 1.2 Distribution & Approvals

  --------------------------------------------------------------------------
  **Name / Role**       **Function**   **Action**   **Signature / Date**
  --------------------- -------------- ------------ ------------------------
  Sponsor --- Head of   Business       Approve      
  Private Banking /                                 
  Wealth                                            

  Head of Wealth KYC /  Operations     Approve      
  CDD                                               

  MLRO / Compliance     Risk &         Approve      
  Lead                  Compliance                  

  Platform Owner        Technology     Approve      

  Delivery Partner      Programme      Review       
  --------------------------------------------------------------------------

## 1.3 Purpose of this Document

This Business Requirements Document defines the scope, business needs,
functional requirements, and acceptance criteria for the AI-augmented
Wealth Individual Onboarding proof of concept. The pilot is anchored in
MAS Circular AMLD 08/2024 (Establishing the Sources of Wealth of
Customers, 26 July 2024) and demonstrates how the AI Platform can
deliver risk-proportionate source-of-wealth (SoW) establishment while
materially reducing onboarding turnaround time and improving customer
experience. The KYC analyst and compliance officer remain the
decision-makers throughout.

# 2. Executive Summary

Wealth onboarding in Singapore is under sustained regulatory and
commercial pressure. MAS Circular AMLD 08/2024 raised expectations on
how financial institutions establish the source of wealth of
high-net-worth (HNW) customers --- requiring corroboration against
documentary evidence or reliable public sources, risk-proportionate
procedures, and senior-management oversight on higher-risk cases. At the
same time, MAS explicitly cautions FIs to design these procedures so
they do not impose undue delay on the onboarding of legitimate
customers.

Today, wealth onboarding typically takes 10 to 20 business days ---
driven by manual document handling, repeated information requests, and
free-form narrative writing that varies analyst to analyst. The pain is
felt most acutely at the moment of highest commercial value: when a
private bank is acquiring a relationship it has competed hard for.

This POC demonstrates that the AI Platform can compress this timeline to
1 to 3 business days for clean cases (and 3 to 5 days for cases with
corroboration challenges) without compromising the MAS-aligned depth of
due diligence. The Platform automates document classification and
extraction, orchestrates screening and corroboration (including
public-source corroboration where appropriate), drafts the SoW narrative
with line-level citations, and presents the case to the analyst on a
single review screen. The decision remains human; the work of getting
there is dramatically faster.

+-----------------------------------------------------------------------+
| **Regulatory Anchor --- MAS Circular AMLD 08/2024**                   |
|                                                                       |
| Issued by the Monetary Authority of Singapore on 26 July 2024,        |
| addressed to CEOs of financial institutions in the wealth management  |
| sector.                                                               |
|                                                                       |
| Requires FIs to use appropriate and reasonable means to establish and |
| independently corroborate the source of wealth of customers, using    |
| documentary evidence or reliable public information.                  |
|                                                                       |
| Risk principles: Materiality (focus on more material or higher-risk   |
| SoW), Prudence (use reliable third-party corroboration), Relevance    |
| (fit-for-purpose evidence).                                           |
|                                                                       |
| Procedures must be risk-proportionate --- no one-size-fits-all ---    |
| and must not impose undue delay on the onboarding of legitimate       |
| customers.                                                            |
|                                                                       |
| Senior management must exercise close oversight of higher-risk        |
| accounts; ongoing monitoring must reflect the customer\'s risk        |
| profile.                                                              |
|                                                                       |
| Reinforced by the ACIP Best Practices on Source of Wealth Due         |
| Diligence (May 2025).                                                 |
+=======================================================================+
+-----------------------------------------------------------------------+

# 3. Business Context

## 3.1 The Current Pain

Three intertwined issues are driving the case for change:

-   Time. Wealth onboarding routinely takes 10 to 20 business days. For
    cases involving complex SoW corroboration, it can extend further.
    Competing private banks with faster onboarding win the relationship.

-   Customer experience. HNW customers are repeatedly asked for
    documents, narratives, and clarifications. The experience at the
    most sensitive point of the relationship is the opposite of premium.

-   Consistency. Free-form narratives vary substantially across analysts
    and shifts. Audit findings increasingly cite inconsistency in how
    SoW is established and documented across similar profiles.

## 3.2 The Regulatory Opportunity

The MAS Circular AMLD 08/2024 explicitly invites FIs to design
risk-proportionate procedures that minimize undue delay for legitimate
customers. The Circular permits corroboration against reliable public
information --- particularly for customers with prominent public
profiles --- and emphasizes focusing diligence effort on the more
material or higher-risk components of a customer\'s wealth.

The AI Platform is uniquely placed to operationalize this: it can read
documents at scale, search reliable public sources, focus the analyst\'s
attention on material SoW components, and produce a defensible,
fully-cited narrative ready for compliance review. In other words, the
Platform turns the Circular\'s risk-proportionate principle into an
executable workflow.

## 3.3 Business Objectives

-   Reduce wealth onboarding TAT from 10--20 business days to 1--3 days
    for clean cases, 3--5 days for cases requiring deeper corroboration.

-   Eliminate redundant information requests to customers --- ask once,
    ask the right question, with the right justification.

-   Demonstrate MAS-aligned, risk-proportionate SoW establishment with
    line-level citations, ready for compliance and audit review.

-   Improve consistency of SoW narratives across analysts, shifts, and
    case complexity.

-   Establish a reusable AI Platform pattern that downstream use cases
    (BRD-002 and beyond) inherit.

## 3.4 In Scope

-   Onboarding of a new individual HNW customer into the private banking
    / wealth segment.

-   Single primary jurisdiction of residence; one or two source
    jurisdictions for wealth.

-   Source-of-wealth establishment for the customer (and a single
    beneficial owner if a simple wrapper, such as an investment company,
    is involved).

-   Public-source corroboration for prominent profiles, where applicable
    and reliable.

-   Drafting of a MAS-aligned SoW narrative with line-level citations.

-   Screening: sanctions, PEP, adverse media --- via stubbed vendor APIs
    for POC.

## 3.5 Out of Scope

-   Highly complex structures --- multi-layer offshore vehicles,
    discretionary trusts with multiple settlers and beneficiaries
    (deferred to a Phase 2 BRD).

-   Corporate or institutional onboarding (covered by separate BRDs).

-   Live production integration with screening vendors, core banking, or
    regulator portals --- POC uses stubs.

-   Ongoing transaction monitoring (touched on for completeness; full TM
    is a separate use case).

## 3.6 Assumptions

-   A small set of synthetic / anonymized HNW customer profiles will be
    made available for the POC.

-   Anonymized excerpts of the bank\'s internal SoW and EDD policy can
    be shared with the Platform team for grounding.

-   Stubbed vendor responses are acceptable for the POC.

-   Analyst and compliance officer remain the decision-makers.

## 3.7 Dependencies

-   Document repository for uploaded customer documents (real,
    lightweight).

-   Anonymized internal SoW / EDD policy library (real).

-   Lightweight case-management UI to capture analyst review and
    overrides (real).

-   Stubs for sanctions, PEP, adverse-media, ID verification, and core
    account-open APIs.

-   Curated set of reliable public information sources for corroboration
    demonstration (e.g. business registries, regulatory filings,
    established news archives --- anonymised or sandbox).

# 4. Stakeholders

Wealth onboarding involves more roles than retail. Each stakeholder is
cast to a real individual from the delivery team for showcase
walkthroughs.

  -------------------------------------------------------------------------
  **Stakeholder**      **Role in POC**                     **Engagement**
  -------------------- ----------------------------------- ----------------
  Sponsor --- Head of  Owns the business case; approves    Steering /
  Private Banking      go/no-go for scaling.               approval

  Private Banker /     Originates the relationship;        Daily during
  Relationship Manager submits the onboarding pack on the  demo cycles
                       customer\'s behalf.                 

  Wealth KYC Analyst   Reviews AI extraction, screening,   Primary user
  (Maker)              SoW narrative, and risk             
                       recommendation. Primary user.       

  Wealth KYC Checker / Second-line review; confirms        Secondary user
  QC                   accuracy before compliance          
                       sign-off.                           

  Compliance Officer / Reviews and signs off the SoW       Primary reviewer
  EDD Reviewer         narrative and risk rating.          
                       Authorised approver for HNW.        

  MLRO / Senior        Per MAS Circular, exercises close   Periodic / end
  Management           oversight of higher-risk cases.     of pilot
                       Reviews override telemetry.         

  Platform Owner / IT  Owns Platform integrations,         Throughout
                       guard-rails, and prompt versioning. 

  Delivery Partner     Programme management, build,        Throughout
                       testing, showcase orchestration.    
  -------------------------------------------------------------------------

