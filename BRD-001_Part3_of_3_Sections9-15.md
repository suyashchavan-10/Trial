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
