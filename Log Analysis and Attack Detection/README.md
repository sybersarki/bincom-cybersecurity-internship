# 03 – Log Analysis & Attack Detection

## Overview

Analysed Apache web server access and error logs — over 21,000 entries — to identify attack patterns, classify threats, and document findings in a structured incident analysis report. This project focuses on the blue-team skill of reading logs and understanding what attackers leave behind.

---

## Objective

> Given raw server logs from an active attack scenario, identify all attack types, classify severity, determine attacker source, and produce an incident report.

---

## Log Files Analysed

| File | Contents | Volume |
|------|---------|--------|
| `apache_access_logs.txt` | Last 2,000 entries from Apache access log | 21,568 total events in full log |
| `apache_error_logs.txt` | Last 1,000 Apache error log entries | Server errors, ModSecurity alerts |
| `Incident_Analysis_Report.docx` | Final structured incident report | — |

---

## Attack Patterns Identified

### Attacker Profile
- **Source IP:** `192.168.2.1` (Kali Linux machine)
- **Attack window:** Sustained campaign across multiple tools

### Attack Types Detected

| Attack Type | Tool Used | Indicator in Logs |
|------------|-----------|------------------|
| Vulnerability scanning | Nikto | Rapid sequential GET requests to known vulnerability paths |
| SSH brute force | Hydra | Repeated failed SSH auth from single source |
| Web login brute force | Hydra | High-frequency POST requests to login endpoints |
| SQL injection | Manual / automated | URL-encoded SQL syntax in query parameters |
| XSS attempts | Manual / automated | `<script>` tags in request parameters |
| Directory enumeration | Nmap/Nikto | Requests to `/phpMyAdmin/`, `/phpinfo.php`, `/admin/`, etc. |

### ModSecurity Alerts

The error logs showed ModSecurity triggering on attack traffic — confirming the WAF was active and detecting threats in real time. Alert messages identified specific rule matches for SQLi and XSS payloads.

---

## Detection Rate

| Metric | Value |
|--------|-------|
| Total web events logged | 21,568 |
| Total SSH attempts logged | 78 |
| Attack tools detected | Nmap, Nikto, Hydra, curl |
| Detection rate | **100%** |
| False negatives | 0 |

---

## Incident Report

The final incident report documents:
- Timeline of the attack
- Tools and techniques used by the attacker
- Impact assessment
- Recommendations to prevent recurrence

---

## Key Takeaway

Log analysis is how defenders understand what happened during or after an attack. Even without real-time alerts, the logs told a complete story: who attacked, what they used, what they targeted, and whether it worked. Good log hygiene and centralised log storage are non-negotiable for incident response.

---

## Files

```
03-log-analysis-attack-detection/
├── Incident_Analysis_Report.docx              ← Final incident report
└── (logs in 06-network-attack-simulation/     ← Full log files are in the network sim project)
```
