# NexusTask — Audit Methodology

**Finding taxonomy, severity, confidence, and scoring model**

---

## Design Principle

Audit findings must be **actionable, not just descriptive**. Every finding includes: what, why, how bad, how sure, what to do, can it be auto-fixed, what's the risk, is backup needed.

---

## Finding Taxonomy (Verified)

### 6 Categories

| Category | Scope | Example Findings |
|----------|-------|------------------|
| `product_data` | Catalogue/master data attributes | Missing SKU, duplicate name, invalid unit, format violation |
| `pricing` | Price integrity & logic | Negative margin, stale price, currency mismatch, margin < threshold |
| `inventory` | Stock accuracy & valuation | Qty mismatch, UoM inconsistency, negative stock, valuation error |
| `relational_integrity` | Referential integrity | Orphaned records, broken FKs, missing parent, circular refs |
| `master_data` | Reference data quality | Incomplete records, standardization gaps, format violations |
| `database_health` | Schema & structural issues | Missing indexes, constraint violations, schema drift |

### 5 Severity Levels

| Severity | Definition | SLA Target |
|----------|------------|------------|
| `critical` | Data loss, financial misstatement, compliance violation | Immediate (24h) |
| `high` | Significant business impact, wrong decisions likely | 72h |
| `medium` | Measurable quality degradation, efficiency loss | 2 weeks |
| `low` | Minor inconsistency, cosmetic | Next sprint |
| `informational` | Observation, best practice, tech debt | Backlog |

### 3 Confidence Levels

| Confidence | Definition | Evidence Required |
|------------|------------|-------------------|
| `certain` | Rule violation proven deterministically | Schema constraint, exact match |
| `strong_signal` | High-probability pattern, minor ambiguity | Statistical threshold, heuristic match |
| `suspected` | Anomaly detected, needs human validation | Outlier, pattern deviation, incomplete data |

---

## Finding Structure (Verified — 12 Fields)

| Field | Purpose | Example |
|-------|---------|---------|
| `ruleId` | Audit rule identifier | `PRD-001` |
| `whatWasFound` | Finding description | "12 products missing unit of measure" |
| `whyItMatters` | Business impact rationale | "Cannot calculate inventory value; shipping errors likely" |
| `affectedTables` | Table names | `["products", "inventory"]` |
| `affectedFields` | Field names | `["unit_of_measure", "base_uom"]` |
| `affectedRecordCount` | Quantitative scope | `12` |
| `sampleRecords` | Representative examples | `[{sku: "PRD-001", ...}, ...]` |
| `recommendedAction` | High-level guidance | "Populate UoM from master data; add NOT NULL constraint" |
| `proposedFix` | Specific remediation | `UPDATE products SET unit_of_measure = 'EA' WHERE sku IN (...)` |
| `autoFixability` | Can be automatically fixed | `true` / `false` |
| `riskOfFix` | Risk assessment of proposed fix | `LOW` / `MEDIUM` / `HIGH` |
| `backupRequirement` | Backup needed before fix | `REQUIRED` / `RECOMMENDED` / `NOT_NEEDED` |

---

## Auto-Fixability Triad

Every finding includes three safety fields:

| Field | Values | Decision Logic |
|-------|--------|----------------|
| `autoFixability` | `true` / `false` | Deterministic, reversible, no data loss |
| `riskOfFix` | `LOW` / `MEDIUM` / `HIGH` | Scope, reversibility, dependency impact |
| `backupRequirement` | `REQUIRED` / `RECOMMENDED` / `NOT_NEEDED` | Data modification + risk level |

**Decision Matrix:**
| autoFixability | riskOfFix | backupRequirement | Action |
|----------------|-----------|-------------------|--------|
| true | LOW | NOT_NEEDED | Safe for AUTO |
| true | MEDIUM | RECOMMENDED | SUGGEST with backup prompt |
| true | HIGH | REQUIRED | ASK_FIRST + mandatory backup |
| false | any | any | Human remediation only |

---

## Scoring Model (Verified — 6 Dimensions + Composite)

| Score | Weight | Description |
|-------|--------|-------------|
| **Data Quality** | 25% | Completeness, consistency, validity, format |
| **Structural Integrity** | 20% | Referential integrity, FKs, orphans, constraints |
| **Pricing Integrity** | 20% | Margin logic, currency, staleness, anomalies |
| **Inventory Integrity** | 15% | Stock accuracy, UoM, valuation |
| **Master Data Quality** | 10% | Standardization, completeness, format |
| **Database Health** | 10% | Schema, indexes, constraints, performance |

**Composite Health Score** = Σ(dimensionScore × weight)

Each dimension: 0–100 scale derived from finding counts by severity/confidence.

---

## Severity → Score Impact

| Severity | Weight in Dimension Score |
|----------|---------------------------|
| `critical` | 10× |
| `high` | 5× |
| `medium` | 2× |
| `low` | 1× |
| `informational` | 0.1× |

Confidence multiplier: `certain=1.0`, `strong_signal=0.7`, `suspected=0.3`

---

## Remediation Workflow

```
Finding created (with autoFixability, riskOfFix, backupRequirement)
         │
         ▼
┌────────────────────────────────────────────┐
│          Remediation Tracker               │
│  Status: open → in_progress → verified     │
│  Assignee, due date, notes                 │
└──────────────────┬─────────────────────────┘
                   │
         ┌─────────┴─────────┐
         ▼                   ▼
   autoFixable           manual
         │                   │
         ▼                   ▼
  Backup (per req)    Human implements
         │                   │
         ▼                   ▼
  Execute fix         Verify + close
         │                   │
         └─────────┬─────────┘
                   ▼
         Finding.verified = true
         AuditProject scores recomputed
```

---

## AuditProject Status Flow

```
draft → running → completed → archived
  │        │
  │        └→ failed (data source error)
  │
  └→ cancelled
```

---

## Data Source Flexibility

| Source | Description | Use Case |
|--------|-------------|----------|
| `demo` | Built-in synthetic dataset | Evaluation, training |
| `CSV` | Comma-separated upload | Ad-hoc audit |
| `XLSX` | Excel upload | Business user friendly |
| `connector_future` | Base44 connectors (81 types) | Production ERP/CRM/DB |

---