# NexusTask — Development Lineage

**Database Analysis App → Data Triage → NexusTask**

---

## Lineage Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                    NEXUSTASK (Current)                          │
│  Base44 App ID: 6aa13210ce4c55dd6b129374                       │
│  4 entities • RLS • 6-dim scoring • 6×5×3 taxonomy            │
└─────────────────────────────────────────────────────────────────┘
                              ▲
                              │ Evolution
                              │
┌─────────────────────────────────────────────────────────────────┐
│              DATA TRIAGE / PROTOTYPE (2026-08)                 │
│  Portfolio Evidence Package tooling layer                      │
│  • SAMPLE_CATALOGUE_AUDIT.md (specification)                   │
│  • transmat_product_audit_structure_v1.xlsx (workbook)         │
│  • Quality rules, exception handling, issue logs, QA controls  │
│  • Local synthetic implementations                             │
└─────────────────────────────────────────────────────────────────┘
                              ▲
                              │ Concept refinement
                              │
┌─────────────────────────────────────────────────────────────────┐
│            DATABASE ANALYSIS APP (Concept Phase)               │
│  Early terminology for database health analysis product        │
│  Referenced in: AI_AUTOMATION_LAB/BASELINE.md                  │
│  Referenced in: PROJECTS.md (Project 03)                       │
└─────────────────────────────────────────────────────────────────┘
```

---

## Phase 1: Database Analysis App (Concept)

**Timeframe:** Early 2026 (referenced in planning docs)  
**Scope:** Conceptual product for database health analysis  
**Artifacts:** Planning references only; no implementation

**Key Documents:**
- `PROJECTS.md` → Project 03: "Portfolio Evidence Package" references "database-analysis application"
- `AI_AUTOMATION_LAB/BASELINE.md` → Lists "Database analysis app" as learning target

**Status:** Superseded by NexusTask as canonical name

---

## Phase 2: Data Triage / Prototype (Portfolio Evidence Package)

**Timeframe:** August 2026  
**Scope:** Local tooling for portfolio evidence demonstrations  
**Artifacts:**

| Artifact | Location | Purpose |
|----------|----------|---------|
| `SAMPLE_CATALOGUE_AUDIT.md` | `OUTPUT/` | Audit specification with issue log, rules, before/after |
| `transmat_product_audit_structure_v1.xlsx` | `PORTFOLIO_EVIDENCE_INBOX/` | Product audit workbook template |
| Quality rules | `PORTFOLIO_EVIDENCE_PLAN.md` | Validation rules, exception handling |
| Issue log template | `SAMPLE_CATALOGUE_AUDIT.md` | Structured: issue ID, type, evidence, action, confidence |
| QA controls | `PORTFOLIO_EVIDENCE_PLAN.md` | Reconciliation, traceability, ambiguity handling |

**Key Innovations (Later Adopted by NexusTask):**
| Prototype Feature | NexusTask Evolution |
|-------------------|---------------------|
| Issue log with confidence | Finding.confidence (3 levels) |
| Exception handling (non-auto-fix) | Finding.autoFixability + riskOfFix + backupRequirement |
| Quality rules per field | Audit rule engine (6 categories) |
| Before/after metrics | AuditProject scoring (6 dimensions) |
| Traceability (issue ID → action) | Finding.ruleId + proposedFix |
| Ambiguity preservation | Finding.confidence = suspected |

**Status:** **Separate evidence layer** — Not the product. Portfolio Evidence Package uses methodology for demonstrations; NexusTask is the product implementation.

---

## Phase 3: NexusTask (Base44 Implementation — Current)

**Timeframe:** Deployed on Base44 (date unknown; inspected 2026-09-22)  
**Platform:** Base44  
**App ID:** `6aa13210ce4c55dd6b129374`  
**Canonical Name:** NexusTask (NOT "Nexys Task")

**Architecture (Verified):**
- 4 entities: Company, AuditProject, Finding, User
- Row-level security on all business entities
- 6-dimensional scoring + composite
- Finding taxonomy: 6 categories × 5 severities × 3 confidences
- 12-field finding structure with auto-fixability triad
- Data sources: demo, CSV, XLSX, connector_future
- 81 connector types available; 0 connected

**Advances Over Prototype:**
| Prototype Limitation | NexusTask Solution |
|----------------------|-------------------|
| Single-domain (catalogue) | Multi-domain (6 categories) |
| No security model | Row-level access controls |
| No scoring | 6-dim health scoring + composite |
| Manual issue log | Structured Finding with 12 fields |
| No auto-fix assessment | Auto-fixability + risk + backup triad |
| Local only | Deployed, multi-tenant ready |
| Single source (spreadsheet) | 4 sources + connector framework |

---

## Portfolio Evidence Package (Separate Layer)

**Relationship:** Uses NexusTask methodology for portfolio demonstrations  
**Artifacts:** Synthetic catalogue audit, case study narratives  
**Status:** Active, independent deliverable for job-market positioning

**Key Distinction:**
- **NexusTask** = Product (Base44 deployed, commercial audit tool)
- **Portfolio Evidence Package** = Evidence layer (synthetic demos, case studies)

They share methodology but are **separate deliverables** with different audiences.

---

## Naming History

| Name | Phase | Status |
|------|-------|--------|
| Database Analysis App | Concept | **SUPERSEDED** |
| Data Triage | Prototype tooling | **SUPERSEDED** (as product name) |
| Nexys Task | Early recovery | **INCORRECT** (typo) |
| **NexusTask** | Base44 product | **CANONICAL** |

---

## Reconciliation Decision

| Local Reference | Consolidation |
|-----------------|---------------|
| "Database Analysis App" | → NexusTask (concept phase) |
| "Data Triage" | → NexusTask (prototype tooling phase) |
| "Database Analysis / Data Triage" (Project 03) | → NexusTask lineage |
| "Nexys Task" (recovery manifest) | → NexusTask (canonical name) |

**Portfolio Evidence Package** remains separate as evidence/case-study layer.

---

## Evidence Mapping

| NexusTask Feature | Prototype Evidence | Base44 Verified |
|-------------------|-------------------|-----------------|
| Finding.category (6) | Implicit in issue types | ✅ |
| Finding.severity (5) | Implicit in confidence | ✅ |
| Finding.confidence (3) | High/Medium/Low in issue log | ✅ |
| Finding.autoFixability | Exception handling rules | ✅ |
| Finding.riskOfFix | Not in prototype | ✅ (new) |
| Finding.backupRequirement | Not in prototype | ✅ (new) |
| AuditProject scoring (6 dim) | Before/after metrics | ✅ |
| RLS (creator/admin) | Not in prototype | ✅ (new) |
| Data sources (4) | Spreadsheet only | ✅ |