# Creation of Handler Journal: Incident Documentation (EN Version 🇺🇸🇬🇧)

## Introduction
This document aims to present a professional example of documentation within the framework of cybersecurity incident response. It is presented within the context of a practical case and is structured based on the international NIST Cybersecurity Framework (CSF), as well as relevant European and Spanish regulations. This content is aimed at managers, human resources professionals, recruiters, and IT managers, providing a structured and understandable view of the incident lifecycle and its proper documentation.

---

## Reference Framework: NIST CSF and other regulations

### NIST-CSF Core Functions:
The core functions of the NIST-CSF framework describe essential capabilities:

- **Identify**: Understanding the organizational context and cybersecurity risks.
- **Protect**: Safeguards to ensure the delivery of critical services.
- **Detect**: Timely identification of cybersecurity incidents.
- **Respond**: Actions in the event of a detected incident.
- **Recover**: Restoration of affected capabilities or services.

---

### Complementary Frameworks

| Framework | Description | Practical Application |
|-------|-------------|---------------------|
| **ENS (National Security Scheme - Spain)** | Spanish legal framework that establishes minimum security requirements for public administration systems and entities that provide services to the public sector. | Classifies systems by criticality (Low, Medium, High) and requires measures according to level. E.g., access policy, traceability, encryption. Widely used in government tenders and projects. |
| **ISO/IEC 27001** | International standard for managing information security through an ISMS (Information Security Management System). | It requires risk analysis, policies, technical and procedural controls. In private companies, it improves trust and compliance. |
| **ISO/IEC 27035** | Complements 27001 by focusing on security incident management: detection, reporting, assessment, response, and lessons learned. | It offers a framework that you can adapt with step-by-step procedures for each phase of the incident. Ideal for cybersecurity SOC (Security Operations Center) teams (a specialized team responsible for monitoring, detecting, analyzing, and responding to cybersecurity incidents within an organization) or small IT departments. |
| **GDPR / LOPDGDD** | General Data Protection Regulation (EU) + its Spanish adaptation. They regulate how personal data is collected, processed, and protected. | It involves reporting incidents to the AEPD (Spanish Data Protection Agency, the public authority responsible for ensuring compliance with data protection regulations in Spain) within 72 hours if they affect personal data. Significant fines apply. Journal documentation can be a test of diligence. |
| **CCN-STIC** | A series of technical guides from the National Cryptologic Center (Spain), related to ENS compliance and national cyber defense. | Precise technical recommendations: system hardening, log analysis, incident response. Very useful for defining robust policies in public or hybrid environments. |

---

## Incident Response Lifecycle

```Mermaid
Preparation --> Detection and Analysis
 Detection and Analysis --> Containment
 Containment --> Eradication and Recovery
 Eradication and Recovery --> Post-Incident
 Post-Incident Controls Update
```

---

## Phase Development

### 1. Preparation
- Policies, procedures, training, tools, and agreements.
- Principle of least privilege and multi-factor authentication.

### 2. Detection and Analysis
- Log collection, SIEM alerts, anomalous behavior.
- Event analysis to confirm incidents.

> ⚠️ *All incidents are events, but not all events are incidents.*

- **Event**: An observable occurrence on a system, network, or device.

### 3. Containment
- Isolation of compromised machines.
- Network segmentation and access deactivation.

### 4. Eradication and Recovery
- Malware removal.
- Restoration from validated backups.
- Review of compromised access points and passwords.

### 5. Post-incident Activity
- Lessons learned.
- Improvements to policies and controls.
- Generation of the final report.

---

## Case Study: Premeditated Insider Attack

### Description:
An employee with administrative privileges and technical knowledge, before being fired, inserted malicious executables into administrative systems. These scripts were executed weeks later, generating sabotage without leaving obvious traces that directly incriminate him.

---

### How did the saboteur do it?

The insider attacker (the former employee with advanced IT skills) acted in a planned and stealthy manner. Here's a breakdown of his techniques in technical and contextual detail:

| Technique Used | Explanation and Risk |
|-------------------|----------------------|
| **Delayed Scheduled Tasks** | Used Windows Task Scheduler or cron on UNIX systems to schedule deferred execution of malicious scripts (days or weeks later). Difficult to directly link to their output. |
| **Living Off the Land (LotL)** | Technique that leverages legitimate operating system tools for malicious activities. E.g., PowerShell, WMI, Certutil, without raising suspicion. Avoids AVs and EDRs (AVs: Antivirus are security software that focuses on detecting and removing known malware, while EDRs (Endpoint Detection and Response) are more advanced solutions that analyze system behavior to detect threats, even unknown ones, and respond to them.) Misconfigured. |
| **Disguised Names** | Renamed malicious executables such as `svchosts.exe`, `winupdater.bat`, or `system32check.vbs` to remain invisible. He also modified attributes to hide them. |
| **Modified Permissions** | Altered ACLs (Access Control Lists) to prevent deletion or scanning of his files. He also disabled logs or inhibited system notifications. |
| **Silent Disabling of Antivirus** | Executed commands that modified local policies or registry keys to temporarily disable defenses such as Windows Defender. E.g., `Set-MpPreference`. |

> Detection Advice: Monitor for new scheduled tasks, changes to ACLs (network devices such as routers, firewalls, and switches. Their primary function is to filter incoming and outgoing data traffic, allowing or denying network access based on various criteria), unsigned scripts, and anomalous use of PowerShell.

---

## Incident Investigation – The 5 Ws Method

- **Who?**: Former employee with privileged access.
- **What?**: Installation of executables that alter ciphers, delete files, or redirect documentation.
- **When?**: Weeks after departure, using timers and persistence.
- **Where?**: Administrative management systems and shared servers.
- **Why?**: Revenge and hidden sabotage against the employer.

> Keeping meticulous records of this information is essential, not only during an investigation, but also during the incident closure phase.

---

## Additional Insider Threat Cases

- **Administrators creating backdoors (a special sequence or term within the programming code, through which security algorithms (authentication) can be bypassed to access the system).**
- **Employees leaking data to competitors.**
- **Non-malicious but serious errors (accidentally deleting files).**

---

## Technical Annexes

- Detected scripts (example of obfuscated payload).
- List of processes and scheduled tasks.
- SIEM configuration and generated alerts.
- Critical access and execution logs.

---

## Preventive Measures

- Implement EDR and alerts on anomalous behavior.
- Review and control of scheduled tasks.
- Post-employment audit (immediate removal of access).
- Version control, file integrity, and hashing.

---

## Incident Response Checklist

- [x] Incident Confirmation
- [x] System Isolation
- [x] Notification to Responsible Parties
- [x] Forensic Analysis
- [x] Eradication
- [x] Data Restoration
- [x] Final Report and Lessons Learned

---

## Roles and Responsibilities

| Role | Responsibility |
|-----|-----------------|
| CISO | Overall Incident Oversight |
| SOC Analyst | Initial Identification and Analysis |
| Digital Forensics | Evidence Collection |
| Systems Administrator | Containment and Recovery |
| Legal / HR | Internal Consequence Management |

---

## Handler's Journal: Documentation Tool

- **Incident Date**: 2025-04-21
- **Entry Number**: 1
- **Event Description**: Planned internal sabotage with delayed execution.
- **Tools Used**: Sysinternals, ELK, Wireshark, Velociraptor.

| Tool | What does it do? | Application in this case |
|------------|------------|------------------------|
| **Sysinternals Suite** (Microsoft) | A set of utilities for deep analysis of Windows systems. E.g., `Autoruns`, `Process Explorer`, `PsExec`. | It was used to detect suspicious processes, analyze automatic startups, and trace hidden activity. Useful for finding persistence. |
| **ELK Stack (Elastic, Logstash, Kibana)** | Log and event analysis platform. Allows you to centralize, visualize, and correlate activity. | Ideal for detecting anomalous behavior patterns: after-hours access, error events, writing sensitive files. |
| **Wireshark** | Network traffic analyzer. Captures packets and allows for in-depth communication studies. | Used to detect unusual outgoing connections or communication with C2 (command & control) servers. Very useful if the executable was attempting to exfiltrate data. |
| **Velociraptor** | Forensic and live hunting tool. Collects endpoint artifacts (processes, logs, events) and allows remote queries. | Very effective in post-mortem analysis. It was used to audit system changes, examine scheduled tasks, and detect indicators of compromise. |

- **Development of the 5 Ws**: See previous section.
- **Additional observations**: It is recommended to review sick leave procedures.

---

## Conclusion

This document exemplifies how proper and methodical documentation can serve both as a technical record and as a professional and pedagogical resource. Using the Handler's Journal in conjunction with frameworks such as the NIST CSF and ENS ensures an effective response, minimizing risks and strengthening organizational resilience.

---

_Created by: Gerardo Arca López_

_Date: April 22, 2025_
