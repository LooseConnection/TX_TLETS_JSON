# ConnectCIC — MANDATORY PROCESS RULES

> **STOP. Read this entire section before doing ANY work in this repo.**
>
> These rules are MANDATORY for every Claude instance working on ConnectCIC provider JSON repos.
> The user (Rob) has established these through repeated corrections. They are not optional.
> They are not suggestions. They cannot be deferred, batched, or skipped.
>
> **If you are tempted to skip a step to save time — STOP. The step exists because
> skipping it caused real failures. Do it now or do not report progress.**

**Source of truth:** `C:\Users\RobSgambellone\.local\bin\ConnectCIC-KB\PROCESS_RULES.md`
**Last synced:** 2026-04-30

## 1. SESSION START — Before Any Work

1. Read this CLAUDE.md completely
2. Read the KB master rules: `C:\Users\RobSgambellone\.local\bin\ConnectCIC-KB\CLAUDE.md`
3. Run `git status` — confirm working tree is CLEAN and branch is synced with remote
4. Verify `docs/` contains: STATUS.txt, SQVR.txt, BUILD_NOTES.txt, `base/` with 5 report files
5. Verify `tests/` directory exists
6. **If ANYTHING from steps 3-5 is wrong: fix it FIRST before starting the requested task**

## 2. TRIGGER RULES — Automatic Chaining

When a trigger fires, ALL chained actions are part of the same unit of work.

**You edit or create any `.json` →** Run `build_report.ps1` → Verify 0 FAIL → Commit JSON + reports → `git push` → Update STATUS.txt / SQVR.txt if changed

**You complete a live test →** Fill test log → `git add/commit/push` → Update STATUS.txt test matrix → Update SQVR.txt ([PENDING] → [CONFIRMED])

**You update any KB file →** Push KB → Check cross-repo impact → Update affected repos

**You discover a new limitation/anti-pattern/import error →** Add to KB → Fire KB trigger

## 3. MANDATORY GATES — Blocking Requirements

**GATE 1 (post-build):** `build_report.ps1` + 0 FAIL + commit reports + push. BLOCKED until done.
**GATE 2 (pre-test):** `new_test_log.ps1` creates stub in `tests/`. BLOCKED until stub exists on disk.
**GATE 3 (post-test):** Fill log + commit + push. BLOCKED from next test until done.
**GATE 4 (post-session):** Update STATUS.txt + commit + push.
**GATE 5 (pre-DONE):** Verify: `ls tests/` (count matches), `docs/base/` (5 reports), STATUS.txt current, SQVR.txt current, `git status` clean. BLOCKED until all pass.
**GATE 6 (post-KB-update):** Push KB + check cross-repo impact + update affected repos.

## 4. END-OF-RESPONSE VERIFICATION

Before ending ANY response with file changes: (1) all committed+pushed? (2) reports generated? (3) logs saved? (4) STATUS current? (5) SQVR current? (6) KB pushed if updated? (7) anything deferred? Fix now or state why.

## 5. TOOLS

```powershell
# Build report (5 reports) — GATE 1
powershell -ExecutionPolicy Bypass -File C:\Users\RobSgambellone\.local\bin\build_report.ps1 -Path <json>
# Test log stub — GATE 2
powershell -ExecutionPolicy Bypass -File C:\Users\RobSgambellone\.local\bin\new_test_log.ps1 -Provider <NAME> -Variant BASE -Version <ver> -Entity <entity> -Combo <combo> -Description "<desc>"
# Validator only
powershell -ExecutionPolicy Bypass -File C:\Users\RobSgambellone\.local\bin\connectcic-validator\validate.ps1 -Path <json>
```

## 6. KB REFERENCE — Read Before Every Session

- `C:\Users\RobSgambellone\.local\bin\ConnectCIC-KB\CLAUDE.md` — Master build rules
- `C:\Users\RobSgambellone\.local\bin\ConnectCIC-KB\knowledge-base\README.txt` — KB index (13 docs)
- `C:\Users\RobSgambellone\.local\bin\ConnectCIC-KB\knowledge-base\BUILD_CHECKLIST.txt` — Full checklists

## 7. CANONICAL REPO STRUCTURE

```
<PROVIDER>/
├── CLAUDE.md, .gitignore, <PROVIDER>_BASE.json, <PROVIDER>_MC.json
├── docs/ (STATUS.txt, BUILD_NOTES.txt, SQVR.txt, JSON_INVENTORY.md, base/, mc/)
├── tests/ (one log file per live test)
├── phases/, release/, scripts/, source/
```

If this repo does not match, fix it before doing any other work.

---
<!-- END PROCESS RULES — Provider-specific content below -->

# TX_TLETS Provider JSON

Owner: rob.sgambellone@mark43.com

## ATTENTION

This build has **open issues** that must be resolved before import. See below.

## Status

| Variant | Version | Validator | Live Test | Date |
|---------|---------|-----------|-----------|------|
| BASE | v1.0 | 60 PASS / 0 FAIL / 1 WARN | NOT TESTED | 2026-04-21 |

BASE is **REVIEWED** but has known issues. Build script is a stub (needs completion).

## Recent Changes (2026-04-28)

- New HIDLE.json ingested — adds null `conditions`/`defaults` to RMS combo requirements (no-op)
- JSON not yet rebuilt with new HIDLE (build script needs completion first)

## Open Issues

1. **Build script is a stub** — `scripts/build_tx_tlets.ps1` has placeholder text and hardcoded path (`C:\Users\Gordon Hallof\TX_TLETS`). Needs to be completed following the AZ_AZDPS or NJ_NJCJIS build script pattern. Update path to use `$PSScriptRoot`.

2. **WARN: MessageKey sourceField missing from Person QIF** — QIDM `TX_TLETS_DriverLicenseQuery` references `MessageKey` but no matching fieldId in Person QIF. Either add the field or remove from QIDM.

3. **TX-specific queries** — TX has non-standard query types (DPSI, REG, CPL, RSDWW) that need validation against TX metadata.

4. **No MC variant** — Only BASE exists. Phase 2 (multi-card) not started.

## Build & Validate

```powershell
# BASE (stub — needs completion)
powershell -ExecutionPolicy Bypass -File scripts/build_tx_tlets.ps1

# Full report
powershell -ExecutionPolicy Bypass -File C:\Users\RobSgambellone\.local\bin\build_report.ps1 -Path TX_TLETS_BASE.json

# Release bundle
powershell -ExecutionPolicy Bypass -File C:\Users\RobSgambellone\.local\bin\build_report.ps1 -Path TX_TLETS_BASE.json -Release
```

## GitHub

https://github.com/LooseConnection/TX_TLETS_JSON

## Knowledge Base

Read before every session:
- `C:\Users\RobSgambellone\.local\bin\ConnectCIC-KB\CLAUDE.md` — Master build rules
- `C:\Users\RobSgambellone\.local\bin\ConnectCIC-KB\knowledge-base\README.txt` — KB index (11 docs)

## Source Materials

- `source/TX_TLETS.xml` — XML metadata (primary build authority)
- `source/TX_TLETS.pdf` — CommSys devdoc
- `source/HIDLE.json` — RMS/auth base template
