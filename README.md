# API Security Testing Lab

A hands-on project documenting REST API security testing methodology, findings, and remediation recommendations. All testing was performed on intentionally vulnerable practice applications in a controlled lab environment.

---

## Objective

To identify and document common API security vulnerabilities including broken authentication, insecure endpoints, and input validation flaws using industry-standard tools and the OWASP API Security Top 10 as a reference framework.

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Postman | Sending and manipulating API requests |
| Burp Suite | Intercepting and analyzing HTTP traffic |
| DVWS / Juice Shop | Vulnerable target applications |
| OWASP API Top 10 | Vulnerability reference framework |

---

## Target Application

**DVWS (Damn Vulnerable Web Services)** — an intentionally vulnerable web application designed for security testing practice.

> All testing was conducted in a local lab environment on applications designed to be vulnerable. No real systems were tested.

---

## Vulnerabilities Found

| # | Vulnerability | OWASP API Category | Severity | Status |
|---|--------------|-------------------|----------|--------|
| 1 | Broken Object Level Authorization | API1:2023 | High | Documented |
| 2 | Broken Authentication (no token validation) | API2:2023 | High | Documented |
| 3 | Missing input validation on user fields | API3:2023 | Medium | Documented |
| 4 | Sensitive data exposed in API response | API3:2023 | Medium | Documented |
| 5 | Insecure endpoint accessible without auth | API2:2023 | High | Documented |

---

## Testing Methodology

1. **Reconnaissance** — Mapped all available API endpoints using Postman
2. **Authentication Testing** — Tested login endpoints for missing/bypassable token validation
3. **Input Validation Testing** — Injected malformed inputs (special characters, oversized payloads, null values) into request parameters
4. **Response Analysis** — Analyzed JSON responses for sensitive data leakage (tokens, passwords, internal IDs)
5. **Authorization Testing** — Attempted to access other users' data by modifying object IDs in requests
6. **Documentation** — Recorded each finding with payload, response, impact, and fix

---

## Sample Finding — Broken Authentication

**Endpoint:** `POST /api/login`

**Issue:** The API accepted requests without validating the JWT token on protected endpoints. An attacker could access user data by sending a request with a missing or malformed token.

**Request sent via Postman:**
```json
POST /api/v1/user/profile
Authorization: Bearer invalid_token_here

{
  "user_id": "102"
}
```

**Response received:**
```json
{
  "status": "success",
  "username": "john_doe",
  "email": "john@example.com"
}
```

**Impact:** Unauthorized access to any user's profile data.

**Remediation:** Implement strict server-side JWT validation on every protected route. Return `401 Unauthorized` for missing or invalid tokens.

---

## Sample Finding — Input Validation Flaw

**Endpoint:** `POST /api/register`

**Issue:** The username field accepted special characters and excessively long strings with no server-side validation, potentially enabling injection attacks or application crashes.

**Payload used:**
```json
{
  "username": "' OR '1'='1",
  "password": "test123"
}
```

**Response received:**
```json
{
  "status": "success",
  "message": "User registered"
}
```

**Impact:** Input not sanitized — opens the door to SQL injection or stored XSS depending on how the value is used.

**Remediation:** Validate input length, allowed character sets, and sanitize all user-supplied data before processing.

---

## Folder Structure
api-security-testing-lab/
├── README.md
├── findings/
│   ├── 01-broken-authentication.md
│   ├── 02-input-validation-flaws.md
│   └── 03-insecure-endpoints.md
├── postman-collections/
│   ├── auth-tests.json
│   └── input-validation-tests.json
├── screenshots/
│   ├── postman-auth-bypass.png
│   └── response-leak.png
└── remediation-report.md
---

## Key Learnings

- REST APIs require authentication validation on every endpoint, not just the login route
- JSON responses should never return sensitive fields (passwords, tokens, internal IDs) unnecessarily
- Input validation must happen server-side — client-side validation alone is not sufficient
- OWASP API Security Top 10 is an essential checklist for any API security assessment

---

## References

- [OWASP API Security Top 10](https://owasp.org/www-project-api-security/)
- [DVWS - Damn Vulnerable Web Services](https://github.com/snoopysecurity/dvws-node)
- [Postman Documentation](https://learning.postman.com/)

---

## Disclaimer

This project was created for educational purposes only. All testing was performed on intentionally vulnerable applications in an isolated lab environment. Unauthorized testing of real systems is illegal and unethical.

---

*Developed by Siva Kumar Kondredy | Cybersecurity
