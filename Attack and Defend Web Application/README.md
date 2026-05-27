# 02 – Attack & Defend Web Application

## Overview

Took on both attacker and defender roles against a PrestaShop e-commerce web application. First attacked the application to identify weaknesses, then hardened it against those same attack vectors. Validated the defences by re-running attacks and confirming they were blocked.

---

## Objective

> Simulate realistic web application attacks against a PrestaShop installation, then apply security controls to defend against them — and prove the defences work.

---

## Environment

| Component | Details |
|-----------|---------|
| Target | PrestaShop running on Apache (local VM) |
| Attacker | Kali Linux |
| WAF | ModSecurity (Apache module) |

---

## Tools Used

- **Hydra** — brute-force login attempts (SSH + web form)
- **Nikto** — web vulnerability scanner
- **ModSecurity** — Web Application Firewall
- **Apache** — web server with security module integration
- **Browser DevTools** — manual testing

---

## Attack Phase

### Findings Before Hardening

| Finding | Risk | Impact |
|---------|------|--------|
| Debug mode enabled | Critical | Leaked internal file paths, DB queries, stack traces, framework version |
| Weak admin credentials | High | Susceptible to brute-force via Hydra |
| Missing WAF | High | No protection against SQLi, XSS, or brute-force |
| Insecure file permissions | Medium | Sensitive files accessible |
| No rate limiting | Medium | Unlimited login attempts allowed |

**Debug mode** was the most critical finding — with it enabled, an attacker could see the application's internal architecture, database structure, and exact error locations just by triggering any error.

---

## Defence Phase

### Controls Applied

**1. Disabled Debug Mode**
- Navigated to: Admin Panel → Advanced Parameters → Performance
- Set debug mode to `Off`
- Verified: Error pages no longer expose sensitive information

**2. ModSecurity WAF Configuration**
- Enabled and configured ModSecurity on the Apache server
- Applied OWASP Core Rule Set (CRS) rules
- Validated WAF was blocking attack traffic with `403 Forbidden` responses

**3. Brute-Force Protection**
- Configured account lockout policy after failed login attempts
- Verified Hydra brute-force attempts were now blocked

**4. File Permission Hardening**
- Restricted permissions on sensitive directories and configuration files

**5. PrestaShop Security Checklist**
- Completed a 20+ point security checklist covering admin URL obfuscation, HTTPS enforcement, cookie security, and more

---

## Validation

After hardening, re-ran attack tools and confirmed:

- ModSecurity logs showed blocked requests with WAF alert messages
- Admin login returned `403 Forbidden` for automated brute-force attempts
- Debug mode confirmed disabled — no internal information leaked
- Screenshot `06-403-Forbidden.png` shows WAF blocking in action
- Screenshot `07-waf-detection-logs.png` shows ModSecurity alert logs
- Screenshot `08-site-working-normally.png` confirms site still functional for legitimate users

---

## Key Takeaway

The same tools used to attack the application (Hydra, Nikto) were used to verify it was properly defended. A WAF alone isn't enough — disabling debug mode and fixing configuration issues is equally important, and often simpler to do.

---

## Files

```
02-web-attack-and-defense/
├── ASSIGNMENT3_ Attack and Defend Web Application.pdf   ← Full assignment report
├── prestashop-security-checklist.txt                    ← 20+ point checklist
├── waf-configuration.txt                                ← ModSecurity config
├── security-validation-report.txt                       ← Post-hardening validation
└── screenshots/
    ├── 01-admin-dashboard.png
    ├── 02.0-debug-mode-ON.png
    ├── 02.1-debug-mode-DISABLED.png
    ├── 03-modules-status.png
    ├── 04-file-permissions.png
    ├── 05-database-prefix.png
    ├── 06-403-Forbidden.png
    ├── 06-waf-blocks-test.png
    ├── 07-waf-detection-logs.png
    └── 08-site-working-normally.png
```
