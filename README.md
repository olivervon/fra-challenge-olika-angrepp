# FRA challenge: Olika angrepp

## Initial observation

Opened the PCAP file in Wireshark.

Initial impression:
- Number of packets: 9901
- Duration: 00:16:15
- First timestamp: 2022-05-22 13:04:28
- Last timestamp: 2022-05-22 13:20:43

No filtering applied yet.

![overview](/screenshots/01_overview.png)

## File statistics

- Total packets: [9901]
- Duration: 00:16:15
- Average packet rate: 10.2

This helps estimate how much data we are dealing with.

![file stats](/screenshots/02_file_stats.png)

## Network overview

Identified IP addresses:

Internal (likely):
- 172.22.0.1
- 172.22.0.2

External (suspicious/unknown):
- None observed

Observations:
- Only two hosts are present in the capture, communicating exclusively with each other
- Both 172.22.0.1 and 172.22.0.2 have similar traffic volume (~2 MB each)
- The communication appears to be internal, suggesting attacker activity within the network or a simulated environment

![endpoints](/screenshots/03_endpoints.png)

## Protocol analysis

Observed protocols:
- Ethernet
- IPv4
- TCP
- HTTP

Potential attack surfaces:
- HTTP → possible credential exposure, command execution, and data exfiltration
- TCP → underlying transport, useful for session tracking and stream analysis

![protocols](/screenshots/04_protocols.png)

### HTTP Traffic Overview

Filtered all HTTP traffic to focus on potential web-based attacks.

![HTTP Traffic](/screenshots/05_http_filter.png)

### Attack 1 – Credential Brute Force / Credential Stuffing

Multiple HTTP GET requests were observed targeting the login functionality.

Observed patterns:
- Multiple usernames were tested
- Multiple passwords were attempted for each username
- Credentials were transmitted in cleartext via HTTP

Analysis:
- The attacker is performing a credential stuffing / brute force attack
- The attack targets multiple accounts rather than a single user
- This increases the likelihood of successful compromise

Note:
- All responses returned HTTP 200 OK, indicating that status codes alone cannot determine authentication success
- Response content must be analyzed for confirmation

![Brute Force Attempts](/screenshots/06_bruteforce_attempts.png)

### TCP Stream Analysis

Two representative login attempts were analyzed:

- password=123456
- password=princesa

Findings:
- Both responses returned HTTP/1.1 200 OK
- Content-Length was identical (677 bytes)
- Response structure and content appeared consistent

Conclusion:
- The application returns identical responses regardless of authentication success or failure
- It is not possible to determine successful login attempts based on response analysis alone

![TCP Stream](screenshots/07_tcp_stream.png)

### Attack 2 – Remote Command Execution (Web Shell)

After filtering out login attempts, new HTTP requests were identified targeting a file:

/hackable/uploads/funny.php

Observed parameters:
- cmd=id
- cmd=ls -la
- cmd=cat /etc/passwd
- cmd=cat /etc/shadow
- cmd=uname -r
- cmd=arch

Analysis:
- The attacker is executing system commands via a web-accessible script
- This indicates the presence of a web shell

Conclusion:
- The attacker has achieved remote code execution on the target system

![RCE Activity](/screenshots/08_attack2_rce.png)

### Vulnerability

- File upload vulnerability (malicious PHP file)
- Lack of input validation
- Remote command execution via unsanitized input

### Severity

Critical

The attacker is able to execute arbitrary system commands and access sensitive system files, including password hashes.

### MITRE ATT&CK

- T1059 – Command and Scripting Interpreter
- T1105 – Ingress Tool Transfer
- T1003 – OS Credential Dumping
