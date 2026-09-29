# OWASP Juice Shop – Security Assessment & Risk Analysis

> A software testing / security testing project that evaluates the security posture of the intentionally vulnerable **OWASP Juice Shop** web application.

**Author:** Vaishali Srivastava

---

## Table of Contents

1. [Overview](#overview)
2. [Objectives](#objectives)
3. [Scope of Assessment](#scope-of-assessment)
4. [About the Target Application](#about-the-target-application)
5. [Methodology](#methodology)
6. [Tools Used](#tools-used)
7. [Reconnaissance Findings](#reconnaissance-findings)
8. [Attack Surface Assessment](#attack-surface-assessment)
9. [OWASP Risk Mapping & Risk Register](#owasp-risk-mapping--risk-register)
10. [Summary of Findings](#summary-of-findings)
11. [Recommendations](#recommendations)
12. [Conclusion](#conclusion)
13. [Disclaimer](#disclaimer)

---

## Overview

This project is a security assessment and risk analysis of **OWASP Juice Shop**, an open-source, intentionally insecure web application that simulates a modern e-commerce platform. The goal was to identify vulnerabilities, security misconfigurations, and weaknesses that an attacker could exploit, then evaluate and prioritize them by likelihood and business impact using a structured risk management approach.

## Objectives

- Evaluate the security posture of the OWASP Juice Shop application.
- Identify vulnerabilities, misconfigurations, and weaknesses.
- Map findings to the **OWASP Top Ten**.
- Build a **Risk Register** that prioritizes risks by likelihood and impact.
- Provide actionable recommendations and a conclusion.

## Scope of Assessment

The assessment covered the following components:

- Application endpoint
- Finding the Score Board (hacking challenges)
- Information gathering (key vulnerabilities and technology stack)
- Attack surface identification
- Vulnerability exploitation (using Damn Vulnerable Web Application)
- Access control measures
- OWASP risk mapping
- Risk register
- Recommendations and conclusion

## About the Target Application

| Item | Details |
|------|---------|
| **Application** | OWASP Juice Shop |
| **Type** | Open-source, intentionally insecure e-commerce web app |
| **Maintained by** | OWASP (Open Worldwide Application Security Project), volunteer-developed |
| **Demo URL** | https://demo.owasp-juice.shop |
| **Pages** | Home, About Us, Photo Wall, Score Board, Login, etc. |

**Typical users:** security learners and students (CEH, OSCP, eWPT preparation), developers learning how flaws like XSS, SQLi, and broken authentication appear in real code, and security trainers/organizations running awareness workshops and red-team exercises.

**Technology stack (via Wappalyzer / BuiltWith):**

- **JavaScript frameworks:** Angular, Zone.js
- **Languages:** TypeScript, Node.js
- **Fonts:** Google Font API, Font Awesome
- **Web server:** Apache 2.4
- **Firewall (via wafw00f):** Fastly

## Methodology

The assessment followed a structured, phased approach:

1. **Reconnaissance (passive):** gathering information without directly interacting with the application (OSINT, Google Dorking, WHOIS/ICANN lookup, DNS, certificate transparency).
2. **Reconnaissance (active):** technology fingerprinting, port scanning, and web server scanning.
3. **Attack surface assessment:** identifying every place an attacker can interact with the application.
4. **Exploitation and testing:** testing vulnerabilities and access control measures.
5. **Risk analysis:** mapping findings to the OWASP Top Ten and building a risk register.
6. **Reporting:** recommendations and conclusion.

## Tools Used

| Tool | Purpose |
|------|---------|
| Google Dorking | OSINT and information gathering |
| Wappalyzer | Technology stack detection |
| BuiltWith | Technology profiling |
| wafw00f | Firewall / WAF detection |
| WhatWeb | Technology fingerprinting and server information |
| ICANN / WHOIS lookup | Domain registration details |
| Nslookup | DNS record lookup |
| Nmap | Port and service scanning |
| Nikto (Kali Linux) | Web server vulnerability scanning |
| crt.sh | Certificate Transparency log search |
| Damn Vulnerable Web Application (DVWA) | Vulnerability exploitation practice |

## Reconnaissance Findings

**Passive information gathering**

- **Server IP:** 81.169.145.156 (hosting country: Germany), HTTP server reported as Heroku, markup language HTML5.
- **Domain:** owasp-juice.shop, status active, created 2017-11-20, registry expiration 2026-11-20.
- **Nameservers:** docks10.rzone.de, shades01.rzone.de.
- **DNS:** IPv4 (A) 81.169.145.156, IPv6 (AAAA) 2a01:238:20a:202:1156::0.
- **Certificate transparency (crt.sh):** 13 SSL/TLS certificates found; public key algorithm RSA (3072 bits).
- **Certificate:** issued by Sectigo Public Server Authentication CA DV R36, valid until October 17, 2026.

**Active scanning**

- **Nikto:** server banner changed from `Heroku` to `Apache/2.4.68 (Unix)`; the `Content-Encoding: deflate` header may indicate exposure to the **BREACH** attack. The scan ended with 19 errors and 12 items reported.
- **Nmap:** port **3000/TCP** open, running an unencrypted HTTP service on the Node.js Express framework. No HTTPS was detected on port 443, and the `X-Powered-By: Express` header disclosed the technology in use. These findings map to **A02 – Cryptographic Failures** and **A05 – Security Misconfiguration**.

## Attack Surface Assessment

The attack surface assessment identifies all entry points where an attacker might interact with the application or its supporting infrastructure. Identifying entry points matters because it:

- **Prioritizes risk** by focusing testing on the most exposed components.
- **Supports defence planning** by showing where authentication, input validation, rate limiting, and monitoring controls are needed.

## OWASP Risk Mapping & Risk Register

Each identified vulnerability was mapped to the OWASP Top Ten categories and recorded in a Risk Register, which rates risks by likelihood and potential business impact. See the full project report (`Final_project.docx`) for the complete risk mapping diagram and register.

## Summary of Findings

The assessment identified **18 security risks**:

| Severity | Count |
|----------|-------|
| Critical | 4 |
| High | 7 |
| Medium | 7 |

**Most significant risks:** Brute Force / Credential Stuffing, SQL Injection, Price / Quantity Tampering, and SSRF.

**Other findings:** IDOR, Reflected XSS, Session Fixation, CSRF, Race Conditions, Coupon Brute Force, Technology Fingerprinting, and OSINT / information harvesting.

These span authentication, authorization, input validation, session management, business logic, and client-side security controls.

## Recommendations

| Vulnerability | Recommended Mitigation |
|---------------|------------------------|
| Brute Force / Credential Stuffing | MFA, account lockout, rate limiting, CAPTCHA, IP reputation filtering, password breach detection |
| SQL Injection | Parameterized queries, prepared statements, ORM protections, input validation, WAF rules, secure code review |
| IDOR on Saved Cart | Object-level authorization checks on every request |
| Session Fixation | Regenerate session IDs after authentication and privilege changes |
| Reflected XSS | Context-aware output encoding and Content Security Policy (CSP) |
| Technology Fingerprinting | Remove version banners; minimize information disclosure |
| OSINT / Info Harvesting | Review publicly exposed information and metadata |
| Coupon Brute Force | Rate limiting, coupon complexity, monitoring |

## Conclusion

This project provided valuable insight into common web application vulnerabilities and their business risks. It reinforced the importance of integrating security into every stage of the software development lifecycle and adopting a proactive approach to vulnerability management. By addressing the identified findings and running regular security assessments, organizations can improve their security posture, reduce the likelihood of successful attacks, and strengthen the resilience of their web applications.

## Disclaimer

OWASP Juice Shop is an intentionally vulnerable application built for education and training. All testing in this project was performed for **learning and academic purposes only**. Do not use these techniques against systems you do not own or do not have explicit permission to test.

---

**Author:** Vaishali Srivastava
