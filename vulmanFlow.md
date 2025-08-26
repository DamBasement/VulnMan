# 🛡️ Vulnerability Management 

## Scope
- Applies to all IT assets (servers, endpoints, apps, cloud services).
- Covers identification, prioritization, remediation, and reporting.

---

## Roles
- **CISO** → oversight, exception approvals, reporting to execs  
- **CIO** → alignment with IT strategy  
- **Security Team** → scanning, analysis, reporting, tracking remediation  
- **System Owners** → patching, remediation, temporary mitigations  

---

## Scanning
- **Critical assets** → weekly  
- **All assets** → monthly  
- **Ad-hoc** → after major alerts, zero-days, or system changes  
- **Pentest** → annually + after major releases  

---

## Prioritization (CVSS + business impact)
| Category | SLA | Owner | Notes |
|----------|-----|-------|-------|
| **Critical (9.0–10)** | 48h | SecTeam + SysAdmin | Emergency patch/mitigation |
| **High (7.0–8.9)** | 7 days | SysAdmin | Weekly validation |
| **Medium (4.0–6.9)** | 30 days | SysAdmin | Monthly patch cycle |
| **Low (0.1–3.9)** | 90 days | SysAdmin | Monitor / backlog |

---

## Exceptions
- Allowed only with **CISO documented approval**.  
- Must include **compensating controls** (segmentation, monitoring, access restrictions).  

---

## Reporting
- **Monthly**: report to CISO, CIO, department heads  
- **Quarterly**: exec summary to CEO/Board  
- **KPIs**: % vulnerabilities remediated in SLA, MTTR, open exceptions  

---

## Process Flow (ASCII Diagram)

```text
        ┌───────────┐
        │   Scan    │
        │ (VA + PT) │
        └───────────┘
              │
              v
       ┌─────────────┐
       │ Prioritize  │
       │ (CVSS +     │
       │ Business    │
       │ Context)    │
       └─────────────┘
              │
              v
     ┌─────────────────┐
     │ Can Remediate?  │
     └─────────────────┘
         Yes  │   No
              │
              │
              v
       ┌──────────────┐        ┌─────────────────┐
       │ Remediate    │        │ Mitigate        │
       │ (Patch/Fix)  │        │ (Firewall, ACL, │
       └──────┬───────┘        │ Segmentation)   │
              │                └─────────────────┘
              └─────────────────────────┘
                            │
                            v
                    ┌─────────────────┐
                    │ Report          │
                    │ (KPIs, SLA,     │
                    │ Exceptions)     │
                    └─────────────────┘
                            │
                            v
                    ┌─────────────────┐
                    │ Review &        │
                    │ Continuous      │
                    │ Improvement     │
                    └─────────────────┘
