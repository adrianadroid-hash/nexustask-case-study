# NexusTask — Company Entity Specification

**Business entity being audited**

---

## Purpose

Represents the **organization whose data is being audited**. Contains identity, classification, and contact metadata. All audit projects and findings link to a Company.

---

## Fields (Verified)

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| `name` | String | Required | Legal or trading name |
| `industry` | String | Optional | Sector classification |
| `country` | String | Optional | Jurisdiction (ISO 3166) |
| `taxVatId` | String | Optional | Tax/VAT registration number |
| `contact` | String | Optional | Primary contact (name, email, phone) |
| `notes` | String | Optional | Internal context |

---

## Relationships

| Relationship | Cardinality | Notes |
|--------------|-------------|-------|
| Company → AuditProject | 1 : N | One company, many audit engagements |
| Company → User | N : 1 | Owner (creator) |

---

## RLS Policy (Verified)

```sql
creator_id = current_user_id OR role = 'admin'
```

- User sees only their own companies
- Admin sees all

---

## Indexing

- (userId) — user's company list
- (userId, name) — search
- (userId, country) — jurisdiction filtering

---

## Cascade Behavior

| Event | Effect |
|-------|--------|
| Company deleted | Soft-archive: AuditProjects → archived, Findings → archived |
| Company.creator_id changed | Re-evaluate RLS for all linked AuditProjects/Findings |

---

## Data Sensitivity

| Field | Sensitivity | Notes |
|-------|-------------|-------|
| `name` | HIGH | Client identity |
| `taxVatId` | HIGH | Regulatory identifier |
| `contact` | MEDIUM | PII |
| `notes` | VARIABLE | May contain sensitive context |

---

## Audit Trail

| Event | Logged |
|-------|--------|
| Create | ✅ |
| Update | ✅ |
| Archive/Delete | ✅ |
| Export | ✅ |