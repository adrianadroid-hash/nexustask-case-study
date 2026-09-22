# NexusTask — Security Model & Source Availability Notice

**Last updated:** 2026-09-22  
**Repository type:** Documentation-only case study (no source code)  
**Canonical name:** NexusTask (NOT "Nexys Task")

---

## Source Availability Status

| Aspect | Status | Detail |
|--------|--------|--------|
| **Source code in this repository** | **NONE** | This repository contains only documentation |
| **Canonical source location** | Base44 (App ID: `6aa13210ce4c55dd6b129374`) | Platform-hosted; not locally exportable without Builder plan |
| **Source export capability** | **BLOCKED** | Requires Base44 Builder plan subscription |
| **Claimed source ownership** | **NO CLAIM MADE** | This repo does not assert ownership of exportable source |
| **Future source integration** | **PLANNED** | If/when export available, will be added under `src/` after audit |

> **IMPORTANT:** Do not interpret this repository as an open-source release of the NexusTask application. It is a documented case study of a product whose source is currently platform-locked.

---

## Security Architecture (Verified from Base44 Inspection)

| Layer | Mechanism | Status |
|-------|-----------|--------|
| **Platform authentication** | Base44-managed | ✅ VERIFIED (platform default) |
| **Data storage** | Base44-managed backend | ✅ VERIFIED (platform default) |
| **Row-level access controls** | On Company, AuditProject, Finding | ✅ VERIFIED (explicitly confirmed) |
| **Access restriction scope** | Creator or administrators only | ✅ VERIFIED |
| **Multi-tenant isolation** | By design via RLS | ✅ VERIFIED |
| **Environment variables / secrets** | Base44-managed | ❓ NOT AUDITED (names only visible in export) |
| **External API integrations** | 81 connector types available; 0 connected | ✅ VERIFIED (zero active integrations) |
| **AI provider integrations** | Not verified | ❓ NOT AUDITED (requires export) |
| **Data retention / deletion** | Not verified | ❓ NOT AUDITED (requires export) |
| **Backup / export controls** | Base44 platform | ❓ NOT AUDITED (requires export) |

---

## Data Sensitivity Considerations

| Data Category | Present in NexusTask? | Sensitivity | Notes |
|---------------|----------------------|-------------|-------|
| **Client business data** (Company: name, industry, tax ID, contacts) | YES | **HIGH** | Commercial confidential |
| **Audit project data** (scores, finding counts, dataset summaries) | YES | **HIGH** | Reveals client data health |
| **Individual findings** (affected tables/fields, sample records, proposed fixes) | YES | **HIGH** | May expose specific data quality issues |
| **Auto-fixability/risk assessments** | YES | **MEDIUM-HIGH** | Operational guidance |
| **User/account data** | YES (User entity) | **MEDIUM** | Standard account data |

**Key privacy principles for any future source integration:**
- No production client/audit data in repository
- Synthetic fixtures only for demonstrations (sanitized)
- Schema/migrations without data
- Configuration templates without secrets
- `.gitignore` must exclude: `.env*`, `*.key`, `*.pem`, `secrets/`, `data/`, `*.db`, `*.sqlite`, `*.csv`, `*.xlsx` (test fixtures only in controlled locations)

---

## Credential & Secret Handling

| Item | Current Status | Required for Source Integration |
|------|----------------|--------------------------------|
| Base44 API tokens | Not in this repo | Must be excluded via `.gitignore` |
| AI provider API keys (if any) | Not in this repo | Must be excluded via `.gitignore` |
| Database credentials | Not in this repo (Base44-managed) | N/A — platform managed |
| Third-party service credentials | Not in this repo (0 connectors active) | Must be excluded via `.gitignore` |
| `PROGRAMS/login-Nex.txt` | **Excluded by path** — relationship to NexusTask unknown | **Human review required** to clarify; contents not read; exclude from all repos regardless |

**No secret values have been read, exposed, or committed in this repository's preparation.**

---

## Repository Security Checklist (This Repo)

| Check | Status | Method |
|-------|--------|--------|
| No `.env`, `.env.*`, `secrets/`, `*.key`, `*.pem` files | ✅ PASS | File enumeration |
| No credentials in documentation | ✅ PASS | Content review |
| No production/client data | ✅ PASS | No data files present |
| No private identifiers (emails, phones, IDs) | ✅ PASS | Content review |
| No Base44 project credentials | ✅ PASS | Not present |
| `login-Nex.txt` excluded | ✅ PASS | Not in repo; path excluded |
| Clear source limitation notice | ✅ PASS | README + this file |
| License appropriate for documentation | ✅ PASS | CC BY 4.0 declared |

---

## Platform Dependency Risk (Base44)

| Risk | Assessment | Mitigation |
|------|------------|------------|
| **Source export never available** | Medium | Document architecture thoroughly now; treat as design portfolio piece |
| **Exported source tightly coupled to Base44 runtime** | Medium | Audit immediately post-export; document coupling; consider adapter layer |
| **Platform deprecation / pricing change** | Low | Base44 is active; monitor; architecture is portable conceptually |
| **Finding auto-fix logic has safety implications** | Medium | Document risk/backup fields; verify during export audit |

---

## Commercial Sensitivity Note

NexusTask operates in the **commercial data audit domain**. Its methodology (finding taxonomy, scoring, auto-fixability) represents **proprietary product IP**. While this case-study repository documents the architecture for portfolio purposes:

- The **finding taxonomy** (6 categories × 5 severities × 3 confidences) is a designed classification system
- The **6-dimensional scoring model** is a designed assessment framework
- The **12-field finding structure** with auto-fixability triad is a designed remediation workflow
- These designs are **portfolio evidence of product design capability** — not open-source methodologies

Any future source publication should evaluate whether implementation details expose trade secrets beyond the architectural documentation already presented here.

---

## Compliance Notes

| Regulation | Applicability | Current State |
|------------|---------------|---------------|
| **GDPR** (personal data in client records) | HIGH — Company, Finding may contain personal data | No production data in repo; schema-only if source exported |
| **Trade secret protection** | HIGH — Audit methodology is product IP | Documentation describes architecture; implementation details not exposed |
| **License compliance** (third-party assets) | UNKNOWN | Requires asset audit post-export |

---

## Incident Response (If Source Becomes Available)

1. **Immediately audit** exported source for secrets, credentials, private data
2. **Apply `.gitignore`** before any `git add`: `.env*`, `*.key`, `*.pem`, `secrets/`, `data/`, `*.db`, `*.sqlite`, `node_modules/`, `dist/`, `build/`, `.vercel`, `.netlify`, `*.csv`, `*.xlsx` (except controlled test fixtures)
3. **Redact** any hardcoded configuration values referencing production resources
4. **Verify** no client records, audit data, session data, or production database exports included
5. **Document** any Base44-specific runtime dependencies
6. **Update** this SECURITY.md with findings
7. **Only then** consider GitHub publication with appropriate visibility

---

## Contact for Security Questions

Adriana Andreeva — contact via the repository owner's GitHub profile  
(No security.txt published; this is a public case-study documentation repository)