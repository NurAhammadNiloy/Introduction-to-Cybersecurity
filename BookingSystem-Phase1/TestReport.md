# 1️⃣ Introduction

**Tester(s):**  
- Name: Nur Ahammad Niloy  

**Purpose:**  
- To identify vulnerabilities and weaknesses in the registration system, including form validation, server-side input handling, authentication preparation, and exposure of sensitive errors.  

**Scope:**  
- Tested components:  
  - `/register` page  
  - Email, password, birthdate, and role fields  
  - Server response and validation behavior  
- Exclusions:  
  - Login page  
  - Booking functionality  
  - Admin endpoints  
- Test approach: Gray-box  

**Test environment & dates:**  
- Start: 27.11.2025  
- End: 28.11.2025  
- Test environment details:  
  - OS: Windows 11  
  - Browser: Chrome  
  - DB: PostgreSQL in Docker  
  - Backend: Deno runtime in Docker  
  - Testing tool: OWASP ZAP  

**Assumptions & constraints:**  
- Limited to local environment only  
- No privileged credentials  
- No backend code modifications allowed  

---

# 2️⃣ Executive Summary

**Short summary (1-2 sentences):**  
Testing revealed 3 medium-risk and 5 low-risk issues, mainly missing security headers, lack of CSRF protection, and visible internal error messages in the registration flow.

**Overall risk level:** Medium  

**Top 5 immediate actions:**  
1. Implement CSRF token in registration POST requests  
2. Add security headers (CSP, X-Frame-Options, X-Content-Type-Options)  
3. Restrict backend from exposing database exceptions  
4. Enforce strong password policy  
5. Implement backend-side validation on email and form fields  

---

# 3️⃣ Severity scale & definitions

|  **Severity Level**  | **Description**                                                                                                              | **Recommended Action**           |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------- | -------------------------------- |
| 🔴 High | A serious vulnerability that can lead to full system compromise or data breach. | Immediate fix required |
| 🟠 Medium | A significant issue such as XSS or CSRF that may require specific conditions to exploit. | Fix ASAP |
| 🟡 Low | Minor weakness or misconfiguration. | Fix soon |
| 🔵 Info | No direct risk, for system hardening only. | Monitor or patch later |

---

# 4️⃣ Findings

| ID | Severity | Finding | Description | Evidence / Proof |
|------|----------|----------|-------------|------------------|
| F-01 | 🟠 Medium | No CSRF protection | Registration form submits POST without CSRF token | Verified via HTML form + ZAP |
| F-02 | 🟠 Medium | Missing CSP header | Allows potential script injection | ZAP: CSP Header Not Set |
| F-03 | 🟠 Medium | Missing Anti-Clickjacking header | Page can be embedded in iframe | ZAP: Missing Anti-clickjacking Header |
| F-04 | 🟡 Low | Application error disclosure | Internal error messages revealed on /register | ZAP: Application Error Disclosure |
| F-05 | 🟡 Low | Server exposes version info | HTTP "Server" header reveals version info | ZAP: Server Leaks Version Information |
| F-06 | 🟡 Low | No Strict-Transport-Security | Application can be accessed without HSTS | ZAP: Strict-Transport-Security Header Not Set |
| F-07 | 🟡 Low | Timestamp disclosure | Unix timestamps returned in responses | ZAP: Timestamp Disclosure – Unix |
| F-08 | 🟡 Low | X-Content-Type-Options missing | Browser MIME-sniffing enabled | ZAP: X-Content-Type-Options Header Missing |
| F-09 | 🔵 Info | Cache-control directives not optimal | Some responses may be cached longer than needed | ZAP: Re-examine Cache-control Directives |



---

# 5️⃣ OWASP ZAP Test Report (Attachment)

**Purpose:**  
- Provide the full automated scan results from OWASP ZAP

**Report location:**  
📁 `BookingSystem-Phase1/ZAPReport.md`

---

