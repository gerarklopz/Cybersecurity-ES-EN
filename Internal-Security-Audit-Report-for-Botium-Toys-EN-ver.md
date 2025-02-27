# EN ver.🇺🇸🇬🇧
# Internal Security Audit Report for Botium Toys

## Context and Scenario

Botium Toys is a small U.S. company specializing in the development and sale of toys. With the expansion of its online presence, the company has experienced rapid growth, attracting customers both nationally and internationally. As a result, the Information Technology (IT) department faces increasing pressure to support this expanded online market.

The IT department manager decided to conduct an internal IT audit to ensure compliance with regulations and business continuity as the company grows. The audit focuses on identifying and mitigating risks, threats, or vulnerabilities in critical assets. A particular concern is ensuring compliance with internal payment processing policies and international regulations such as the General Data Protection Regulation (GDPR) in the European Union (EU) if handling data from EU citizens.

The audit is being conducted using the **National Institute of Standards and Technology Cybersecurity Framework (NIST CSF)**. This framework helps identify existing risks and gaps in the company's security posture. The audit also aims to assess whether the necessary controls are in place and determine the impact of non-compliance with relevant regulations.

## Audit Objective

- **Review the scope, objectives, and risk assessment report** generated by the IT manager and the audit team.
- **Conduct an internal audit** by completing the controls and compliance checklist.
- **Evaluate the security posture** of Botium Toys and determine areas where controls need to be implemented to improve security and compliance.

---

# Controls and Compliance Checklist

## Evaluation Controls

| Check | Control |
|---|---|
| ✅ | Least privilege |
| ❌ | Disaster recovery plans |
| ❌ | Password policies |
| ✅ | Segregation of duties |
| ✅ | Firewall |
| ❌ | Intrusion Detection System (IDS) |
| ❌ | Backups |
| ✅ | Antivirus software |
| ❌ | Monitoring, maintenance, and manual intervention for legacy systems |
| ❌ | Encryption of sensitive data |
| ❌ | Password management system |
| ✅ | Locks (offices, stores, warehouse) |
| ✅ | CCTV surveillance |
| ✅ | Fire detection and prevention (fire alarm, sprinkler system, etc.) |

### **Payment Card Industry Data Security Standard (PCI DSS)**

| Check | Best Practice |
|---|---|
| ✅ | Only authorized users have access to customers' credit card information. |
| ❌ | Credit card information is stored, accepted, processed, and transmitted securely within an internal environment. |
| ❌ | Data encryption procedures are implemented to enhance transaction and credit card data security. |
| ❌ | Secure password management policies are enforced. |

### **Responses to Task Questions**

### **Explanation of SOC 1 and SOC 2 Controls**

These controls ensure the security, integrity, and availability of data. They are essential to demonstrate Botium Toys' commitment to security in its operations and to protect critical information of customers and employees.

| Check | Best Practice |
|---|---|
| ✅ | User access policies are established. |
| ✅ | Sensitive data (PII/SPII) is kept confidential/private. |
| ✅ | Data integrity ensures that data is consistent, complete, accurate, and validated. |
| ✅ | Data is available to authorized individuals. |

---

## Final Conclusions and Recommendations

1. **Implement an access control system based on the principle of least privilege**.
2. **Strengthen firewalls and antivirus systems** with customized configurations and regular updates.
3. **Encrypt sensitive data**, such as credit card information and PII, using industry-standard algorithms like AES-256.
4. **Enhance physical security** with biometric or ID card access controls.
5. **Develop and implement a disaster recovery plan (DRP)**, including the creation of full, incremental, and differential backups.
6. **Strengthen the password policy**, requiring complex passwords and enforcing multi-factor authentication (MFA).

By implementing these controls and best practices, Botium Toys can significantly improve its security posture and comply with relevant regulations, protecting both its assets and customer data.

--

