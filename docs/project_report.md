# Academic Project Report

# STRIDE-Based Threat Modelling of an AI Campus System

**Course:** Data Security and Privacy (Academic Micro-Project)  
**Program:** Bachelor of Engineering in Artificial Intelligence and Machine Learning (BE AIML)  
**Academic Year:** 2026–2027  
**Document Status:** Final Official Submission  

---

## Abstract

Modern higher-education institutions are rapidly adopting artificial intelligence (AI) microservices to deliver conversational tutoring, automated academic advising, student performance forecasting, and personalized elective recommendations. However, the architectural integration of large language models (LLMs) and automated analytics expands the institutional cyber-attack surface, introducing novel vulnerability classes such as prompt injection, training-corpus data leakage, retrieval-augmented generation (RAG) context leakage, and API-driven privilege escalation alongside traditional web security threats. 

This academic micro-project presents a comprehensive architectural threat model of a synthetic, AI-enabled university campus information system. Utilizing a Level-1 Data Flow Diagram (DFD) decomposition comprising 4 External Entities, 8 Processes, 4 Data Stores, 4 Trust Boundaries, and 22 Logical Data Flows, the system is systematically analyzed through Microsoft's **STRIDE** (Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, Elevation of Privilege) methodology. A total of **50 distinct, realistic threat scenarios** are identified, cataloged, and evaluated using a formal 3×3 qualitative risk scoring matrix ($\text{Risk} = \text{Likelihood} \times \text{Impact}$). 

The evaluation reveals **7 Critical-risk (Score 9)** and **18 High-risk (Score 6)** vulnerabilities, identifying the public API Gateway ingress and the external cloud AI service boundary as the highest-risk structural zones. For each identified threat, concrete architectural security controls—spanning preventive, detective, and corrective mechanisms—are formulated. This project demonstrates that early-stage architectural threat modeling is essential for safeguarding institutional data privacy, academic grading integrity, and AI service resilience prior to physical implementation.

---

## 1. Title

**STRIDE-Based Threat Modelling of an AI Campus System**  
*An Architectural Security and Privacy Evaluation of an AI-Augmented University Information Ecosystem.*

---

## 2. Executive Summary / Abstract

*(See preliminary Abstract above for the comprehensive institutional summary).*

---

## 3. Introduction

Higher education institutions have transitioned from conventional static database systems to highly interconnected, distributed digital environments. With the advent of modern generative artificial intelligence and machine learning pipelines, universities are deploying automated conversational tutors, early-warning dropout predictive models, and intelligent course recommendation engines. These systems operate on diverse data repositories containing highly sensitive personal, biographical, financial, and academic records.

Despite the operational advantages offered by AI integration, the incorporation of cloud-hosted large language models and automated data ingestion pipelines introduces complex security and privacy vulnerabilities. When third-party AI endpoints process student queries over wide-area networks, the institutional perimeter is extended across external cloud environments. Furthermore, client-facing conversational agents are vulnerable to non-traditional input manipulation, such as prompt injection and context poisoning, which can bypass conventional web application firewalls.

To ensure that modern campus platforms adhere to strict data privacy mandates—such as India's Digital Personal Data Protection Act (DPDPA), the European Union's General Data Protection Regulation (GDPR), and the Family Educational Rights and Privacy Act (FERPA)—security must be designed into the foundational software architecture rather than added retroactively. Threat modeling represents the foundational "Shift-Left" discipline in secure software engineering, allowing system architects to discover design-level flaws, define trust perimeters, and prescribe robust cryptographic and architectural controls before writing production code.

---

## 4. Problem Statement

Deploying generative AI assistants, predictive analytics engines, and centralized API gateways in university information ecosystems introduces critical vulnerabilities that cannot be addressed by conventional perimeter firewalls alone:
1. **Unbounded Third-Party Data Egress:** Conversational interfaces frequently transmit unredacted student queries—often containing personally identifiable information (PII), academic records, or personal disclosures—to commercial cloud LLM providers, risking regulatory violations and model training absorption.
2. **Adversarial Semantic Manipulation:** Natural language inputs allow students or external adversaries to execute direct and indirect prompt injection attacks, overriding system instructions and exploiting conversational access to internal data.
3. **Complex Trust Boundary Transitions:** Data travels across disparate trust domains—from untrusted student mobile devices across the public internet, into private microservices subnets, and finally into protected grade databases and external third-party cloud APIs.
4. **Academic Record Tampering:** Centralized databases storing grades, transcripts, and attendance records represent high-value targets for unauthorized manipulation, grade alteration, and repudiation by malicious actors.

Without a rigorous, structured architectural threat model, university engineering teams cannot systematically catalog these failure modes, prioritize limited security resources, or implement provably resilient defenses.

---

## 5. Objective

The official objective of this micro-project is:  
**"Identify threats using STRIDE and recommend controls"**

Specifically, the project aims to:
1. Formulate a comprehensive, realistic, and fully specified synthetic architecture for an AI-enabled higher-education campus information system.
2. Construct a detailed Level-1 Data Flow Diagram (DFD) identifying all external entities, processes, persistent data stores, logical communication flows, and trust boundaries.
3. Execute an exhaustive, element-by-element STRIDE threat analysis across the architecture to identify realistic cybersecurity and privacy threats.
4. Establish a mathematically consistent 3×3 qualitative risk assessment framework ($\text{Risk} = \text{Likelihood} \times \text{Impact}$) to prioritize threats into Low, Medium, High, and Critical tiers.
5. Prescribe actionable, defense-in-depth architectural security controls (Preventive, Detective, and Corrective) mapped to each identified threat.
6. Provide end-to-end traceability linking DFD components, STRIDE categories, risk scores, and recommended controls.

---

## 6. Scope

### 6.1 In-Scope Components and Activities
* **User Ingress:** Student access via mobile/web apps (`E1`, `P1`), faculty portal access (`E2`, `P2`), and administrative management access (`E3`, `P3`).
* **Authentication and Authorization:** Centralized identity provider (`P5`), token issuance, role-based access validation, and credential protection.
* **Core Microservices:** Central API Gateway (`P4`), Campus AI Assistant (`P6`), Student Analytics Service (`P7`), and Course Recommendation Service (`P8`).
* **Persistent Data Repositories:** Student Information Database (`D1`), Academic and Attendance Database (`D2`), AI Logs (`D3`), and Security Audit Logs (`D4`).
* **External Integrations:** Third-party cloud LLM API connectivity (`E4`) across wide-area network boundary `TB4`.
* **Flows and Boundaries:** All 22 logical data flows (`F01`–`F22`) and 4 trust boundaries (`TB1`–`TB4`).
* **Methodological Scope:** STRIDE threat modeling, qualitative risk scoring, and security control specification.

### 6.2 Out-of-Scope Items
* Physical campus security (locks, biometric gates, server room access, CCTV surveillance).
* Facility electrical power, HVAC, and data center physical cooling systems.
* Active, intrusive network penetration testing, vulnerability exploitation, or fuzzing.
* Real student, faculty, or institutional records; no real personal data is collected or processed.
* Production cloud deployment, CI/CD pipeline automation, or Kubernetes cluster orchestration.
* Development of weaponized exploit scripts, malicious binaries, or network packet-crafting tools.
* Physical 802.11 Wi-Fi RF attacks or hardware-level side-channel probing.

---

## 7. Synthetic Campus Architecture

To provide an academically rigorous evaluation environment while eliminating data privacy and institutional liability concerns, the threat model operates exclusively on a **synthetic campus architecture**. 

> **Academic Disclaimer:** This architecture is entirely conceptual and synthetic. It was developed solely for academic instructional purposes within the Data Security and Privacy course. It does **not** reflect, model, or describe the proprietary systems, internal networks, or infrastructure of Dayananda Sagar College of Engineering (DSCE) or any real educational institution.

The architecture reflects modern microservices best practices:
1. **Client Tier:** Decoupled web and mobile user interfaces communicating via standard HTTPS REST/JSON APIs.
2. **Gateway Tier:** Centralized API Gateway managing transport security termination, route dispatch, token inspection, and rate limiting.
3. **Service Tier:** Decoupled domain microservices executing business logic (Identity, AI Tutoring, Learning Analytics, Course Recommendations).
4. **Data Persistence Tier:** Segmented relational and document databases partitioned by confidentiality and operational domain.
5. **External Cloud AI Tier:** Integration with foundational LLM APIs for generative intelligence.

---

## 8. System Components

The synthetic architecture comprises exactly 16 discrete structural components categorized into 4 External Entities, 8 Processes, and 4 Data Stores:

### 8.1 External Entities ($E$)
* **E1 - Student:** Enrolled undergraduate or postgraduate student accessing academic dashboards, course materials, attendance percentages, and AI tutoring services.
* **E2 - Faculty:** Academic instructors and professors responsible for grading examinations, posting daily attendance rosters, managing course modules, and reviewing academic records.
* **E3 - Campus Administrator:** Privileged IT and academic office staff managing user identity lifecycles, role permissions, system configuration flags, and security telemetry.
* **E4 - External Cloud AI/LLM Service:** Third-party hosted commercial generative AI API providing natural language processing and inference via secure external HTTPS endpoints.

### 8.2 Processes ($P$)
* **P1 - Student Mobile/Web Application:** Cross-platform client application rendering student dashboards, transmitting credentials, and submitting queries.
* **P2 - Faculty Web Portal:** Browser-based academic portal handling grade submissions, attendance rosters, and instructor communications.
* **P3 - Administrator Portal:** Restricted web console for global configuration, user role assignments, and security audit inspection.
* **P4 - API Gateway:** Centralized ingress controller: terminates external TLS, performs initial request validation, token inspection, rate limiting, and reverse-proxy routing.
* **P5 - Authentication and Authorization Service:** Identity provider responsible for verifying credentials, issuing cryptographically signed JWT tokens, and evaluating role-based permissions.
* **P6 - Campus AI Assistant:** Conversational agent orchestrating query intake, prompt sanitization, RAG context assembly, and external LLM dispatch.
* **P7 - Student Analytics Service:** Processing engine computing academic risk indicators, engagement metrics, and GPA trajectories.
* **P8 - Recommendation Service:** Machine learning suggestion engine generating personalized elective options, remedial study paths, and academic interventions.

### 8.3 Data Stores ($D$)
* **D1 - Student Information Database:** Confidential repository storing student PII, contact addresses, guardian information, enrollment status, and payment flags.
* **D2 - Academic and Attendance Database:** Integrity-critical repository housing official course grades, GPA histories, transcripts, and daily attendance logs.
* **D3 - AI Conversation and Application Logs:** Privacy-sensitive repository storing student prompt histories, chatbot completions, session context, and interaction latency metrics.
* **D4 - Security Audit Logs:** Append-only repository capturing system-wide security telemetry, authentication attempts, authorization failures, and administrative modifications.

---

## 9. Data Flow Diagram (DFD)

A detailed Level-1 Data Flow Diagram was designed to model the exact movement of data across the synthetic campus system. In strict adherence to DFD principles, all directed connectors represent the flow of information rather than procedural control sequence.

```
       [TB1: Public/External User Zone]
       E1: Student  |  E2: Faculty  |  E3: Campus Administrator
             │             │              │
           (F01)         (F02)          (F03)
             ▼             ▼              ▼
       P1: Student   P2: Faculty    P3: Admin
           App           Portal         Portal
             │             │              │
           (F04)         (F05)          (F06)
             ▼             ▼              ▼
════════════════════════════════════════════════════════════════════════════
       [TB2: Campus Application Zone]
                    ┌────────────────────────┐
                    │    P4: API Gateway     │◄───(F20, F21, F22)───┐
                    └───────────┬────────────┘                      │
           ┌────────────┬───────┴────────┬─────────────┐            │
         (F07)        (F11)            (F15)         (F18)          │
           │            │                │             │            │
           ▼            ▼                ▼             │            │
      ┌─────────┐  ┌─────────┐      ┌─────────┐        │            │
      │ P5: Auth│  │ P6: AI  │      │ P7:     │        │            │
      │ Service │  │Assistant│      │Analytics│        │            │
      └────┬────┘  └────┬────┘      └────┬────┘        │            │
         (F08)          │                │             │            │
           └───────────┼────────────────(F17)─────────┼────────────┘
                        │                │             ▼
                        │                │        ┌─────────┐
                        │                │        │ P8: Rec │
                        │                │        │ Service │
                        │                │        └─────────┘
════════════════════════╪════════════════╪══════════════════════════════════
 [TB4: External AI]     │                │     [TB3: Protected Data Zone]
  ┌─────────────┐       │ (F14)          │ (F16)   ┌─────────────────┐
  │ E4: Cloud   │◄(F12)─┤   │            ├───┐     │ D1: Student Info│
  │ LLM Service │─(F13)─┘   │            │   └────►│ Database        │
  └─────────────┘           ▼            │         └─────────────────┘
                   ┌─────────────────┐   │         ┌─────────────────┐
                   │ D3: AI Logs &   │   ├────────►│ D2: Academic &  │
                   │ App Logs        │   │  (F10)  │ Attendance DB   │
                   └─────────────────┘   │         └─────────────────┘
                                         │ (F09)   ┌─────────────────┐
                                         └────────►│ D4: Security    │
                                            (F19)  │ Audit Logs      │
                                                   └─────────────────┘
```

### Complete Logical Data Flow Inventory ($F01$ – $F22$)
* **F01:** `E1: Student` $\rightarrow$ `P1: Student Mobile/Web Application`: Student credentials, requests, AI queries.
* **F02:** `E2: Faculty` $\rightarrow$ `P2: Faculty Web Portal`: Faculty credentials, academic requests.
* **F03:** `E3: Campus Administrator` $\rightarrow$ `P3: Administrator Portal`: Administrative credentials and management requests.
* **F04:** `P1: Student Mobile/Web Application` $\rightarrow$ `P4: API Gateway`: Student API requests.
* **F05:** `P2: Faculty Web Portal` $\rightarrow$ `P4: API Gateway`: Faculty API requests.
* **F06:** `P3: Administrator Portal` $\rightarrow$ `P4: API Gateway`: Administrative API requests.
* **F07:** `P4: API Gateway` $\rightarrow$ `P5: Authentication and Authorization Service`: Authentication and authorization requests.
* **F08:** `P5: Authentication and Authorization Service` $\rightarrow$ `P4: API Gateway`: Authentication result and authorization token/session.
* **F09:** `P4: API Gateway` $\rightarrow$ `D1: Student Information Database`: Student information queries/updates.
* **F10:** `P4: API Gateway` $\rightarrow$ `D2: Academic and Attendance Database`: Academic and attendance queries/updates.
* **F11:** `P4: API Gateway` $\rightarrow$ `P6: Campus AI Assistant`: Student/faculty AI requests.
* **F12:** `P6: Campus AI Assistant` $\rightarrow$ `E4: External Cloud AI/LLM Service`: AI prompt/context.
* **F13:** `E4: External Cloud AI/LLM Service` $\rightarrow$ `P6: Campus AI Assistant`: AI-generated response.
* **F14:** `P6: Campus AI Assistant` $\rightarrow$ `D3: AI Conversation and Application Logs`: AI interaction metadata/logs.
* **F15:** `P4: API Gateway` $\rightarrow$ `P7: Student Analytics Service`: Student activity/academic data.
* **F16:** `P7: Student Analytics Service` $\rightarrow$ `D1: Student Information Database`: Analytics-related student data retrieval.
* **F17:** `P7: Student Analytics Service` $\rightarrow$ `P8: Recommendation Service`: Analytics results/features.
* **F18:** `P8: Recommendation Service` $\rightarrow$ `P4: API Gateway`: Personalized recommendations.
* **F19:** `P4: API Gateway` $\rightarrow$ `D4: Security Audit Logs`: API/security events.
* **F20:** `P4: API Gateway` $\rightarrow$ `P1: Student Mobile/Web Application`: API response/data.
* **F21:** `P4: API Gateway` $\rightarrow$ `P2: Faculty Web Portal`: API response/data.
* **F22:** `P4: API Gateway` $\rightarrow$ `P3: Administrator Portal`: API response/data.

---

## 10. Trust Boundaries

Trust boundaries represent the perimeters across which administrative privileges, physical environments, or network trust levels transition.

1. **TB1 - Public/External User Zone:** Encompasses end-user client devices (`P1`–`P3`) and actors (`E1`–`E3`). Operates across untrusted public telecommunication networks, cellular carriers, and commercial Wi-Fi hotspots. All communication entering `TB2` across `TB1` must be treated as hostile and untrusted.
2. **TB2 - Campus Application Zone:** Encompasses internal microservices (`P4`–`P8`) deployed within private subnets behind perimeter firewalls. Network access is restricted, but zero-trust principles apply between microservices.
3. **TB3 - Protected Data Zone:** The internal persistence tier hosting relational databases and log stores (`D1`–`D4`). This zone is isolated via database firewalls and network security groups, accepting connections exclusively from authorized backend service accounts over mutual TLS (mTLS).
4. **TB4 - External AI Service Boundary:** Demarcates the boundary between the internal campus network (`P6`) and the third-party commercial cloud provider (`E4`). Any data crossing this boundary exits campus institutional control, requiring strict anonymization and filtering.

---

## 11. Threat Modeling Methodology

The project adheres to a four-stage engineering methodology:
1. **Decompose the System:** Model all components, communication channels, and trust perimeters using standard Level-1 DFD conventions.
2. **Determine Threats (STRIDE):** Systematically evaluate each component and boundary crossing against the six STRIDE failure classes.
3. **Determine Risk (Qualitative Scoring):** Apply a consistent 3×3 matrix ($\mathcal{L} \times \mathcal{I}$) to calculate discrete risk scores.
4. **Determine Controls (Mitigation):** Formulate actionable, layered security safeguards (Preventive, Detective, Corrective) and map them to engineering priorities.

---

## 12. STRIDE Framework

The STRIDE mnemonic provides a standardized methodology for analyzing software vulnerabilities:

| STRIDE Category | Security Property Compromised | Definition in Campus Context |
| :--- | :--- | :--- |
| **Spoofing (S)** | **Authenticity** | Pretending to be someone or something else (e.g., student impersonating faculty or forging JWT tokens). |
| **Tampering (T)** | **Integrity** | Modifying data in transit or at rest without authorization (e.g., altering official course grades or injecting adversarial AI prompts). |
| **Repudiation (R)** | **Non-Repudiation** | Claiming not to have performed an action without the system being able to prove otherwise (e.g., instructor denying submitting altered attendance). |
| **Information Disclosure (I)** | **Confidentiality** | Exposing sensitive data to unauthorized parties (e.g., student PII exfiltration or sending unredacted grades to a third-party cloud LLM). |
| **Denial of Service (D)** | **Availability** | Degrading or halting service operations for legitimate users (e.g., DDoS flooding of the API Gateway or token-exhaustion attacks against the AI assistant). |
| **Elevation of Privilege (E)** | **Authorization** | Gaining unauthorized capabilities or administrative access (e.g., student exploiting broken authorization to access faculty routes). |

---

## 13. Threat Identification

Using STRIDE, **50 realistic threats** were identified across the campus architecture. Care was taken to avoid forcing all six categories onto every component; instead, threats were derived from realistic attack vectors, including credential stuffing, cross-site scripting (XSS), prompt injection, broken object-level authorization (BOLA), SQL injection, and audit log starvation.

---

## 14. STRIDE Threat Matrix

The identified threats are synthesized in the master matrix. Below is an excerpt of representative critical and high-priority threats across each STRIDE category:

| Threat ID | Element | Component Name | STRIDE | Risk Level | Score | Recommended Architectural Control |
| :---: | :---: | :--- | :---: | :---: | :---: | :--- |
| **T01** | `E1` | `Student` | Spoofing | **Critical** | 9 | Multi-Factor Authentication (MFA), IP rate limiting, compromised credential checks. |
| **T04** | `E4` | `External Cloud AI/LLM Service` | Information Disclosure | **Critical** | 9 | Zero-Data-Retention (ZDR) agreements, pre-transit PII pseudonymization/tokenization. |
| **T14** | `P4` | `API Gateway` | Denial of Service | **Critical** | 9 | Cloud edge DDoS protection (Cloudflare/AWS Shield), distributed rate limiting, WAF. |
| **T17** | `P5` | `Authentication and Authorization Service` | Spoofing | **High** | 6 | Asymmetric RS256 token signing with private keys in HSM; reject unapproved algorithms. |
| **T18** | `P5` | `Authentication and Authorization Service` | Elevation of Privilege | **High** | 6 | Server-side authorization verifying authenticated identity matches requested resource ID. |
| **T20** | `P6` | `Campus AI Assistant` | Tampering | **Critical** | 9 | Dual LLM guardrail architecture (NeMo Guardrails), strict prompt isolation, regex filtering. |
| **T21** | `P6` | `Campus AI Assistant` | Information Disclosure | **Critical** | 9 | Identity-aware RAG document retrieval filtering; egress Data Loss Prevention (DLP). |
| **T30** | `D2` | `Academic and Attendance Database` | Tampering | **High** | 6 | Parameterized SQL queries, Row-Level Security (RLS), cryptographic grade hash-chains. |
| **T36** | `D4` | `Security Audit Logs` | Repudiation | **High** | 6 | Comprehensive audit policy capturing who, what, when, where; structured logging to SIEM. |
| **T44** | `F12` | `Campus AI Assistant -> External Cloud AI` | Information Disclosure | **Critical** | 9 | Automated client-side Named Entity Recognition (NER) PII scrubbing pipeline; DLP. |

*(Refer to `docs/stride_threat_matrix.md` and `docs/stride_threat_matrix.csv` for the complete 50-threat catalog).*

---

## 15. Risk Assessment

Threats were evaluated using the formal 3×3 risk matrix defined in `docs/risk_methodology.md`:

$$\text{Risk Score} = \text{Likelihood} \times \text{Impact} \quad (\mathcal{L} \in \{1, 2, 3\}, \mathcal{I} \in \{1, 2, 3\})$$

### Risk Distribution
* **Critical (Score 9):** 7 threats (14%) – Public ingress DDoS, Cloud LLM PII leakage, Prompt Injection, RAG Context Leakage, Credential Stuffing, Chat Log Exposure.
* **High (Score 6):** 18 threats (36%) – Account takeover, SQL injection on grades/PII, JWT forgery, Insecure LLM output handling, Wi-Fi MitM, Bcrypt flooding.
* **Medium (Score 3–4):** 20 threats (40%) – BOLA authorization, Replay attacks, Parameter tampering, DB backup exfiltration, Audit flooding, Analytics feature poisoning.
* **Low (Score 1–2):** 5 threats (10%) – Client application crashes, Course recommendation shilling, Small-cohort inferential analytics privacy loss.

```
Critical (14%) ── [ 7 Threats ]
High (36%)     ────── [ 18 Threats ]
Medium (40%)   ──────── [ 20 Threats ]
Low (10%)      ── [ 5 Threats ]
```

---

## 16. Recommended Security Controls

Controls are organized across three primary operational classes:

### 16.1 Preventive Controls (Primary Defense)
* **Cryptographic Perimeter Protection:** Enforce TLS 1.3 across all external connections (`TB1`, `TB4`) and mutual TLS (mTLS) with ephemeral certificates across internal microservices (`TB2`, `TB3`).
* **Identity and Session Hardening:** Mandate Multi-Factor Authentication (MFA) via TOTP or WebAuthn hardware tokens for all user accounts; sign JWTs using asymmetric RS256/ES256 keys managed within a dedicated Hardware Security Module (HSM).
* **Database Defenses:** Enforce parameterized queries (Prepared Statements / ORM) across all database interactions (`F09`, `F10`) to eliminate SQL injection; implement Row-Level Security (RLS) and AES-256 Transparent Data Encryption (TDE) at rest.
* **AI Security Guardrails:** Deploy dual-layer prompt-filtering proxies (e.g., NVIDIA NeMo Guardrails or Meta Llama Guard) to intercept adversarial prompt injections and prevent system instruction tampering.

### 16.2 Detective Controls (Monitoring and Accountability)
* **Immutable Security Auditing:** Stream all authentication events, privilege escalations, and data modifications to an isolated, append-only Write-Once-Read-Many (WORM) cloud repository (`D4`).
* **SIEM Event Correlation:** Ingest structured JSON security logs into a centralized Security Information and Event Management (SIEM) engine with automated alerting on brute-force attempts and geo-velocity anomalies.
* **Cryptographic Ledger Integrity:** Implement cryptographic hash chaining on academic grade ledgers to detect any unauthorized out-of-band database modifications.

### 16.3 Corrective Controls (Resilience and Recovery)
* **Automated Rate Throttling:** Deploy adaptive API Gateway throttling to temporarily block offending IP subnets during volumetric flooding or Bcrypt authentication attacks.
* **Point-in-Time Database Recovery:** Maintain automated, immutable write-ahead log backups permitting zero-loss point-in-time recovery in the event of ransomware or accidental database corruption.

---

## 17. AI-Specific Security Considerations

Because this architecture features an AI Assistant (`P6`), an Analytics Service (`P7`), and a Recommendation Engine (`P8`), specific threat vectors were evaluated under the STRIDE methodology:

1. **Prompt Injection as Tampering (`T20`):** Natural language prompts allow users to bypass application logic and tamper with internal bot personas. Mitigation relies on programmatic prompt isolation, strict delimiter framing, and pre-inference classification.
2. **PII Egress as Information Disclosure (`T44`):** Transmitting student queries containing personal identifiers to external LLMs violates privacy regulations. Mitigation requires a local, client-side Named Entity Recognition (NER) tokenization pipeline that redacts student names, registration numbers, and contact details prior to dispatching prompts across `TB4`.
3. **Insecure LLM Output as Cross-Site Scripting (`T45`):** Generative language models can output malicious script tags or markdown links. Downstream web clients (`P1`, `P2`) must treat all AI completions as untrusted input, applying rigorous HTML entity escaping and DOMPurify sanitization.
4. **Token Exhaustion as Denial of Service (`T22`):** Adversaries can craft recursive prompts to consume token allowances. Mitigation requires per-student daily token budgets and hard output length constraints.
5. **RAG Context Leakage (`T21`):** In multi-tenant environments, retrieval pipelines can accidentally inject peer student records into prompt context buffers. Mitigation mandates identity-scoped document retrieval where vector embeddings are tagged and filtered by user ID.

---

## 18. Results and Discussion

The architectural analysis demonstrates that the highest-risk structural zones are the **API Gateway ingress (`TB1` $\rightarrow$ `TB2`)** and the **External Cloud AI boundary (`TB2` $\rightarrow$ `TB4`)**. 

```
                                  RISK HEATMAP SUMMARY
┌───────────────────────────────┬───────────────────────────────┬───────────────────────────────┐
│ TB1: Public Ingress           │ TB2: Microservices Subnet     │ TB3: Protected Data Tier      │
│ • Volumetric DDoS (Crit-9)    │ • Request Smuggling (High-6)  │ • Grade Ledger SQLi (High-6)  │
│ • Credential Stuffing (Crit-9)│ • JWT Secret Forgery (High-6) │ • Backup Exfiltration (Med-3) │
│ • Wi-Fi MitM Eavesdrop(High-6)│ • BOLA Privilege Esc (High-6) │ • Audit Log Deletion (Med-3)  │
├───────────────────────────────┼───────────────────────────────┼───────────────────────────────┤
│ TB4: External Cloud AI Egress │ P6: Campus AI Assistant       │ D4: Security Audit Repository │
│ • PII Leakage in Prompts(Crit-9│ • Prompt Injection (Crit-9)   │ • Event Dropping (High-6)     │
│ • Cloud Training Use (Crit-9) │ • RAG Context Leakage(Crit-9) │ • Storage Exhaustion (Med-4)  │
│ • Malicious Completions(High-6│ • Token Exhaustion DoS(High-6)│ • Inadequate Telemetry(High-6)│
└───────────────────────────────┴───────────────────────────────┴───────────────────────────────┘
```

The concentration of Critical and High threats at these perimeters illustrates that modern threat modeling must account for both network-level boundaries and semantic application boundaries. Implementing the recommended controls reduces the unmitigated risk exposure by establishing clear defense-in-depth across every architectural tier.

---

## 19. Limitations

1. **Analytical Abstraction:** This project is strictly an analytical threat-modeling exercise. It evaluates theoretical failure modes and does not perform active software development or penetration testing exploits.
2. **Qualitative Risk Approximation:** Likelihood and Impact ratings are based on qualitative engineering judgment rather than empirical telemetry from live enterprise production environments.
3. **Synthetic Model Bounds:** Real-world university IT environments encompass legacy ERP software, disparate department databases, unmanaged IoT devices, and physical campus networks, which were excluded to maintain focus on the core logical architecture.

---

## 20. Conclusion

This academic micro-project successfully formulated and executed an exhaustive STRIDE-based threat modeling evaluation of a synthetic AI-enabled university campus information system. By decomposing the architecture into a Level-1 Data Flow Diagram and systematically analyzing each component and boundary traversal, **50 distinct cybersecurity and privacy threats** were cataloged and scored.

The findings underscore that while AI microservices offer transformative educational capabilities, they introduce critical vulnerabilities regarding prompt manipulation, third-party data egress, and contextual privacy leaks. Addressing these challenges requires a disciplined "Shift-Left" approach where strong transport cryptography, strict server-side authorization, immutable audit logging, and specialized AI guardrails are embedded directly into the architectural blueprint.

---

## 21. Future Scope

1. **Automated Threat Modeling CI/CD Integration:** Extend the methodology by embedding threat-model validation checks into automated DevOps pipelines using tools like `pytm` or Threat Dragon CLI.
2. **On-Premise / Edge AI Hosting:** Model the transition from third-party commercial cloud LLMs (`E4`) to private, on-premise open-source LLM instances (e.g., Llama 3 / Mistral) hosted within the campus data center to eliminate `TB4` wide-area data egress.
3. **Formal Privacy Impact Assessment (PIA):** Conduct a formal regulatory Privacy Impact Assessment under the provisions of India's Digital Personal Data Protection Act (DPDPA 2023) and GDPR Article 35.
4. **Zero-Trust Architecture (ZTA) Implementation:** Develop a detailed micro-segmentation blueprint utilizing service mesh proxies (e.g., Istio/Envoy) to enforce cryptographic mutual TLS and fine-grained SPIFFE/SPIRE identity attestation across all backend services.

---

## 22. References

1. Kohnfelder, L., & Garg, P. (1999). *The threats to our products*. Microsoft Interface.
2. Shostack, A. (2014). *Threat Modeling: Designing for Security*. John Wiley & Sons.
3. Open Worldwide Application Security Project (OWASP). (2023). *OWASP Threat Dragon Documentation & Architectural Guide*.
4. Open Worldwide Application Security Project (OWASP). (2023). *OWASP Top 10 for Large Language Model Applications*.
5. National Institute of Standards and Technology (NIST). (2018). *Framework for Improving Critical Infrastructure Cybersecurity (Version 1.1)*. NIST CSWP 04162018.
6. Howard, M., & LeBlanc, D. (2003). *Writing Secure Code (2nd Edition)*. Microsoft Press.
7. Government of India. (2023). *The Digital Personal Data Protection Act, 2023*. The Gazette of India.
8. European Union. (2016). *Regulation (EU) 2016/679 (General Data Protection Regulation)*. Official Journal of the European Union.
