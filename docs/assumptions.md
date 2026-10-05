# Threat Model Assumptions

## 1. Context and Objective

This document defines the baseline assumptions governing the threat model for the academic micro-project titled **"STRIDE-Based Threat Modelling of an AI Campus System"**. Threat modeling is fundamentally an abstraction technique; establishing explicit, unambiguous assumptions ensures that boundaries are clearly defined, threat assessments remain grounded, and evaluations avoid unwarranted architectural ambiguity.

---

## 2. Core Architectural and Operational Assumptions

### Assumption 1: Synthetic Campus Architecture
The campus architecture is entirely synthetic and conceptual. It represents a typical modern AI-augmented higher education university ecosystem designed specifically for the Data Security and Privacy course. It does not represent, model, or reflect the proprietary or physical infrastructure of Dayananda Sagar College of Engineering (DSCE) or any specific educational institution.

### Assumption 2: Web and Mobile Access Vectors
All end users—Students (`E1`), Faculty (`E2`), and Campus Administrators (`E3`)—interact with campus systems exclusively through client-tier applications: the Student Mobile/Web Application (`P1`), the Faculty Web Portal (`P2`), and the Administrator Portal (`P3`). Users do not possess direct network access (e.g., direct socket or raw database connections) to backend database engines or internal microservices.

### Assumption 3: Public Internet as an Untrusted Network Environment
The external network zone hosting end-user client devices (`TB1 - Public/External User Zone`) is considered hostile and untrusted. Intermediate network nodes, cellular carriers, open Wi-Fi access points, and routing hops are assumed to be susceptible to packet sniffing, eavesdropping, DNS spoofing, and man-in-the-middle (MitM) inspection.

### Assumption 4: Backend Services Trusted Only Within Defined Security Boundaries
Backend services (`P4`–`P8`) reside within `TB2 - Campus Application Zone` and are considered trusted only to the extent of their isolated microservice responsibilities. Zero-trust principles apply across boundaries: possession of network connectivity within the campus private subnet does not confer automatic authorization to query sensitive databases or invoke administrative routines.

### Assumption 5: Databases Contain Highly Sensitive Synthetic Data Assets
The synthetic data stores house assets that, in a real institution, would be governed by data privacy laws (such as India's Digital Personal Data Protection Act [DPDPA], GDPR, and FERPA):
* `D1 - Student Information Database`: PII, contact records, enrollment status, biographical data.
* `D2 - Academic and Attendance Database`: Course registrations, GPA/grades, examination marks, official attendance logs.
* `D3 - AI Conversation and Application Logs`: Student prompt histories, academic queries, mental health or counseling inquiries, and bot responses.
* `D4 - Security Audit Logs`: Cryptographic nonces, authentication attempts, privilege escalation events, and administrative access logs.

### Assumption 6: Centralized Authentication Service
Authentication is decoupled from application business logic and managed centrally by the dedicated Authentication and Authorization Service (`P5`). All user authentication flows pass through the API Gateway (`P4`) and are verified by `P5`, which issues cryptographically signed session tokens (e.g., JSON Web Tokens or secure opaque session cookies).

### Assumption 7: Server-Side Authorization Enforcement
Authorization is strictly enforced by backend services (`P4`, `P5`, and downstream processing engines) using server-side validation. Client-side state, user agent flags, and mobile app declarations are treated as completely untrusted and non-authoritative for access control decisions.

### Assumption 8: Mandatory Transport Layer Encryption Across Untrusted Boundaries
All data flows traversing untrusted boundaries (`TB1`, `TB4`) and internal cross-zone boundaries (`TB2` to `TB3`) are assumed to require modern cryptographic protocols (TLS 1.3 with forward secrecy) to mitigate passive eavesdropping and active packet tampering.

### Assumption 9: External Cloud AI / LLM Service Resides Outside Campus Trust Boundary
The External Cloud AI/LLM Service (`E4`) is operated by a commercial third-party cloud provider outside the campus institutional perimeter (`TB4 - External AI Service Boundary`). It operates under a shared responsibility model where data transmitted across flow `F12` exits institutional control, necessitating pre-transit sanitization, anonymization, and prompt isolation.

### Assumption 10: Security Audit Logs Support Non-Repudiation and Forensic Accountability
The Security Audit Logs (`D4`) are assumed to function as an immutable, append-only security telemetry repository. Their architectural objective is to establish tamper-evident records of all high-privilege activities, failed logins, permission escalations, and record updates to prevent repudiation.

### Assumption 11: Absolute Exclusion of Real Student Data
Under no circumstances does this academic threat model employ real student, faculty, or institutional records. All data items, user identities, credentials, tokens, and scenario logs are purely synthetic abstractions designed to evaluate security failure modes.

### Assumption 12: Non-Exploitative Threat Modeling Framework
This academic project is strictly an analytical threat-modeling exercise. No actual cyberattacks, vulnerability exploitations, network stress tests, denial-of-service simulations, or intrusive penetration testing activities were conducted against any live computer system, university network, or production cloud environment.

---

## 3. Summary Mapping Table

| Assumption ID | Dimension | System Components Affected | Modeling Impact |
| :--- | :--- | :--- | :--- |
| **A-01** | Architecture | System-wide | Establishes synthetic academic foundation |
| **A-02** | Client Ingress | `E1`–`E3`, `P1`–`P3` | Restricts threat surface to application APIs |
| **A-03** | Network Trust | `TB1`, `F01`–`F06` | Mandates identification of MitM & Spoofing threats |
| **A-04** | Zone Isolation | `TB2`, `P4`–`P8` | Prevents assumptions of implicit internal trust |
| **A-05** | Asset Value | `D1`–`D4` | Elevates impact severity ($\mathcal{I} = 3$) for data breach |
| **A-06** | Identity Mgmt | `P5`, `F07`, `F08` | Focuses authentication threats on single service |
| **A-07** | Authorization | `P4`, `P5`, `P7`, `P8` | Highlights Elevation of Privilege / IDOR vectors |
| **A-08** | Cryptography | `F01`–`F06`, `F12`, `F13` | Recommends TLS 1.3 and cryptographic hashing |
| **A-09** | Cloud AI Trust | `E4`, `TB4`, `F12`, `F13` | Focuses on prompt injection & PII leakage |
| **A-10** | Accountability | `D4`, `F19` | Underpins Repudiation countermeasures |
| **A-11** | Privacy / Ethics | System-wide | Eliminates institutional privacy risks |
| **A-12** | Methodology | Project Scope | Enforces STRIDE analytical modeling only |
