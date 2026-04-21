# Finding 01 — Broken Authentication

**Severity:** High  
**OWASP Category:** API2:2023 — Broken Authentication  
**Endpoint:** `POST /api/v1/user/profile`  
**Tool Used:** Postman  

---

## Description
The API did not validate JWT tokens on protected endpoints. Requests with missing or malformed tokens were still processed and returned user data.

## Payload Used
```json
POST /api/v1/user/profile
Authorization: Bearer invalid_token_here
```

## Actual Response
```json
{
  "status": "success",
  "username": "john_doe",
  "email": "john@example.com"
}
```

## Impact
An attacker can access any user's data without valid credentials.

## Remediation
Validate JWT tokens server-side on every protected route. Return `401 Unauthorized` for invalid or missing tokens.
