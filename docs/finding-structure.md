# NexusTask — Finding Structure Design

**12-field finding record with auto-fixability triad**

---

## Design Principle

A finding is not just a bug report — it's a **remediation package**. Every field serves the fix workflow.

---

## 12 Fields (Verified)

### Identification
| Field | Type | Required | Purpose |
|-------|------|----------|---------|
| `ruleId` | String | Yes | Links to audit rule definition; enables rule-level analytics |
| `category` | Enum (6) | Yes | Routing, filtering, category-level scoring |
| `severity` | Enum (5) | Yes | Prioritization, SLA, score impact |
| `confidence` | Enum (3) | Yes | Determines auto-fix eligibility, human review need |

### Context
| Field | Type | Required | Purpose |
|-------|------|----------|---------|
| `whatWasFound` | String | Yes | Human-readable finding description |
| `whyItMatters` | String | Yes | Business justification for remediation priority |
| `affectedTables` | Array<String> | Yes | Scope for fix impact analysis |
| `affectedFields` | Array<String> | Yes | Column-level precision for fix generation |
| `affectedRecordCount` | Number | Yes | Quantitative scope; prioritization |
| `sampleRecords` | Array | Yes | Evidence for human review; fix validation |

### Remediation
| Field | Type | Required | Purpose |
|-------|------|----------|---------|
| `recommendedAction` | String | Yes | Strategic guidance (what to do) |
| `proposedFix` | String | Conditional | Tactical fix (SQL, script, procedure) |
| `autoFixability` | Boolean | Yes | Determines automation path |
| `riskOfFix` | Enum (3) | Conditional | Safety assessment |
| `backupRequirement` | Enum (3) | Conditional | Operational safety |

---

## Auto-Fixability Triad Logic

```
IF autoFixability = true:
    IF riskOfFix = LOW AND backupRequirement = NOT_NEEDED:
        → Eligible for AUTO execution
    ELSE IF riskOfFix = MEDIUM AND backupRequirement = RECOMMENDED:
        → SUGGEST with backup prompt
    ELSE IF riskOfFix = HIGH AND backupRequirement = REQUIRED:
        → ASK_FIRST + mandatory backup confirmation
    ELSE:
        → ASK_FIRST (safety default)
ELSE:
    → Human remediation only (track in Remediation Tracker)
```

---

## Field Dependencies

| Field | Depends On | Validation |
|-------|------------|------------|
| `proposedFix` | `autoFixability = true` | Required if autoFixable |
| `riskOfFix` | `autoFixability = true` | Required if autoFixable |
| `backupRequirement` | `autoFixability = true` | Required if autoFixable |
| `sampleRecords` | `affectedRecordCount > 0` | At least 1 if count > 0 |
| `affectedFields` | `affectedTables` non-empty | Must reference valid columns |

---

## Sample Record Structure

```json
{
  "ruleId": "PRD-003",
  "category": "product_data",
  "severity": "high",
  "confidence": "certain",
  "whatWasFound": "12 products missing unit_of_measure (NOT NULL constraint violated)",
  "whyItMatters": "Inventory valuation fails; shipping labels print empty UoM; ERP rejects orders",
  "affectedTables": ["products"],
  "affectedFields": ["unit_of_measure"],
  "affectedRecordCount": 12,
  "sampleRecords": [
    {"sku": "PRD-001", "name": "Widget A", "unit_of_measure": null},
    {"sku": "PRD-002", "name": "Widget B", "unit_of_measure": null}
  ],
  "recommendedAction": "Populate missing UoM from master data; add NOT NULL constraint",
  "proposedFix": "UPDATE products SET unit_of_measure = 'EA' WHERE unit_of_measure IS NULL AND sku IN ('PRD-001','PRD-002',...); ALTER TABLE products ALTER COLUMN unit_of_measure SET NOT NULL;",
  "autoFixability": true,
  "riskOfFix": "LOW",
  "backupRequirement": "RECOMMENDED"
}
```

---

## Remediation Tracker Integration

| Tracker Field | Source |
|---------------|--------|
| `findingId` | Finding PK |
| `status` | `open` / `in_progress` / `verified` / `wont_fix` |
| `assignee` | User |
| `dueDate` | Derived from severity SLA |
| `notes` | Free text |
| `verificationQuery` | SQL to verify fix |
| `verifiedAt` | Timestamp |
| `verifiedBy` | User |

---

## Analytics Enabled

| Query | Purpose |
|-------|---------|
| Findings by category × severity | Portfolio risk heatmap |
| Auto-fixable % by category | Automation opportunity |
| Average riskOfFix by category | Systemic risk profile |
| Backup compliance rate | Operational safety |
| Time-to-verify by severity | Process efficiency |
| RuleId frequency | Rule effectiveness |