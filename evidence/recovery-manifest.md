# Nexys Task Recovery Manifest

**Prepared:** 2026-09-22  
**Recovery mode:** Non-destructive documentation only  
**Project classification:** PRODUCT — standalone software application; newly recovered Master Register candidate  
**SOURCE_STATUS:** `BASE44_SOURCE_PENDING`

## Recovery conclusion

Nexys Task's canonical/current application is **reported by the user to be in Base44**. No complete local source export was found or verified. It must not be marked abandoned or missing because local source is absent.

Local references found:

- `OUTPUT/GITHUB_PORTFOLIO_PLAN.md`
- `OUTPUT/GITHUB_PORTFOLIO_PLAN.json`
- `PROGRAMS/login-Nex.txt` — filename/metadata only; contents were **not read**. Its relationship to Nexys Task is unverified, and it must remain excluded from Git.

Prior documentation states that Nexys Task was accidentally omitted from the earlier reconciliation and requires Base44 source retrieval. No local description, screenshot, specification, prompt, asset bundle, or prior source export was found by the workspace-only targeted search. Searches included `Nexys Task`, `Nexus Task`, `Nexys`, and `Nexus`.

## Evidence status

| Item | Result | Evidence grade |
|---|---|---|
| Canonical source location | Base44 | **REPORTED / NOT YET VERIFIED** |
| Newly recovered project candidate | Yes | **REPORTED + VERIFIED IN PRIOR LOCAL PLAN** |
| Complete local export | Not found | **VERIFIED FROM LOCAL SEARCH** |
| Possible credential artifact | `PROGRAMS/login-Nex.txt` exists; relationship unknown | **VERIFIED BY METADATA ONLY** |
| Current live application | Not inspected | **NOT YET VERIFIED** |
| Purpose, workflows, schema, integrations | Not documented locally | **NOT YET VERIFIED** |
| Current development state | Unknown pending Base44 audit | **NOT YET VERIFIED** |

## Base44 retrieval checklist

Retrieve from the authenticated Nexys Task Base44 project without publishing or changing it:

1. **Source and structure:** full export; file/application tree; framework/runtime versions; manifests and lockfiles; pages/screens; routes/navigation; layouts; components; themes; assets; build/preview/deployment instructions.
2. **Behavior and logic:** current features and live walkthrough; workflows; validation; automations; background jobs; notifications; error states; known TODOs and disabled/broken flows.
3. **AI behavior:** prompts/system instructions; model/provider settings; tools/functions; retrieval or recommendation logic; safety rules; token/cost controls.
4. **Data/schema:** tables/collections; fields/types; relations; indexes; constraints; migrations; access controls; retention/deletion; import/export; data categories. Do not retrieve private production rows unless separately authorized and necessary.
5. **Authentication:** providers; roles/permissions; session lifecycle; onboarding; password/reset or invitation flows; account deletion.
6. **Integrations/config:** APIs; webhooks; email/calendar/storage/analytics services; environment-variable names and setup requirements, never secret values; domains; project IDs; runtime/deployment details and Base44 coupling.
7. **Assets/ownership:** icons, images, fonts, templates, audio/video, third-party libraries, authorship, and licenses.
8. **Verification:** redacted screenshots/recording; export timestamp and checksum; private project URL/ID record; deployed version; tests and safe build/run results after export review.

## Source versus application data

Preserve source, architecture, behavior, schema, migrations, safe configuration templates, and synthetic fixtures independently from application data. Never commit private user/task content, production database exports, session data, credentials, keys, or tokens. The existing `login-Nex.txt` must be excluded by filename/path and not opened for repository preparation.

## Behavioral snapshot

- **PURPOSE:** Not established. The title suggests task-related functionality, but that inference is not treated as fact.
- **PRIMARY USER:** Not established.
- **CORE WORKFLOW:** Not established.
- **CURRENT FEATURES:** Not established.
- **NAVIGATION:** Not established.
- **INPUTS / OUTPUTS:** Not established.
- **PERSISTED STATE:** Base44 data storage may exist; schema and data categories are not verified.
- **AI BEHAVIOR:** Not established.
- **AUTOMATIONS / INTEGRATIONS:** Not established.
- **CURRENT LIMITATIONS / FUTURE IDEAS:** Not established.

All behavior fields are **REPORTED / NOT YET VERIFIED** until the Base44 application and export are audited.

## Relationships

- Treat as a **standalone product/project** and a **new Master Register candidate**.
- AI Operations tracks recovery only; no evidence makes Nexys Task a subproject of AI Operations Core or AI Automation Lab.
- Possible future Portfolio Website/Portfolio Evidence case study after source and claims verification.
- No verified relationship to MindFlow, NYX, ADDY_OS, Database Analysis / Data Triage, Traceable Works, or Nyx Designs.

## GitHub assessment

- **GITHUB_RECOMMENDATION:** `REVIEW REQUIRED`
- **PORTFOLIO_VALUE:** `MEDIUM`
- **OPEN_SOURCE_VALUE:** `LOW`
- **COMMERCIAL_SENSITIVITY:** `MEDIUM`
- **PUBLICATION_RISK:** `HIGH`

Ratings are conservative because the implementation, ownership, data handling, Base44 dependencies, and license position remain unseen.

## Proposed Master Project Register record

- **NAME:** Nexys Task
- **TYPE:** Software application / product
- **STATUS:** Newly recovered candidate; recovery pending; current development state not verified
- **PARENT:** None evidenced
- **RELATIONSHIPS:** Tracked by AI Operations; possible future Portfolio Website/Portfolio Evidence case study after audit
- **SOURCE LOCATION:** Base44 (canonical/current, user-reported); no complete local export verified
- **RECOVERY STATUS:** `BASE44_SOURCE_PENDING`

## Immediate next action

Retrieve a complete, timestamped Base44 export and schema/configuration record through an authenticated, read-only recovery session. Separately identify whether `PROGRAMS/login-Nex.txt` relates to Nexys Task through human review without exposing its contents; keep it outside all repositories.
