# 🛡️ Cybersecurity Incident Report: Web Server Compromise & Redirect Malware Analysis

## Executive Summary
This report documents an investigation into a security incident involving **yummyrecipesforme.com**. A malicious actor executed a successful brute-force attack on default administrative credentials, compromised the web server source code, and injected JavaScript to distribute malware and redirect users to a rogue domain.

---

## 🔍 Section 1: Network Protocols Identified
During packet capture analysis using `tcpdump`, three core network protocols were identified across the TCP/IP model:

* **DNS (Domain Name System):** Resolved target domain names (`yummyrecipesforme.com` and `greatrecipesforme.com`) to their corresponding IP addresses (`203.0.113.22` and `192.0.2.17`).
* **TCP (Transmission Control Protocol):** Established reliable connection sessions between the client and servers via standard three-way handshakes (`[S]`, `[S.]`, `[.]`) on port 80.
* **HTTP (Hypertext Transfer Protocol):** Transmitted resource requests (`HTTP: GET / HTTP/1.1`) and delivered compromised web pages containing malicious scripts.

---

## 📝 Section 2: Incident Timeline & Forensic Log Documentation

| Timestamp / Sequence | Source / Destination | Event Description |
| :--- | :--- | :--- |
| **Initial Attack Phase** | Admin Panel | Former employee brute-forced default admin credentials, changed administrative credentials, and injected malicious JavaScript into the source code. |
| **14:18:32** | `your.machine` ➡️ `dns.google` | Client issued a DNS request resolving `yummyrecipesforme.com` to `203.0.113.22`. |
| **14:18:36** | `your.machine` ↔️ `yummyrecipesforme.com` | TCP three-way handshake established on port 80 followed by an `HTTP GET / HTTP/1.1` request. |
| **Malware Delivery** | Web Server ➡️ Client | Web server delivered modified page source code containing embedded JS, prompting an unauthorized browser update/executable download. |
| **14:20:32** | `your.machine` ➡️ `dns.google` | Downloaded payload triggered DNS resolution for malicious domain `greatrecipesforme.com` (`192.0.2.17`). |
| **14:25:29** | `your.machine` ↔️ `greatrecipesforme.com` | Client completed TCP handshake and HTTP GET request targeting the secondary rogue server. |

---

## 🛠️ Section 3: Remediation & Defensive Security Measures

### Recommended Action: Enforce Multi-Factor Authentication (MFA) & Account Lockout Controls
* **Primary Defense (MFA/2FA):** Mandatory multi-factor authentication prevents single-factor credential abuse. Even if default or weak passwords are compromised via brute-force dictionary attacks, access is denied without an out-of-band secondary authentication token.
* **Secondary Defense (Rate-Limiting & Lockouts):** Configure web application firewalls (WAF) or local authentication modules to automatically lock accounts or drop IP connections after 3–5 consecutive failed login attempts, effectively neutralizing automated brute-force tools. 
