# NexusTask — User Entity Specification

**Account entity with RLS context**

---

## Purpose

Standard account entity. Serves as **ownership anchor** for RLS on Company, AuditProject, Finding.

---

## Fields (Inferred — Not Fully Verified)

| Field | Type | Description |
|-------|------|-------------|
| `email` | String | Primary identifier |
| `name` | String | Display name |
| `role` | Enum | `user` / `admin` |
| `createdAt` | DateTime | Account creation |
| `lastLoginAt` | DateTime | Last authentication |
| `preferences` | Object | UI/config preferences |

---

## RLS Role (Critical)

| Role | Permissions |
|------|-------------|
| `user` | CRUD own Company, AuditProject, Finding |
| `admin` | CRUD all; user management; system config |

**RLS Implementation:** Every query on Company/AuditProject/Finding automatically filters by `creator_id = current_user_id` unless `role = 'admin'`.

---

## Ownership Chain

```
User (creator)
    │
    ├── Company (creator_id = user.id)
    ├── AuditProject (creator_id = user.id)
    └── Finding (creator_id = user.id)
```

Direct ownership on all three entities enables efficient queries without joins for RLS.

---

## Authentication (Base44-Managed)

| Aspect | Status |
|--------|--------|
| Providers | Base44 platform (not verified) |
| Session | Base44 platform |
| Password policy | Base44 platform |
| MFA | Base44 platform |
| Account recovery | Base44 platform |

---

## Indexing

- email (unique)
- role (admin queries)

---

## Data Sensitivity

| Field | Sensitivity |
|-------|-------------|
| email | HIGH (PII) |
| preferences | LOW-MEDIUM |

---

## GDPR Compliance

| Right | Implementation |
|-------|----------------|
| Access | Export all owned entities |
| Rectification | Update profile |
| Erasure | Anonymize owned entities; soft-delete account |
| Portability | JSON export of all data |
| Restriction | Suspend account |

---

## Unverified (Requires Base44 Export)

- Exact field definitions
- Authentication provider configuration
- Session token structure
- Role assignment mechanism
- Audit log for admin actions