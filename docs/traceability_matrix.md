# Threat Model Traceability Matrix

## 1. Traceability Purpose and Architecture Alignment

The primary goal of the **Threat Model Traceability Matrix** is to guarantee complete bidirectional traceability across all tiers of the synthetic campus threat model:

$$\text{DFD Component / Data Flow} \iff \text{Threat Identifier (T01–T50)} \iff \text{STRIDE Category} \iff \text{Recommended Security Control}$$

This ensures that:
1. **No Orphan Threats:** Every documented threat directly originates from an identifiable element or boundary crossing in the Level-1 Data Flow Diagram.
2. **No Unmitigated Components:** Every process, external entity, persistent data store, and sensitive communication flow has evaluated failure modes and concrete mitigating controls.
3. **Audit Compliance:** Academic evaluators can verify how each specific security requirement traces back to the underlying synthetic campus topology.

---

## 2. Complete Bidirectional Traceability Matrix

| DFD Element ID | Element Name | Element Type | Threat ID | STRIDE Category | Risk Level | Primary Recommended Security Control |
| :---: | :--- | :---: | :---: | :---: | :---: | :--- |
| **E1** | `Student` | External Entity | **T01** | Spoofing | **Critical** (9) | Multi-Factor Authentication (MFA), IP rate limiting, compromised credential detection. |
| **E2** | `Faculty` | External Entity | **T02** | Spoofing | **High** (6) | FIDO2/WebAuthn hardware tokens, adaptive MFA, geo-velocity anomalous login detection. |
| **E3** | `Campus Administrator` | External Entity | **T03** | Spoofing | **Medium** (3) | Dedicated hardened jump hosts, hardware U2F tokens, source IP whitelisting, ephemeral sessions. |
| **E4** | `External Cloud AI/LLM Service` | External Service | **T04** | Information Disclosure | **Critical** (9) | Enterprise Zero-Data-Retention (ZDR) agreements, pre-transit PII pseudonymization/tokenization. |
| **E4** | `External Cloud AI/LLM Service` | External Service | **T05** | Tampering | **Low** (2) | Response schema validation, client-side CSP, link domain whitelisting, output sanitization. |
| **P1** | `Student Mobile/Web Application` | Process (Client) | **T06** | Tampering | **Medium** (3) | Authoritative server-side parameter validation, signed state tokens, stateless client verification. |
| **P1** | `Student Mobile/Web Application` | Process (Client) | **T07** | Information Disclosure | **Medium** (4) | OS hardware-backed cryptographic keystores (KeyStore/Keychain), SQLCipher encrypted mobile DB. |
| **P1** | `Student Mobile/Web Application` | Process (Client) | **T08** | Denial of Service | **Low** (1) | Defensive JSON schema deserialization, global exception handlers, automated crash reporting. |
| **P2** | `Faculty Web Portal` | Process (Client) | **T09** | Spoofing | **High** (6) | Context-aware output encoding, strict CSP, HttpOnly/Secure/SameSite=Strict cookie flags. |
| **P2** | `Faculty Web Portal` | Process (Client) | **T10** | Elevation of Privilege | **High** (6) | Server-side Role-Based Access Control (RBAC) middleware verifying cryptographic token claims. |
| **P3** | `Administrator Portal` | Process (Client) | **T11** | Elevation of Privilege | **Medium** (3) | Principle of Dual Control (Four-Eyes Principle) for sensitive tasks, JIT privilege elevation (PAM). |
| **P3** | `Administrator Portal` | Process (Client) | **T12** | Tampering | **Medium** (3) | Cryptographically signed immutable configuration repositories, multi-party change approvals. |
| **P3** | `Administrator Portal` | Process (Client) | **T13** | Repudiation | **High** (6) | Prohibition of shared accounts, named individual accounts tied to hardware MFA, audit logging. |
| **P4** | `API Gateway` | Process (Core) | **T14** | Denial of Service | **Critical** (9) | Cloud DDoS mitigation shields, edge rate limiting, distributed WAF, adaptive IP throttling. |
| **P4** | `API Gateway` | Process (Core) | **T15** | Tampering | **High** (6) | Strict HTTP/2 enforcement, RFC-compliant parsers, stripping/overwriting untrusted headers. |
| **P4** | `API Gateway` | Process (Core) | **T16** | Information Disclosure | **Medium** (3) | Centralized custom error handlers returning generic RFC 7807 responses; suppress debug traces. |
| **P5** | `Authentication and Authorization Service` | Process (Core) | **T17** | Spoofing | **High** (6) | Asymmetric RS256/ES256 token signing with private keys in HSM; strict algorithm whitelisting. |
| **P5** | `Authentication and Authorization Service` | Process (Core) | **T18** | Elevation of Privilege | **High** (6) | Server-side authorization verifying authenticated identity matches requested resource ID context. |
| **P5** | `Authentication and Authorization Service` | Process (Core) | **T19** | Denial of Service | **High** (6) | IP rate limiting on `/auth/login`, CAPTCHA challenges, maximum password length truncation. |
| **P6** | `Campus AI Assistant` | Process (Core) | **T20** | Tampering | **Critical** (9) | Layered AI guardrail frameworks (NeMo Guardrails), prompt isolation, input regex filtering. |
| **P6** | `Campus AI Assistant` | Process (Core) | **T21** | Information Disclosure | **Critical** (9) | Identity-aware RAG document retrieval filtering, user-scoped embeddings, egress DLP redaction. |
| **P6** | `Campus AI Assistant` | Process (Core) | **T22** | Denial of Service | **High** (6) | Per-user daily token budgets, sliding-window prompt rate limiting, max prompt length limits. |
| **P6** | `Campus AI Assistant` | Process (Core) | **T23** | Elevation of Privilege | **Medium** (3) | Restrict AI assistant tools to strictly read-only operations; human-in-the-loop approval. |
| **P7** | `Student Analytics Service` | Process (Core) | **T24** | Tampering | **Medium** (4) | Cryptographic signature validation on telemetry events, statistical anomaly detection pipelines. |
| **P7** | `Student Analytics Service` | Process (Core) | **T25** | Information Disclosure | **Low** (2) | Differential privacy mechanisms, suppression of cohort query outputs where group size $k < 10$. |
| **P8** | `Recommendation Service` | Process (Core) | **T26** | Tampering | **Low** (2) | Verified course completion requirements for ratings, account age weighting, outlier detection. |
| **P8** | `Recommendation Service` | Process (Core) | **T27** | Information Disclosure | **Medium** (3) | Client-side privacy controls, prevent public recommendation broadcast, suppress reasoning metadata. |
| **D1** | `Student Information Database` | Data Store | **T28** | Information Disclosure | **Medium** (3) | Network Security Groups (NSGs), binding database to private VPC subnets, mandatory mTLS. |
| **D1** | `Student Information Database` | Data Store | **T29** | Tampering | **Medium** (3) | Least-privilege DB user permissions restricting service accounts to specific tables, DAM auditing. |
| **D2** | `Academic and Attendance Database` | Data Store | **T30** | Tampering | **High** (6) | Parameterized queries, database row-level security (RLS), cryptographic ledger hash-chaining. |
| **D2** | `Academic and Attendance Database` | Data Store | **T31** | Repudiation | **Medium** (4) | Cryptographic digital signatures on attendance submissions, immutable audit timestamps. |
| **D2** | `Academic and Attendance Database` | Data Store | **T32** | Information Disclosure | **Medium** (3) | AES-256 Transparent Data Encryption (TDE) on backups, strict IAM backup bucket policies. |
| **D3** | `AI Conversation and Application Logs` | Data Store | **T33** | Information Disclosure | **High** (6) | Automatic PII redaction before log insertion, column-level AES-256 encryption, access limits. |
| **D3** | `AI Conversation and Application Logs` | Data Store | **T34** | Tampering | **Low** (2) | Append-only database user permissions, streaming logs to external write-once (WORM) storage. |
| **D4** | `Security Audit Logs` | Data Store | **T35** | Tampering | **Medium** (3) | Write-Once-Read-Many (WORM) storage, cryptographic log chaining, external isolated SIEM export. |
| **D4** | `Security Audit Logs` | Data Store | **T36** | Repudiation | **High** (6) | Comprehensive audit policy capturing who, what, when, where; structured logging into SIEM. |
| **D4** | `Security Audit Logs` | Data Store | **T37** | Denial of Service | **Medium** (4) | Auto-scaling cloud log volumes, automated log rotation and cold archiving, rate limiting. |
| **F01** | `Student -> Student Mobile/Web App` | Data Flow | **T38** | Spoofing | **High** (6) | Mandatory TOTP / mobile push MFA, biometric app unlock, masked credential entry fields. |
| **F04** | `Student Mobile/Web App -> API Gateway` | Data Flow | **T39** | Tampering | **High** (6) | TLS 1.3 mandatory ciphers, HSTS with preloading, SSL/TLS certificate pinning in mobile apps. |
| **F07** | `API Gateway -> Auth Service` | Data Flow | **T40** | Tampering | **Medium** (3) | Mutual TLS (mTLS) with ephemeral keys, single-use cryptographically signed nonces, TTL <30s. |
| **F09** | `API Gateway -> Student Info DB` | Data Flow | **T41** | Information Disclosure | **High** (6) | Parameterized SQL queries and ORM frameworks exclusively; strict input validation schemas. |
| **F10** | `API Gateway -> Academic & Attendance DB` | Data Flow | **T42** | Tampering | **High** (6) | Parameterized queries, prepared statements, stored procedures, least-privilege DB user permissions. |
| **F11** | `API Gateway -> Campus AI Assistant` | Data Flow | **T43** | Tampering | **Medium** (4) | Gateway header sanitization, stripping untrusted client headers, strongly typed JSON RPC schemas. |
| **F12** | `Campus AI Assistant -> External Cloud AI` | Data Flow | **T44** | Information Disclosure | **Critical** (9) | Automated client-side PII scrubbing pipeline (NER), student tokenization, egress DLP filtering. |
| **F13** | `External Cloud AI -> Campus AI Assistant` | Data Flow | **T45** | Tampering | **High** (6) | Treat LLM output as untrusted user input; strict HTML escaping, DOMPurify Markdown sanitization. |
| **F14** | `Campus AI Assistant -> AI Conversation Logs` | Data Flow | **T46** | Information Disclosure | **Critical** (9) | Automated log sanitization filters to mask sensitive fields, field-level encryption on query columns. |
| **F15** | `API Gateway -> Student Analytics Service` | Data Flow | **T47** | Information Disclosure | **Medium** (4) | Principle of Data Minimization; API response projection filtering to forward minimal attributes. |
| **F16** | `Student Analytics Service -> Student Info DB` | Data Flow | **T48** | Elevation of Privilege | **Medium** (3) | Dedicated least-privilege DB accounts with read-only permissions restricted to analytics views. |
| **F18** | `Recommendation Service -> API Gateway` | Data Flow | **T49** | Tampering | **Medium** (3) | Standardize on safe declarative serialization (JSON); avoid native language object deserialization. |
| **F19** | `API Gateway -> Security Audit Logs` | Data Flow | **T50** | Repudiation | **High** (6) | Resilient asynchronous messaging brokers (Kafka/RabbitMQ) with disk spooling and delivery guarantees. |

---

## 3. Element Coverage Verification

* **External Entities Covered:** 4 of 4 (`E1`, `E2`, `E3`, `E4`) $\rightarrow$ **100%**
* **Processes Covered:** 8 of 8 (`P1`, `P2`, `P3`, `P4`, `P5`, `P6`, `P7`, `P8`) $\rightarrow$ **100%**
* **Data Stores Covered:** 4 of 4 (`D1`, `D2`, `D3`, `D4`) $\rightarrow$ **100%**
* **Key Cross-Boundary Data Flows Explicitly Analyzed:** 13 key flows (`F01`, `F04`, `F07`, `F09`, `F10`, `F11`, `F12`, `F13`, `F14`, `F15`, `F16`, `F18`, `F19`) $\rightarrow$ **100% of high-risk boundary traversals**
* **All 50 Threats Traced to Mitigating Controls:** $\rightarrow$ **100%**
