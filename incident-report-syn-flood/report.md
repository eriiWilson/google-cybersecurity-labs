# Security Incident Report - SYN Flood Attack

**Organization:** Travel Agency XYZ  
**Analyst:** Eric Wilson  
**Date:** April 20, 2025

---

## 1. Incident Summary

On the afternoon of the event, the network monitoring system triggered an alert about a web server instability. Attempts to access the company’s website returned connection timeout errors, disrupting access for both employees and customers.

A packet capture analysis using Wireshark revealed an unusually high number of TCP SYN packets originating from a suspicious IP address (`203.0.113.0`) targeting the web server (`192.0.2.1`) on port 443 (HTTPS). This pattern is characteristic of a **SYN Flood**, a form of **Denial of Service (DoS)** attack.

---

## 2. Attack Description

The detected attack was a **SYN Flood**, a DoS method that exploits the TCP three-way handshake process (SYN, SYN-ACK, ACK). In this attack:

- The attacker sends numerous SYN packets to initiate connections.
- The server replies with SYN-ACKs and awaits the final ACK, which never arrives.
- The server’s connection table fills up, preventing legitimate connections from completing.

### Key Symptoms:

- Website slowdown and inaccessibility.
- Repeated SYN packets without completing handshakes.
- High CPU and memory usage on the web server.

---

## 3. Impact on the Organization

This attack severely impacted the availability of digital services. The website is a key channel for presenting vacation offers to customers and providing information to employees. Consequences included:

- Loss of online sales opportunities.
- Reduced customer trust and satisfaction.
- Decreased internal productivity.

If such attacks persist without mitigation, the organization risks:

- Revenue loss.
- Reputational damage.
- Increased exposure to further attacks during downtime.

---

## 4. Immediate Actions Taken

1. Temporarily took the affected web server offline for recovery.
2. Blocked the attacker's IP address (`203.0.113.0`) via firewall.
3. Collected and preserved packet capture logs for forensic analysis.

---

## 5. Recommendations for Prevention

To prevent similar attacks in the future, the following countermeasures are recommended:

- Deploy **Next-Generation Firewalls (NGFW)** with behavioral analysis.
- Configure **connection rate limiting per IP** and reduce TCP connection timeout values.
- Implement **DDoS mitigation services** like Cloudflare or AWS Shield.
- Set up **Intrusion Detection and Prevention Systems (IDS/IPS)**.
- Harden the operating system of the web server:
  - Disable unused services.
  - Keep systems updated.
  - Use strong authentication for remote access.

---

## 6. Conclusion

A SYN Flood attack was identified and mitigated. While the attacker was temporarily blocked, additional defenses must be put in place to avoid recurrence. This event highlights the importance of proactive monitoring, network hardening, and robust incident response.

---
