# Vulnerability: SQL Injection (Login Page)

## Summary
- **Title:** SQL Injection — Authentication Bypass (login page)  
- **Severity:** High  
- **Short description:** A classic SQL injection was found in the login endpoint. By injecting a tautology payload into the username field, authentication was bypassed and an admin email was retrieved from the application database.

---

## How this maps to the internship task
This PoC meets the internship deliverables by:
- Demonstrating manual testing with proxy tools (Burp Suite) and a simple SQLi payload.  
- Providing evidence (request/response captures and screenshots).  
- Mapping the finding to the OWASP Top 10 (Injection).  
- Including reproduction notes, impact assessment, and remediation guidance suitable for the final PDF report.

---

## Environment & Tools (short)
- **Target app:** (insert tested app name or URL, e.g. `http://127.0.0.1:3000`)  
- **Platform:** Docker / local VM as applicable.  
- **Tools used:** Burp Suite Community Edition (proxy + saved item), browser (for screenshots).  
- **Artifacts included (in `PoCs/`):**
  - `PoCs/Request.txt` — captured HTTP request containing the SQLi payload (sanitized).  
  - `PoCs/Response.txt` — captured HTTP response showing the result of the injected request (sanitized).  
  - `PoCs/Screenshot_Browser_Admin.png` — browser screenshot showing the application logged in as admin.  
  - `PoCs/Screenshot_BurpSuite.png` — Burp Suite screenshot showing the injected request/response.  
  - `PoCs/Authentication_bypass` — saved Burp item demonstrating the authentication bypass (exported from Burp; sanitized).

> All artifacts have been sanitized to remove session tokens or other sensitive identifiers before committing.

---

## Proof of Concept (concise, high-level)
> **Only attempt these steps on systems you own or have explicit permission to test.**

1. Navigate to the application's login page and proxy the traffic through Burp Suite.  
2. In the login form, inject the following payload into the **username** field: `' OR 1=1 --`
3. Use any value for the password field (example: `admin`).  
4. Submit the form while capturing the HTTP request in Burp Proxy. Save the request as `PoCs/Request.txt`.  
5. The server responds indicating successful authentication (response and session data captured in `PoCs/Response.txt`), and the browser shows an authenticated admin view (see `PoCs/Screenshot_Browser_Admin.png`).  
6. The Burp saved item `PoCs/Authentication_bypass` contains the successful request/response and was used for documentation and verification.  
7. Evidence screenshots from Burp and the browser are included (`PoCs/Screenshot_BurpSuite.png`, `PoCs/Screenshot_Browser_Admin.png`). From the successful session, an admin email was identified and recorded.

---

## Impact
- **Authentication bypass:** attacker can assume any account identity (including admin) without valid credentials.  
- **Data exposure:** sensitive data (e.g., admin email, user records) may be retrieved.  
- **Privilege abuse:** attacker may perform privileged actions, modify data, or pivot to other components.  
- **Business risk:** potential data breach, regulatory exposure, and loss of trust.

---

## Remediation (recommended)
1. **Use parameterized queries / prepared statements** for all database access — never concatenate untrusted input into SQL.  
2. **Input validation & sanitization:** validate inputs on server side and apply appropriate type/length checks.  
3. **Least privilege DB accounts:** application DB accounts should have minimal privileges (no admin/superuser level for web-facing queries).  
4. **Error handling:** avoid returning detailed DB errors to users.  
5. **WAF / detection:** consider WAF rules to detect common SQLi patterns and monitor for suspicious payloads.  
6. **Test & confirm fixes:** re-test using automated scanners and manual verification after applying fixes.

---

## OWASP Top 10 mapping
- **A03:2021 – Injection (SQL Injection)** — primary mapping.

---

## Inclusion in final deliverables
The final PDF Security Assessment Report will include:
- Executive summary describing the finding and business impact.  
- Technical appendix with the sanitized `PoCs/Request.txt`, `PoCs/Response.txt`, screenshots, and the exported Burp saved item (`PoCs/Authentication_bypass`).  
- Risk rating and prioritized remediation steps.  
- OWASP Top 10 checklist entry for Injection.

---

## Notes & Disclosure
This PoC was conducted as part of an internship assessment on an authorized test application. Do **not** apply these steps to systems without explicit permission. Artifacts are sanitized for safe sharing.
