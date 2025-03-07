# Security Report: Analysis of the SYN Flood Attack ES ver. 🇺🇸🇬🇧

---

## Section 1: Identifying the type of attack

As security analysts, we have identified that the type of attack that has caused the service interruption on the travel agency's website is a **SYN Flood attack**. This type of attack is characterized by sending a large number of SYN requests to the server, which consumes its resources and prevents it from responding to legitimate requests.

### Evidence in Wireshark logs

# TCP/HTTP Traffic Analysis

## Legitimate Traffic (Green)
| No. | Time (sec) | Source | Destination | Protocol | Info | Type |
|------|------------|------------------|---------------|-----------|---------------|---------------|
| <span style="color:green">47</span> | 3.144521 | 198.51.100.23 | 192.0.2.1 | TCP | SYN (handshake start) | Legitimate |
| <span style="color:green">48</span> | 3.195755 | 192.0.2.1 | 198.51.100.23 | TCP | SYN-ACK (server response) | Legitimate |
| <span style="color:green">50</span> | 3.298223 | 198.51.100.23 | 192.0.2.1 | HTTP | GET /sales.html | Legitimate |
| <span style="color:green">51</span> | 3.349457 | 192.0.2.1 | 198.51.100.23 | HTTP | HTTP 200 OK | Legitimate |

## Attacks (Red)
| No. | Time (sec) | Source | Destination | Protocol | Info | Type |
|------|------------|-------------|-----------|-----------|-------------------------------|------------------|
| <span style="color:red">52</span> | 3.390692 | 203.0.113.0 | 192.0.2.1 | TCP | SYN (no subsequent ACK) | Attack |
| <span style="color:red">57</span> | 3.664863 | 203.0.113.0 | 192.0.2.1 | TCP | SYN repeated (flood pattern) | Attack |
| <span style="color:red">59</span> | 3.795332 | 203.0.113.0 | 192.0.2.1 | TCP | SYN with anomalous payload | Attack |

## Failed Connections (Yellow)
| No. | Time (sec) | Source | Destination | Protocol | Info | Type |
|------|------------|-------------|---------------|-----------|-------------------------------|------------|
| <span style="color:yellow">73</span> | 6.230548 | 192.0.2.1 | 198.51.100.16 | TCP | RST, ACK (connection interrupted) | Failed |
| <span style="color:yellow">77</span> | 7.330577 | 192.0.2.1 | 198.51.100.5 | HTTP | 504 Gateway Timeout | Failed |

---

### Detected Patterns 🔍
1. **Legitimate Traffic**:
- Full TCP handshake (`SYN → SYN-ACK → ACK`).
- Normal HTTP requests with `200 OK` responses.

2. **Attacks**:
- **SYN Flood**: IP `203.0.113.0` sends multiple SYNs without completing the handshake.
- **Anomalous Payloads**: SYN with `Len=120` (not common in normal traffic).

3. **Failed Connections**:
- `RST, ACK` responses from the server (possible saturation).
- HTTP `504` Timeout (blocking due to attack).

- We have observed a large number of SYN requests coming from an unknown IP address (**203.0.113.0**). This IP belongs to the **203.0.113.0/24** range, which is reserved for documentation and testing. However, in a real-world scenario, this IP could be fake or masked through techniques such as using VPNs or IP spoofing.
- These requests do not complete the three-step handshake (they do not send the final ACK), causing the server to reserve resources for each SYN request without releasing them.
- As a result, the server becomes overloaded and is unable to respond to legitimate requests from employees trying to access the website.

### Conclusion

The SYN Flood attack has caused the server to be overwhelmed by the volume of malicious traffic, resulting in a **"connection timed out"** error for legitimate users. In addition, this type of attack can generate other associated errors, such as:

- **HTTP 504 Errors (Gateway Timeout)**: The server does not respond in time due to overload.
- **RST (Reset) Packets**: The server unexpectedly closes connections due to lack of resources.

---

## Section 2: Explaining the Impact of the Attack

The SYN Flood attack directly affects website performance and user experience. Below we explain how this attack occurs and its effects.

### TCP Three-Step Handshake

1. **SYN**: The client sends a SYN request to the server to initiate the connection.
2. **SYN-ACK**: The server responds with a SYN-ACK, indicating that it is ready to establish the connection.
3. **ACK**: The client sends an ACK to confirm the connection.

In a SYN Flood attack, the attacker sends many SYN requests but does not complete the handshake, leaving the server waiting for responses that never arrive.

### Server Overload

- Each SYN request causes the server to reserve resources (memory and CPU) for the connection.
- By not receiving the final ACK, these resources are not released, eventually exhausting the server's capacity.
- As a result, the server is unable to respond to legitimate requests, resulting in the **"connection timed out" error.**

### Impact on the organization

- **Service interruption**: Employees are unable to access the website to search for vacation packages, which negatively impacts productivity and business.
- **Loss of reputation**: If customers are unable to access the website, they may lose trust in the company.
- **Financial costs**: Service interruption could result in financial losses, especially if the website is a major source of revenue.

### Immediate measures

1. **Block the attacking IP**: Configure the firewall to block the suspicious IP address (203.0.113.0).
2. **Enable SYN Cookies**: Enable the SYN Cookies option on the server to prevent unnecessary resource reservation.
3. **Notify Users**: Inform employees and customers about the service interruption and the measures being taken to resolve it.

---

## Section 3: Mitigation and Prevention Measures

To protect the server from future SYN Flood attacks, we recommend implementing the following measures:

### 1. SYN Cookies

- The server will not reserve resources for each SYN request. Instead, it will send a SYN-ACK with a "cookie" (a value calculated from the information in the request).
- Only if the client responds with a valid ACK will the server reserve resources for the connection.

### 2. Firewalls and Traffic Filters

- **Configure rules in the firewall**: Block suspicious IP addresses or those that send an abnormal number of SYN requests.
- **Use iptables on Linux**: This tool allows you to limit the number of SYN connections per second from the same IP. For example:

iptables -A INPUT -p tcp --syn -m limit --limit 1/s -j ACCEPT

- **This command limits 1 SYN request per second from a single IP.**

### 3. Load balancers

Distribute traffic across multiple servers to prevent one from being overloaded. This also helps mitigate DDoS attacks.

### 4. Intrusion detection systems (IDS)

- Monitor traffic for suspicious patterns, such as a large number of SYN requests from a single IP.
- Automatically block malicious traffic.

### 5. Rate limiting

- Limit the number of SYN requests the server accepts per second from a single IP.
- This prevents an attacker from flooding the server with requests.

---

## Section 4: Consequences of the attack

The SYN Flood attack has had the following negative consequences for the organization:

- **Service interruption**: Employees cannot access the website, which affects productivity and business.
- **Loss of reputation**: If customers cannot access the website, they may lose confidence in the company.
- **Financial costs**: Service interruption could lead to financial losses, especially if the website is an important source of income.

### How to inform users

- **Official communication**: Send an email or post a message on the website informing about the interruption and the measures being taken.
- **Transparency**: Explain that the problem is due to a cyber attack and that work is being done to resolve it as soon as possible.

---

## Section 5: Additional Recommendations

### Staff Training

- Ensure that the IT team is trained to identify and respond to these types of attacks.
- An Entry Level Cybersecurity Analyst could be tasked with monitoring traffic and implementing basic security measures.

### Continuous Monitoring

- Implement real-time monitoring tools, such as Nagios, Zabbix, or Wireshark, to proactively detect and mitigate attacks.
- Set up automatic alerts to notify about suspicious activity.

### Incident Response Plan

- Develop a clear plan to respond to future security incidents.

