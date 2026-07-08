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

