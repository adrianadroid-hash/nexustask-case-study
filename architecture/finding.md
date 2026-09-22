# NexusTask — Finding Entity Specification

**Individual audit finding with auto-fixability triad**

---

## Purpose

The atomic unit of audit output. Every finding is a **complete remediation package**: what, why, how bad, how sure, what to do, can it be auto-fixed, risk, backup needed.

---

## Fields (Verified — 12 Fields)

### Identification
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `ruleId` | String | Yes | Audit rule identifier (e.g., `PRD-003`) |
| `category` | Enum (6) | Yes | `product_data` / `pricing` / `inventory` / `relational_integrity` / `master_data` / `database_health` |
| `severity` | Enum (5) | Yes | `critical` / `high` / `medium` / `low` / `informational` |
| `confidence` | Enum (3) | Yes | `certain` / `strong_signal` / `suspected` |

### Context
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `whatWasFound` | String | Yes | Finding description |
| `whyItMatters` | String | Yes | Business impact rationale |
| `affectedTables` | Array<String> | Yes | Table names |
| `affectedFields` | Array<String> | Yes | Column names |
| `affectedRecordCount` | Number | Yes | Quantitative scope |
| `sampleRecords` | Array | Yes | Representative examples |

### Remediation
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `recommendedAction` | String | Yes | Strategic guidance |
| `proposedFix` | String | Conditional | Tactical fix (SQL, script) |
| `autoFixability` | Boolean | Yes | Can be automatically fixed |
| `riskOfFix` | Enum (3) | Conditional | `LOW` / `MEDIUM` / `HIGH` |
| `backupRequirement` | Enum (3) | Conditional | `REQUIRED` / `RECOMMENDED` / `NOT_NEEDED` |

---

## Category × Severity × Confidence Matrix (Verified)

| Category | Severity (5) | Confidence (3) |
|----------|--------------|----------------|
| `product_data` | critical/high/medium/low/informational | certain/strong_signal/suspected |
| `pricing` | critical/high/medium/low/informational | certain/strong_signal/suspected |
| `inventory` | critical/high/medium/low/informational | certain/strong_signal/suspected |
| `relational_integrity` | critical/high/medium/low/informational | certain/strong_signal/suspected |
| `master_data` | critical/high/medium/low/informational | certain/strong_signal/suspected |
| `database_health` | critical/high/medium/low/informational | certain/strong_signal/suspected |

---

## Auto-Fixability Triad Logic

```
IF autoFixability = true:
    IF riskOfFix = LOW AND backupRequirement = NOT_NEEDED:
        → AUTO execution eligible
    ELSE IF riskOfFix = MEDIUM AND backupRequirement = RECOMMENDED:
        → SUGGEST with backup prompt
    ELSE IF riskOfFix = HIGH AND backupRequirement = REQUIRED:
        → ASK_FIRST + mandatory backup confirmation
    ELSE:
        → ASK_FIRST (safety default)
ELSE:
    → Human remediation only (tracked in Remediation Tracker)
```

---

## RiskOfFix Assessment Criteria

| Risk Level | Criteria |
|------------|----------|
| `LOW` | Single-table, reversible, no dependencies, idempotent |
| `MEDIUM` | Multi-table, reversible with care, some dependencies |
| `HIGH` | Schema changes, irreversible, cascading effects, data loss risk |

---

## BackupRequirement Decision

| Requirement | When |
|-------------|------|
| `REQUIRED` | Any data modification + risk ≥ MEDIUM |
| `RECOMMENDED` | Data modification + risk = LOW |
| `NOT_NEEDED` | Read-only fixes, metadata only, autoFixability=false |

---

## Relationships

| Relationship | Cardinality | Notes |
|--------------|-------------|-------|
| Finding → AuditProject | N : 1 | Required |
| Finding → Company | N : 1 | Via AuditProject |
| Finding → User | N : 1 | Owner (creator) |

---

## RLS Policy (Verified)

```sql
creator_id = current_user_id OR role = 'admin'
```

---

## Remediation Tracker Integration

| Tracker Field | Source |
|---------------|--------|
| `status` | `open` / `in_progress` / `verified` / `wont_fix` |
| `assignee` | User |
| `dueDate` | Derived from severity SLA |
| `verificationQuery` | SQL to verify fix |
| `verifiedAt` | Timestamp |
| `verifiedBy` | User |

---

## SLA by Severity

| Severity | Target Resolution |
|----------|-------------------|
| `critical` | 24 hours |
| `high` | 72 hours |
| `medium` | 2 weeks |
| `low` | Next sprint |
| `informational` | Backlog |

---

## Indexing

- (userId, auditProjectId) — project findings
- (userId, category) — category filtering
- (userId, severity) — severity filtering
- (userId, confidence) — confidence filtering
- (userId, autoFixability) — automation opportunity
- (auditProjectId, category, severity) — project analytics

---

## Data Sensitivity

| Field | Sensitivity | Notes |
|-------|-------------|-------|
| `sampleRecords` | HIGH | May contain real client data |
| `proposedFix` | MEDIUM | Operational guidance |
| `affectedRecordCount` | MEDIUM | Quantitative scope |
| `whatWasFound` / `whyItMatters` | MEDIUM | Business context |