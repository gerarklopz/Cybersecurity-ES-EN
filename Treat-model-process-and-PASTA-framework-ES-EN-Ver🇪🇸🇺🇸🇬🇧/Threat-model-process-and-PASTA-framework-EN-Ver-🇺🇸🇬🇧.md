# Threat Modeling Process and PASTA Framework (EN version 🇺🇸🇬🇧)

## Project: What2SMS - Redirect WhatsApp Messages to SMS for Classic Mobile Phones

---

## Scenario

Many people still rely on basic mobile phones ("clamshells"/ No Smartphones) without access to WhatsApp or modern apps. **What2SMS** seeks to redirect messages received via WhatsApp through **plain SMS**, allowing real-time notifications without the need for a smartphone.

---

## Service Objective

**Facilitate the reception of WhatsApp messages via SMS**, useful for:

- Elderly people or those with technological difficulties
- Areas without data connectivity
- Travelers and field workers
- Users who want to reduce their digital dependence ("Digital detox", LightPhones)

---

## Service Advantages

| Advantage | Description |
|------------------------|--------------------------------------------------------------|
| Digital inclusion | No need for a smartphone or apps. |
| Low power consumption | Only uses a GSM network. |
| Operational simplicity | Plain text without layers or complex configurations. |
| Controlled privacy | Reduced digital exposure. |

---

## ⚠️ Technical and Security Challenges

| Challenge | Potential Risk |
|----------------------------|------------------------------------------------------------|
| WhatsApp synchronization | Legal and technical difficulties. |
| SMS Limitations | No encryption, reduced length. |
| Third-party exposure | Unintentional message redirection. |
| Unauthorized automation | Violation of WhatsApp policies. |
| Spoofing | Impersonation if the source is not validated. |

---

## Introduction to Threat Modeling

**Threat Modeling** allows you to identify and anticipate security risks **before** they occur, structuring defenses from the design phase.

> It's thinking like an attacker to build like a defender.

---

## Threat Model Process – Key Stages

| Step | Action | Example in What2SMS |
|------|----------------------------|----------------------------------------------|
| 1 | Identify assets | Messages, users, API, backend. |
| 2 | List actors | Hackers, scripts, insiders. |
| 3 | Analyze threats | Spoofing, data leakage, unauthorized access. |
| 4 | Model attacks | Phishing, interception, manipulation. |
| 5 | Assess impact/probability | Critical vs. acceptable risks. |
| 6 | Apply controls | Encryption, validations, authentication. |

---

## PASTA Framework – Phased Security Process

**PASTA** = **P**rocess for **A**ttack **S**imulation and **T**hreat **A**nalysis**

A modern and structured approach to assessing risks by aligning security and system objectives.

| Phase | Approach | Application in What2SMS |
|------|----------------------------|-----------------------------------------------|
| I | Business objectives | Redirect WhatsApp to SMS without apps. |
| II | Technical scope | WhatsApp Web, modular backend, SMS gateway, sockets. Technologies prioritized for their compatibility with classic mobile devices, low resource requirements, and ease of integration without closed dependencies. <br><br>**Justification:**<br>• *WhatsApp Web* allows capturing messages without an official API.<br>• *Sockets* allow real-time communication without polling.<br>• *Modular backend* allows scaling, splitting functions, and applying layered security.<br>• *API SMS Gateway* guarantees delivery to unconnected devices.<br>• *PKI and tokens* ensure authentication and spoofing protection.<br>• *AES/SHA-256* encrypt sensitive data in transit and at rest.<br>• *SQL* guarantees integrity, persistence, and auditing. |
| III | Data flow modeling | WhatsApp → Backend → SMS → User. |
| IV | Threat identification | Unauthorized access, spoofing, exposure. |
| V | Vulnerability analysis | Open APIs, weak validations. |
| VI | Attack simulation | Sniffing, abusive automation. |
| VII | Controls and Mitigations | Encryption, Authentication, Flow Control |

---

## **What2SMS Under the PASTA Microscope**

This model allows you to anticipate real risks and apply specific defenses to mitigate identified threats:

---

### 🔐 **Main Risks:**

- **SMS Interception**: Risk of messages being read by unauthorized actors.
- **Wrong Redirects**: Possibility of messages being diverted to the wrong recipients.
- **Exposure of Sensitive Data**: Unauthorized access to personal or confidential information.
- **External Policy Violations**: Misuse of WhatsApp and other platforms without proper permissions.

---

### 🛡️ **Applied Defenses:**

#### ✔️ **Issuer Validation**
**What is it?**
Confirms that the origin of a message is legitimate, preventing impersonation.

**How ​​is it implemented?**
- **Digital signatures (PKI)**: Verification using public and private keys.
- **Number/issuer whitelist**: Acceptance only from trusted sources.
- **JWT token validation**: Ensures the authenticity of the sender through secure authentication.

---

#### 🔒 **End-to-End Encryption (E2EE)**
**What is it?**
Protects the confidentiality of messages throughout the entire process, from sender to receiver.

**How ​​is it implemented?**
- **AES-256**: Strong message encryption in transit.
- **RSA/ECDH**: Secure key exchange between the sender and receiver.
- **Final decryption**: The message can only be read by the receiver, with no exposure during transit.

---

#### 📝 **Activity Logs and Alerts**
**What is it?**
Constant monitoring of actions performed by the system to detect incidents and generate reports.

**How ​​is it implemented?**
- **Logging Systems**: Examples such as ELK stack, Loki, or Fluentd to store detailed logs (IP, time, action, user).
- **Automatic alerts**: Notification by email or webhook in the event of suspicious or anomalous activity.

---

#### ⚙️ **Automation Control**
**What is it?**
Prevents abuse of the service by bots or unauthorized users.

**How ​​is it implemented?**
- **Rate limiting**: Restrictions on the number of messages per minute (e.g., 10 messages/minute per user).
- **Captchas**: Human vs. machine validations to prevent automated interactions.
- **Automated behavior detection**: Use of machine learning algorithms or predefined rules to identify and block suspicious activity.

---

## How do we approach it?

- We apply **PASTA** as an analytical framework.
- We use **DFDs** and **attack trees** to visualize flows and threats.
- We design controls from the beginning of development.

---


## Final Evaluation

We have built a resilient system focused on security, knowing that **protection is not static**. We will continue to audit and adapt the model as the project and its risks evolve.

---

## References

- [OWASP Threat Modeling Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html)

---

##  Visual Representations

### 🔄 Data Flow Diagram (DFD - Level 1)

```plaintext
+-------------+       SMS/API        +------------+        SQL         +--------------+
|    User     |  <---------------->  |  Backend   | <----------------> |   Database   |
|  (Clamshell)|                      |  (Server)  |                    |  (Messages)  |
+-------------+                      +------------+                    +--------------+
       |                                  |
       |         WhatsApp Web             |
       +------------------------------->  |
             (Capture and Forwarding)
```

---

###  Attack Tree (Simplified Summary)

```plaintext
                         [Access to Messages]
                                 |
            +--------------------+----------------------+
            |                                           |
     [Intercept SMS]                          [Compromise Backend]
            |                                           |
   +--------+--------+                        +--------+--------+
   |                 |                        |                 |
[SMS Spoofing]   [GSM Sniffing]        [SQL Injection]   [API Token Leak]
```

---

## **Security Team Involvement**

The **Threat Modeling** process was implemented in 6 key phases:

---

1. **Asset Identification**
Critical assets such as messages, keys, and tokens.

2. **Attacker Profiles**

Consideration of threats such as spoofers, interceptors, and insiders.

3. **Attack Vectors**

Identification of potential vectors: phishing, hijacking, and DoS.

4. **Attack Tree Design**

Application of the PASTA methodology to structure potential attacks.

5. **Risk Assessment**

Vulnerability analysis using CWE, CVE, and OWASP references.

6. **Controls and Mitigations**

Implementation of PKI, dual tokenization, and alerts to protect assets.

---

## PASTA Framework Applied to What2SMS

| Stage | Action | Applied Example |
|----------------------------------------|------------------------------------------------------------------------|------------------------------------------------------------------------|
| **I. Business Objectives** | Ensure receipt of important SMS messages. | Critical WhatsApp messages delivered to simple mobile devices. |
| **II. Technical Scope** | WhatsApp Web → Backend → SMS Gateway. | Integration of sockets, APIs, and reader bots. |
| **III. Data Flow** | Capture → Validation → Conversion → SMS Sending. | Represented with DFD. |
| **IV. Threat Identification** | Spoofing, sniffing, DoS to the gateway. | Based on STRIDE. |
| **V. Vulnerability Analysis** | Open API, exposed backend, unencrypted SMS. | Scanning for technical and design vulnerabilities. |
| **VI. Attack Simulation** | SIM cloning, SQL injection, token replay. | Attack trees are built. |
| **VII. Recommended controls** | PKI, monitoring, strong authentication. | Defense in depth. |

---

## Lessons Learned

### 1. **Technological simplicity as an advantage**
> "Adapting the modern to the classic is also innovation."

- Basic mobile phones are still useful.
- Inclusion must consider technological limitations.
- What matters is access, not sophistication.

### 2. **Responsible integration**
- WhatsApp does not allow automated integrations.
- Unofficial APIs should be used with caution.
- Legal, technical, and ethical risks are present.

### 3. **Threat Modeling from the Start**
- Anticipating threats from the design stage avoids surprises.
- PASTA helps structure security from the ground up.
- Attack trees make the exposure surface tangible.

### 4. **SMS is not secure**
- It lacks end-to-end encryption.
- Spoofing, SIM swapping, and sniffing are real threats.
- Validation and encryption are required from the backend.

### 5. **Modular and robust backend**
- The backend is the nerve center of the system.
- Separating roles and responsibilities improves control.
- Event-driven architecture enables scalability and resilience.

### 6. **Security/usability balance**
- Do not overload the user with simple mobile devices.
- Frictionless security should be a goal.
- Automatic verification without interaction whenever possible.

### 7. **Multidisciplinary collaboration**
- Security, development, design, and user must collaborate.
- Cross-functional views reduce errors.
- Threat modeling is not the task of a single role.

---

## Conclusion

**What2SMS** is a project that recaptures the power of simplicity, adapting modern technology to classic contexts. But in doing so, it faces security challenges that must be addressed from day one.

The use of the **PASTA framework** together with the **Threat Model Process** ensures a secure, understandable, and scalable architecture. This approach allows for protection without complication, innovation without neglecting accessibility, and progress without leaving anyone behind.

---

> Security is not a product, but a continuous process.
> The best defense begins with the design.

---
**Created by**: Gerardo Arca López
**Date**: April 21, 2025

---




