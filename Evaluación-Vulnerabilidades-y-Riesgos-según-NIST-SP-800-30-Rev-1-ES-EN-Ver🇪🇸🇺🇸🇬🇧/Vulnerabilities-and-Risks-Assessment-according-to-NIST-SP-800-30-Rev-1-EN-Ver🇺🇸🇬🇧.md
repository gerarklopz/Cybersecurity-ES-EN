## Technical Report: Vulnerability and Risk Assessment in Hybrid Environment (Cloud + On-premises)EN-Ver🇺🇸🇬🇧

### Scenario: Global E-commerce Hybrid System
The company hosts customer data on a database server, with many employees working remotely worldwide. Employees frequently access or query this server to manage leads. The database has been publicly accessible since the company's inception three years ago.

### Objective
Evaluate threats, vulnerabilities, and associated risks to publicly exposed critical assets.

### Standards Referenced
- **NIST SP 800-30 Rev. 1**
- **ISO 27001**
- **ENS (Spain's National Security Framework)**
- **GDPR (EU)**

---

### Additional Context:
| Framework | What is it? | Why is it important? | Impact on the company |
|----------|-------------|----------------------|-------------------------|
| NIST SP 800-30 Rev. 1 | U.S. guide for risk assessment in IT systems. | Provides structured methodology for cyber risk identification and reduction. | Enables investment prioritization and audit-ready documentation. |
| ISO 27001 & ENS | International/national security management standards. | Required in public tenders, certifications, audits. | Shows compliance, builds trust, reduces penalties, and enables contracts. |
| GDPR | EU law protecting personal data. | Imposes heavy penalties for exposure/misuse of personal data. | Requires both technical (encryption, access control) and organizational measures (policies, training). |

---

## Evaluated Architecture: Hybrid System
| Component | Location | Technical Justification |
|----------|----------|-------------------------|
| Customer Database | On-premises | Protect sensitive data; GDPR compliance |
| Web Frontend (HTML/CSS) | Cloud (CDN + S3) | Low latency, global delivery |
| API Backend (REST) | Hybrid (active/passive) | Fault tolerance, cloud scalability |
| Logging + SIEM | Private cloud | Secure retention, real-time analysis |
| Perimeter Security | WAF + Physical Firewall | Defense in depth |
| Remote Access | VPN, Cloud IAM | Controlled, secure access |

---

## Additional Details:
- **API Backend**: Bridge between frontend and database. Controls access and scales services.
- **Logging & SIEM**: Centralized activity log. Detects attacks and audits access.
- **WAF + Firewall**: Protects web applications and internal networks.
- **VPN, IAM**: Secured remote access with precise permissions.

---

## Asset & Threat Identification
### Critical Assets
- Database server (3 years in production)
  - Contains PII: names, credit cards, order history
  - Relational database accessible via API/Web

### PII (Personally Identifiable Information)
- **Spain/EU (GDPR)**: Mandatory protection. Mishandling = high fines.
- **Worldwide (e.g., CCPA, LGPD)**: Similar data protection laws.
- **Business**: Loss of PII = legal + reputational risk.

### Involved Users
- Internal staff (admin, support)
- End-users/customers
- Third-party providers (payment gateways, CRM, integrators)

---

## Threats (Based on STRIDE / NIST)
| Type | Examples |
|------|----------|
| External | Brute force, SQLi, port scans, CVE exploits |
| Internal | Privilege abuse, data exfiltration, human error |
| Accidental | Misconfigurations (open S3, open ports) |
| Environmental | Power outage, hardware failure |

### Threat Details
- **Third Parties**: Expand attack surface. Validate, isolate, contractually bind.
- **Brute Force**: Prevent with MFA, CAPTCHA, lockouts.
- **SQL Injection**: Prevent with input validation, prepared statements, WAF.
- **Port Scans**: Detect via Nmap; defend with firewalls, IDS.
- **CVE Exploits**: Patch frequently; monitor MITRE/NIST.
- **Open S3 Buckets**: Misconfigured cloud storage; mitigate with IAM policies and encryption.

---

## Vulnerability Assessment Tools
| Tool | Primary Use |
|------|-------------|
| Nessus / OpenVAS | Network scan, CVE detection |
| Nikto | HTTP analysis |
| Nmap | Port scanning |
| Burp Suite | Web app security testing (XSS, CSRF, etc.) |
| Shodan | Exposure verification |

### Key Sources:
- **CVE/MITRE**: Official vulnerabilities
- **NVD**: Risk scoring (CVSS)
- **OWASP Top 10**: Global web security risks

---

## Highlighted Finding
🔴 **Server publicly visible on Shodan with no active perimeter defense**
- No WAF or firewall at scan time.
- Implies critical exposure requiring immediate remediation.

---

<table>
  <thead>
    <tr>
      <th>Risk</th>
      <th>Likelihood</th>
      <th>Impact</th>
      <th>Risk Level</th>
      <th>CVSS</th>
      <th>Business Impact</th>
      <th>Final Risk (NIST)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>SQL Injection in REST API</td>
      <td style="background-color:#f8d7da;">High</td>
      <td style="background-color:#f5c6cb;">Critical</td>
      <td style="background-color:#dc3545; color:white;">Critical</td>
      <td><strong>9.8</strong></td>
      <td>Sensitive data at risk, service disruption</td>
      <td style="background-color:#dc3545; color:white;">Critical</td>
    </tr>
    <tr>
      <td>SSH Unauthorized Access</td>
      <td style="background-color:#fff3cd;">Medium</td>
      <td style="background-color:#ffeeba;">High</td>
      <td style="background-color:#fd7e14; color:white;">High</td>
      <td><strong>8.2</strong></td>
      <td>Infrastructure compromise, lateral movement</td>
      <td style="background-color:#fd7e14; color:white;">High</td>
    </tr>
    <tr>
      <td>Open S3 Bucket Leak</td>
      <td style="background-color:#f8d7da;">High</td>
      <td style="background-color:#f5c6cb;">Critical</td>
      <td style="background-color:#dc3545; color:white;">Critical</td>
      <td><strong>9.5</strong></td>
      <td>Public PII exposure</td>
      <td style="background-color:#dc3545; color:white;">Critical</td>
    </tr>
    <tr>
      <td>LDAP Config Error</td>
      <td style="background-color:#d4edda;">Low</td>
      <td style="background-color:#d1ecf1;">Medium</td>
      <td style="background-color:#17a2b8; color:white;">Moderate</td>
      <td><strong>6.1</strong></td>
      <td>Impacts internal auth systems</td>
      <td style="background-color:#17a2b8; color:white;">Moderate</td>
    </tr>
  </tbody>

### NIST Recommendation:
Use **CVSS** to assess technical risk and complement with **qualitative business impact**.

---

## Remediation Plan & KPIs
| Action | Deadline | Owner | KPI |
|--------|----------|--------|-----|
| Apply critical patches | 24–48h | SysAdmin | Remediation time < 48h |
| Enable WAF + IDS | 1 week | SecOps | Malicious attempts reduced |
| Network segmentation | 15 days | Infra Team | Isolation verified via scan |
| Review IAM policies | 7 days | CISO | 100% MFA + RBAC enforcement |

---

## Continuous Monitoring
- **SIEM (Elastic, Splunk, Sentinel)**: Real-time security event correlation
- **UEBA**: Detect insider threats and anomalies
- **Access Auditing**:
  - Central syslog
  - Cron job-based periodic review
  - Automated alerts (email, SIEM, etc.)

---

## Best Practices Summary
- Use **regression testing** post-remediation
- Apply **zero trust model** for internal threats
- Use **signed logs** and **scheduled reviews**
- Implement **AAA Model**:
  - Authentication: MFA, SSO
  - Authorization: RBAC, least privilege
  - Audit: Alerts, log integrity, weekly reviews

---

## Active Defense & Hardening 🛡️
| Layer | Tool | Description |
|-------|------|-------------|
| WAF | ModSecurity, AWS WAF | OWASP Top 10 rule enforcement |
| Firewall | pfSense, Azure NSG | IP filtering, geofencing |
| IDS/IPS | Snort, Suricata | Intrusion detection/prevention |
| Hardening | CIS Benchmarks | Baseline hardening |
| Patching | WSUS, Ansible | Centralized update management |

---

## Reflection Questions
- **What does public exposure mean?**: Open IP, service discoverable, no VPN or DMZ.
- **What if the attack is internal?**: Zero trust, behavioral monitoring, segmented access.

---

## Strategic Conclusion
Hybrid environments enhance flexibility, but demand:
- Segmented architecture
- Continuous monitoring
- Access governance
- Hardened systems
- NIST-based risk evaluation

---

## Lessons Learned
| Lesson | Description |
|--------|-------------|
| Visibility ≠ Security | Shodan detection showed being online doesn't mean being protected |
| Internal risk = External risk | Privilege abuse and human error are critical |
| Controls must be adaptive | Security is an ongoing process |
| NIST = Method + Discipline | Provided a structured, audit-ready workflow |
| Communication is key | Colored risk matrix improved stakeholder understanding |

---

**Prepared by**: Gerardo Arca López*
**Date**: April 17, 2023*
