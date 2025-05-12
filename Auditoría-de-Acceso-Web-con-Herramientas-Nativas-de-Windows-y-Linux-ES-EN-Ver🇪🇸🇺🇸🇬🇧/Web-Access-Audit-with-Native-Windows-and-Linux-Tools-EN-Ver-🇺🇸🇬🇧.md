# 🛡️ Web Access Audit with Native Windows and Linux Tools

**Authors:** Gerard and research team
**Date:** May 12, 2025
**Objective:** To investigate whether an employee has accessed the domain `www.example.com` using only native or low-profile tools on Windows and Linux. The goal is to create a clear and detailed report, bring it to the desktop as evidence, and provide a way to clean up traces without compromising system integrity.

---

## 🧭 Justification

In corporate or professional environments where the use of external software is not permitted or where it is crucial to avoid invasive tools, we have been able to efficiently perform web access audits using only native Windows and Linux utilities. This approach is especially useful when additional software is not permitted or when we are looking to minimize the footprint of our actions during an audit.

In this report, we detail the process we followed to detect, log, and report access to specific domains, such as `www.example.com`, using both native Windows and Linux tools. Using these tools, we were able to generate a forensic analysis of potential access to this domain, compare results across platforms, and present the findings in a structured manner, ensuring that we do not inadvertently alter or compromise the system. Additionally, we include the ability to clean up traces of our investigation, always respecting the ethical and legal context in which it is conducted.

---

## 🖥️ Tools and Approach Used

### Tools in Windows:
In Windows, we used low-profile tools such as `nslookup`, `curl`, `Get-WinEvent`, and `Get-Process` to obtain critical network, process, and connection information. These tools allowed us to track activity related to web domain access and generate detailed reports without the need for elevated administrator permissions.

### Tools on Linux:
On the other hand, on **Linux**, we used native tools such as `getent`, `curl`, `ss`, `ps`, `journalctl`, `lsof`, and `find` to perform similar audits. These tools allowed us to conduct the same web access investigation in a different environment, comparing the effectiveness and ease of use of the native utilities between the two operating systems.

### Comparison: **tshark** and **tcpdump**
Regarding network traffic capture and analysis, on both **Windows** and **Linux**, we used tools such as **tshark** and **tcpdump**. Both tools allow detailed network packet capture, but have key differences in terms of usability and operating system compatibility:

| Tool | Operating System | Functionality | Why is it relevant? |
|-------------|---------------|----------------|-----------------------|
| **tshark** | Windows / Linux | Network packet capture and analysis | Allows detailed analysis of HTTP/HTTPS traffic with advanced filtering options. Requires root permissions on Linux, but is powerful for in-depth audits. |
| **tcpdump** | Windows / Linux | Network packet capture | Simpler and more widely available tool on Linux. It provides fast and straightforward traffic analysis without the complexity of `tshark`. |

Both tools are essential when looking to investigate domain access and connections in detail, allowing us to compare observed traffic and confirm whether access to `www.example.com` was made on both operating systems.

---

### **Why is this approach relevant?**

Using native tools offers several key advantages:
- **Low profile:** We don't generate new traces that could reveal our activities or interfere with system operation.
- **No need for administrator permissions:** In many cases, we have been able to perform detailed audits without requiring elevated privileges, making it easier to work in restricted access environments.
- **Efficiency:** Despite not having external tools, we have been able to access critical information about network connections, DNS resolution, and active processes related to web browsing, on both Windows and Linux.

In this way, we offer a solution for both one-time audits and more in-depth investigations, without compromising the integrity or security of the system, regardless of the operating system.

---

### **Clearing Traces:**

Regarding clearing traces, we have included the process in Windows using the `Clear-EventLog` command, warning that its use should only be done in controlled environments and with proper authorization. In Linux, the approach was more technical, without the need for invasive commands, but rather based on viewing and analyzing logs without compromising the integrity of the system data.

---

### Conclusion

This procedure demonstrates how, using only native and low-profile tools in **Windows** and **Linux**, it is possible to:

- Verify DNS activity.
- Query HTTP responses.
- Trace network routes.
- Generate exportable reports.
- Securely clear traces.

The method is ideal for software-restricted environments and can be expanded to more comprehensive audits by integrating scripts, scheduled tasks, or alerts. It adapts to both Windows and Linux environments, with the added benefit of not requiring administrator permissions in many cases. """

---

## 🧰 Tools Used

| Tool | Type | Main Use |
|------------|------|---------------|
| `nslookup` | CMD | DNS Resolution |
| `Resolve-DnsName` | PowerShell | Advanced DNS Queries |
| `curl` | CMD / PowerShell | HTTP Header Check |
| `tracert` | CMD | Trace Routes |
| `Get-DnsClientCache` | PowerShell | View Local DNS Cache |
| `Get-WinEvent` / `Get-EventLog` | PowerShell | Reviewing System Events |
| `Findstr` | CMD | Log File Filtering |
| `Export-Csv` | PowerShell | Report Generation |

***

## 📡 Steps Performed

### 1. Check if `www.example.com` is present in the DNS cache
```powershell
Get-DnsClientCache | Where-Object { $_.Name -match "example.com" }
```
## 📌 If the site was recently visited, it will appear here.

## 2. Get more domain information

```powershell
Resolve-DnsName -Name www.example.com -Type A
Resolve-DnsName -Name example.com -Type MX
curl -I https://www.example.com
```
***
## 3. Review browsing history (if possible)

⚠️ In professional environments, you should not directly access user profiles without authorization. However, if necessary and you have permissions, you could review it:
```powershell
Get-ChildItem "C:\Users\*\AppData\Local\Microsoft\Windows\WebCache" -Recurse
```
### 4. View network event logs

```powershell
Get-WinEvent -LogName "Microsoft-Windows-DNS-Client/Operational" |
Where-Object { $_.Message -match "example.com" } |
Select TimeCreated, Message |
Export-Csv "$env:USERPROFILE\Desktop\informe_dns.csv" -UTF8 Encoding
```
---

## 📊 Network and System Forensics

This block complements the DNS audit with network and system data extracted during the investigation. It provides additional evidence of possible access to the domain `www.example.com`.

---

### 🔌 Active TCP connections

```plaintext
LocalAddress LocalPort RemoteAddress RemotePort State OwningProcess
------------ --------- --------------- ---------- ---------- -------------
10.0.2.15 50370 3.233.158.26 443 Established 6412
10.0.2.15 50369 150.171.29.11 443 Established 6412
10.0.2.15 50367 2.16.54.142 80 Established 2332
10.0.2.15 50360 3.233.158.24 443 Established 6412
10.0.2.15 50176 3.233.158.24 443 Established 6412
10.0.2.15 50148 172.64.155.209 443 Established 6412
10.0.2.15 50146 172.64.155.209 443 Established 6412
10.0.2.15 49973 4.207.247.137 443 Established 2764
```
## 📝 Interpretation:

* Process 6412 is making multiple HTTPS connections, likely browsing the web (possibly Microsoft Edge, see processes).

* Process 2332 is communicating on port 80 (HTTP).

* Destination 172.64.155.209 belongs to Cloudflare, indicating CDN/web usage.

## 📥 DNS Cache

```plaintext 207.red-81-46-38.customer.static.ccgg.telefonica.net
29.red-81-46-45.customer.static.ccgg.telefonica.net
169.red-81-46-44.customer.static.ccgg.telefonica.net
a.nel.cloudflare.com -> 35.190.80.1
1.red-80-58-78.staticip.rima-tde.net
a2-23-212-34.deploy.static.akamaitechnologies.com
```
## 📝 Interpretation:

* **a.nel.cloudflare.com** and **akamaitechnologies.com** records involve contact with common CDNs used in web browsing.

* All PTRs point to residential providers in Spain, confirming legitimate, not malicious, traffic.

## 🧠 Relevant Active Processes

```plaintext
PID Name CPU (%) Memory (MB)
---- ------------- -------- -------------
6412 msedge 23.81 39.5
7112 msedge 230.54 160.7
8104 msedge 1325.30 734.2
3252 powershell 25.59 106.8
1560 svchost 39.53 53.6
1000 dwm 67.59 53.1
2288 MsMpEng 56.87 162.5
```
## 🔗 Relationship between PID and Connections

To confirm which processes are making connections, you can cross-reference the PID with the **OwningProcess** column in the output. `netstat`:
```plaintext
| PID (of processes) | Match in netstat | Interpretation |
|-------------------|--------------------------|---------------------------------------|
| `6412` | Present | Multiple active HTTPS connections |
| `2332` | Present | Access via HTTP |
| `3252` | Possible PowerShell session | Manual analyst activity |
```

🔍 This allows processes to be associated with remote domains/contacts observed during the audit.

## 📝 Comments:

* Multiple instances of Microsoft Edge (msedge) are detected with high CPU and memory usage.

* The active PowerShell process suggests manual console activity (possibly by the analyst).

* MsMpEng (Microsoft Defender) is also running normally.

## 📁 Export as Evidence
It is recommended to export these three sets as individual files on the desktop:
```plaintetxt
netstat -anob | Out-File "$env:USERPROFILE\Desktop\tcp_connections.txt"
Get-DnsClientCache | Out-File "$env:USERPROFILE\Desktop\dns_cache.txt"
Get-Process | Sort CPU -Descending | Out-File "$env:USERPROFILE\Desktop\active_processes.txt"
```
---
## 🧼 Cleanup: How to Erase Traces

Delete Specific Logs (⚠️ only if ethically permitted)
#This command can erase evidence and should only be used in laboratories or controlled environments.

⚠️ **IMPORTANT WARNING:**
The use of `Clear-EventLog` may be considered **evidence tampering** in real-life environments.

This step **should only be performed in test environments, for educational purposes, or with explicit authorization.**

🔐 **Additional Note:**
In real-life audits or legal proceedings, evidence preservation is critical. Any deletion action must be documented, authorized, and within protocols accepted by the organization or applicable legal framework.

```powershell
Clear-EventLog -LogName "System"

```
## 📁 Export Report
```powershell
# Already created: dns_report.csv on the desktop
Start-Process "$env:USERPROFILE\Desktop\dns_report.csv"
```
* You can also generate a .txt or .md report with personal comments:
```powershell
"Report generated on $(Get-Date)" | Out-File "$env:USERPROFILE\Desktop\web_report.md"
```
## Bonus: ✅ PowerShell Commands Used Without Administrator Permissions

| #️⃣ | PowerShell Command | Description | Relevance | |
| --- | ------------------------------------------------------------------------------ | ---------------------------------------------- | ------------------------------------------------------------------------ | ------------------------------------------------------------------------ |
| 1️⃣ | `Get-DnsClientCache` | Displays the local DNS cache | Detects if the domain was recently resolved on the computer. | |
| 2️⃣ | `Resolve-DnsName -Name www.example.com -Type A` | Query the DNS for a specific domain | Allows you to find the IP address of a domain and confirm the connection. | |
| 3️⃣ | `Get-WinEvent -LogName "Microsoft-Windows-DNS-Client/Operational"` | Displays DNS client event logs | Reviews DNS resolution activity from the system. | |
| 4️⃣ | \`Get-Process | Sort CPU -Descending\` | Displays processes in order of CPU usage | Allows you to identify processes with high resource usage, such as browsers. |
| 5️⃣ | `Export-Csv "$env:USERPROFILE\Desktop\informe_dns.csv"` | Export data to CSV | Save audit results for later review. | |
| 6️⃣ | `Get-NetTCPConnection` | Displays active TCP connections | Detects active connections to specific domains/ports. | |
| 7️⃣ | `Findstr` (to filter logs) | Filters event results or log files | Helps locate events or connections related to a domain. | |
| 8️⃣ | `Get-EventLog` | Reviews system event logs | Detects event activity and network access. | |
| 9️⃣ | `Get-ChildItem "C:\Users\*\AppData\Local\Microsoft\Windows\WebCache" -Recurse` | Displays web cache files | Possible traces of recent web browsing. | |
| 🔟 | `curl -I https://www.example.com` | Checks a website's HTTP headers | Confirms the availability of a website with an HTTP response code. | |

---
## ✅ Conclusion
This procedure demonstrates how, without external tools, it is possible to:

* Verify DNS activity.

* Query HTTP responses.

* Trace network routes.

* Generate exportable reports.

* Securely clean up traces.

The method is ideal for software-restricted environments and can be expanded to more comprehensive audits by integrating scripts, scheduled tasks, or alerts.

---

# 🛡️ Web Access Audit with Native Linux Tools

Objective: Detect if a user accessed www.example.com using only default tools included in common Linux distributions (Debian, Ubuntu, Fedora, CentOS, etc.).

## 🧾 Express Web Audit (Native Linux Tools Only)
Objective: Confirm if www.example.com was accessed without using external software

## 📊 Summary of Effective Commands in Local Web Auditing

| #️⃣ | Custom Command | What was investigated? | Why is it relevant? | |
| --- | ---------------------------------------------------------------------------- | ------------------------------------------------------------------- | ---------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| 1️⃣ | `getent hosts www.example.com`<br>`host www.example.com` | Previous DNS resolution (IPv4/IPv6) | ✔️ If it has already been resolved, there was probably a browsing attempt | |
| 2️⃣ | `curl -I https://www.example.com`<br>`wget --spider https://www.example.com` | HTTP response from the server | ✔️ We confirm that the server responded (`200 OK`), evidencing successful access | |
| 3️⃣ | \`ss -ntp | grep ':443\|:80'\` | Active TCP connections to web ports (HTTPS) | ✔️ We saw active connections to remote IPs, possibly from `example.com` or a CDN |
| 4️⃣ | \`ps aux | grep -i chrome\` | Active processes | ✔️ `chrome` was running; possible browser used to access the web |
| 5️⃣ | \`journalctl | grep resolved`<br>`journalctl -u systemd-resolved\` | System logs over DNS | ⚠️ Doesn't show direct access, but confirms recent name resolution activity |
| 6️⃣ | `find ~/.config/google-chrome -type f -iname '*Cache*'` | Trace in local browser caches | ✔️ Recent Chrome cache files suggest website browsing | |
| 7️⃣ | \`lsof -i :443 | grep chrome\` | Processes with open HTTPS sockets | ✔️ Chrome with encrypted connections — clear evidence of real-time browsing |
| 8️⃣ | `mkdir -p ~/web_audit` | Create a dedicated directory to store results | ✔️ Evidence organization — good practice for reproducible audits |
| 9️⃣ | `ss -ntp > ~/web_audit/connections.txt` | Save active TCP connections along with processes | ✔️ Real-time capture — useful for detecting suspicious connections |
| 🔟 | `ps aux > ~/web_audit/processes.txt` | List all active processes and dump them to a file | ✔️ Full system view — basis for correlation with ports and processes |
| 1️⃣1️⃣ | `curl -I https://www.example.com > ~/web_audit/curl_response.txt` | Get and save HTTP headers from the visited site | ✔️ Web service verification — allows you to audit status and headers without downloading content |
| 1️⃣2️⃣ | `cat ~/web_audit/*.txt` | View the contents of generated files | ✔️ Direct verification — confirms that the files have been generated correctly |
| 1️⃣3️⃣ | `ls -la ~/web_audit` | List all generated files in detail | ✔️ Integrity control — ensures correct presence and permissions of logs |

## ✅ CONCLUSION
We performed an accurate and effective audit without installing any external tools, using only the native capabilities of the Linux operating system. This is great for restricted environments or quick forensic audits.

## Bonus: 🧠 Comparison Table: Native Web Audit vs tshark / tcpdump

| #️⃣ | Target | Windows CLI/PowerShell | Native Linux | `tshark` Equivalent | `tcpdump` Equivalent | |
| --- | ---------------------------------- | --------------------------------------------------- | -------------------------------------------------------- | --------------------------------------------------- | ------------------------------------------------------------- | --------------------- |
| 1️⃣ | View DNS Resolution | `Resolve-DnsName www.example.com` | `host www.example.com`<br>`getent hosts www.example.com` | `tshark -Y "dns.qry.name == \"www.example.com\""` | `tcpdump -n port 53 and udp and host www.example.com` | |
| 2️⃣ | View HTTP response (headers) | `curl -I https://www.example.com` | `curl -I https://www.example.com`<br>`wget --spider` | `tshark -Y "http.request and ip.dst==example.com"` | `tcpdump -A -s 512 'tcp port 80 or 443 and host example.com'` | |
| 3️⃣ | View active TCP connections | `netstat -ano` | `ss -ntp` | `tshark -Y "tcp.flags.syn==1 and tcp.flags.ack==1"` | `tcpdump -n tcp` | |
| 4️⃣ | View processes browsing | `Get-Process`<br>`tasklist /fi "PID eq 6412"` | \`ps aux | grep chrome\` | — (check PID external to `tshark`) | — (does not cover processes) |
| 5️⃣ | View DNS logs in system | `Get-WinEvent -LogName DNS-Client/Operational` | `journalctl -u systemd-resolved` | `tshark -f "port 53"` | `tcpdump -vvv port 53` | |
| 6️⃣ | Detect trace in browser cache | Search `WebCache` with `Get-ChildItem` | `find ~/.config/google-chrome -iname "*Cache*"` | — (traffic only, not local files) | — | |
| 7️⃣ | Identify open HTTPS sockets | `netstat -anob` (with PID)<br>`Get-NetTCPConnection` | `lsof -i :443`<br>\`ss -ntp | grep :443\` | `tshark -Y "tcp.port == 443"` | `tcpdump -n port 443` |
| 8️⃣ | Export evidence | `Out-File`, `Export-Csv` | `> file.txt` | `tshark -w evidence.pcap` | `tcpdump -w evidence.pcap` | |
| 9️⃣ | Trace routes to destination IP | `tracert www.example.com` | `traceroute www.example.com` | `tshark -Y "icmp"` | `tcpdump -n icmp` | |
| 🔟 | View web traffic in real time | (Not easily native) | (Nor without external tools) | `tshark -i eth0 -Y "http"` | `tcpdump -i eth0 -A -s 512 'tcp port 80 or 443'` | |

