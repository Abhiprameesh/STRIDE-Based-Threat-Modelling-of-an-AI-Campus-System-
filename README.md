# STRIDE-Based Threat Modelling of an AI Campus System

**Course:** Data Security and Privacy (Academic Micro-Project)  
**Program:** Bachelor of Engineering in Artificial Intelligence and Machine Learning (BE AIML)  
**Academic Year:** 2026–2027  
**Official Objective:** *"Identify threats using STRIDE and recommend controls"*  
**Dataset:** Synthetic campus architecture  
**Tools Required:** OWASP Threat Dragon, Draw.io  

---

> [!IMPORTANT]
> **Academic Disclaimer:** This is an academic threat-modeling micro-project, **NOT** a real campus application. It does **NOT** build an actual production web application, nor does it use real student data, real credentials, API keys, IP addresses, passwords, or personal information. The architecture is purely synthetic and created specifically for academic threat modeling. It does **not** model the real Dayananda Sagar College of Engineering (DSCE) or any real educational institution.

---

## 1. Project Overview

As universities rapidly integrate generative artificial intelligence into academic ecosystems—such as conversational tutors, predictive learning analytics, and course recommendation engines—the attack surface expands dramatically. Alongside traditional web application vulnerabilities (SQL injection, XSS, broken authentication), AI campus systems face novel semantic vulnerabilities, including prompt injection, context poisoning, training data exfiltration, and model token exhaustion.

This project delivers a complete, academically rigorous cybersecurity threat model of a synthetic AI-augmented campus system. By applying Microsoft's **STRIDE** methodology to a formal Level-1 Data Flow Diagram (DFD), **50 distinct, realistic threat scenarios** are identified, prioritized via a 3×3 qualitative risk assessment matrix ($\text{Risk} = \text{Likelihood} \times \text{Impact}$), and mapped to actionable architectural security controls.

---

## 2. Fixed System Architecture

The architecture consists of 16 canonical components partitioned across 4 trust boundaries:

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        TB1: Public/External User Zone                       │
│  External Entities:                                                         │
│    • E1 - Student                                                           │
│    • E2 - Faculty                                                           │
│    • E3 - Campus Administrator                                              │
│  Client Applications:                                                       │
│    • P1 - Student Mobile/Web Application                                    │
│    • P2 - Faculty Web Portal                                                │
│    • P3 - Administrator Portal                                              │
├─────────────────────────────────────────────────────────────────────────────┤
│                        TB2: Campus Application Zone                         │
│  Core Services:                                                             │
│    • P4 - API Gateway                                                       │
│    • P5 - Authentication and Authorization Service                          │
│    • P6 - Campus AI Assistant                                               │
│    • P7 - Student Analytics Service                                         │
│    • P8 - Recommendation Service                                            │
├─────────────────────────────────────────────────────────────────────────────┤
│                        TB3: Protected Data Zone                             │
│  Data Stores:                                                               │
│    • D1 - Student Information Database                                      │
│    • D2 - Academic and Attendance Database                                  │
│    • D3 - AI Conversation and Application Logs                              │
│    • D4 - Security Audit Logs                                               │
├─────────────────────────────────────────────────────────────────────────────┤
│                     TB4: External AI Service Boundary                       │
│  External Service:                                                          │
│    • E4 - External Cloud AI/LLM Service                                     │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 3. Logical Data Flows ($F01$ – $F22$)

The system defines exactly 22 formal data flows connecting all components without isolated nodes:

* **F01:** `Student` $\rightarrow$ `Student Mobile/Web Application` *(Student credentials, requests, AI queries)*
* **F02:** `Faculty` $\rightarrow$ `Faculty Web Portal` *(Faculty credentials, academic requests)*
* **F03:** `Campus Administrator` $\rightarrow$ `Administrator Portal` *(Administrative credentials and management requests)*
* **F04:** `Student Mobile/Web Application` $\rightarrow$ `API Gateway` *(Student API requests)*
* **F05:** `Faculty Web Portal` $\rightarrow$ `API Gateway` *(Faculty API requests)*
* **F06:** `Administrator Portal` $\rightarrow$ `API Gateway` *(Administrative API requests)*
* **F07:** `API Gateway` $\rightarrow$ `Authentication and Authorization Service` *(Authentication and authorization requests)*
* **F08:** `Authentication and Authorization Service` $\rightarrow$ `API Gateway` *(Authentication result and authorization token/session)*
* **F09:** `API Gateway` $\rightarrow$ `Student Information Database` *(Student information queries/updates)*
* **F10:** `API Gateway` $\rightarrow$ `Academic and Attendance Database` *(Academic and attendance queries/updates)*
* **F11:** `API Gateway` $\rightarrow$ `Campus AI Assistant` *(Student/faculty AI requests)*
* **F12:** `Campus AI Assistant` $\rightarrow$ `External Cloud AI/LLM Service` *(AI prompt/context)*
* **F13:** `External Cloud AI/LLM Service` $\rightarrow$ `Campus AI Assistant` *(AI-generated response)*
* **F14:** `Campus AI Assistant` $\rightarrow$ `AI Conversation and Application Logs` *(AI interaction metadata/logs)*
* **F15:** `API Gateway` $\rightarrow$ `Student Analytics Service` *(Student activity/academic data)*
* **F16:** `Student Analytics Service` $\rightarrow$ `Student Information Database` *(Analytics-related student data retrieval)*
* **F17:** `Student Analytics Service` $\rightarrow$ `Recommendation Service` *(Analytics results/features)*
* **F18:** `Recommendation Service` $\rightarrow$ `API Gateway` *(Personalized recommendations)*
* **F19:** `All important security-sensitive services` $\rightarrow$ `Security Audit Logs` *(Security events and audit records)*
* **F20:** `API Gateway` $\rightarrow$ `Student Mobile/Web Application` *(API response/data)*
* **F21:** `API Gateway` $\rightarrow$ `Faculty Web Portal` *(API response/data)*
* **F22:** `API Gateway` $\rightarrow$ `Administrator Portal` *(API response/data)*

---

## 4. STRIDE Threat Methodology & Risk Scoring

Threats are categorized using the six STRIDE domains:
* **S - Spoofing:** Masquerading as an authorized student, faculty, or system component.
* **T - Tampering:** Modifying academic records, request parameters, or AI prompts.
* **R - Repudiation:** Denying grade submissions or system updates without audit evidence.
* **I - Information Disclosure:** Leaking student PII, chat logs, or academic transcripts.
* **D - Denial of Service:** Flooding API endpoints or exhausting AI token quotas.
* **E - Elevation of Privilege:** Escalating permissions from student to faculty or admin.

### 3×3 Qualitative Risk Formula
$$\text{Risk Score} = \text{Likelihood} \times \text{Impact}$$
* **Likelihood ($\mathcal{L}$):** Low (1), Medium (2), High (3)
* **Impact ($\mathcal{I}$):** Low (1), Medium (2), High (3)
* **Risk Levels:**
  * **Low:** Score 1 – 2 (Priority P4)
  * **Medium:** Score 3 – 4 (Priority P3)
  * **High:** Score 6 (Priority P2)
  * **Critical:** Score 9 (Priority P1)

---

## 5. Project Directory Structure

```
STRIDE-Based Threat Modelling of an AI Campus System/
│
├── README.md                          # Master project documentation and guide
│
├── docs/
│   ├── AI_Campus_DFD.jpg              # Authoritative Level-1 Data Flow Diagram image
│   ├── project_report.md              # Complete 22-section academic project report
│   ├── dfd_specification.md           # Level-1 DFD specifications and Mermaid diagram
│   ├── stride_threat_matrix.md        # Comprehensive Markdown threat matrix (50 threats)
│   ├── stride_threat_matrix.csv       # Machine-readable tabular threat matrix
│   ├── threat_dragon_mapping.md       # DFD-to-Threat-Dragon mapping specification
│   ├── risk_methodology.md            # Detailed 3x3 qualitative risk framework
│   ├── assumptions.md                 # 12 core threat model assumptions
│   ├── scope.md                       # In-scope and out-of-scope boundaries
│   ├── traceability_matrix.md         # End-to-end component-threat-control mapping
│   └── viva_questions.md              # 28 comprehensive viva questions and answers
│
├── model/
│   ├── campus_architecture.json       # Formal JSON schema representing the system
│   └── threat_dragon_model.json       # Official OWASP Threat Dragon model (42 cells, 50 threats)
│
└── validation/
    ├── consistency_check.md           # 20-point quality assurance verification report
    └── threat_dragon_consistency.md   # Threat Dragon compliance audit against DFD image
```

---

## 6. How to Recreate the DFD in Draw.io

To generate the visual Level-1 Data Flow Diagram in Draw.io (diagrams.net):
1. **Launch Draw.io:** Open [app.diagrams.net](https://app.diagrams.net/) in your web browser.
2. **Page Configuration:**
   * Select a **Blank Diagram**.
   * Go to the right sidebar: Set Paper Size to **A3 Landscape** or **A4 Landscape**; check **Grid** (10pt).
3. **Draw Trust Boundaries (Containers):**
   * Drag a large rectangle for each zone and set Fill Opacity to 15–20% with dashed borders:
     * `TB1: Public/External User Zone` (Top third, Orange dashed `#ffa000`)
     * `TB2: Campus Application Zone` (Middle third, Blue dashed `#0288d1`)
     * `TB3: Protected Data Zone` (Bottom-right, Green dashed `#388e3c`)
     * `TB4: External AI Service Boundary` (Bottom-left, Purple dashed `#7b1fa2`)
4. **Place Elements:**
   * **External Entities (`E1`–`E4`):** Use sharp rectangles with solid borders. Place `E1`, `E2`, `E3` in `TB1` and `E4` in `TB4`.
   * **Processes (`P1`–`P8`):** Use rounded rectangles or circles. Place `P1`, `P2`, `P3` in `TB1` below the entities; place `P4`–`P8` in `TB2`.
   * **Data Stores (`D1`–`D4`):** Use the Database cylinder or double-horizontal parallel lines shape in `TB3`.
5. **Route Connectors (`F01`–`F22`):**
   * Use **Orthogonal Connectors** with arrowheads pointing from source to destination.
   * Double-click each line to add the exact flow label (e.g., `F04: Student API requests`).
   * Reference [`docs/dfd_specification.md`](file:///c:/DSP%20lab%20Micro%20Project/STRIDE-Based%20Threat%20Modelling%20of%20an%20AI%20Campus%20System/docs/dfd_specification.md) for complete visual positioning and connector routing.

---

## 7. How to Recreate the Model in OWASP Threat Dragon

1. **Launch Threat Dragon:** Open the OWASP Threat Dragon desktop application or web interface ([threatdragon.org](https://threatdragon.org/)).
2. **Create New Threat Model:**
   * Title: `STRIDE-Based Threat Modelling of an AI Campus System`
   * Contributor / Owner: Academic Micro-Project (DSP Course)
   * Add a new diagram titled `Campus System Level-1 DFD`.
3. **Add Elements to the Canvas:**
   * Add 4 **Actor (External Entity)** elements: `E1: Student`, `E2: Faculty`, `E3: Campus Administrator`, `E4: External Cloud AI/LLM Service`.
   * Add 8 **Process** elements: `P1` through `P8`.
   * Add 4 **Store** elements: `D1` through `D4`.
   * Add 4 **Trust Boundary** lines surrounding the corresponding components.
4. **Connect Data Flows:**
   * Draw directional data flows matching `F01` through `F22`.
5. **Populate STRIDE Threats:**
   * Select an element or flow and click **Add Threat**.
   * Threat Dragon automatically filters STRIDE categories based on element type.
   * Input the Threat Title, Category, Description, Severity, and Recommended Mitigation using the exact data from [`docs/stride_threat_matrix.csv`](file:///c:/DSP%20lab%20Micro%20Project/STRIDE-Based%20Threat%20Modelling%20of%20an%20AI%20Campus%20System/docs/stride_threat_matrix.csv).
6. **Export Report:** Export the model as a JSON threat file or PDF report for examination submission.

---

## 8. Verifying Consistency Between DFD and Threat Matrix

To independently verify complete consistency across the project artifacts:
1. **Component Check:** Open [`docs/traceability_matrix.md`](file:///c:/DSP%20lab%20Micro%20Project/STRIDE-Based%20Threat%20Modelling%20of%20an%20AI%20Campus%20System/docs/traceability_matrix.md) and confirm that all 16 components and 13 boundary-crossing flows map directly to threat IDs `T01`–`T50`.
2. **Mathematical Check:** Verify that every Risk Level in [`docs/stride_threat_matrix.csv`](file:///c:/DSP%20lab%20Micro%20Project/STRIDE-Based%20Threat%20Modelling%20of%20an%20AI%20Campus%20System/docs/stride_threat_matrix.csv) equals $\text{Likelihood} \times \text{Impact}$ as defined in [`docs/risk_methodology.md`](file:///c:/DSP%20lab%20Micro%20Project/STRIDE-Based%20Threat%20Modelling%20of%20an%20AI%20Campus%20System/docs/risk_methodology.md).
3. **Automated Audit:** Inspect [`validation/consistency_check.md`](file:///c:/DSP%20lab%20Micro%20Project/STRIDE-Based%20Threat%20Modelling%20of%20an%20AI%20Campus%20System/validation/consistency_check.md), which documents the successful automated validation of all 20 quality criteria.

---

## 9. Key Analytical Findings

* **Total Identified Threats:** 50
* **Critical Threats (Score 9):** 7 (14%) – Public ingress DDoS, External AI PII exfiltration, Prompt injection, RAG context leakage, Credential stuffing, and Plaintext chat logging.
* **High Threats (Score 6):** 18 (36%) – Grade modification via SQLi, Faculty account takeover, JWT token forgery, Insecure LLM output handling, and Audit event dropping.
* **Medium Threats (Score 3–4):** 20 (40%)
* **Low Threats (Score 1–2):** 5 (10%)
* **Highest-Risk Architectural Zone:** The intersection of **API Gateway (`P4`)** at `TB1` ingress and the **Campus AI Assistant (`P6`)** at `TB4` egress.
