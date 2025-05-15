## 🧾 Artifact Analysis (Hash Spreadsheet) + VirusTotal and Pyramid of Pain

**Activity Objective**:
Analyze a suspicious file (malware) using VirusTotal and classify indicators of compromise (IoCs) found according to how difficult they are for the attacker to block, using the Pyramid of Pain model.

**Tools Used**:

*VirusTotal*: Platform that analyzes files, domains, IPs, and URLs to identify threats.

*SHA256 Hash*: Unique signature of the suspicious file.

**Realistic Scenario**:
A junior analyst (level 1) in a SOC (Security Operations Center) at a financial firm receives an alert:

"An employee received an email containing a password-protected spreadsheet.

Upon opening it, a malicious payload executes.

The Intrusion Detection System (IDS) generates an alert after detecting multiple executables."

**Suspicious file hash**:
```powershell
SHA256: 54e6ea47eb04634d3e87fd7787e2136ccfbcc80ade34f246a12cf93bab527f6b
```

**Timeline**:

Time and events:

* 1:11 PM Employee receives email with attachment
* 1:13 PM Downloads and opens file using password
* 1:15 PM Multiple unauthorized executables are created
* 1:20 PM IDS detects suspicious activity and triggers an alert

**VirusTotal scan steps**:

* Search for the hash to see the file analysis

**Explore the following sections**:

**Sections and what to look for**

* Detection: How many antivirus programs flag it as malicious? What malware name appears?
* Details Other hash versions: MD5, SHA1
* Relations Which IPs/domains are they connecting to? Are they malicious?
* Behavior Actions observed in a sandbox environment: processes, traffic, MITRE ATT&CK techniques

## CYBERSECURITY: DETAILED EXPLANATIONS AND ANSWERS

| Concept | Didactic Explanation | Is it installed by default? | Useful Comments |
| ------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------ |
| **IDS (Intrusion Detection System)** | A system that **detects intrusions or anomalous behavior** on a network or computer, without blocking them. It simply generates alerts. | ❌ It is not installed by default on Windows or Linux. It is installed separately (e.g., **Snort**, **Suricata**, **OSSEC**) | Not to be confused with **IPS**, which does block. IDS is like a "silent alarm." Ideal for SOCs. |
| **Malicious Payload** | It is the **active part** of malware that executes a harmful action: stealing data, opening backdoors, encrypting files, etc. | - | Every exploit has **an entry vector + a payload**. It is what **does** the damage after exploitation. |
| **Password-enabled spreadsheet** | Even if it has a password, it can hide **malicious macros**, **embedded scripts**, **vulnerabilities**. | ⚠️ You should not rely on the password alone. | ​​Passwords in Office files do not always use strong encryption. It can be broken with tools like **John the Ripper** or **hashcat**. |
| **SHA-256** | Secure cryptographic hash algorithm, from the SHA-2 family. Produces a 256-bit hash. It is used to verify data and file integrity. | ✅ Widely used in cybersecurity (signatures, integrity, passwords). | Much more secure than MD5. |
| **MD5** | Old hash algorithm, now considered **insecure**. Vulnerable to **collisions** (two different inputs can have the same hash). | - | You still see it in **old checksums**. Not recommended for new implementations. |
| **AV Engines (Antivirus Engines)** | These are the "brains" of antivirus software: they use signatures, heuristics, sandboxing, and machine learning to detect threats. | ✅ Windows: **Windows Defender** comes by default. Linux: It usually doesn't include AV, but you can install it (e.g., **ClamAV**, **Sophos**, etc.) | These are accompanied by cloud services for deep analysis. |
| **Mimikatz** | Post-exploitation tool used to **extract credentials** from Windows (hashes, passwords, Kerberos tickets). | ❌ Not installed. Must be downloaded by the attacker. | Very powerful. Detected by most AVs. Classic pentester tool. |
| **PsExec** | Sysinternals tool for executing processes on remote systems. **Used legitimately**, but also by attackers. | ❌ Not included by default, but is from Microsoft. | It's a classic **Living off the Land** (LOLBins) tool. Be careful if you see it in environments where it shouldn't be. |
| **Netcat (nc)** | Network tool for reading/writing data over TCP/UDP. Used for scans, reverse shells, etc. | ✅ Linux: Usually included. Windows: No. Must be installed. | Nicknamed the "Swiss Army knife of networks." If used maliciously, it can be a significant threat.

## 📊 Maliciousness Assessment
Key indicators to determine if the file is malicious:

🔺 High vendors' ratio (many AV engines detect it)

👎 Negative community score (analyst opinion)

⚠️ Malware detection by name and typical behavior

✅ If it meets these indicators, the file should be considered malicious.

## Pyramid of Pain – Classification of Indicators of Compromise (IoCs)

A concept by David Bianco on the effectiveness and difficulty of defending against cyberattacks based on the type of indicator used.

| Level | Indicator Type | Blocking Difficulty | Pain for the Attacker | Example |
|-------|-----------------------------------------|------------------------|------------------------|------------------------|
| 🔺 6 | TTPs (Tactics, Techniques, Procedures) | Very Difficult | Very High | Remote Powershell, Lateral Movement |
| 🔻 5 | Tools | Difficult | High | Mimikatz, PsExec, Netcat |
| 🔻 4 | Infrastructure (C2: IPs/Domains) | Medium-High | Medium | `bad-domain[.]com`, `192.168.2.5` |
| 🔻 3 | File hashes | Medium | Low | `SHA256:54e6ea...` |
| 🔻 2 | Specific IPs | Easy | Very low | `185.72.25.7` |
| 🔻 1 | File names | Very easy | Null | `invoice_protected.xlsm` |

---

> The higher up the pyramid you are, the harder it is for an organization to defend itself, **but the more damage we cause to the attacker if implemented correctly**.

This document shows how to identify Indicators of Compromise (IoCs) based on their difficulty level to replace, following the **Pyramid of Pain**. It includes practical commands for **Windows** and **Linux** systems.

## 🔴 Very difficult to replace
**IoC Type**: TTPs (Tactics, Techniques, Procedures)
**Description**: Attacker behaviors such as persistence, privilege escalation, remote execution, etc.

### 🪟 Windows
``` powershell
Get-WinEvent -LogName Security | Where-Object { $_.Message -match "PowerShell" }
```
### Linux
``` powershell
ausearch -k exec
journalctl | grep -i powershell
auditctl -l
```
## 🟠 Difficult to replace
IoC Type: Tools (Mimikatz, PsExec, netcat, etc.)
Description: Tools used for lateral movement or data exfiltration.

### 🪟 Windows
```powershell
tasklist /v | findstr /i "mimikatz"

Get-Process | Where-Object { $_.Path -like "*netcat*" }
```
### 🐧 Linux
```powershell
ps aux | grep -Ei "mimikatz|nc|nmap|psexec"
lsof -nP | grep '/tmp'
```
### 🟡 Moderately difficult to replace

**IoC Type**: Network or system artifacts
**Description**: Suspicious files, registry keys, services, or processes created by malware.

### 🪟 Windows
```powershell
reg query HKCU\Software\Microsoft\Windows\CurrentVersion\Run
Get-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run'
```

### 🐧 Linux
```powershell
find /etc/systemd/ -type f -name "*.service"
ls -alh /tmp /var/tmp
crontab -l
```
### 🟦 Easy to replace
**IoC Type**: Malicious domain name
**Description**: Domain the malware connects to.

### 🪟 Windows
```powershell
netstat -nob
Resolve-DnsName example.com
Get-DnsClientCache
```

### 🐧 Linux
```powershell
netstat -pant
dig example.com
tcpdump -i eth0 -nnn host bad-domain.com
```

### 🟦 Easy to replace
**IoC Type**: Malicious IP Address
**Description**: IP used as C2 (Command and Control) or for exfiltration.

### 🪟 Windows
```powershell
netstat -ano | findstr:443
Get-NetTCPConnection
```
### 🐧 Linux
```powershell
ss -tupn
netstat -antp
tcpdump -nn dst host 192.168.1.100
```

### 🟦 Very easy to replace
**IoC Type**: Hash (SHA256, MD5, etc.)
**Description**: Unique signature of the malicious file.

### 🪟 Windows
```powershell
Get-FileHash C:\path\file.exe -Algorithm SHA256
```
### 🐧 Linux
```powershell
sha256sum file.exe
md5sum file.exe
sha1sum file.exe
```

---

## 🧠 Notes and Best Practices
🔐 TTPs and tools are at the top of the pyramid because they change less and define the actor's attack style.

🔁 Hashes, IPs, and domains change easily; they are useful for alerts, but less effective for preventing persistence.

🎯 As an analyst, it's ideal to move your detection to the higher levels (TTPs, tools), where attackers have to redesign their campaigns.

**🧰 Useful Extras**:

View recent suspicious processes

**Windows (PowerShell)**:

```powershell
Get-WinEvent -LogName Security | Where-Object { $_.Id -eq 4688 } | Select-Object -First 10
```
**Linux**:
```powershell
grep -i "execve" /var/log/audit/audit.log
```
**List active connections by program**:

**Windows**:
```powershell
netstat -ano | findstr ESTABLISHED
```
**Linux**:
```powershell
ss -pant
```

## What should we include in the report submission:

* ✅ Clear statement of whether the file is malicious or not.

* ✅ Three distinct indicators of compromise:

*E.g., another hash, a malicious IP, a domain, a system artifact, a tool used, or a TTP*.

* ✅ Record these IoCs in the Pyramid of Pain template (one slide per type).

## Important Considerations

* VirusTotal shares files publicly: never upload sensitive or personal information.

* Don't rely on a single scan or tool: VirusTotal is part of a good analyst's toolset.

* Use the information to support your detection, analysis, and incident response capabilities.

## Personal Reflections:

# 🧠 Critical Questions about VirusTotal Scanning and Operational Security

| Question / Doubt | Critical and Professional Analysis |
|------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Shouldn't you be very naive to open a suspicious spreadsheet?** | **It depends on the context.** It's not always a matter of naivety; many times it's due to a lack of specific training or a corporate culture that doesn't prioritize cybersecurity. End users rarely act with a defensive mindset. In this sense, the failure is both human and systemic. |
| **Isn't it the operator's fault?** | **Partially.** In cybersecurity, the principle of **shared responsibility** applies. The user must maintain a certain level of judgment, but it is the organization's responsibility to establish controls, ongoing education, and secure environments to prevent these types of incidents. |
| **Relying on third-party software to upload sensitive files? Isn't that dangerous?** | **Indeed.** This is a very pertinent observation. VirusTotal is a public tool: uploading artifacts directly could compromise privacy and reveal behavior to attackers. In critical environments, internal solutions such as an on-premise sandbox should be used. |
| **Can attackers use the data we upload to VirusTotal?** | **Yes, definitely.** Advanced threat actors (APT) monitor VirusTotal to detect whether their samples have been uploaded and thus adapt their TTPs. This activity is known as **VT hunting**. Therefore, **sensitive samples should never be uploaded without considering the technical and strategic consequences.** |
| **Are we targets for attack simply by using third-party tools?** | **It can happen, although it is not automatic.** Uploading a hash is not usually problematic, but uploading files with **metadata, internal paths, or active macros** increases the risk. Furthermore, analyzing malware without anonymizing the connection (for example, without a VPN or Tor) exposes your IP to attackers. |

---

## Final Recommendations:
* Use controlled environments for malware analysis (private sandbox, disconnected virtual environment, etc.).

* Never upload files containing internal, personal, or confidential data.

* Continuously train staff in cybersecurity culture.

* Use VPNs or anonymizing environments when performing online analysis, especially with public tools like VirusTotal.

*Report author*: Gerardo Arca López

*Date*: May 15, 2025
