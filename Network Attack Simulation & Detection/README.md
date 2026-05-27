# 06 – Network Attack Simulation & Detection

## Overview

The most comprehensive project of the internship — a full end-to-end network attack and defence simulation with real-time monitoring. Set up a complete lab environment with an attacker machine, a vulnerable web application (DVWA), an SSH honeypot (Cowrie), and an ELK Stack (Elasticsearch, Logstash, Kibana) for centralised log analysis and dashboards.

Then carried out a realistic multi-stage attack and monitored every step from the defender's side.

---

## Objective

> Build a realistic attack/defence lab, execute a multi-stage network attack using professional tools, capture all attack traffic, and demonstrate detection using ELK Stack dashboards.

---

## Lab Environment

```
┌─────────────────┐         ┌──────────────────────────┐
│  Kali Linux     │ ──────► │  DVWA (Web Target)        │
│  192.168.2.1    │         │  Apache + ModSecurity WAF │
│  (Attacker)     │         └──────────────────────────┘
│                 │         ┌──────────────────────────┐
│                 │ ──────► │  Cowrie SSH Honeypot      │
└─────────────────┘         └──────────────────────────┘
                                         │
                             ┌───────────▼──────────────┐
                             │  ELK Stack               │
                             │  Elasticsearch           │
                             │  Logstash                │
                             │  Kibana Dashboards       │
                             └──────────────────────────┘
```

Full network diagram: `Network diagram.png`

---

## Tools Used

**Attack tools:** Nmap, Nikto, Hydra  
**Defence/monitoring:** Cowrie Honeypot, ModSecurity, ELK Stack (Elasticsearch + Logstash + Kibana)  
**Target:** DVWA (Damn Vulnerable Web Application)  
**Packet capture:** Wireshark

---

## Attacks Performed

### Phase 1 – Reconnaissance
```
Tool: Nmap
Command: nmap -sV --script=vuln [target]
Result: Service versions and open ports enumerated
Evidence: Nmap scan output + screenshot 01-Nmap-Scan-Results.png
```

### Phase 2 – Web Vulnerability Scanning
```
Tool: Nikto
Result: Web vulnerabilities identified including missing headers, exposed files
Evidence: screenshot 02-Nikto-Vulnerability-Scan.png
```

### Phase 3 – SSH Brute Force
```
Tool: Hydra
Target: Cowrie SSH Honeypot
Result: 78 login attempts captured — all logged with usernames, passwords, and source IP
Evidence: cowrie_honeypot_logs.json (78 events), screenshot 03-Hydra-SSH-Brute-Force.png
```

### Phase 4 – Web Login Brute Force
```
Tool: Hydra
Target: DVWA login page
Result: Brute force attempts logged in Apache access logs
Evidence: screenshot 04-Hydra Web Brute Force.png
```

### Phase 5 – SQL Injection
```
Target: DVWA SQL injection module
Result: Injection attempts logged in Apache logs and flagged by ModSecurity
Evidence: screenshot 14-Apache-Logs-SQL-Injection.png
```

### Phase 6 – WAF Testing
```
Tool: Custom attack payloads
Result: ModSecurity blocked attack traffic — all attempts returned 403 Forbidden
Evidence: screenshot 07-ModSecurity_Status.png
```

---

## Detection & Monitoring

### Cowrie Honeypot

Captured all 78 SSH attack attempts in structured JSON format, including:
- Attacker IP address
- Usernames tried (admin, root, ubuntu, pi, etc.)
- Passwords attempted
- Timestamps
- Session duration

### ELK Stack Dashboards

Built custom Kibana dashboards to visualise attack data in real time:

| Dashboard | Screenshot |
|-----------|-----------|
| Cowrie Honeypot logs in Kibana Discover | `09-Kibana Discover - Cowrie Logs.png` |
| Successful SSH logins filter | `10-Kibana - Successful SSH Logins Filter.png` |
| Failed SSH logins filter | `11-Kibana - Failed SSH Logins Filter.png` |
| DVWA Web Attack Dashboard | `12-DVWA Web Attack Dashboard.png` |
| Cowrie Honeypot Dashboard | `13-Cowrie Honeypot Dashboard.png` |
| Apache Logs in Kibana Discover | `08-Kibana Discover - Apache Logs.png` |
| SQL Injection in Apache Logs | `14-Apache Logs - SQL Injection.png` |

---

## Attack Statistics

| Metric | Value |
|--------|-------|
| Total web events logged | 21,568 |
| SSH brute-force attempts | 78 |
| Attack tools detected | Nmap, Nikto, Hydra, curl |
| SQLi attempts detected | Multiple |
| XSS attempts detected | Multiple |
| WAF blocks | 100% of attack traffic |
| Detection rate | **100%** |

---

## Incident Report

A full incident report was produced based on the captured network traffic and logs, documenting the attack timeline, tools used, indicators of compromise (IOCs), and remediation recommendations.

Packet capture file: `cowrie_attack_capture.pcapng` (12MB — full network capture of the attack session, openable in Wireshark)

---

## Key Takeaway

This project demonstrated the full attack/defence cycle. The same attack that generated 21,568 Apache log entries and 78 SSH attempts was simultaneously being monitored in real time through Kibana dashboards. The Cowrie honeypot was especially effective — it let the attacker "succeed" at connecting while capturing everything they tried, without exposing any real system.

Centralised logging with ELK Stack turned raw log files into actionable intelligence.

---

## Files

```
06-network-attack-simulation/
├── Test-Incident Report.docx                   ← Full incident report
├── Network diagram.png                          ← Lab topology
├── Security Logs & Scan/
│   ├── LOGS_SUMMARY.txt                         ← Summary of all log files
│   ├── cowrie_honeypot_logs.json                ← 78 SSH attack events (JSON)
│   ├── dvwa&Cowrie_Nmap_scan.txt                ← Nmap scan results
│   ├── apache_access_logs.txt                   ← Web attack traffic
│   └── apache_error_logs.txt                    ← Server errors + ModSecurity alerts
├── INCIDENT REPORT/
│   ├── Enhanced Incident Report.pdf
│   ├── cowrie_attack_capture.pcapng             ← Full packet capture (Wireshark)
│   └── nmap_scan_results.txt
└── Screenshots/
    ├── 01-Nmap-Scan-Results.png
    ├── 02-Nikto-Vulnerability-Scan.png
    ├── 03-Hydra-SSH-Brute-Force.png
    ├── 04-Hydra Web Brute Force.png
    ├── 05-Apache-Logs(Attack Traffic).png
    ├── 06-Cowrie-Honeypot-Logs.png
    ├── 07-ModSecurity_Status.png
    ├── 08-Kibana Discover - Apache Logs.png
    ├── 09-Kibana Discover - Cowrie Logs.png
    ├── 10-Kibana - Successful SSH Logins Filter.png
    ├── 11-Kibana - Failed SSH Logins Filter.png
    ├── 12-DVWA Web Attack Dashboard.png
    ├── 13-Cowrie Honeypot Dashboard.png
    └── 14-Apache Logs - SQL Injection.png
```
