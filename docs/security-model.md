# NexusTask — Security Model

**Row-level access controls and multi-tenant isolation**

---

## Design Principle

**Data isolation by default.** Every audit project, company, and finding belongs to a creator — and only that creator (or administrators) can access it. No cross-tenant leakage.

---

## RLS Configuration (Verified)

| Entity | RLS Policy | Access Rule |
|--------|------------|-------------|
| **Company** | Enabled | `creator_id = current_user_id OR role = 'admin'` |
| **AuditProject** | Enabled | `creator_id = current_user_id OR role = 'admin'` |
| **Finding** | Enabled | `creator_id = current_user_id OR role = 'admin'` (via AuditProject) |
| **User** | Platform default | Self + admin |

---

## Ownership Chain

```
User (creator)
    │
    ├── Company (owned by User)
    │       │
    │       └── AuditProject (owned by User, linked to Company)
    │               │
    │               └── Finding (owned by User, linked to AuditProject)
    │
    └── (direct) AuditProject, Finding
```

**Implication:** Deleting a Company cascades to its AuditProjects and Findings (or soft-archives). User owns all three directly for query simplicity.

---

## Access Scenarios

| Scenario | Access Granted? |
|----------|-----------------|
| User views own Company | ✅ |
| User views own AuditProject | ✅ |
| User views own Finding | ✅ |
| User views another user's Company | ❌ |
| User views another user's AuditProject | ❌ |
| User views another user's Finding | ❌ |
| Admin views any Company | ✅ |
| Admin views any AuditProject | ✅ |
| Admin views any Finding | ✅ |
| Connector/service account | ❌ (unless impersonation configured) |

---

## Multi-Tenant Safety

| Risk | Mitigation |
|------|------------|
| Cross-tenant query | RLS at database level; impossible to bypass |
| Finding leakage in exports | Export filtered by current_user |
| Aggregated analytics | Admin-only; anonymized |
| Connector data mixing | Connector runs in user context; RLS applies |

---

## Authentication (Base44-Managed)

| Aspect | Status |
|--------|--------|
| **Providers** | Base44 platform (not verified) |
| **Session management** | Base44 platform |
| **Password policy** | Base44 platform |
| **MFA** | Base44 platform (if available) |
| **Account lifecycle** | Base44 platform |

---

## Authorization Model

| Role | Permissions |
|------|-------------|
| **User (creator)** | CRUD own Company, AuditProject, Finding |
| **Admin** | CRUD all; user management; system config |
| **Viewer (future)** | Read-only own data |
| **Auditor (future)** | Read assigned projects |

---

## Data Sensitivity

| Data Category | Entity | Sensitivity | Controls |
|---------------|--------|-------------|----------|
| Client identity | Company | HIGH | RLS + encryption at rest |
| Audit results | AuditProject | HIGH | RLS + audit trail |
| Specific findings | Finding | HIGH | RLS + sample record redaction |
| Remediation actions | Finding.proposedFix | MEDIUM | RLS |
| User credentials | User | CRITICAL | Base44-managed |

---

## Audit Trail

| Event | Logged |
|-------|--------|
| Company CRUD | ✅ |
| AuditProject CRUD | ✅ |
| Finding CRUD | ✅ |
| Finding verification | ✅ |
| Export/download | ✅ |
| RLS policy change | ✅ (admin only) |
| Login/access | Base44 platform |

---

## Compliance Considerations

| Regulation | Applicability | Current State |
|------------|---------------|---------------|
| **GDPR** | HIGH — Company/Finding may contain personal data | RLS + export/delete on request |
| **SOC 2** | MEDIUM — Audit trail, access controls | RLS + logging; needs formal audit |
| **ISO 27001** | MEDIUM — Data classification, access control | RLS + classification; needs certification |

---

## Unverified (Requires Base44 Export)

- Exact RLS policy syntax (SQL/ORM level)
- Base44 platform encryption details
- Connector credential isolation
- Admin role granularity
- Session timeout / concurrent session limits