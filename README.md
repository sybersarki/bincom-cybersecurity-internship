# Bincom Cybersecurity Internship Portfolio

A collection of hands-on cybersecurity projects completed during my internship at Bincom. The work spans offensive security, defensive security, cloud security, threat detection, and log analysis — covering both attack and defence perspectives across real-world tooling.

---

## Projects

| # | Project | Key Skills |
|---|---------|------------|
| 01 | [Vulnerability Remediation Report](#01---vulnerability-remediation-report) | Nmap, Metasploit, CVE analysis, patch management |
| 02 | [Attack & Defend Web Application](#02---attack--defend-web-application) | OWASP, ModSecurity WAF, Hydra, PrestaShop hardening |
| 03 | [Log Analysis & Attack Detection](#03---log-analysis--attack-detection) | Apache logs, Kibana, ELK Stack, incident analysis |
| 04 | [AWS IAM Privilege Escalation Simulation](#04---aws-iam-privilege-escalation-simulation) | AWS IAM, least-privilege policy, cloud security |
| 05 | [Capture The Flag – Pickle Rick](#05---capture-the-flag--pickle-rick) | TryHackMe, Linux enumeration, web exploitation |
| 06 | [Network Attack Simulation & Detection](#06---network-attack-simulation--detection) | Cowrie Honeypot, DVWA, Nikto, Kibana dashboards |

---

## 01 - Vulnerability Remediation Report

**Target:** Metasploitable 2 (intentionally vulnerable Linux VM)

Performed a full vulnerability assessment using Nmap (`--script=vuln`) against a Metasploitable 2 target. Identified and documented critical CVEs across multiple services, then produced a structured remediation report.

**Key findings:**
- `vsftpd 2.3.4` backdoor — CVE-2011-2523 (CVSS 10.0), exploited via Metasploit to gain root shell
- `OpenSSH 4.7p1` — multiple high-severity CVEs
- `Apache 2.2.8` — SQL injection vectors, Slowloris DoS (CVE-2007-6750), CSRF vulnerabilities
- `MySQL 5.0.51a`, `PostgreSQL 8.3`, `ProFTPD 1.3.1` — all with known critical exploits
- `UnrealIRCd` — trojaned backdoor version detected

**Tools:** Nmap 7.98, Metasploit Framework, Wireshark

📁 [`01-vulnerability-remediation/`](./01-vulnerability-remediation/)

---

## 02 - Attack & Defend Web Application

**Target:** PrestaShop e-commerce application on a local server

Simulated both attacker and defender roles on a PrestaShop web application. Carried out attacks including brute-force login attempts and SQL injection testing, then hardened the application against those same attacks.

**What was done:**
- Disabled debug mode (was leaking internal paths, DB queries, and stack traces)
- Configured and validated ModSecurity WAF rules
- Brute-force protection via Hydra testing + lockout policy
- File permission hardening
- WAF validation confirming 403 Forbidden responses to attack traffic
- Completed a PrestaShop security checklist covering 20+ hardening steps

**Tools:** Hydra, ModSecurity, Apache, Nikto

📁 [`02-web-attack-and-defense/`](./02-web-attack-and-defense/)

---

## 03 - Log Analysis & Attack Detection

**Target:** Apache web server + SSH service logs

Analysed over 21,000 Apache access log entries and 1,000 error log entries to identify and document attack patterns. Produced a structured incident analysis report.

**What was found:**
- Nikto vulnerability scan traffic from 192.168.2.1 (Kali Linux attacker)
- Hydra brute-force attempts against SSH and web login
- SQL injection and XSS attempts in HTTP request parameters
- ModSecurity alerts triggered by attack traffic
- 100% attack detection rate across all logged events

**Tools:** Apache log analysis, manual review, incident report writing

📁 [`03-log-analysis-attack-detection/`](./03-log-analysis-attack-detection/)

---

## 04 - AWS IAM Privilege Escalation Simulation

**Platform:** AWS (IAM, CLI)

Simulated a real-world cloud privilege escalation scenario. Started as a low-privilege `junior-dev` IAM user and demonstrated how misconfigured IAM policies could allow escalation to Administrator access. Then remediated the vulnerability with a least-privilege policy.

**Attack path demonstrated:**
1. Created `junior-dev` user with access keys
2. Attached `DangerousIAMPolicy` (overly permissive `iam:*` rights)
3. Escalated privileges to `AdministratorAccess`
4. Created a `backdoor-admin` user as proof of full compromise

**Remediation:**
- Removed dangerous policies
- Authored `least-privilege-policy.json` — S3 read-only + EC2 describe, all IAM actions explicitly denied
- Validated escalation was blocked after applying the fix

**Tools:** AWS CLI, IAM policy editor, `aws sts`, `aws iam`

📁 [`04-aws-iam-privilege-escalation/`](./04-aws-iam-privilege-escalation/)

---

## 05 - Capture The Flag – Pickle Rick

**Platform:** TryHackMe — "Pickle Rick" room

Completed the Pickle Rick CTF room on TryHackMe, which involves exploiting a web application to find 3 hidden flags. The room simulates a real-world Linux web server compromise.

**Approach:**
- Discovered username in HTML source code
- Exploited a command execution panel to enumerate the server
- Retrieved all 3 flags through file system enumeration and privilege escalation

**Room status:** ✅ Completed

**Tools:** Browser dev tools, command injection, Linux enumeration

📁 [`05-ctf-pickle-rick/`](./05-ctf-pickle-rick/)

---

## 06 - Network Attack Simulation & Detection

**Environment:** Full lab with Kali Linux attacker, DVWA target, Cowrie SSH Honeypot, ELK Stack (Elasticsearch, Logstash, Kibana)

The most comprehensive project — a full network attack and defence simulation with real-time monitoring. Deployed an ELK Stack to ingest and visualise logs from both a web application target (DVWA) and an SSH honeypot (Cowrie).

**Attacks performed:**
- Nmap port scan and service enumeration
- Nikto web vulnerability scan
- Hydra SSH brute force (78 attempts captured)
- Hydra web login brute force
- SQL injection against DVWA
- ModSecurity WAF bypass attempts (blocked)

**Defence & detection:**
- Cowrie Honeypot captured all 78 SSH attack attempts including usernames, passwords, and source IPs
- ModSecurity WAF active — blocked attack traffic with logged alerts
- ELK Stack dashboards built for both DVWA and Cowrie logs
- Kibana filters for successful vs failed SSH logins, SQL injection patterns, and Apache attack traffic
- Network diagram documenting the full lab topology

**Attack detection rate:** 100% (21,568 web events + 78 SSH attempts all detected)

**Tools:** Nmap, Nikto, Hydra, Cowrie, ModSecurity, ELK Stack, Kibana, DVWA, Wireshark (`.pcapng` capture included)

📁 [`06-network-attack-simulation/`](./06-network-attack-simulation/)

---

## Skills Demonstrated

**Offensive:** Nmap, Metasploit, Hydra, Nikto, SQL injection, web brute-force, CVE exploitation, privilege escalation (cloud + local)

**Defensive:** ModSecurity WAF, Cowrie Honeypot, ELK Stack, Apache log analysis, IAM least-privilege, security hardening checklists, incident reporting

**Cloud:** AWS IAM, AWS CLI, policy authoring, access key management

**Documentation:** Vulnerability reports, incident reports, CTF writeups, network diagrams, Kibana dashboards

---

## Tools & Technologies

`Nmap` · `Metasploit` · `Hydra` · `Nikto` · `ModSecurity` · `Cowrie` · `ELK Stack` · `Kibana` · `DVWA` · `AWS IAM` · `AWS CLI` · `Wireshark` · `TryHackMe` · `PrestaShop` · `Apache` · `Kali Linux`
