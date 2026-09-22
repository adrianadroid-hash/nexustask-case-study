# NexusTask — Architecture Overview

**High-level system architecture**

---

## System Context

```
┌─────────────────────────────────────────────────────────────┐
│                        Base44 Platform                       │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                  NexusTask Application               │    │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌────────┐  │    │
│  │  │   Data   │ │  Logic   │ │   UI     │ │  Auth  │  │    │
│  │  │  Layer   │ │  Layer   │ │  Layer   │ │        │  │    │
│  │  └──────────┘ └──────────┘ └──────────┘ └────────┘  │    │
│  └─────────────────────────────────────────────────────┘    │
│         │              │              │              │        │
│         ▼              ▼              ▼              ▼        │
│  ┌─────────────────────────────────────────────────────┐    │
│  │              Base44 Services                         │    │
│  │  (Database, Auth, Connectors, Deploy, Runtime)       │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

---

## Data Layer (4 Entities)

| Entity | Responsibility | Key Relationships |
|--------|---------------|-------------------|
| **Company** | Business being audited | 1:N AuditProjects, 1:1 User (owner) |
| **AuditProject** | Audit engagement + scoring | N:1 Company, 1:N Findings, N:1 User |
| **Finding** | Individual audit finding | N:1 AuditProject, N:1 User (via project) |
| **User** | Account + RLS context | Owns Companies, AuditProjects, Findings |

---

## Logic Layer

### Audit Engine
```
Data Source (CSV/XLSX/Demo/Connector)
         │
         ▼
┌──────────────────────────────────────────┐
│         Audit Rule Engine                │
│  • Structural integrity checks           │
│  • Data quality rules (completeness,     │
│    consistency, validity, format)        │
│  • Pricing integrity rules               │
│  • Inventory integrity rules             │
│  • Master data quality rules             │
│  • Referential integrity checks          │
└──────────────────┬───────────────────────┘
                   │
                   ▼
         ┌─────────┴─────────┐
         ▼                   ▼
   Findings              Scores
 (6 cats × 5 sev        (6 dims +
  × 3 conf)             composite)
         │                   │
         └─────────┬─────────┘
                   ▼
        AuditProject + Findings
```

### Scoring Pipeline
```
Raw findings → Category aggregation → Dimension scores → Composite health
                    │                        │
                    ▼                        ▼
          Finding counts by         Weighted combination:
          severity/confidence       data quality (25%)
                                    structural (20%)
                                    pricing (20%)
                                    inventory (15%)
                                    master data (10%)
                                    database health (10%)
```

---

## UI Layer (Not Verified — Requires Base44 Access)

### Anticipated Screens
| Screen | Primary Entities | Purpose |
|--------|-----------------|---------|
| **Dashboard** | AuditProject (list + scores) | Portfolio health view |
| **Project Detail** | AuditProject, Finding | Drill-down: scores, findings, dataset summary |
| **Finding Explorer** | Finding (filterable) | Search by category, severity, confidence, auto-fixability |
| **Remediation Tracker** | Finding (status) | Track proposed fixes, risk, backup completion |
| **Data Source Manager** | AuditProject.source | Upload CSV/XLSX, configure connectors |
| **Company Manager** | Company | Client onboarding, settings |
| **Settings** | User, RLS | Preferences, security |

---

## Integration Layer

| Integration Type | Status | Details |
|-----------------|--------|---------|
| **Base44 Connectors** | 81 available, 0 connected | Extensible for ERP, CRM, databases |
| **File Upload** | Verified | CSV, XLSX parsers |
| **Demo Dataset** | Verified | Built-in demonstration |
| **Auth** | Base44-managed | Platform default + RLS |

---

## Security Boundaries (Verified)

| Boundary | Mechanism |
|----------|-----------|
| **Tenant isolation** | Row-level security on Company, AuditProject, Finding |
| **Access rule** | Records visible to creator OR administrators only |
| **Data ownership** | Company → AuditProject → Finding ownership chain |
| **Secrets** | Base44 environment management |
| **API keys** | Base44 connector configuration |

---

## Deployment & Runtime

| Aspect | Status |
|--------|--------|
| **Platform** | Base44 (hosted) |
| **Export capability** | Requires Builder plan |
| **Build process** | Base44-managed |
| **Runtime** | Base44 runtime (not verified) |
| **Scaling** | Base44-managed |

---

## Scalability Considerations

| Concern | Architectural Approach |
|---------|------------------------|
| **Finding volume** | Finding partitioned by AuditProject; indexed by category/severity |
| **AuditProject scoring** | Computed on finding changes; cached composite |
| **Connector fan-out** | 81 connector types; lazy initialization |
| **RLS evaluation** | Database-level; minimal application overhead |

---

## Technical Debt / Known Gaps

| Gap | Impact | Resolution Path |
|-----|--------|-----------------|
| **No local export** | Portability blocked | Builder plan required |
| **AI behavior unknown** | Transparency gap | Export prompts/models/tools if any |
| **Connector config not audited** | Integration surface unknown | Export + review |
| **Asset licenses not audited** | Compliance risk | Export + audit |