# OWASP Threat Dragon Architectural Mapping Specification

## 1. Executive Summary & Mapping Overview

This document defines the formal mapping between the approved **Level-1 Data Flow Diagram (DFD)** and the **OWASP Threat Dragon** threat model for the academic micro-project: **"STRIDE-Based Threat Modelling of an AI Campus System"**.

The Threat Dragon model represents the exact architecture depicted in the authoritative Level-1 DFD diagram without modifications, additions, deletions, or renaming of any component or communication channel. It embeds all **50 realistic STRIDE threats** directly into their corresponding diagram elements and data flows, complete with attack scenarios, risk evaluations, and defense-in-depth architectural control recommendations.

---

## 2. DFD to Threat Dragon Element Type Mapping

OWASP Threat Dragon uses specific visual element primitives to model data architectures. The table below outlines how each canonical component maps to Threat Dragon’s native cell types:

| DFD Element Category | Canonical System Elements | Threat Dragon Type (`type`) | Visual Representation in Tool |
| :--- | :--- | :---: | :--- |
| **External Entities** | `E1: Student`<br>`E2: Faculty`<br>`E3: Campus Administrator`<br>`E4: External Cloud AI/LLM Service` | `tm.Actor` | Solid sharp rectangle representing actors or third-party systems outside institutional control. |
| **Processes (Clients & Services)** | `P1: Student Mobile/Web Application`<br>`P2: Faculty Web Portal`<br>`P3: Administrator Portal`<br>`P4: API Gateway`<br>`P5: Authentication and Authorization Service`<br>`P6: Campus AI Assistant`<br>`P7: Student Analytics Service`<br>`P8: Recommendation Service` | `tm.Process` | Rounded rectangle / circle representing active computational and routing components. |
| **Data Stores** | `D1: Student Information Database`<br>`D2: Academic and Attendance Database`<br>`D3: AI Conversation and Application Logs`<br>`D4: Security Audit Logs` | `tm.Store` | Database cylinder / parallel lines representing persistent or semi-persistent state. |
| **Trust Boundaries** | `TB1: Public / External User Zone`<br>`TB2: Campus Application Zone`<br>`TB3: Protected Data Zone`<br>`TB4: External AI Service Boundary` | `tm.Boundary` | Dashed border lines partitioning distinct network privilege and trust domains. |
| **Data Flows** | `F01` through `F22` (All 22 directional flows) | `tm.Flow` | Directed arrows representing the transfer of payloads, credentials, queries, and logs. |

---

## 3. Structural Decomposition in Threat Dragon

### 3.1 Trust Boundaries (`tm.Boundary`)
Threat Dragon models trust boundaries as dashed perimeters enclosing specific sets of components:

1. **TB1: Public / External User Zone**
   * *Enclosed Elements:* `E1` (Student), `E2` (Faculty), `E3` (Campus Administrator), `P1` (Student App), `P2` (Faculty Portal), `P3` (Admin Portal).
   * *Significance:* Unmanaged external network environment (cellular/internet) subject to sniffing, spoofing, and client-side inspection.
2. **TB2: Campus Application Zone**
   * *Enclosed Elements:* `P4` (API Gateway), `P5` (Auth Service), `P6` (Campus AI Assistant), `P7` (Student Analytics Service), `P8` (Recommendation Service).
   * *Significance:* Private campus microservices subnet protected by firewalls; enforces zero-trust between services.
3. **TB3: Protected Data Zone**
   * *Enclosed Elements:* `D1` (Student Info DB), `D2` (Academic & Attendance DB), `D3` (AI Logs), `D4` (Security Audit Logs).
   * *Significance:* Highly restricted persistence enclave; accepts connections exclusively via internal mTLS from authenticated microservices.
4. **TB4: External AI Service Boundary**
   * *Enclosed Elements:* `E4` (External Cloud AI/LLM Service).
   * *Significance:* WAN perimeter demarcating the transition from institutional infrastructure to commercial third-party cloud infrastructure.

---

### 3.2 Complete Logical Data Flow Mapping (`tm.Flow`)

All 22 flows are instantiated with explicit source-to-target bindings matching the authoritative DFD image:

| Flow ID | Source Element | Target Element | Threat Dragon Label & Payload | Crosses Boundary |
| :---: | :---: | :---: | :--- | :---: |
| **F01** | `E1` (Student) | `P1` (Student App) | `F01: Student credentials, requests, AI queries` | None (Within `TB1`) |
| **F02** | `E2` (Faculty) | `P2` (Faculty Portal) | `F02: Faculty credentials, academic requests` | None (Within `TB1`) |
| **F03** | `E3` (Campus Admin) | `P3` (Admin Portal) | `F03: Administrative credentials and management requests` | None (Within `TB1`) |
| **F04** | `P1` (Student App) | `P4` (API Gateway) | `F04: Student API requests` | `TB1` $\rightarrow$ `TB2` |
| **F05** | `P2` (Faculty Portal) | `P4` (API Gateway) | `F05: Faculty API requests` | `TB1` $\rightarrow$ `TB2` |
| **F06** | `P3` (Admin Portal) | `P4` (API Gateway) | `F06: Administrative API requests` | `TB1` $\rightarrow$ `TB2` |
| **F07** | `P4` (API Gateway) | `P5` (Auth Service) | `F07: Authentication and authorization requests` | None (Within `TB2`) |
| **F08** | `P5` (Auth Service) | `P4` (API Gateway) | `F08: Authentication result and authorization token/session` | None (Within `TB2`) |
| **F09** | `P4` (API Gateway) | `D1` (Student Info DB) | `F09: Student information queries/updates` | `TB2` $\rightarrow$ `TB3` |
| **F10** | `P4` (API Gateway) | `D2` (Academic DB) | `F10: Academic and attendance queries/updates` | `TB2` $\rightarrow$ `TB3` |
| **F11** | `P4` (API Gateway) | `P6` (AI Assistant) | `F11: Student/faculty AI requests` | None (Within `TB2`) |
| **F12** | `P6` (AI Assistant) | `E4` (Cloud AI/LLM) | `F12: AI prompt/context` | `TB2` $\rightarrow$ `TB4` |
| **F13** | `E4` (Cloud AI/LLM) | `P6` (AI Assistant) | `F13: AI-generated response` | `TB4` $\rightarrow$ `TB2` |
| **F14** | `P6` (AI Assistant) | `D3` (AI Logs) | `F14: AI interaction metadata/logs` | `TB2` $\rightarrow$ `TB3` |
| **F15** | `P4` (API Gateway) | `P7` (Analytics Service) | `F15: Student activity/academic data` | None (Within `TB2`) |
| **F16** | `P7` (Analytics Service) | `D1` (Student Info DB) | `F16: Analytics-related student data retrieval` | `TB2` $\rightarrow$ `TB3` |
| **F17** | `P7` (Analytics Service) | `P8` (Rec Service) | `F17: Analytics results/features` | None (Within `TB2`) |
| **F18** | `P8` (Rec Service) | `P4` (API Gateway) | `F18: Personalized recommendations` | None (Within `TB2`) |
| **F19** | `P4` (API Gateway) | `D4` (Security Audit Logs) | `F19: API/security events` | `TB2` $\rightarrow$ `TB3` |
| **F20** | `P4` (API Gateway) | `P1` (Student App) | `F20: API response/data` | `TB2` $\rightarrow$ `TB1` |
| **F21** | `P4` (API Gateway) | `P2` (Faculty Portal) | `F21: API response/data` | `TB2` $\rightarrow$ `TB1` |
| **F22** | `P4` (API Gateway) | `P3` (Admin Portal) | `F22: API response/data` | `TB2` $\rightarrow$ `TB1` |

---

## 4. Threat Dragon STRIDE Threat Allocation

In OWASP Threat Dragon, threats are attached as children objects under their respective parent cell (`cells[i].threats`). All 50 threats (`T01` through `T50`) are embedded into the generated model file `model/threat_dragon_model.json`.

### Threat Distribution by Component:
* **Entities:**
  * `E1: Student` $\rightarrow$ `T01` (Spoofing)
  * `E2: Faculty` $\rightarrow$ `T02` (Spoofing)
  * `E3: Campus Administrator` $\rightarrow$ `T03` (Spoofing)
  * `E4: External Cloud AI/LLM Service` $\rightarrow$ `T04` (Information disclosure), `T05` (Tampering)
* **Processes:**
  * `P1: Student Mobile/Web Application` $\rightarrow$ `T06` (Tampering), `T07` (Information disclosure), `T08` (Denial of service)
  * `P2: Faculty Web Portal` $\rightarrow$ `T09` (Spoofing), `T10` (Elevation of privilege)
  * `P3: Administrator Portal` $\rightarrow$ `T11` (Elevation of privilege), `T12` (Tampering), `T13` (Repudiation)
  * `P4: API Gateway` $\rightarrow$ `T14` (Denial of service), `T15` (Tampering), `T16` (Information disclosure)
  * `P5: Authentication and Authorization Service` $\rightarrow$ `T17` (Spoofing), `T18` (Elevation of privilege), `T19` (Denial of service)
  * `P6: Campus AI Assistant` $\rightarrow$ `T20` (Tampering), `T21` (Information disclosure), `T22` (Denial of service), `T23` (Elevation of privilege)
  * `P7: Student Analytics Service` $\rightarrow$ `T24` (Tampering), `T25` (Information disclosure)
  * `P8: Recommendation Service` $\rightarrow$ `T26` (Tampering), `T27` (Information disclosure)
* **Data Stores:**
  * `D1: Student Information Database` $\rightarrow$ `T28` (Information disclosure), `T29` (Tampering)
  * `D2: Academic and Attendance Database` $\rightarrow$ `T30` (Tampering), `T31` (Repudiation), `T32` (Information disclosure)
  * `D3: AI Conversation and Application Logs` $\rightarrow$ `T33` (Information disclosure), `T34` (Tampering)
  * `D4: Security Audit Logs` $\rightarrow$ `T35` (Tampering), `T36` (Repudiation), `T37` (Denial of service)
* **Data Flows:**
  * `F01` $\rightarrow$ `T38` (Spoofing)
  * `F04` $\rightarrow$ `T39` (Tampering)
  * `F07` $\rightarrow$ `T40` (Tampering)
  * `F09` $\rightarrow$ `T41` (Information disclosure)
  * `F10` $\rightarrow$ `T42` (Tampering)
  * `F11` $\rightarrow$ `T43` (Tampering)
  * `F12` $\rightarrow$ `T44` (Information disclosure)
  * `F13` $\rightarrow$ `T45` (Tampering)
  * `F14` $\rightarrow$ `T46` (Information disclosure)
  * `F15` $\rightarrow$ `T47` (Information disclosure)
  * `F16` $\rightarrow$ `T48` (Elevation of privilege)
  * `F18` $\rightarrow$ `T49` (Tampering)
  * `F19` $\rightarrow$ `T50` (Repudiation)

---

## 5. Threat Dragon JSON File Structure

The generated Threat Dragon model file follows the official OWASP Threat Dragon format:

```json
{
  "summary": {
    "title": "STRIDE-Based Threat Modelling of an AI Campus System",
    "owner": "Data Security and Privacy Academic Micro-Project",
    "description": "..."
  },
  "detail": {
    "contributors": [
      { "name": "Cybersecurity Threat-Modeling Student Engineer" }
    ],
    "diagrams": [
      {
        "title": "Level-1 Data Flow Diagram",
        "diagramType": "STRIDE",
        "id": 0,
        "size": { "width": 1600, "height": 1000 },
        "diagramJson": {
          "cells": [
            // tm.Boundary cells (TB1 - TB4)
            // tm.Actor cells (E1 - E4) with attached threats
            // tm.Process cells (P1 - P8) with attached threats
            // tm.Store cells (D1 - D4) with attached threats
            // tm.Flow cells (F01 - F22) with attached threats
          ]
        }
      }
    ]
  }
}
```

### Threat Object Schema Inside Each Cell
Each threat inside `cells[i].threats` is structured with:
* `title`: Formatted as `"[Threat ID]: [Threat Description]"` (e.g., `"T01: Student Credential Stuffing and Automated Impersonation"`).
* `type`: Standard STRIDE category (`Spoofing`, `Tampering`, `Repudiation`, `Information disclosure`, `Denial of service`, `Elevation of privilege`).
* `status`: Set to `"Open"` (or `"Mitigated"`).
* `severity`: Qualitative severity matching risk score (`"Critical"`, `"High"`, `"Medium"`, `"Low"`).
* `description`: Detailed text containing the full Attack Scenario, Risk Score formula ($\mathcal{L} \times \mathcal{I}$), Likelihood, Impact, Priority, and Rationale.
* `mitigation`: The specific Recommended Architectural Control and Control Type (Preventive, Detective, Corrective).
* `modelType`: `"STRIDE"`.
* `threatId`: Unique RFC 4122 UUID.

---

## 6. How to Open and Import the Model in OWASP Threat Dragon

### 6.1 Using OWASP Threat Dragon Desktop Application
1. **Download & Install:** Ensure OWASP Threat Dragon (v1.x or v2.x) is installed from the official repository or release page.
2. **Launch Application:** Open the Threat Dragon desktop client.
3. **Open Threat Model:**
   * On the dashboard, click **"Open an existing threat model"** (or press `Ctrl+O`).
   * In the file picker, navigate to:
     `c:\DSP lab Micro Project\STRIDE-Based Threat Modelling of an AI Campus System\model\threat_dragon_model.json`
   * Select the file and click **Open**.
4. **View Diagram and Threats:**
   * Click on the diagram titled **"Level-1 Data Flow Diagram"**.
   * The complete canvas will render with all 4 trust boundaries, 16 elements, and 22 flows.
   * Elements with open threats will display the Threat Dragon red threat badge.
   * Click on any element (e.g., `P4: API Gateway` or `P6: Campus AI Assistant`) to open the right-side properties panel and inspect its attached STRIDE threats, severity levels, and mitigation controls.

### 6.2 Using OWASP Threat Dragon Web Application
1. **Access Web App:** Navigate to [threatdragon.org](https://threatdragon.org/) in Google Chrome or any modern browser.
2. **Log In / Select Provider:** Choose your storage provider (GitHub, GitLab, Bitbucket, or Local File / Demo mode).
3. **Load Model:**
   * Select **"Open Model from File"** or import `threat_dragon_model.json`.
   * The web application will load the JSON structure, migrate coordinates if running v2.x, and display the diagram canvas.
4. **Export / Report Generation:**
   * Threat Dragon allows exporting the full model as a PDF or JSON report for submission alongside your academic viva.

---

## 7. Model Validation & Integrity Summary

* **DFD Fidelity:** 100% match with the authoritative DFD diagram.
* **Component Parity:** Exactly 4 Entities, 8 Processes, 4 Stores, 4 Boundaries, and 22 Flows.
* **Threat Coverage:** Exactly 50 threats across all 6 STRIDE categories.
* **Critical & High Priority Risks:** 25 threats (7 Critical, 18 High) with clear rationales and mitigations.
* **No Orphan Elements:** Every cell and flow connects properly with source and target references.
