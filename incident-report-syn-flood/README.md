# SYN Flood Attack - Incident Response

This activity is part of the Google Cybersecurity Certificate training path.  
As a cybersecurity analyst for a travel agency, I investigated and mitigated a SYN Flood attack on the company’s website.

## 📌 Scenario Summary

- The company's web server experienced a service outage.
- A high number of TCP SYN requests from a single suspicious IP address overloaded the server.
- The website returned connection timeout errors to both employees and customers.
- A TCP packet capture was performed using Wireshark to identify the root cause.

## 🧪 Evidence

- Packet sniffer (Wireshark) logs show abnormal SYN request activity from IP `203.0.113.0` to the server `192.0.2.1`.
- The pattern matched a SYN Flood attack.

## 📄 Files in this directory

- `report.md`: Incident report detailing the attack and response.
- `wireshark-log.txt`: Raw packet data from Wireshark.

## 🎯 Skills Demonstrated

- Network packet analysis using Wireshark
- Incident response and attack mitigation
- Denial of Service (DoS) detection
