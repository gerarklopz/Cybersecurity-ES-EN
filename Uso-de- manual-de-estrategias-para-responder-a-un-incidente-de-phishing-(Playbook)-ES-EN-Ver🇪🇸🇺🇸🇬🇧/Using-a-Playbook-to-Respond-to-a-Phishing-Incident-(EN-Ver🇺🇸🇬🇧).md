# Using a Playbook to Respond to a Phishing Incident (EN-Ver🇺🇸🇬🇧)

## Overview

In this report, we must respond to a phishing incident involving a malicious file hash (SHA256).

## Context

As we know, playbooks detail the steps necessary to effectively manage security incidents. Their goal is to coordinate quick and effective actions, minimize the impact of the incident, and reduce response time. As security analysts, these playbooks help us respond systematically and in line with the organization's internal policies.

## Activity/Incident Scenario

As future level one analysts in an organization's security operations center (SOC), we previously received a phishing alert related to a suspicious file downloaded to an employee's computer.

After analyzing the hash of the email attachment, it was confirmed that the file is malicious.

The task to be performed is to follow the organization's defined procedure to complete the investigation and resolve the alert.

## Alert Ticket Resolution

### Phishing Alert Ticket Details

| Field | Detail |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| Ticket ID | A-2703 |
| Alert Message | SERVER-MAIL Phishing attempt – possible download of malware |
| Severity | Medium |
| Ticket Status | Open |
| Malicious Hash | `54e6ea47eb04634d3e87fd7787e2136ccfbcc80ade34f246a12cf93bab527f6b` |
| Suspicious Email | From: Def Communications `<76tguyhh6tgftrt7tg.su>`<br>Source IP: `114.114.114.114`<br>To: `hr@inergy.com`<br>Destination IP: `176.157.125.93` |
| Subject | "Re: Infrastructure Engineer role" |
| Email Date | July 20, 2022 – 09:30 AM |
| Attachment | `bfsvc.exe` (password protected: `paradise10789`) |

### Phishing Playbook Assessment

### Step 1: Evaluate the alert

Analysis of the suspicious email content:

| Element | Observations |
| ---------------- | ------------------------------------------------------------------------------------------------------------- |
| Severity | Medium → Possible escalation according to the playbook |
| Sender | Suspicious domain + generic IP (`114.114.114.114`) → possible spoofing |
| Message | Contains grammatical errors → common phishing sign (e.g., subject: "Re: Infrastructure 'Egnieer' role") |
| Attachment | Password-protected `.exe` → technique used to evade antivirus |
| File hash | `54e6ea47eb04634d3e87fd7787e2136ccfbcc80ade34f246a12cf93bab527f6b` → malicious |

### Step 2: Basic Incident Identification

### 5W: Who, What, When, Where, Why?

| Question | Answer |
| --------- | ---------------------------------------------------------------------------------------- |
| Who? | Clyde West (alleged candidate) from `<76tguyhh6tgftrt7tg.su>` |
| What? | Email with malicious `.exe` file confirmed by hash |
| When? | Wednesday, July 20, 2022, at 9:30 AM |
| Where? | Inergy HR Appliance (Destination IP: 176.157.125.93) |
| Why? | Attempt to execute malicious code via an attachment disguised as a CV |

### Step 3: Does it contain attachments? Are they malicious?

* Yes, it contains an .exe file attached.
* Yes, the hash is confirmed as malicious by tools such as VirusTotal.
* Ticket escalation is warranted.

Ticket Comments

Brief description of the incident:
A suspicious email was received addressed to the HR team from an unknown address, with a password-protected executable (.exe) attachment.

Reasons for escalating the incident:

1. The attached file (`bfsvc.exe`) has a hash previously verified as malicious: `54e6ea47eb04634d3e87fd7787e2136ccfbcc80ade34f246a12cf93bab527f6b`.
2. The sender uses a domain and IP address not associated with any known legitimate entity.
3. The message content contains spelling and grammatical errors and follows typical phishing patterns (executable file disguised as a CV).

Action Taken:
The ticket has been escalated for review by a Level 2 SOC analyst.

Ticket Status:
Change from "Open" to Escalated

## Flowchart – Visual Proposal

```
+-----------------------------+
| Step 1: Receive ticket from |
| phishing alert |
+--------------+---------------+
|
v
+--------------+---------------+
| Step 2: Evaluate the alert |
| - Severity |
| - Sender/Recipient |
| - Subject/Message body |
| - Links or attachments |
+--------------+---------------+
|
v
+--------------+---------------+
| Step 3.0: Are there any links or |
| attachments? |
+-------+---------+------------+
| |
| No | Yes
v v
[Step 4] +----------------------------+
| Step 3.1: Are they malicious? |
+-------------+-------------+
|
+-----------------------------+
| Yes | v v
[Step 3.2: Escalate] [Step 4: Close]
```

---

## Critical Reflection / Final Observations

As a Level 1 analyst, after reviewing this case and simulating a response following the playbook, some reasonable doubts arise about the flow and operational structure for this type of incident:

### 1. Lack of pre-filtering before reaching the SOC

* Observation: It is surprising that a suspicious email reaches the view of a Level 1 SOC analyst directly without having gone through more efficient pre-filters (such as secure email gateways, DLP, or automated sandboxing).
* Technical comment: In well-structured environments, the email should have been intercepted by solutions such as Proofpoint, Mimecast, or Microsoft Defender for Office 365, even before generating a ticket. If this does not occur, there is a clear gap in the perimeter protection systems.

### 2. Why not first confirm the sender's legitimacy with HR?

* Note: If the sender presents themselves as a candidate, why not immediately validate with HR if they were expecting such a communication?
* Best practice: Before investing time in analyzing the email as a technical threat, it might be efficient to validate whether "Clyde West" is in a recruitment process or has been previously contacted.

### 3. Blind trust in VirusTotal

* Criticism: While VirusTotal is a useful tool, it shouldn't be the sole source of validation. A hash flagged as malicious requires more context:

* Which engines flagged it as malicious?
* What was the date of analysis?
* Does it match known campaigns?
* Better approach: Correlate with other sources such as Hybrid Analysis, Any.run, or even the organization's own Threat Intel feeds.

### 4. Who is Clyde West and how did he obtain the corporate emails?

* Key question: Is this phishing or was there a prior data leak? How did this supposed candidate obtain direct HR management? Is it a leaked list?
* Latent risk: This could be part of a broader spear-phishing campaign, where real employee or department data is being used.

### General Personnel Conclusion

While the playbook helps structure an orderly technical response, it cannot replace critical judgment, situational awareness, and cross-departmental validation. Cybersecurity is not just about hashes or malicious files, but also about human and organizational context. In this case, many questions remain open: Did the filters fail? Is there a procedural error? How was this external contact possible? Therefore, even at the operational level, we must question every step and not assume anything is "normal."

---

Prepared by: Gerardo Arca López

Date: May 16, 2024




