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
