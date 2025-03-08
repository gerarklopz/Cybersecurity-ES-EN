# Security Incident Report - EN ver. 🇺🇸🇬🇧

## 1. Executive Summary

This report documents a security breach on the website **yummyrecipesforme.com**, where an attacker gained unauthorized access to the admin panel, modified the source code, and introduced a malicious file that compromised user security.

The attack was made possible due to the use of default credentials for the admin account, facilitating a brute-force attack. As a result, website visitors were redirected to a malicious site, and their devices were affected.

It is recommended to implement **two-factor authentication (2FA)**, review logs, restore backups, and strengthen server access security.

---

## 2. Incident Description

**Incident Date:** 2025-03-08 
**Affected Company:** yummyrecipesforme.com 
**Impacted Area:** Website and affected customers

### 2.1. Attack Details

- An attacker gained access to the admin panel via a **brute-force** attack.
- The website's source code was modified, including a **JavaScript script** prompting visitors to download a malicious file.
- Upon execution of the file, users were **redirected to a fake website** (`greatrecipesforme.com`) that compromised their devices.
- The legitimate website lost credibility and generated multiple customer complaints.

### 2.2. Incident Evidence

Below are pieces of evidence extracted from traffic analysis with `tcpdump`:

| **Step** | **Action** | **Explanation** |
|----------|-----------|----------------|
| 1 | DNS Request to `yummyrecipesforme.com` | The browser requests the IP of the legitimate domain. |
| 2 | DNS Response | The real IP of the website is returned. |
| 3 | HTTP Request | An HTTP connection is established (port 80). |
| 4 | Malware Download | A malicious file is automatically downloaded. |
| 5 | DNS Request to `greatrecipesforme.com` | The IP of the fraudulent site is requested. |
| 6 | DNS Response | The IP of the malicious server is received. |
| 7 | HTTP Request to `greatrecipesforme.com` | A connection is established to the attacker’s website. |

---

## 3. Recommendations to Prevent Future Attacks

### 3.1. Implement 2FA (Two-Factor Authentication)

To prevent unauthorized access to the admin panel, it is recommended to activate **two-factor authentication (2FA)** on admin accounts and server access.

**Implementation in three key environments:**

1. **For server access (SSH on Ubuntu):**
   - Install `libpam-google-authenticator`.
   - Configure 2FA codes on each admin account.
   - Enable authentication in the SSH configuration file.

2. **For AWS Console access (if used for hosting):**
   - Enable MFA on IAM accounts.
   - Use Google Authenticator or Authy.

3. **For user access on the website (using AWS Cognito):**
   - Set up a **User Pool** in Amazon Cognito.
   - Enable 2FA via SMS or app.
   - Integrate Cognito with the site’s API to protect authentication.

4. **Implement HTTPS for YummyRecipesForMe.com**

   - To improve the security of the website **yummyrecipesforme.com**, it is recommended to implement **HTTPS** and use **VPNs** for system administration. These measures will help protect the integrity of the site and the security of administrators and customers.

### **Why use HTTPS?**
✅ **Encrypts communication** between the user and server, preventing MITM (Man-in-the-Middle) attacks. 
✅ **Builds trust** with visitors, as browsers warn about insecure HTTP sites. 
✅ **Protects credentials and sensitive data** from being intercepted in transit.

### **Steps to implement HTTPS**
   **Obtain an SSL/TLS certificate:**
   - It is recommended to use **Let’s Encrypt** (free) or **Cloudflare SSL**.
   - Alternatively, purchase a certificate from **DigiCert, GlobalSign, or Sectigo**.

---

5. **Using VPNs for Secure Administration on YummyRecipesForMe.com**

### **Why use a VPN?**
✅ **Hides the real IP of the administrator**, preventing targeted attacks. 
✅ **Encrypts data traffic**, protecting it from surveillance on public networks. 
✅ **Bypasses geographic blocks and access restrictions** to the site’s administration.

### **Recommendations for the IT team**
1. **Use a reliable VPN** like **NordVPN, ExpressVPN, or ProtonVPN**.
2. **Set up a corporate VPN** on a self-hosted server using **WireGuard or OpenVPN**.
3. **Restrict administrative access** to connections from authorized IP addresses only.
4. **Enable two-factor authentication (2FA)** for admin panel access.

---

### 3.2. Additional Measures

✅ **Log Review**: Monitor access attempts and detect suspicious activity. 
✅ **Password Change**: Implement strong passwords and avoid default credentials. 
✅ **Backup Restoration**: Ensure no data has been compromised before restoring backups. 
✅ **Official Customer Notification**: Inform users about the incident and steps to protect themselves.

---

## 4. Conclusion

This attack could have been prevented with better security practices, such as two-factor authentication and access monitoring. Implementing these measures will significantly reduce the likelihood of similar attacks in the future.

It is recommended to proceed with corrective actions immediately and establish a long-term security plan to prevent similar incidents.

**Signature:** 
Gerardo Arca López 
**Position:** Cybersecurity Analyst (Internship Student) 
**Date:** 2025-03-08 

