# FUTURE_CS_01

## Web App Security Assessment — Internship Summary

**Short summary**  
During my internship I conducted a hands-on vulnerability assessment of a web application using Docker, Kali Linux (VM), Burp Suite and OWASP ZAP. This repository contains brief summaries of the findings, PoC artifacts per vulnerability, and a final report.

### What I learned
- How to approach an unknown web target and systematically test inputs and flows using proxy tools and manual techniques.  
- Practical familiarity with Docker (running vulnerable apps), Burp Suite (proxy & basic testing) and OWASP ZAP (quick scans and spidering).  
- The difference between testing on controlled machines and performing a real assessment against an unfamiliar target — it requires more exploration and careful validation.

### Key findings (four issues)
1. **SQL Injection (login)** — login bypass using a classic SQL payload; led to retrieval of an admin email.  
2. **DOM XSS (search)** — reflected/DOM cross-site scripting in the search parameter, allowing JavaScript execution in a victim's browser.  
3. **Broken Access Control / Credential Brute-force** — after identifying the admin email, a targeted password guess attack revealed the admin password (`admin123`).  
4. **Security Misconfiguration (exposed /ftp)** — `robots.txt` referenced `/ftp`; the directory was accessible and contained readable files.

### Environment & tools (short)
- Vulnerable app: runnable with Docker (example: OWASP Juice Shop).  
- Tools used: Kali Linux (VM), Burp Suite (proxy & basic attacks), OWASP ZAP (scanning/spider).  
- Artifacts: each vulnerability folder includes short PoC files and screenshots.

### Repo structure (example)
/
├─ README.md ← this file
├─ vuln-01-sqli/
├─ vuln-02-xss/
├─ vuln-03-broken-access/
├─ vuln-04-misconfig/
└─ report/


### Notes & Disclosure
Work performed as an internship assignment in a controlled environment. Do **not** run PoCs against systems you do not own or have permission to test. High-level remediation: validate & sanitize inputs, use prepared statements, apply output encoding, enforce rate limits and proper directory permissions.

### Contact
Name: Yehya Hamdy Shehata
Email: yehya.hamdy1111@gmail.com
LinkedIn: https://www.linkedin.com/in/yehya-shehata/
Portfolio: https://decisive-catshark.super.site/

