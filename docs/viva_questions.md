# Comprehensive Viva Voce Questions and Answers

## Subject: Data Security and Privacy (Academic Micro-Project)
### Project Title: STRIDE-Based Threat Modelling of an AI Campus System

---

### Q01: What is threat modeling?
**Answer:** Threat modeling is a structured engineering procedure for identifying, quantifying, and addressing security risks and potential attack vectors in a software system or network architecture before deployment. It involves decomposing the system, pinpointing potential security flaws, understanding adversarial objectives, and prioritizing countermeasures (controls) to mitigate vulnerabilities in the design phase.

### Q02: What is STRIDE?
**Answer:** STRIDE is a mnemonic threat classification framework developed by Loren Kohnfelder and Praerit Garg at Microsoft. It categorizes cybersecurity threats into six discrete domains:
1. **S**poofing (authenticity violations)
2. **T**ampering (integrity violations)
3. **R**epudiation (non-repudiability/accountability violations)
4. **I**nformation Disclosure (confidentiality violations)
5. **D**enial of Service (availability violations)
6. **E**levation of Privilege (authorization violations).

### Q03: Why choose STRIDE for this campus system over other frameworks?
**Answer:** STRIDE was selected because it provides systematic, exhaustive, and element-by-element mapping directly to Data Flow Diagrams (DFDs). Unlike organizational frameworks like NIST CSF or vulnerability taxonomies like MITRE ATT&CK, STRIDE is an architectural threat-modeling methodology that evaluates every entity, process, data store, and flow against software-centric failure modes.

### Q04: What is Spoofing? Give an example from this project.
**Answer:** Spoofing is the act of falsely assuming the identity of a legitimate user, entity, or system component to gain unauthorized access. In this project, threat `T01` is an example where an adversary uses credential stuffing to masquerade as an authenticated student (`E1`) on the Student Mobile/Web Application (`P1`).

### Q05: What is Tampering? Give an example from this project.
**Answer:** Tampering involves the unauthorized, malicious alteration or modification of data in transit or data at rest, as well as altering system configurations. In this project, threat `T30` represents tampering where an adversary executes unauthorized SQL updates to alter official student GPA or attendance marks in the Academic and Attendance Database (`D2`).

### Q06: What is Repudiation? Give an example from this project.
**Answer:** Repudiation occurs when an individual or system performs an action and subsequently denies having performed it, with the system lacking sufficient forensic evidence or audit telemetry to prove accountability. In this project, threat `T31` occurs when a faculty member disavows submitting an erroneous attendance roster, claiming database corruption due to the absence of cryptographic digital signatures.

### Q07: What is Information Disclosure? Give an example from this project.
**Answer:** Information Disclosure is the unauthorized release, exposure, or leakage of confidential information to parties who do not possess authorized clearance. In this project, threat `T44` represents an information disclosure threat where the Campus AI Assistant transmits raw student PII (names, registration numbers, GPAs) across flow `F12` to the third-party External Cloud AI/LLM Service (`E4`).

### Q08: What is Denial of Service (DoS)? Give an example from this project.
**Answer:** Denial of Service is an attack aimed at exhausting computational resources (CPU, RAM, network bandwidth, connection pools) to render a system or service inaccessible to legitimate users. In this project, threat `T14` models volumetric HTTPS floods targeting the API Gateway (`P4`), which cripples ingress connectivity for the entire campus ecosystem.

### Q09: What is Elevation of Privilege? Give an example from this project.
**Answer:** Elevation of Privilege occurs when an adversary with limited or standard access exploits a flaw to gain higher-level permissions, administrative control, or unauthorized role capabilities. In this project, threat `T10` models an authenticated faculty user on `P2` manipulating HTTP parameters or headers to access restricted administrative user management endpoints.

### Q10: What is a Data Flow Diagram (DFD) and why is it not a flowchart?
**Answer:** A Data Flow Diagram (DFD) is a graphical representation of the logical movement of data through an information system. It depicts how data enters the system from external entities, how it is transformed by processes, where it is persisted in data stores, and where it crosses trust boundaries. A DFD is **not** a flowchart: flowcharts show procedural execution order, conditional branching, and control loops, whereas DFDs strictly trace the movement and transformation of information without implying temporal sequence.

### Q11: What is a Trust Boundary?
**Answer:** A Trust Boundary is a conceptual or physical perimeter on a DFD that separates areas where different levels of trust, security privileges, or administrative ownership apply. Any data flow crossing a trust boundary undergoes an increase or decrease in trust, making it a critical interception, validation, and attack surface point (e.g., untrusted public internet `TB1` entering campus private application zone `TB2`).

### Q12: Why is the public internet treated as an untrusted boundary?
**Answer:** The public internet is an open, decentralized network spanning unmanaged routers, third-party ISPs, public Wi-Fi access points, and cellular towers. It is subject to passive wiretapping, active packet injection, man-in-the-middle (MitM) interception, DNS spoofing, and BGP route hijacking. No assumption of confidentiality or integrity can be made without end-to-end transport layer cryptographic protocols (TLS 1.3).

### Q13: Why is synthetic data used instead of real student data?
**Answer:** Using synthetic data upholds ethical, legal, and regulatory compliance standards (such as India's Digital Personal Data Protection Act, EU GDPR, and US FERPA). Real student records contain sensitive PII, contact info, and confidential academic evaluations. Employing a synthetic architectural model and synthetic data sets eliminates privacy risks, confidentiality leaks, and compliance liability during academic research.

### Q14: What is the purpose of OWASP Threat Dragon in this project?
**Answer:** OWASP Threat Dragon is an open-source, visual threat-modeling tool maintained by the Open Worldwide Application Security Project. It enables threat modelers to construct system DFDs, define component types, and automatically generate STRIDE threat entries linked directly to specific diagram elements, facilitating organized threat tracking and reporting.

### Q15: Why use Draw.io for threat modeling diagrams?
**Answer:** Draw.io (diagrams.net) is an open-source diagramming tool that provides granular control over visual presentation, layout precision, color coding, and layered formatting. It allows modelers to create publication-quality DFDs with explicit trust boundary dashed containers, distinct element shapes, and orthogonal non-overlapping connector routing that enhances diagram clarity.

### Q16: What is the technical difference between a Threat and a Vulnerability?
**Answer:** 
* A **Threat** is a potential event or actor with the capability to adversely impact an asset by exploiting a weakness (e.g., "An adversary attempts to forge session tokens").
* A **Vulnerability** is an inherent weakness, flaw, or bug in software design, configuration, or implementation that makes it susceptible to exploitation by a threat (e.g., "The authentication service accepts JWTs with `alg: none`"). A threat cannot cause harm without an exploitable vulnerability.

### Q17: What is the technical difference between a Threat and a Risk?
**Answer:**
* A **Threat** represents a theoretical failure mode or potential attack scenario.
* A **Risk** is the quantified expectation of loss resulting from that threat being realized, calculated as the mathematical product of the probability (Likelihood) of exploitation and the magnitude of damage (Impact): $\text{Risk} = \text{Likelihood} \times \text{Impact}$.

### Q18: What is a Security Control? Name the three primary control types.
**Answer:** A Security Control is an administrative, technical, or physical safeguard or countermeasure deployed to avoid, detect, counter, or minimize security risks. The three operational classes are:
1. **Preventive Controls:** Prevent security incidents from occurring (e.g., TLS encryption, input sanitization, MFA).
2. **Detective Controls:** Identify and alert on security violations during or after execution (e.g., audit logging, IDS/IPS, SIEM).
3. **Corrective Controls:** Mitigate damage, restore operations, or recover system state post-breach (e.g., database backups, disaster recovery, incident response plans).

### Q19: What is the Principle of Least Privilege?
**Answer:** The Principle of Least Privilege (PoLP) is a foundational security architecture concept dictating that every module, user, or service account must be granted only the minimum necessary system access, privileges, and data exposure required to perform its legitimate operational function, and no more.

### Q20: What is Role-Based Access Control (RBAC)?
**Answer:** Role-Based Access Control (RBAC) is an authorization paradigm where system access permissions are assigned to specific predefined roles (such as `Student`, `Faculty`, `Administrator`), and users are assigned to those roles based on organizational responsibilities. Server-side middleware verifies that the user's validated role possesses explicit permission before executing any protected route or database operation.

### Q21: Why is it critical to protect AI prompts and contextual buffers?
**Answer:** In conversational and RAG-augmented AI systems, prompts act as the execution instructions and data payload combined. If prompt input is not sanitized, adversaries can execute **Prompt Injection** attacks that override safety guardrails, hijack bot personas, or force the model to dump internal system prompts. Furthermore, if student queries contain unredacted personal details, logging or transmitting them to third parties breaches student data privacy.

### Q22: Why is the External Cloud AI/LLM communication channel (Flows F12/F13) a critical risk vector?
**Answer:** Flows `F12` and `F13` cross `TB4 - External AI Service Boundary` into a commercial third-party cloud environment outside institutional campus control. Risks include:
1. Transmitting unredacted student PII/grades across public WAN.
2. Third-party cloud providers caching prompts or ingesting student data into training corpora.
3. Ingesting malicious, unescaped AI completions (`F13`) that may carry Cross-Site Scripting (XSS) or prompt exploits into client browsers.

### Q23: What is the single highest-risk area identified in this synthetic campus architecture?
**Answer:** The highest-risk intersection is the **API Gateway (`P4`) and the Public Ingress boundary (`TB1`)**, specifically regarding volumetric DDoS (`T14`, Critical-9) and Credential Stuffing (`T01`, Critical-9). Because the API Gateway is the central ingress choke-point through which all student, faculty, and administrative traffic flows, an ingress compromise or gateway crash results in total systemic paralysis across all university digital services. In the data tier, **Academic and Attendance Database (`D2`)** tampering (`T30`, High-6) poses the greatest institutional integrity risk.

### Q24: What are the primary limitations of this academic micro-project?
**Answer:**
1. **Analytical Modeling Only:** The project models threats analytically; it does not deploy live software or execute active penetration testing exploits.
2. **Qualitative Risk Scoring:** The 3×3 matrix relies on qualitative assessment criteria rather than empirical incident telemetry.
3. **Synthetic Abstraction:** Real campus networks feature legacy integrations, disparate department ERPs, and IoT device networks not modeled in this clean microservice architecture.

### Q25: What architectural improvements would you recommend before deploying this system to a real university?
**Answer:**
1. **Zero-Trust Network Architecture (ZTNA):** Enforce mutual TLS (mTLS) with cryptographically authenticated service meshes (e.g., Istio) between all internal microservices.
2. **Dedicated On-Premise / Sovereign AI LLM Inference:** Host language models within the private campus data center rather than using commercial public cloud APIs to guarantee zero third-party data egress.
3. **Hardware-Backed Security:** Integrate Hardware Security Modules (HSM) for asymmetric token signing key generation and storage.
4. **Automated Data Loss Prevention (DLP) & Named Entity Recognition (NER):** Deploy real-time PII masking proxy pipelines in front of all conversational AI streams.
5. **SIEM / SOC Integration:** Implement centralized security event correlation with automated SOAR playbooks for rapid threat containment.

### Q26: Can a process communicate directly with another process across trust boundaries?
**Answer:** Yes, processes frequently communicate across trust boundaries (for example, client application `P1` in `TB1` communicates with `P4` in `TB2` via flow `F04`). However, because they cross a trust boundary, all transmitted payloads must be cryptographically protected (TLS 1.3), authenticated, and rigorously validated on the receiving side before processing.

### Q27: How does this project address non-repudiation for faculty grade submissions?
**Answer:** Through threat `T31` and flow `F19`, the threat model recommends requiring asymmetric cryptographic digital signatures for all submitted grade and attendance rosters, combined with recording the submitting faculty's authenticated ID, timestamp, and client IP address in an append-only, tamper-resistant Security Audit Log (`D4`).

### Q28: How does STRIDE help in secure software development lifecycle (SSDLC)?
**Answer:** STRIDE shifts security consideration to the early architectural design phase ("Shift-Left" security). By discovering structural vulnerabilities (e.g., unauthenticated endpoints, missing encryption, unvalidated inputs) during design rather than post-production, organizations save significant remediation costs and prevent systemic security debt.
