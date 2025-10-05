# Vulnerability: Broken Access Control / Credential Brute‑force

## Summary

* **Title:** Broken Access Control / Credential Brute‑force
* **Severity:** High
* **Short description:** After extracting an admin email via a prior SQL injection finding, a targeted credential‑guessing attack against the login endpoint succeeded. Using a publicly‑sourced small wordlist (~100 passwords) stored in `Passwordlist/` and **Burp Suite Community Edition** (proxy + Intruder workflow), the admin password was recovered (`admin123`), resulting in a full account takeover.

---

## How this maps to the internship task

This PoC aligns with the internship requirements by demonstrating:

* Manual testing using Burp Suite Community Edition as the primary tool.
* Evidence collection (sanitized request/response, wordlist, screenshots).
* OWASP Top 10 mapping to Broken Access Control / Authentication issues.
* Documentation suitable for inclusion in the final Security Assessment Report.

---

## Environment & Tools (short)

* **Target app:** (insert tested app name or URL, e.g. `http://127.0.0.1:3000`)
* **Platform:** Docker / local VM as applicable.
* **Tools used:** Burp Suite Community Edition (proxy + Intruder workflow), Kali Linux (VM optional).
* **Wordlist:** publicly available list obtained online, trimmed to ~100 candidate passwords and placed in `Passwordlist/`.
* **Artifacts included in this folder (`PoCs/`):**

  * `PoCs/Intruder_Grep-Match.png` — screenshot showing the Grep‑Match configuration or results in Burp.
  * `PoCs/Intruder_Password_BruteForce.png` — screenshot of the Intruder attack in progress (password candidates).
  * `PoCs/Intruder_Sucess_Request.png` — captured HTTP request for the successful attempt (sanitized).
  * `PoCs/Intruder_Sucess_Response.png` — captured HTTP response showing successful authentication (sanitized).

---

## Proof of Concept (high-level steps)

> **Only run these steps on systems you own or have explicit permission to test.**

1. **Precondition:** obtain a target username/email (from a prior SQLi finding).
2. Perform a single failed login to capture the exact failure message.
3. Capture the login POST request via Burp Proxy and save the capture locally.
4. Send the captured request into Burp’s Intruder workflow and set the password field as the payload position; set the discovered admin email as the fixed username.
5. Load `Passwordlist/wordlist.txt` (≈100 entries) as payload candidates.
6. Use Grep‑Match (or response-length/status checks) to filter and identify responses that differ from the standard failure message.
7. Execute the attack and verify any promising candidate by performing an actual login via the UI.
8. **Result:** admin password found: `admin123` (evidence in `PoCs/Intruder_Sucess_Request.png` and `PoCs/Intruder_Sucess_Response.png`).

---

## Evidence & Artifacts

All artifacts in `PoCs/` are sanitized to remove session tokens and other sensitive identifiers before committing.

* `PoCs/Intruder_Grep-Match.png` — screenshot showing the Grep‑Match configuration or filter used to identify successful attempts.
* `PoCs/Intruder_Password_BruteForce.png` — screenshot showing Intruder running with the password list and candidate results.
* `PoCs/Intruder_Sucess_Request.png` — sanitized HTTP request corresponding to the successful attempt.
* `PoCs/Intruder_Sucess_Response.png` — sanitized HTTP response showing successful authentication or evidence of a successful login.

> **Note:** If your actual filenames differ in case or spelling, update this README to match the exact filenames before publishing.

---

## Impact

* **Account takeover:** attacker can access the admin account and any privileged functionality.
* **Data exposure:** possible access to sensitive data and admin-only operations.
* **Privilege escalation & pivoting:** with admin access an attacker could modify application state or introduce further vulnerabilities.
* **Reputation & compliance risk:** exposure of privileged access may breach policies or regulations.

---

## Remediation (recommended)

1. Implement rate limiting and progressive delays for failed login attempts.
2. Enforce account lockouts or notifications after repeated failures.
3. Require multi-factor authentication (MFA) for admin/privileged accounts.
4. Enforce strong password policies and deny common or known-leaked passwords.
5. Return generic authentication failure messages that do not confirm account existence.
6. Add bot/credential-stuffing protections (CAPTCHA, WAF rules, IP reputation).
7. Monitor and alert on repeated failed login attempts and anomalous patterns.

---

## OWASP Top 10 mapping

* **A01:2021 – Broken Access Control** — primary mapping.
* **A07 / A02 – Identification & Authentication Failures** — related due to weak protections against brute-force attacks.

---

## Inclusion in final deliverables

This finding will be included in the final PDF report with:

* Executive summary and risk rating.
* Technical appendix containing the sanitized request/response screenshots and the passwordlist.
* Recommended remediation and OWASP mapping.

---

## Legal & disclosure note

This testing was performed as part of a supervised internship assessment on authorized test applications only. Do **not** apply these instructions to systems without explicit written permission.
