# STRIDE for DFD Trust Boundaries

```text
                 [User]
                   |
                   v
   --- Trust Boundary (Internet ↔ Application) ---
                   |
             +-------------+
             |   Web App   |
             +-------------+
              /           \
             v             v
   --- Trust Boundary   --- Trust Boundary
   (App ↔ DB)           (App ↔ External Service)
           |                    |
    +-------------+       [External API]
    |  Database   |
    +-------------+
```

---

## 1) User ↔ Web App (Internet → App)

| STRIDE | Realistic Scenario | Severity | Key Mitigations | Pentest Test Case |
|---|---|---:|---|---|
| **S - Spoofing** | Session hijack via stolen cookie | High | Cookie `HttpOnly/SameSite`, MFA, token rotation | Cookie theft + reuse, brute-force session ID |
| **T - Tampering** | Parameter manipulation (price tampering) | High | Server-side validation, parameter signing | Modify hidden fields / JSON and verify server-side |
| **R - Repudiation** | User denies a transaction | Medium | Immutable logs, audit trail, request signing | Check logging with request ID and server-side signing |
| **I - Info Disclosure** | XSS → token/JWT theft | High | Output encoding, CSP, sanitization | Reflected/stored XSS and read `localStorage` |
| **D - DoS** | Flood login/cart APIs | Medium/High | Rate limiting, adaptive CAPTCHA, circuit breaker | Controlled flooding, check 429/backoff |
| **E - Elevation of Privilege** | Authz bypass (IDOR) | High | Strong ABAC/RBAC, tested policies, deny-by-default | Access other users’ resources by changing ID |

---

## 2) Web App ↔ Database

| STRIDE | Realistic Scenario | Severity | Key Mitigations | Pentest Test Case |
|---|---|---:|---|---|
| **S** | Fake DB identity (connect to rogue host) | High | TLS + internal CA pinning, secrets in vault | Poison DNS / change DSN and observe failures |
| **T** | SQLi with UPDATE/DELETE | Critical | Parameterized ORM, least privilege, targeted WAF | Boolean/time-based payloads, check DB permissions |
| **R** | No trace of modifications | Medium | Audit logs on sensitive tables | Perform UPDATE and check audit trail |
| **I** | Dump of sensitive data | Critical | At-rest/row-level encryption, masking | Selective exfiltration and verify encryption |
| **D** | Heavy queries → lock | Medium | Query budget, timeouts, indexes, read replica | Run expensive queries, measure protections |
| **E** | Stored procedures with elevated rights | High | Least privilege, SP review, role separation | Call SP from low-priv account and observe |

---

## 3) Web App ↔ External API (third-party service)

| STRIDE | Realistic Scenario | Severity | Key Mitigations | Pentest Test Case |
|---|---|---:|---|---|
| **S** | MITM/impersonation of the API | High | mTLS/Pinning, IP/DNS allowlist, key rotation | Intercept/TLS-strip in lab, check pinning |
| **T** | Payload tampering (webhook) | High | HMAC signing with timestamp, replay protection | Replay old webhooks without signature |
| **R** | Provider denies requests | Medium | Request ID, signed logs on app side | Compare app logs vs provider logs |
| **I** | API key leaked in logs | High | Secrets redaction, key scoping, vault | Search for keys in logs/builds, revoke & rotate |
| **D** | Rate limit exhaustion → service block | Medium | Caching, retry jitter, graceful degradation | Generate burst, check backoff/fallback |
| **E** | Overly broad OAuth scope | High | Principle of least privilege, short-lived tokens | Use reduced-scope token and attempt forbidden actions |
