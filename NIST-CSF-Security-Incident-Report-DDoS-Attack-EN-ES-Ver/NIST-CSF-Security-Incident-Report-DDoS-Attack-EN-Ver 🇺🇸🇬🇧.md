# Security Incident Report - DDoS Attack EN-Ver 🇺🇸🇬🇧

## 1. Executive Summary

This report documents a security incident that occurred within the internal network of a multimedia company specializing in web design, graphic design, and social media marketing solutions for small businesses. The company experienced a **Distributed Denial of Service (DDoS)** attack, which compromised service availability for two hours. The attack involved a flood of **ICMP (Internet Control Message Protocol)** packets, which saturated the network and prevented access to critical resources.

The incident was caused by a misconfigured **firewall**, which allowed a malicious actor to send a large volume of ICMP requests, overwhelming the network. In response, the incident management team blocked incoming ICMP traffic, restored critical services, and implemented additional measures to prevent future attacks.

It is recommended to implement **stricter firewall rules**, IP address verification, and adopt monitoring and intrusion detection tools to strengthen network security, following the **NIST CSF (National Institute of Standards and Technology Cybersecurity Framework)**.

---

## 2. Incident Description

**Incident Date:** 2025-03-10  
**Affected Company:** Multimedia company offering web design, graphic design, and marketing solutions.  
**Impacted Area:** Internal network, web servers, email servers, and databases.

### 2.1. Attack Details

- An attacker exploited a vulnerability in the **firewall**, sending a flood of ICMP packets to the internal network.
- The malicious traffic saturated the network, preventing employees from accessing critical resources such as web servers, email, and databases.
- The attack lasted approximately two hours, during which services were partially or completely unavailable.

### 2.2. Immediate Response

The security team responded as follows:

1. **Containment**: Incoming ICMP packets were blocked using a specific firewall rule, and non-critical services were taken offline to reduce network load.
2. **Restoration**: Priority was given to restoring critical services, such as email and web servers, using recent backups.
3. **Investigation**: It was identified that the firewall was misconfigured, allowing the attack. Network logs were reviewed to trace the origin of the malicious traffic.

---

## 3. Incident Analysis

### 3.1. Attack Identification

- **Type of Attack**: Distributed Denial of Service (DDoS) via ICMP flood.
- **Affected Systems**: Web servers, email servers, and databases.
  - **Consequences**:
    - **Web Servers**: Client websites were inaccessible, potentially damaging their reputation and causing revenue loss.
    - **Email Servers**: The interruption of email services hindered internal and client communication.
    - **Databases**: Unauthorized access attempts to sensitive data were detected, although no data breach was confirmed.
- **Exploited Vulnerability**: Misconfigured firewall allowing unfiltered ICMP traffic.

### 3.2. Incident Impact

- **Availability**: Internal services were unavailable for two hours, affecting daily operations.
- **Reputation**: Service interruption could damage client trust, especially if repeated in the future.
- **Costs**: Loss of productivity and potential costs associated with recovery and infrastructure improvements.

---

## 4. Improvement Plan and Recommendations

### 4.1. Protection (NIST CSF: **Protect**)

- **Implement stricter firewall rules**: Limit ICMP traffic and verify source IP addresses to prevent spoofing attacks.
- **Update security policies**: Conduct regular audits to identify and address network vulnerabilities.
- **Staff training**: Provide training to employees on recognizing and responding to potential security threats.

### 4.2. Detection (NIST CSF: **Detect**)

- **Continuous network monitoring**: Implement monitoring tools to detect abnormal traffic patterns.
- **Intrusion Detection System (IDS)**: Use an IDS to identify and alert on suspicious network activities.
- **Log review**: Regularly analyze network logs to detect unauthorized access attempts.

### 4.3. Response (NIST CSF: **Respond**)

- **Incident response plan**: Develop a clear protocol to contain and neutralize future attacks.
- **Stakeholder communication**: Establish a process to inform clients and employees about security incidents.
- **Post-incident analysis**: Conduct detailed reviews after each incident to identify areas for improvement.

### 4.4. Recovery (NIST CSF: **Recover**)

- **System restoration**: Ensure affected systems are quickly restored from backups.
- **Integrity verification**: Confirm that restored data and systems have not been compromised.
- **Recovery process improvement**: Implement a Disaster Recovery Plan (DRP) to minimize downtime in future incidents.

---

## 5. Conclusion

This security incident highlights the importance of having a well-configured and protected network infrastructure. Although the attack was contained and services were restored, it is crucial to implement additional measures to prevent future incidents. Adopting a proactive approach, based on the **NIST CSF**, will strengthen the company's security posture and ensure business continuity.

It is recommended to proceed with corrective actions immediately and establish a long-term security plan to avoid similar incidents.

**Signature:**  
Gerardo Arca López  
**Role:** Cybersecurity Analyst (Intern)  
**Date:** 2025-03-10
