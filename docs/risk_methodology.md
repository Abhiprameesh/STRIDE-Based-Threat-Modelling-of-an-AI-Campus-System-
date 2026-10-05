# Risk Assessment Methodology

## 1. Overview and Purpose

This document outlines the formal qualitative risk assessment methodology employed for the academic micro-project: **"STRIDE-Based Threat Modelling of an AI Campus System"**. The objective of this framework is to establish an objective, repeatable, and mathematically consistent approach to evaluating cybersecurity threats identified across the synthetic university campus architecture.

In threat modeling, risk evaluation bridges the gap between raw threat identification (identifying *what can go wrong*) and security engineering (determining *what controls must be prioritized*). This methodology translates qualitative severity and probability parameters into quantifiable threat tiers without requiring historical empirical incident frequencies, which are unavailable for synthetic conceptual architectures.

---

## 2. Mathematical Risk Formulation

The threat evaluation model employs a standardized two-dimensional risk matrix based on the classical cybersecurity formulation:

$$\text{Risk Score} = \text{Likelihood} \times \text{Impact}$$

Where:
* **Likelihood ($\mathcal{L}$)** represents the probability or ease with which an adversary can discover, weaponize, and execute an attack scenario against the campus asset or communication channel.
* **Impact ($\mathcal{I}$)** represents the degree of degradation or damage to Confidentiality, Integrity, Availability, Privacy, and Institutional Operations should the threat be realized.

Both dimensions are discretized into three ordinal values $\{1, 2, 3\}$. The resulting scalar product yields a discrete Risk Score $\mathcal{S} \in \{1, 2, 3, 4, 6, 9\}$.

---

## 3. Likelihood Evaluation Criteria

Likelihood evaluates the threat agent's technical capability, required access level, attack surface visibility, and the presence or absence of baseline safeguards.

| Likelihood Rating | Numeric Value | Description & Operational Criteria |
| :--- | :---: | :--- |
| **Low** | **1** | **Difficult / Unlikely to Occur:**<br>• Requires deep internal network access, physical proximity, or specialized high-privilege access.<br>• Attack complexity is high; requires sophisticated exploitation techniques or chaining of multiple obscure vulnerabilities.<br>• Asset is shielded behind multiple layers of defence (e.g., deep inside `TB3 - Protected Data Zone`). |
| **Medium** | **2** | **Moderately Feasible / Plausible:**<br>• Attack vector requires standard network access (e.g., authenticated campus user or accessible public endpoint).<br>• Moderate technical skill required; publicly documented attack patterns or standard penetration testing tools can be leveraged.<br>• Asset resides in `TB2 - Campus Application Zone` or interfaces directly with user inputs. |
| **High** | **3** | **Readily Feasible / Highly Likely:**<br>• Exploitable by unauthenticated external threat actors over the public internet (`TB1 - Public/External User Zone`).<br>• Trivial execution (e.g., credential stuffing, unvalidated query parameters, standard prompt manipulation).<br>• Requires no specialized privileges; automated scanning tools can discover and exploit the weakness. |

---

## 4. Impact Evaluation Criteria

Impact assesses the prospective consequences of a successful security compromise across the synthetic campus ecosystem, focusing on student privacy, grading integrity, institutional reputation, and service continuity.

| Impact Rating | Numeric Value | Description & Consequence Thresholds |
| :--- | :---: | :--- |
| **Low** | **1** | **Minor / Localized Impact:**<br>• Minimal or negligible compromise of non-sensitive metadata.<br>• Transient service disruption affecting a single user without systemic outage.<br>• No exposure of personally identifiable information (PII) or regulatory non-compliance.<br>• Integrity compromise is easily detected and rolled back via standard logs. |
| **Medium** | **2** | **Significant / Moderate Impact:**<br>• Exposure of internal academic metadata or restricted departmental schedules.<br>• Temporary degradation of core campus applications (e.g., AI assistant query latency or gateway throttling).<br>• Unauthorized modification of non-critical operational records (e.g., course feedback or recommendation preferences).<br>• Localized repudiation risks that require administrative log forensic intervention. |
| **High** | **3** | **Catastrophic / Severe Impact:**<br>• Unauthorized mass alteration or falsification of official academic transcripts, degrees, or attendance records (`D2`).<br>• Mass exfiltration of sensitive student PII, financial details, or confidential counseling dialogues (`D1`, `D3`).<br>• Total denial of service for critical examination, enrollment, or administrative portals (`P4`, `P5`).<br>• Complete compromise of administrative credentials or persistent elevation of privilege (`P3`, `E3`).<br>• Severe legal/compliance penalties under privacy regulations (e.g., DPDP Act, GDPR, FERPA analogues). |

---

## 5. Risk Matrix and Score Mapping

The Cartesian product of Likelihood $\{1, 2, 3\}$ and Impact $\{1, 2, 3\}$ produces the 3×3 Risk Assessment Matrix:

| Likelihood \ Impact | Low (1) | Medium (2) | High (3) |
| :--- | :---: | :---: | :---: |
| **High (3)** | **Medium (3)** | **High (6)** | **Critical (9)** |
| **Medium (2)** | **Low (2)** | **Medium (4)** | **High (6)** |
| **Low (1)** | **Low (1)** | **Low (2)** | **Medium (3)** |

### Risk Level Categorization Thresholds

| Computed Score ($\mathcal{S}$) | Risk Level | Description & Operational Response Directive |
| :---: | :---: | :--- |
| **9** | **Critical** | **Immediate Intervention Mandatory:** Poses severe existential risk to institutional data privacy, academic record integrity, or infrastructure control. Security controls must be implemented as non-negotiable architectural mandates prior to any operational deployment. |
| **6** | **High** | **High Priority Remediation:** Poses significant threat of unauthorized data exposure, privilege escalation, or systemic disruption. Compensating controls must be prioritized in the primary design iteration. |
| **3 – 4** | **Medium** | **Planned Architectural Mitigation:** Manageable threat with moderate operational consequence or moderate exploitation barrier. Addressed via standard secure coding best practices and layered defence-in-depth controls. |
| **1 – 2** | **Low** | **Routine Security Hygiene:** Minimal exposure with high barrier to entry or negligible impact. Managed via baseline configuration baselines and routine audit telemetry. |

---

## 6. Priority and Control Classification Schema

To ensure clear traceability from threat modeling to engineering implementation, each threat in the matrix is mapped to an implementation priority and a control classification.

### Priority Levels
* **P1 (Urgent / Immediate):** Assigned to all **Critical (Score 9)** threats and select high-exposure **High (Score 6)** threats affecting core authentication or database integrity.
* **P2 (High Priority):** Assigned to remaining **High (Score 6)** threats and critical-path **Medium (Score 4)** threats.
* **P3 (Medium Priority):** Assigned to standard **Medium (Score 3)** threats.
* **P4 (Low Priority):** Assigned to all **Low (Score 1–2)** threats.

### Control Types
* **Preventive:** Architectural boundaries, cryptographic protections, validation filters, and access controls that stop an exploit from occurring (e.g., TLS encryption, Mutual Authentication, RBAC, Parameterized Queries).
* **Detective:** Telemetry, audit mechanisms, anomaly detection systems, and logging monitors that identify and alert on security violations in real time or post-incident (e.g., SIEM ingestion, Audit Logs, Rate Limiting alerts).
* **Corrective / Compensating:** Mechanisms designed to limit the blast radius, restore operational integrity, or recover corrupted state post-compromise (e.g., Automated account lockout, Database point-in-time recovery, Backup rollbacks).

---

## 7. Application to the Synthetic AI Campus System

This qualitative framework is tailored specifically for the synthetic AI-enabled university campus architecture:
1. **Academic Records (`D2`) and Student Information (`D1`):** Modifications or breaches are categorized as High Impact ($\mathcal{I} = 3$) due to FERPA/DPDP academic integrity standards.
2. **External Cloud AI / LLM Communication (`F12`, `F13`, `E4`):** Data flows traversing `TB4` are subject to heightened Likelihood ratings ($\mathcal{L} = 3$) due to unconstrained prompt ingress and third-party cloud data transit.
3. **API Gateway (`P4`) and Authentication Service (`P5`):** Act as primary campus choke-points; threats targeting these components propagate across downstream services, yielding compound risk scores.
