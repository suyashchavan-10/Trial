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

# 5. Process Flow

## 5.1 AS-IS --- Today\'s Wealth Onboarding

1.  RM submits the customer pack including identification, address, and
    a free-form SoW declaration.

2.  KYC operations requests further documentation iteratively ---
    business sale agreements, audited accounts, inheritance documents,
    etc.

3.  Analyst manually triggers sanctions, PEP, adverse-media, and ID
    verification checks. Reviews each individually.

4.  Analyst manually searches public sources for corroboration of
    prominent profile elements (LinkedIn, registries, press).

5.  Analyst writes a free-form SoW narrative --- quality variable by
    analyst seniority.

6.  Risk rating proposed; QC reviews; Compliance Officer signs off.

7.  Account opened in core banking; relationship onboarded.

*Typical end-to-end TAT: 10--20 business days. Drivers: sequential
information requests, manual narrative writing, manual public-source
corroboration, analyst-to-analyst inconsistency.*

## 5.2 TO-BE --- With the AI Platform

8.  RM submits an initial pack via the channel; Platform receives
    application and uploaded documents.

9.  Platform classifies documents (ID, address, sale agreement, audited
    accounts, inheritance documents, etc.) and extracts structured
    fields.

10. Platform performs cross-document consistency checks (name, address,
    DOB, SoW declarations vs supporting documents).

11. Platform identifies missing or insufficient evidence for the
    declared SoW and issues a single, consolidated information request
    to the RM --- not iterative back-and-forth.

12. Platform orchestrates sanctions, PEP, adverse-media, and ID
    verification stubs in parallel.

13. Where the customer is a prominent public profile, Platform performs
    targeted public-source corroboration (per MAS Circular para. 3) and
    presents findings with citations.

14. Platform applies the three MAS risk principles --- Materiality,
    Prudence, Relevance --- to focus diligence on more material or
    higher-risk SoW components, and to assess corroboration sufficiency.

15. Platform drafts a MAS-aligned SoW narrative with line-level
    citations to source documents, vendor results, and corroboration
    findings.

16. Platform proposes a risk rating with reasoning trail anchored to the
    policy library.

17. KYC Analyst reviews the package on a single screen; accepts, edits,
    or overrides. Override rationale captured.

18. Compliance Officer signs off the SoW narrative and risk rating. For
    higher-risk cases, MLRO / senior management is notified for
    oversight (per MAS Circular).

19. Platform pushes account-open request to core stub; full reasoning
    trail and decisions written to audit log.

*Target TAT: 1--3 business days for clean cases; 3--5 days where deeper
corroboration is required. The KYC analyst and Compliance Officer remain
the decision-makers --- the Platform compresses the path to the
decision.*

# 6. Mandatory Documents and POC Simplifications

The table below lists the mandatory documents and information for wealth
onboarding in production, together with a simpler alternate or
simplification acceptable for the POC. The POC is designed to be
executable with synthetic / anonymized data and a small set of public
corroboration sources.

### 6.1 Customer Identity & Address

  --------------------------------------------------------------------------
  **Document /       **Mandatory      **Alternate or        **Notes**
  Information**      (Production)**   Simplification for    
                                      POC**                 
  ------------------ ---------------- --------------------- ----------------
  Government photo   Yes              Synthetic passport    Field
  ID (passport or                     sample with PDF /     extraction,
  NRIC)                               image                 expiry, MRZ

  Proof of address   Yes              Synthetic utility     Cross-match
  (\< 3 months)                       bill or bank          against
                                      statement             application

  Tax identification Yes              Synthetic CRS form    Jurisdiction
  (TIN / CRS                                                classification
  self-cert)                                                
  --------------------------------------------------------------------------

### 6.2 Source of Wealth Evidence

  --------------------------------------------------------------------------
  **Document /       **Mandatory      **Alternate or        **Notes**
  Information**      (Production)**   Simplification for    
                                      POC**                 
  ------------------ ---------------- --------------------- ----------------
  SoW declaration    Yes              Free-text declaration AI Platform
  (customer                           in application        parses and
  statement)                                                structures

  Primary            Yes (if SoW =    Synthetic SPA excerpt Material SoW
  corroboration:     business sale)   with parties and      component
  business sale                       consideration         
  agreement                                                 

  Primary            Yes (if SoW =    Synthetic 2-year      Material SoW
  corroboration:     ongoing business audited accounts      component
  audited accounts   income)                                

  Primary            Yes (if SoW =    Synthetic probate /   Material SoW
  corroboration:     inheritance)     grant of              component
  inheritance                         representation        
  documents                                                 

  Secondary          Yes (where       Optional for POC      Prudence
  corroboration: tax reasonably       sample                principle (MAS)
  returns            available)                             

  Public-source      Optional (per    Demonstrated for 1    Reliable public
  corroboration      MAS Circular)    prominent-profile     information
                                      case                  
  --------------------------------------------------------------------------

### 6.3 Risk & Screening

  -------------------------------------------------------------------------
  **Document /        **Mandatory      **Alternate or       **Notes**
  Information**       (Production)**   Simplification for   
                                       POC**                
  ------------------- ---------------- -------------------- ---------------
  Sanctions screening Yes (mandatory)  Stub returning       OFAC, UN, EU,
                                       seeded results       MAS list

  PEP screening       Yes (mandatory)  Stub with            Includes RCAs
                                       prominent-profile    
                                       cases                

  Adverse media       Yes (mandatory)  Stub with seeded     Material
  screening                            false positives      adverse news

  ID verification     Yes (mandatory)  Stub PASS / FAIL /   Liveness
                                       MANUAL               optional for
                                                            POC

  Country / industry  Yes              Real (anonymized)    Lookup +
  risk register                        policy data          narrative
  -------------------------------------------------------------------------

+-----------------------------------------------------------------------+
| **MAS Materiality, Prudence, Relevance --- How the Platform Applies   |
| Them**                                                                |
|                                                                       |
| Materiality: Platform identifies the most material SoW components     |
| first (e.g. a business sale that explains 70% of declared wealth).    |
| Diligence depth is calibrated to that component, not spread evenly.   |
|                                                                       |
| Prudence: Platform prefers reliable third-party documents (audited    |
| accounts, official registries, regulatory filings) over               |
| self-attestations. Where only self-attestation is available, the      |
| narrative flags the gap explicitly.                                   |
|                                                                       |
| Relevance: Platform proposes the most fit-for-purpose corroboration   |
| for each declared SoW source --- a sale agreement for a business      |
| sale, probate for an inheritance, audited accounts for ongoing        |
| business income. No generic checklist; relevance to the actual SoW    |
| claim.                                                                |
+=======================================================================+
+-----------------------------------------------------------------------+

# 7. Functional Requirements

Each requirement is uniquely identified, mapped to priority (MUST,
SHOULD, COULD), and the acceptance test that proves it for the POC.

  -----------------------------------------------------------------------------------
  **ID**   **Requirement**                     **Priority**   **Acceptance Test**
  -------- ----------------------------------- -------------- -----------------------
  FR-01    Platform shall accept a wealth      MUST           Sample payload + 5+
           onboarding payload (JSON) and                      documents accepted;
           uploaded documents via a defined                   case_id returned.
           API.                                               

  FR-02    Platform shall classify uploaded    MUST           15 sample documents
           documents into wealth-specific                     correctly classified.
           categories (ID, address, tax, SPA,                 
           audited accounts, probate, etc.)                   
           with confidence ≥ 90%.                             

  FR-03    Platform shall extract structured   MUST           Field accuracy ≥ 95%
           fields from each document type                     across the sample set.
           (parties, dates, amounts,                          
           signatures, jurisdictions).                        

  FR-04    Platform shall identify the         MUST           All declared SoW
           customer\'s declared SoW components                components are mapped
           and map each to its supporting                     or flagged as
           evidence in the uploaded pack.                     unsupported.

  FR-05    Platform shall apply the MAS        MUST           Top SoW component
           Materiality principle --- rank SoW                 identified; diligence
           components by materiality and focus                narrative leads with
           diligence on the most material.                    it.

  FR-06    Platform shall apply the MAS        MUST           Self-attestation-only
           Prudence principle --- prefer                      SoW components are
           independent third-party                            explicitly flagged in
           corroboration; flag where only                     narrative.
           self-attestation exists.                           

  FR-07    Platform shall apply the MAS        MUST           Correct evidence type
           Relevance principle --- propose                    proposed for SPA,
           fit-for-purpose corroboration for                  audited accounts,
           each declared SoW source.                          inheritance scenarios.

  FR-08    Platform shall perform              SHOULD         Demonstrated for one
           public-source corroboration for                    prominent-profile case
           customers with prominent public                    in the showcase.
           profiles, using a curated set of                   
           sources.                                           

  FR-09    Platform shall issue a single,      MUST           Sample case with gaps:
           consolidated information request to                one RFI issued, all
           the RM (not iterative) when                        gaps listed.
           evidence is insufficient.                          

  FR-10    Platform shall orchestrate          MUST           All four stub calls
           sanctions, PEP, adverse-media, and                 visible in audit log
           ID verification stubs in parallel.                 per case.

  FR-11    Platform shall draft a MAS-aligned  MUST           Narrative present for
           SoW narrative with line-level                      each case; every line
           citations to source documents,                     traceable to source.
           vendor outputs, and public-source                  
           findings.                                          

  FR-12    Platform shall propose a risk       MUST           Rating proposed with at
           rating (LOW / MEDIUM / HIGH) with                  least 3 policy
           reasoning trail anchored to the                    citations per case.
           policy library.                                    

  FR-13    Case-management UI shall present    MUST           Analyst and Compliance
           extraction, screening,                             can review without
           corroboration findings, narrative,                 leaving the screen.
           and rating on a single review                      
           screen.                                            

  FR-14    Analyst and Compliance shall        MUST           Override path tested
           accept, edit, or override AI                       for both roles.
           output. Override rationale                         
           captured.                                          

  FR-15    Platform shall flag cases as        MUST           Seeded higher-risk case
           higher-risk per MAS Circular                       appears in MLRO view.
           criteria (e.g. uncorroborated                      
           material SoW component, PEP,                       
           complex jurisdiction); these                       
           surface to MLRO / senior management                
           view.                                              

  FR-16    Platform shall write an append-only MUST           Audit log contains
           audit record for every step (system                complete trace for
           event, AI output, citations,                       sample cases.
           analyst / compliance action,                       
           override).                                         

  FR-17    Platform shall push an account-open MUST           Core stub returns
           request to the core stub on                        customer_id and
           Compliance sign-off.                               account_id.

  FR-18    Platform shall capture a            SHOULD         Metrics visible on a
           customer-experience signal                         pilot dashboard.
           (time-to-first-RFI, document                       
           re-request count) for each case.                   
  -----------------------------------------------------------------------------------

# 8. Non-Functional Requirements

  --------------------------------------------------------------------------
  **ID**   **Requirement**                          **Target for POC**
  -------- ---------------------------------------- ------------------------
  NFR-01   Time from full document pack received to Under 5 minutes per
           AI output ready for analyst review.      case.

  NFR-02   Concurrent cases supported during        Up to 5 in parallel.
           showcase.                                

  NFR-03   Citation coverage on SoW narrative and   100% --- every line
           risk reasoning.                          traceable.

  NFR-04   Audit log completeness --- including     100% of system +
           override rationale and senior-management analyst + compliance
           notification trail.                      events captured.

  NFR-05   Access control on case-management UI.    Role-based (Analyst,
                                                    Checker, Compliance,
                                                    MLRO read-only).

  NFR-06   PII handling.                            POC uses anonymised /
                                                    synthetic data only.

  NFR-07   Logging of model usage.                  Model, prompt version,
                                                    and timestamp logged per
                                                    call.

  NFR-08   Resilience to source-document quality    Sample includes scans,
           variation.                               mobile photos,
                                                    multi-page PDFs.
  --------------------------------------------------------------------------

# 9. Data Requirements

## 9.1 Inputs

  ------------------------------------------------------------------------
  **Input**                **Source**                 **Mode in POC**
  ------------------------ -------------------------- --------------------
  Wealth onboarding        Origination channel        Live
  payload (JSON)           (simulated)                

  Document uploads (PDF /  Document repository        Live
  image)                                              

  Sanctions / PEP /        Vendor APIs                Stub
  adverse-media                                       

  ID verification          Vendor                     Stub

  Internal SoW / EDD       Policy library             Live (anonymised)
  policy library                                      

  Country / industry risk  Policy library             Live
  register                                            

  Public-source            Curated for POC            Live (sandbox)
  corroboration set        (registries, regulatory    
                           filings, established news) 
  ------------------------------------------------------------------------

## 9.2 Outputs

  -----------------------------------------------------------------------
  **Output**             **Consumer**              **Format**
  ---------------------- ------------------------- ----------------------
  Extracted customer +   Case-management UI        Structured JSON
  SoW data                                         

  Consolidated screening Case-management UI        Structured JSON +
  result                                           narrative

  SoW corroboration      Compliance Officer        Text + citations
  findings                                         

  SoW narrative          Compliance Officer        Text + line-level
  (MAS-aligned)                                    citations

  Risk rating +          Case-management UI        Enum + reasoning trail
  reasoning trail                                  

  Consolidated RFI to RM RM / channel              Structured list
  (if needed)                                      

  Higher-risk            MLRO / senior management  Event + reason
  notification           view                      

  Account-open request   Core banking stub         Structured JSON

  Audit record           Audit log store           Append-only JSON
  -----------------------------------------------------------------------

## 9.3 Sample Data Set for POC

-   8 synthetic HNW customer profiles spanning different SoW patterns.

-   [Profile A: Business sale (clear primary SoW; SPA + tax
    filings]{.mark} [available).]{.mark}

-   Profile B: Ongoing business income (audited accounts for 2 years).

-   Profile C: Inheritance (probate document + a partial gap in older
    records).

-   Profile D: Mixed (part business sale, part long-term investment
    income --- tests materiality principle).

-   Profile E: Prominent public profile (tests public-source
    corroboration).

-   Profile F: Adverse-media false positive (tests disambiguation).

-   Profile G: PEP relative (tests senior-management oversight trigger).

-   Profile H: Uncorroborated material component (tests higher-risk
    flagging + RFI).

# 10. Stub & Integration Plan

The POC keeps integration deliberately minimal. Anything that grounds AI
reasoning is built live; outbound calls to vendors and downstream
systems are stubbed. Total build effort approximately 25 dev-days.

  -------------------------------------------------------------------------------
  **System**        **Mode**    **Stub Contract / Behaviour**        **Effort**
  ----------------- ----------- ------------------------------------ ------------
  Sanctions API     Stub        POST /screen --- deterministic       1.5 d
                                responses for seeded names.          

  PEP /             Stub        Returns scored hits with summaries;  1.5 d
  Adverse-media                 includes seeded false positives and  
                                PEP-relative case.                   

  ID Verification   Stub        POST /verify --- returns PASS / FAIL 1.0 d
                                / MANUAL with confidence.            

  Core Banking ---  Stub        POST /accounts --- returns           1.0 d
  Account Open                  customer_id, account_id.             

  Document          Live        S3-compatible bucket with metadata   2.0 d
  Repository                    table.                               

  SoW / EDD Policy  Live        Anonymised excerpts; retrievable via 3.0 d
  Library                       internal API.                        

  Public-source     Live        Small set of sample registry,        2.5 d
  corroboration set (curated)   filing, and news entries for         
                                prominent-profile demo.              

  Audit Log Store   Live        Append-only JSON / Postgres store.   2.0 d

  Case-management   Live        Single review screen for analyst +   6.0 d
  UI                (minimal)   compliance; override capture;        
                                senior-management view.              

  MLRO / Senior     Live        Read-only view of higher-risk cases  2.0 d
  Management view   (minimal)   with reason and audit trail.         
  -------------------------------------------------------------------------------

# 11. Risks & Mitigations

  ---------------------------------------------------------------------------------
  **Risk**                  **Likelihood**   **Impact**   **Mitigation**
  ------------------------- ---------------- ------------ -------------------------
  MAS-aligned narrative     High             Medium       Compliance review of
  format differs from                                     format in Week 4 UAT;
  current internal                                        adjust template within
  template.                                               POC.

  Policy excerpts arrive    Medium           High         Lock policy access in
  late, delaying grounding                                Week 0; placeholder
  work.                                                   content acceptable for
                                                          early build.

  Sample documents not      Medium           Medium       Sample deliberately mixes
  representative of                                       scans, mobile photos,
  production quality.                                     multilingual.

  Public-source             High             Low          Acceptable --- only one
  corroboration set is                                    prominent-profile case
  limited for POC.                                        demonstrated; production
                                                          needs broader set.

  Analyst trust in AI       Medium           Medium       Line-level citations on
  narrative initially low.                                every claim; override
                                                          always available.

  Audit / model risk does   Medium           High         Dedicated MLRO
  not engage early.                                       walkthrough in Week 5;
                                                          audit log opened in
                                                          showcase.

  Customer experience       Medium           Medium       RM playbook updated;
  improvements not visible                                consolidated RFI
  if RM does not                                          demonstrated in showcase.
  consolidate RFI.                                        
  ---------------------------------------------------------------------------------

# 12. Acceptance Criteria for POC

The POC is considered successful when all of the following are
demonstrated in the showcase environment using the agreed sample data
set.

1.  Clean wealth case (Profile A): end-to-end completes in under 5
    minutes from upload to account-open call. SoW narrative draft
    present with full citations; Compliance Officer accepts after minor
    edits.

2.  Materiality test (Profile D): Platform leads narrative with the most
    material SoW component, not in document-upload order.

3.  Prudence test (Profile H): Uncorroborated material SoW component is
    explicitly flagged in narrative and triggers higher-risk handling.

4.  Relevance test (Profiles A / B / C): Platform proposes
    fit-for-purpose evidence for each SoW type --- SPA for sale, audited
    accounts for business income, probate for inheritance.

5.  Public-source corroboration (Profile E): Platform demonstrates
    targeted public-source check with citations.

6.  Consolidated RFI: Sample case with gaps generates a single,
    consolidated information request --- not iterative.

7.  Senior-management oversight: Higher-risk case (Profile G) surfaces
    in MLRO view with reason and audit trail.

8.  Override scenario: Compliance Officer override captured with
    rationale; visible to MLRO.

9.  Audit log contains complete trace, including citations, for every
    showcase case.

10. Sponsor, Head of Wealth KYC, and Compliance lead sign off on
    continued scaling.

# 13. Indicative Timeline (6--8 weeks)

  ----------------------------------------------------------------------------
  **Week**   **Phase**          **Key Activities**
  ---------- ------------------ ----------------------------------------------
  0          Mobilisation       Sponsor confirm; sample HNW profiles agreed;
                                SoW / EDD policy excerpts shared;
                                public-source corroboration set curated;
                                environment provisioned.

  1          Build ---          Stubs scaffolded; document repository live;
             Foundation         case-management UI shell; MLRO view shell.

  2          Build ---          Document classification + extraction for
             Extraction &       wealth document types; SoW component
             Materiality        identification; materiality ranking.

  3          Build ---          Screening orchestration; public-source
             Corroboration &    corroboration; SoW narrative drafting with
             Narrative          citations; higher-risk flagging.

  4          Internal UAT       Walkthrough with wealth KYC analysts +
                                Compliance; tune narrative format; refine MAS
                                principle application.

  5          Stakeholder UAT    MLRO walkthrough; audit trail review;
                                senior-management notification path tested.

  6          Showcase           Eight scripted profile scenarios run live;
                                KPIs captured; sponsor sign-off.

  7          Stabilisation      Fixes, second showcase, scale plan.
             (optional)         
  ----------------------------------------------------------------------------

# 14. Open Questions

-   Will the POC environment be provisioned within the bank tenancy or
    in a Cognizant-hosted sandbox?

-   What is the current MAS-aligned SoW narrative template, and are
    there variations across booking centres?

-   Which public sources is the bank already comfortable using for
    corroboration of prominent profiles?

-   Is the MLRO senior-management oversight notification preferred via
    email, dashboard, or both?

-   How will the bank\'s existing case management system relate to the
    POC UI --- co-exist, replace, or embed?

# 15. Glossary

  -----------------------------------------------------------------------
  **Term**         **Definition**
  ---------------- ------------------------------------------------------
  ACIP             AML/CFT Industry Partnership --- Singapore industry
                   forum; issued the Best Practices on Source of Wealth
                   Due Diligence (May 2025).

  AI Platform      The reusable AI orchestration platform under pilot;
                   produces recommendations, not decisions.

  AMLD 08/2024     MAS Circular \'Establishing the Sources of Wealth of
                   Customers\', 26 July 2024.

  BRD              Business Requirements Document.

  CDD              Customer Due Diligence --- baseline KYC.

  EDD              Enhanced Due Diligence --- extra checks for
                   higher-risk profiles, including wealth.

  FR / NFR         Functional / Non-Functional Requirement.

  HITL             Human-in-the-Loop --- analyst and compliance remain
                   decision-makers.

  HNW              High Net Worth.

  KYC              Know Your Customer.

  Materiality      MAS principle: focus diligence on more material or
                   higher-risk SoW components.

  MAS              Monetary Authority of Singapore.

  MLRO             Money Laundering Reporting Officer.

  PEP              Politically Exposed Person.

  POC              Proof of Concept.

  Prudence         MAS principle: use reliable third-party corroboration;
                   benchmarks and assumptions must be reasonable and
                   documented.

  RCA              Relative or Close Associate (of a PEP).

  Relevance        MAS principle: corroborative evidence must be fit for
                   purpose for the declared SoW source.

  SoW              Source of Wealth --- origin of the customer\'s overall
                   wealth (cumulative).

  SoF              Source of Funds --- origin of the specific funds being
                   deposited.

  TAT              Turnaround Time.
  -----------------------------------------------------------------------
