# Security Risk Assessment Report EN-Ver🇺🇸

## 📅 Date: 2025-10-03

## 👨‍💻 Prepared by: Cybersecurity Team (Currently pursuing: Google Cybersecurity Analyst Certificate)

## 🌐 Company: Social Media Organization (Small/Medium-scale)

---

## 1. 💡 **Executive Summary**

Following a recent security incident, the company has experienced a data breach that compromised sensitive customer information. In order to strengthen network security and prevent future attacks, a risk assessment has been conducted to identify vulnerabilities and define an improvement plan for the IT infrastructure security.

This report aims to provide high-impact corrective and preventive measures, including the use of advanced Google tools, a **Zero Trust** security approach, and a continuous training strategy for all staff, including technical teams as well as management and administrative personnel.

---

## 2. 🛡️ **Identified Vulnerabilities**

The following critical vulnerabilities have been detected in the network infrastructure:

1. **Shared passwords among employees.**
2. **Default administrator password for the database.**
3. **Lack of rules in firewalls to filter incoming and outgoing traffic.**
4. **Absence of multi-factor authentication (MFA) for critical access points.**
5. **Use of proprietary software with vulnerabilities and high licensing costs.**

If these vulnerabilities are not addressed, the organization remains at risk of attacks that could compromise its security and reputation.

---

## 3. 🔒 **Network Hardening Measures**

### 3.1 🔐 Password Policies

- Implementation of **strong passwords** with a minimum of 16 characters, including uppercase, lowercase, numbers, and special characters.
- Use of **corporate password managers** (e.g., Bitwarden, KeePassXC).
- Application of hashing with **bcrypt or Argon2** for password storage.
- Implementation of **periodic password changes**, with expiry alerts.
- Restriction of shared passwords using **granular access policies**.

### 3.2 🔧 Secure Database Configuration

- **Immediate change** of the database administrator password.
- **Encryption of data at rest and in transit** using AES-256 and TLS 1.3.
- **Access restrictions** to the database, allowing only authorized systems.
- Implementation of **regular backups** following the 3-2-1 strategy (3 copies on 2 different media, 1 off-site).

### 3.3 🛡️ Firewall and Port Filtering

- **Enablement of UFW and iptables firewalls** to control network traffic.
- **Closure of unnecessary ports** and application of strict rules:
  - **Allowed:** 22 (SSH restricted), 80/443 (web), 3306 (MySQL internal).
  - **Denied:** Unauthorized external access.
- **Network segmentation** with VLANs to limit internal access.

### 3.4 🛡️ Multi-Factor Authentication (MFA)

- **Enable 2FA** for all critical access points.
- Use of **TOTP (Google Authenticator, FreeOTP) instead of SMS**.
- Implementation of **FIDO2/Yubikey security keys** for administrative access.

### 3.5 🔢 Open Source Software Implementation

- **Migration to Linux-based systems (Ubuntu LTS)** for better security and reduced licensing costs.
- Use of **open-source software** for servers and workstations.
- Reduction of vulnerabilities present in proprietary systems like Windows.
- Greater control over updates and configurations without vendor dependency.
- Use of **LibreOffice, GIMP** as secure alternatives to proprietary software.

### 3.6 🛡️ **Gmail Security and Google Tools**

- **Enable two-step verification (2FA)** for all employee Gmail accounts using **Google Authenticator** or **FIDO2 security keys**.
- Implementation of **phishing filtering** in Gmail to prevent social engineering attacks and ensure the authenticity of internal communications.
- **Google Safe Browsing** to protect employees against malicious links in emails.

### 3.7 🔢 Log Monitoring and Analysis

- Implementation of **Graylog or ELK Stack** for centralized log management.
- Use of **Fail2Ban** to block unauthorized access attempts.

### 3.8 🛠️ Penetration Testing and Security Audits

- **Quarterly pentests** using Kali Linux and Metasploit.
- **Periodic audits** with Lynis, OSSEC, and Wazuh.
- Simulations of attacks to assess the effectiveness of implemented measures.

---

## 4. 🔄 **Implementation Plan**

| Measure | Responsible | Frequency |
|---------|-------------|-----------|
| Password Policies | IT Security | Ongoing |
| Database Configuration | DBA | Immediate |
| Firewall Rules | IT Networking | Monthly |
| MFA Implementation | IT Security | Immediate |
| Migration to Open Source | IT Management | Progressive |
| Log Analysis | SOC Team | Daily |
| Pentesting | Red Team | Quarterly |

---

## 5. 💼 **Conclusions and Recommendations**

Immediate implementation of these measures is recommended to mitigate the identified risks and ensure the security of the company’s information. Additionally, it is suggested:

- **Continuous training** for staff on best cybersecurity practices. This includes specific training for the **technical team** on new tools and **administrative and management personnel** on the fundamentals of IT security, the importance of **phishing attacks**, and proper management of sensitive information.
- **Gradual transition to open-source software** to enhance security and reduce costs.
- **Periodic review of security policies** and adjustments based on emerging threats.
- **Hiring cybersecurity experts** for annual external audits.

These actions will strengthen the organization’s security posture and minimize the potential impact of possible attacks.

---

**👨‍💼 Signed:** Gerardo Arca López  
**🏢 Department:** Cybersecurity (Currently pursuing: Google Cybersecurity Analyst Certificate)  
**📅 Date:** 2025-10-03

