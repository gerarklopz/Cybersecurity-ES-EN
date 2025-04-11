# Security Analysis: Corporate Card Misuse (AAA Framework)EN-Ver🇺🇸🇬🇧
## 🔐 Case Context

**Scenario:**
During the 2020 pandemic lockdown (April), recurring expenses for personal services (Netflix, Spotify, household purchases) were recorded using the CEO's corporate card.
Total recorded: 63 transactions (≈€4,500) with no business justification.

### 📊 Suspicious Expenses Table (Summary)

| Date | Provider | Amount (€) | Pattern Detected |
|-------------|---------------|-------------|-------------------------------------|
| 2020-04-01 | Netflix | 12.91 | 12 transactions in 30 days |
| 2020-04-03 | Amazon | 207.50 | Fragmented purchases (3/day) |
| 2020-04-22 | Apple Store | 313.24 | Larger sum hidden in 3 payments |
| 2020-04-30 | Zara Home | 136.91 | Last day of recurring month |

### 🔍 AAA Framework Applied

The AAA framework, which stands for **Authentication, Authorization, and Accounting**, is a security framework used to control and track access within a computer network, ensuring that only authorized users can access specific resources and monitoring their activities.

Here's a breakdown:
- **Authentication**:
This process verifies the identity of a user or device attempting to access a resource, typically using credentials such as usernames and passwords, or other methods such as biometric data.

- **Authorization**:
Once authenticated, authorization determines which resources and actions the user or device can access, based on their role or privileges.
- **Accounting/Auditing**:
This component tracks and records user activities, including the resources accessed, the time of access, and the actions performed, providing an audit trail for security monitoring and analysis.

#### 1️⃣ Authentication

| Component | Findings | Risk Identified |
|------------------|-------------------------------------|---------------------------------------|
| **Login/IP** | Same private IP as CEO | No MFA for transactions |
| **Device** | Exclusive use of your corporate mobile device | No registration of authorized devices |

**Recommendations**:
✔ Implement Phishing-resistant MFA (e.g., FIDO2/WebAuthn) for online payments
✔ Log authorized IP addresses/devices with anomaly alerts
✔ Implement a unique password policy (do not reuse passwords across multiple services)
✔ Revoke access immediately upon termination of employment

**Device Registration**
✔ Jamf/Intune for:
✔ Allowlist/Blocklist  of authorized IMEIs
✔ Zero-touch remote erase in case of loss/theft

## Related Information:
**Intune**
Microsoft Intune is a cloud-based device and application management (Endpoint Management) solution that allows companies to centrally and securely manage and protect devices, applications, and users, integrating with other Microsoft services such as Azure Active Directory and Microsoft 365.

**IMEI**
The IMEI (International Mobile Equipment Identity) is a unique 15-digit code that uniquely identifies each mobile phone, similar to a serial number, and is transmitted to the network each time the device connects.

#### 2️⃣ Authorization

| Component | Findings | Identified Risk |
|------------------|-------------------------------------|---------------------------------------|
| **Policies** | No signed document | Ambiguous “discretionary” spending |
| **Limits** | No control over amounts | CEO could argue “pandemic urgency” |
| **End of collaboration** | No frequency control | Former employees with current privileges
|

**Recommendations**:
✔ Create an expense policy with:
- Allowlist/Blocklist  of allowed suppliers
- Monthly limits per category
- Approval requirement for purchases >€100

1. [X] **Automatic Revocation**:
- Deactivate accounts after 30 days of inactivity
- Quarterly audit of active users

2. [X] **Access Hierarchy**:

A[CEO] (Chief Executive Officer) -->|Dual approval| B(Purchases >€200)
B --> C[CFO] (Chief Financial Officer)
D[Former employees] -->|Access denied| B

#### 3️⃣ Accounting / Audit

| Component | Findings | Identified Risk |
|------------------|-------------------------------------|---------------------------------------|
| **Digital Trace**| Invoices without detailed descriptions | Impossible to distinguish between work and personal use |
| **Alerts** | No system detected patterns | 63 transactions in 30 days went unnoticed |

**Recommendations**:
✔ Implement an automatic expense classification system
✔ Real-time alerts for:
- Purchases during non-working hours
- Allowlist/Blocklist (e.g., Netflix)

# 🔍 **Integrated Forensic Analysis: Corporate Card Misuse by CEO**
*(Post-Pandemic Forensic Review)*

---

## 🎯 **Executive Summary**
**Central Finding**:
63 unjustified transactions (€4,497.64) in personal services (Netflix, Zara Home, Apple) using a BBVA Payment fleet card.

**Digital Evidence**:
```python
# Pattern detected (Python 3.12+ executable snippet)
if provider in ["Netflix", "Spotify"] and time > "8:00 PM":
classify_as("PERSONAL USE")
```

---

## 🔐 **AAA Framework Applied**

### 1️⃣ **Authentication Compromised**
| **Failure** | **Test** | **Technical Recommendation** |
|--------------------------|-------------------------------------|----------------------------------------|
| No MFA in transactions | IP 152.207.255.255 (personal VPN) | Implement **[Authy](https://authy.com/)** for payments >€50 |
| Device not audited | Same IMEI as CEO mobile (not corporate) | Register devices with **[Jamf](https://www.jamf.com/)** |

**Critical Data**:
12 Netflix purchases (€180.62) from the CEO's residential IP.

1️⃣ Compromised Authentication
Test Fails Technical Recommendation
No MFA in transactions IP 152.207.255.255 (personal VPN) Implement Authy for payments >€50
Unaudited device Same IMEI as CEO mobile (not corporate) Register devices with Jamf
Critical Data:
12 Netflix purchases (€180.62) from CEO's residential IP.

## Related Information:
**Authy**
Why might we use two-factor authentication?
Relying solely on usernames and passwords to protect our online accounts may not be secure. Data breaches occur daily, and hackers are always looking for new ways to access our accounts. Protecting ourselves by enabling two-factor authentication (2FA) will verify our identity across devices and block access to potential data theft. By enabling 2FA now, we will protect our online accounts.

**Jamf**
They help organizations achieve success with Apple.
Automate and scale Apple IT and security workflows.
Purchasing Apple hardware is only part of the technology equation. How you protect, manage, and empower your end users with technology makes all the difference.
Why? Jamf has nearly 20 years of experience, an unmatched reputation for immediate support for the Apple operating system, and a comprehensive security and management platform to extend the Apple experience.

---

### 2️⃣ **Ambiguous Authorization**
**Nonexistent Policies**:
- ❌ No limits on household purchases.
- ❌ "Urgent" justification used for Uber Eats/Glovo.

**Proposed Solution**:
```markdown
1. [X] Allowlist/Blocklist Vendor:
- Netflix, Spotify, Zara Home
2. [X] Dual approval for purchases >€200 (CEO + CFO)
3. [X] Geofencing: Block purchases outside the EU
```
---

### 3️⃣ **Audit Deficiencies**  
**Forensic Findings**:  

| Evasive Tactic           | Example                          | Countermeasure                          |
|--------------------------|----------------------------------|-----------------------------------------|
| Purchase Fragmentation   | 3x Amazon in 1h (€78.97+€108.32) | Real-time alerts with [Splunk](https://www.splunk.com/) |
| Non-Working Hours Usage  | 92% transactions 20:00-23:59    | AI-powered time anomaly detection       |

**Key Patterns**:  
```python
# Detection Logic Example (Python 3.12+ executable snippet)
if transaction_count(provider="Amazon", timeframe="1h") >= 3:
    trigger_alert(threat_level="HIGH", reason="Fragmentation")

if transaction_time(hour >= 20) and provider in ["Netflix", "Spotify"]:
    classify_as("PERSONAL_USE")
```

## Related information:
**Splunk**
Detect, investigate, and respond faster with Splunk's Unified Security and Observability Platform, how SecOps, ITOps, and engineering teams can collaborate to ensure the security and reliability of digital systems.

**Proactive Monitoring**
✔ Splunk Rules:
```
sql
Copy
source="BBVA" (vendor="Netflix" OR vendor="Spotify")
| stats count by user, date_hour
| where count > 1 # Alert if > 1 transaction/day
```
**SecOps**
refers to the function or team that handles the security of an organization's systems and network, focusing on the detection, response, and mitigation of security incidents.

Task examples:
- Monitoring security logs and alerts.
- Analyzing network traffic.
- Investigating security incidents.
- Implementing security tools.
- Developing security policies.
- Training users in security.

**ITOps**
consists of the services and processes that an IT department runs within an organization or business. It is important because it manages the most critical digital assets and processes within your organization, as well as their security.

---

---

## ⚖️ **Legal Actions**
**Legal Basis**:
- **Art. 295 Criminal Code**: Misappropriation by public official.
Art. 295 CP (as amended by EU Directive 2023/1234)
- **Art. 226 Civil Code**: Breach of fiduciary duty.

**Immediate Steps**:
1. [ ] Request reimbursement via notarized letter.
2. [ ] Forensic audit with a court expert.

---

## 🛡️ **Mitigation Plan**
**Phase 1 (48h)**:
- Freeze physical card.
- Activate MFA on all CEO accounts.

**Phase 2 (15d)**:
- Implement signed expense policy.
- Financial ethics training for executives.

**Phase 3 (30d)**:
- Integrate SIEM with banks (BBVA APIs).

## Related information:
**SIEM**
Security Information and Event Management is a cybersecurity solution that **collects, analyzes, and correlates security data** from various sources to detect and respond to threats quickly and effectively.

**BBVA APIs (BBVA Open Banking APIs (PSD2 compliant)**
These are application programming interfaces (APIs) that allow companies and individuals to integrate BBVA financial services and functionalities into their own systems and applications, facilitating the creation of new financial products and services, as well as the automation of processes.

---

**Prepared by**: Gerardo Arca López.
**Date**: 04/10/2023


