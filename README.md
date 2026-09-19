# brightwave-mitre-attack-capstone-


# Operation Nightfall: Investigating a Simulated Intrusion and Building ATT&CK-Based Detection Coverage

**Name:** Abdullah Akanbi

**Project Type:** Individual Threat Analysis, SOC & Detection Engineering Capstone Project

**Organization:** BrightWave Logistics Ltd. *(Simulated Organization)*

---

**Project Title:** Operation Nightfall: Investigating a Simulated Intrusion and Building ATT&CK-Based Detection Coverage

---

## 2. Scenario and Analyst Role

### Scenario

BrightWave Logistics Ltd. is a simulated logistics and e-commerce support company with approximately 85 employees. Its environment includes Windows endpoints, Microsoft 365 and cloud identity services, a Windows file server, customer-facing applications, finance and invoicing systems, remote employees and contractors, VPN/remote access, endpoint security software, firewall infrastructure, and cloud sign-in logging.

A suspected coordinated intrusion was identified after employees reported that shared documents could no longer be opened and filenames had changed. Security monitoring also identified unusual authentication activity and abnormal outbound network traffic.

### Analyst Role

As the security analyst, my role is to:

* Analyze the provided evidence.
* Build a chronological incident timeline.
* Map observed activity to MITRE ATT&CK tactics and techniques.
* Identify gaps in detection coverage.
* Develop safe defensive detection use cases.
* Create threat-hunting hypotheses.
* Recommend containment and recovery actions.
* Present the investigation and findings in a professional format.

---

## 3. Objectives

The main objectives of this project are to:

1. Reconstruct the suspected intrusion from the available evidence.
2. Identify the likely attack path.
3. Map evidence to MITRE ATT&CK tactics and techniques.
4. Analyze all 14 tactics required by the course.
5. Develop an ATT&CK-based detection coverage matrix.
6. Create defensive detection use cases.
7. Develop threat-hunting hypotheses.
8. Recommend incident containment and recovery actions.
9. Identify major security and detection gaps.
10. Present practical recommendations for improving the organization's security posture.

---

## 4. ATT&CK Version / Date Consulted

**ATT&CK Version:** To be recorded after verification against the official MITRE ATT&CK website.

**Date Consulted:** __________________

This project records the ATT&CK version and consultation date because MITRE ATT&CK is continuously updated. Technique names, sub-techniques, and tactic relationships may change between versions.

---

## 5. Course 14-Tactic Alignment

This project follows the 14-tactic structure required by the course:

1. Reconnaissance
2. Resource Development
3. Initial Access
4. Execution
5. Persistence
6. Privilege Escalation
7. Defense Evasion
8. Credential Access
9. Discovery
10. Lateral Movement
11. Collection
12. Command and Control
13. Exfiltration
14. Impact

The analysis distinguishes between **observed evidence**, **reasonable inference**, and **unconfirmed hypotheses**.

---

## 6. Top Findings

The investigation identified activity consistent with a multi-stage intrusion involving:

* Targeted information gathering about employees and organizational roles.
* A suspicious external domain resembling the company's identity.
* An unusual finance-user authentication.
* Office-related scripting activity.
* Creation of a scheduled execution mechanism.
* Modification of a local security-protection setting.
* Suspicious privileged-account activity.
* Attempted access to stored credential material.
* Discovery of users, groups, systems, and network shares.
* Unusual remote access to a file server.
* Collection and staging of finance/customer data.
* Compression of staged data.
* Periodic outbound communications with a rare external domain.
* A large encrypted outbound transfer.
* Significant disruption to shared documents.

Detailed evidence and confidence levels are documented in the incident-analysis and attack-mapping directories.

---

## 7. Attack Path Summary

The working attack-path hypothesis is:

```text
Reconnaissance
      ↓
Resource Development
      ↓
Initial Access
      ↓
Execution
      ↓
Persistence
      ↓
Defense Evasion
      ↓
Credential Access
      ↓
Discovery
      ↓
Lateral Movement
      ↓
Collection
      ↓
Command and Control
      ↓
Exfiltration
      ↓
Impact
```

This represents the current analytical hypothesis rather than proof that every stage occurred exactly as shown.

Each stage will be supported by evidence references such as **E-01 through E-15**, with confidence levels and alternative explanations documented where appropriate.

---

## 8. Detection Coverage Summary

The project develops detection coverage for activities including:

* Unusual user and administrator authentication.
* Office applications launching scripting/interpreter processes.
* New scheduled persistence mechanisms.
* Security-tool configuration changes.
* Credential-access alerts.
* Abnormal discovery activity.
* Unexpected remote access to servers.
* Large-scale data staging.
* Archive/compression activity.
* Periodic connections to rare external domains.
* Large encrypted outbound transfers.
* Suspicious file modification or encryption activity.

Each detection use case documents:

* Data sources and relevant fields.
* Detection logic.
* Reason for detection.
* Possible false positives.
* Tuning recommendations.
* Severity.
* Recommended response.

---

## 9. Top Five Improvements

The following improvement areas will be prioritized after completing the detailed assessment:

1. **Strengthen identity security**

   * Improve MFA coverage and monitor unusual authentication activity.

2. **Improve endpoint detection**

   * Monitor suspicious scripting, persistence mechanisms, credential-access activity, and security-tool changes.

3. **Improve network monitoring**

   * Detect unusual remote access, rare external destinations, periodic connections, and abnormal outbound data transfers.

4. **Strengthen data protection and recovery**

   * Improve monitoring of sensitive data staging and validate backup and restoration procedures.

5. **Improve centralized detection and incident response**

   * Improve log collection, correlation, alerting, threat hunting, and incident-response procedures.

---

## 10. Repository Structure

```text
brightwave-mitre-attack-capstone/
│
├── README.md
│
├── incident-analysis/
│   ├── executive-summary.md
│   ├── evidence-timeline.csv
│   └── final-incident-conclusion.md
│
├── attack-mapping/
│   ├── attack-mapping.csv
│   ├── tactic-analysis.md
│   ├── attack-path.png
│   └── navigator-layer.json
│
├── detection-engineering/
│   ├── coverage-matrix.csv
│   ├── detections.md
│   └── data-sources.md
│
├── threat-hunting/
│   └── hunt-plan.md
│
├── response/
│   ├── containment-plan.md
│   └── recovery-priorities.md
│
├── sample-data/
│   └── synthetic-events.csv
│
├── report/
│   └── MITRE_ATTACK_Incident_Analysis_Abdullah_Akanbi.pdf
│
├── presentation/
│   └── MITRE_ATTACK_Presentation_Abdullah_Akanbi.pdf
│
└── references/
    └── sources.md
```

---

## 11. Safety Statement

This project is designed for **defensive cybersecurity education and authorized analysis**.

All investigation data is simulated, instructor-provided, or synthetically generated.

The project does not contain:

* Real credentials.
* Private employee or customer information.
* Malware.
* Ransomware.
* Credential-dumping tools or payloads.
* Exploit code.
* Unauthorized scanning instructions.
* Phishing campaigns targeting real people.
* Instructions for bypassing security controls.

Detection logic and threat-hunting activities are designed for defensive and educational purposes.

---

## 12. References

Primary references will include official MITRE ATT&CK documentation and other authoritative cybersecurity sources used during the investigation.

* MITRE ATT&CK Enterprise
* MITRE ATT&CK Technique Documentation
* MITRE ATT&CK Tactic Documentation
* Relevant cybersecurity standards and defensive guidance
* Course-provided evidence and project requirements

Full references and URLs will be documented in:

```text
references/sources.md

