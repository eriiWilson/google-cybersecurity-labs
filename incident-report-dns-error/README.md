# DNS Protocol Incident Report

This folder contains the incident report for a DNS-related issue identified during the Google Cybersecurity Certificate course.

## Scenario

Users reported that the domain `www.yummyrecipesforme.com` was inaccessible. An analysis using `tcpdump` revealed ICMP packets indicating that DNS requests were not reaching port 53 of the DNS server. This prevented the domain name from being resolved to an IP address.

## Contents

- `report.md`: A detailed incident report documenting the problem and analysis.
- `logs/tcpdump-log.txt`: Packet capture showing the ICMP error.


## ✅ Skills Demonstrated

- Packet log analysis using tcpdump
- Identifying affected network protocols
- Writing a professional cybersecurity incident report
- Proposing remediation actions (e.g., DNS service configuration)

> **Activity from:** Google Cybersecurity Certificate – Module 3: Connect and Protect
