# NexusTask — Entity Relationships

**Complete entity-relationship diagram (textual)**

---

## Overview

```
User (1) ─────< Company (N)
User (1) ─────< AuditProject (N)
User (1) ─────< Finding (N) [via AuditProject ownership]

Company (1) ─────< AuditProject (N)
AuditProject (1) ─────< Finding (N)

Finding (N) ─────> AuditProject (1)
Finding (N) ─────> Company (1) [via AuditProject]
```

---

## Relationship Cardinalities

| Relationship | Cardinality | Notes |
|--------------|-------------|-------|
| User → Company | 1 : N | Owner |
| User → AuditProject | 1 : N | Owner |
| User → Finding | 1 : N | Owner (direct for query simplicity) |
| Company → AuditProject | 1 : N | Business being audited |
| AuditProject → Finding | 1 : N | Findings belong to project |
| Finding → AuditProject | N : 1 | Required |
| Finding → Company | N : 1 | Via AuditProject.companyId |

---

## RLS Policy (Verified)

| Entity | Policy |
|--------|--------|
| **Company** | `creator_id = current_user_id OR role = 'admin'` |
| **AuditProject** | `creator_id = current_user_id OR role = 'admin'` |
| **Finding** | `creator_id = current_user_id OR role = 'admin'` |

**Implication:** Users see only their own companies, projects, findings. Admins see all.

---

## Key Indices (Logical)

| Entity | Index | Purpose |
|--------|-------|---------|
| Company | (userId) | User's companies |
| AuditProject | (userId, companyId) | User's projects per company |
| AuditProject | (userId, status) | Active project list |
| AuditProject | (userId, createdAt) | Chronological |
| Finding | (userId, auditProjectId) | Project findings |
| Finding | (userId, category) | Category filtering |
| Finding | (userId, severity) | Severity filtering |
| Finding | (userId, confidence) | Confidence filtering |
| Finding | (userId, autoFixability) | Automation opportunity |
| Finding | (auditProjectId, category, severity) | Project analytics |

---

## Cascade Behaviors

| Parent Deleted | Child Behavior |
|----------------|----------------|
| User | Soft-delete owned entities (GDPR: anonymize or delete) |
| Company | Cascade: Archive AuditProjects → Archive Findings |
| AuditProject | Cascade: Archive Findings |
| Finding | No children |

---

## Data Retention

| Entity | Retention Policy |
|--------|------------------|
| Company | Indefinite (client record) |
| AuditProject | Indefinite (audit trail) |
| Finding | Indefinite (linked to AuditProject) |
| User | Platform-managed |

---

## Ownership Verification

Every query automatically filtered by `creator_id = current_user_id` at database level. Application-layer checks redundant but present for defense-in-depth.