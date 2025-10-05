# Vulnerability: Security Misconfiguration — Exposed `/ftp` Directory

## Summary
- **Title:** Security Misconfiguration — exposed `/ftp` directory  
- **Severity:** High (sensitive files publicly accessible)  
- **Short description:** The site's `robots.txt` referenced a disallowed path `/ftp`. Despite being listed as disallowed, the `/ftp` directory and its contents were publicly accessible and readable. Multiple confidential files and folders inside `/ftp` were retrievable without authentication.

---

## How this maps to the internship task
This finding fulfills the internship requirements by:
- Demonstrating manual discovery and verification steps (robots.txt review, directory access).  
- Providing visual evidence (screenshots) and sanitized artifacts for the report.  
- Mapping to OWASP Top 10 (Security Misconfiguration) and including remediation steps suitable for the final PDF.

---

## Environment & Tools (short)
- **Target app:** (insert tested app name or URL, e.g. `http://127.0.0.1:3000`)  
- **Tools used:** Browser, Burp Suite Community Edition (optional, for HTTP capture), simple file listing inspection.  
- **Artifacts included (in `PoCs/`):**
  - `PoCs/robots_file_screenshot.png` — screenshot of `robots.txt` showing `/ftp` as disallowed.  
  - `PoCs/access_to_forbidden_folder.png` — screenshot showing access to the `/ftp` folder listing.  
  - `PoCs/access_to_forbidden_files.png` — screenshot showing access to files inside `/ftp`.  
  - `PoCs/acess_confidential_data.png` — screenshot showing readable confidential file content from `/ftp`.

> All artifacts are sanitized to avoid exposing sensitive contents before committing.

---

## Proof of Concept (concise, high-level)
> **Only verify these steps on systems you own or have explicit permission to test.**

1. **Check robots.txt:** open `https://<target>/robots.txt` and note any disallowed paths. `PoCs/robots_file_screenshot.png` shows `/ftp` listed as disallowed.  
2. **Attempt access:** navigate to the disallowed path (e.g., `https://<target>/ftp/`) in a browser. Observe whether directory listing or index files are returned. Evidence: `PoCs/access_to_forbidden_folder.png`.  
3. **Enumerate files:** browse files and folders in `/ftp/` (if directory listing is enabled) and capture screenshots of accessible file listings. Evidence: `PoCs/access_to_forbidden_files.png`.  
4. **Read confidential files:** open readable files and capture sensitive contents (sanitized in committed artifacts). Evidence: `PoCs/acess_confidential_data.png`.  
5. **Conclusion:** the `/ftp` directory is publicly accessible despite being marked in `robots.txt`, indicating a misconfiguration that exposes sensitive data.

---

## Impact
- **Sensitive data exposure:** confidential files intended to be private are publicly retrievable.  
- **Information leakage:** attacker can enumerate internal files, configuration data, credentials, or backups.  
- **Pre-auth data access:** attackers may gain footholds or intelligence useful for further attacks (credential reuse, target mapping).  
- **Regulatory & reputational risk:** leakage of sensitive or personal data may violate policies or laws.

---

## Remediation (recommended)
1. **Remove sensitive files from web-accessible directories:** keep backups, config files, and secrets out of the web root.  
2. **Disable directory listing:** ensure the web server does not list directory contents by default (disable `autoindex` or equivalent).  
3. **Enforce authentication & authorization:** protect any administrative or file-sharing directories with proper access controls.  
4. **Use proper file permissions:** limit read access to necessary accounts only (principle of least privilege).  
5. **Do not rely on `robots.txt` for protection:** `robots.txt` is advisory for crawlers and not an access control mechanism.  
6. **Harden server config:** review web server and application configurations for misconfigurations (default files, sample apps, open ports).  
7. **Scan & monitor:** include discovery of exposed files in regular security scans and monitor access to sensitive paths.

---

## OWASP Top 10 mapping
- **A05:2021 – Security Misconfiguration** — primary mapping.

---

## Inclusion in final deliverables
This finding will be included in the final PDF Security Assessment Report with:
- Executive summary describing the exposure and business impact.  
- Technical appendix containing the sanitized screenshots from `PoCs/`.  
- Risk rating and prioritized remediation steps.

---

## Notes & Disclosure
This testing was performed as part of a supervised internship assessment on authorized test applications only. Do **not** access or attempt to retrieve files from systems without explicit written permission. Artifacts in `PoCs/` have been sanitized to remove sensitive content before committing.
