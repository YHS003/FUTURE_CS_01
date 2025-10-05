# Vulnerability: DOM XSS (Search)

## Summary
- **Title:** DOM Cross-Site Scripting (Search parameter)  
- **Severity:** Medium — High (depends on context and available privileges)  
- **Short description:** A DOM-based XSS vulnerability was identified in the search functionality. The `q` URL parameter is reflected into the page in a way that allows JavaScript execution in the victim's browser. A non-script payload was successful after simple `<script>` attempts failed.

---

## How this maps to the internship task
This PoC satisfies internship requirements by:
- Demonstrating manual testing of input-reflecting parameters and payload tuning.  
- Providing evidence (payload screenshot and result screenshot).  
- Mapping the finding to OWASP Top 10 client-side XSS considerations.  
- Including reproduction notes, impact assessment, and remediation guidance suitable for the final PDF report.

---

## Environment & Tools (short)
- **Target app:** (insert tested app name or URL, e.g. `http://127.0.0.1:3000`)  
- **Platform:** Docker / local VM as applicable.  
- **Tools used:** Browser (developer console / proxy as needed), Burp Suite Community Edition (proxy for capture), simple manual payload testing.  
- **Artifacts included (in `PoCs/`):**
  - `PoCs/DOM_XSS_Payload_Screenshot.png` — screenshot showing the payload in the URL / input.  
  - `PoCs/Result_of_Payload_Screenshot.png` — screenshot showing the alert or effect after the payload executed.

---

## Proof of Concept (concise, high-level)
> **Only test on systems you own or have explicit permission to test.**

1. Identify the search functionality and locate the `q` parameter in the URL (e.g., `/?q=...`).  
2. Attempted simple script tags (e.g., `<script>alert(1)</script>`) — these did not succeed.  
3. Test a DOM-style payload that triggers on element error handling:
```html
<img src=x onerror=alert("XSS")>
```
4. Inject the payload into the `q` parameter and submit the search.  
   - Capture the page state showing the payload in the DOM: `PoCs/DOM_XSS_Payload_Screenshot.png`.

5. Observe and capture the resulting browser behavior when the payload executes:  
   - `PoCs/Result_of_Payload_Screenshot.png`.  
   - The captured screenshots demonstrate the payload placement and the resulting JavaScript execution.

> **Note:** Payloads and evidence have been captured for documentation and sanitized where applicable.

---

## Impact

- **Client-side code execution:** an attacker can execute JavaScript in the context of a victim's browser, potentially stealing cookies, session tokens, or performing actions on behalf of the user.  
- **Phishing / UI redress:** attackers can manipulate page content to trick users.  
- **Chaining attacks:** combined with social engineering or other flaws, DOM XSS can lead to significant account compromise.

---

## Remediation (recommended)

1. **Output encoding / sanitization:** ensure any untrusted input reflected into the DOM is properly sanitized or encoded before insertion.  
2. **Use safe DOM APIs:** avoid using `innerHTML` with untrusted data; prefer `textContent`, `setAttribute`, or controlled element creation.  
3. **Content Security Policy (CSP):** implement a strong CSP that restricts inline script execution and untrusted sources.  
4. **Input validation & canonicalization:** validate inputs and normalize them before use.  
5. **Avoid dangerous sinks:** review client-side code for sinks that allow execution (e.g., `innerHTML`, `eval`, `document.write`) and remove or guard them.  
6. **Testing & monitoring:** include client-side XSS checks in automated scans and code review, and monitor for unusual client-side errors or script loads.

---

## OWASP Top 10 mapping

- **A03:2021 – Injection (client-side / script injection)** — related through XSS categories.  
- **Relevant guidance:** OWASP XSS Prevention Rules and secure client-side coding practices.

---

## Inclusion in final deliverables

This finding will be documented in the final PDF Security Assessment Report with:

- Executive summary and impact statement.  
- Technical appendix containing `PoCs/DOM_XSS_Payload_Screenshot.png` and `PoCs/Result_of_Payload_Screenshot.png`.  
- Remediation recommendations and OWASP mapping.  
- Risk rating and suggested priority.

---

## Notes & Disclosure

This testing was performed as part of a supervised internship assessment against authorized test applications only. **Do not** attempt these payloads on systems without explicit written permission. Evidence and screenshots are sanitized for safe sharing where applicable.
