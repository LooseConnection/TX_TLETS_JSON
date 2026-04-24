# TX_TLETS Provider JSON

Owner: rob.sgambellone@mark43.com

## ATTENTION

This build has **open issues** that must be resolved before import. See below.

## Status

| Variant | Version | Validator | Live Test | Date |
|---------|---------|-----------|-----------|------|
| BASE | v1.0 | 60 PASS / 0 FAIL / 1 WARN | NOT TESTED | 2026-04-21 |

BASE is **REVIEWED** but has known issues. Build script is a stub (needs completion).

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

## Knowledge Base

Read before every session:
- `C:\Users\RobSgambellone\.local\bin\ConnectCIC-KB\CLAUDE.md` — Master build rules
- `C:\Users\RobSgambellone\.local\bin\ConnectCIC-KB\knowledge-base\README.txt` — KB index (11 docs)

## Source Materials

- `source/TX_TLETS.xml` — XML metadata (primary build authority)
- `source/TX_TLETS.pdf` — CommSys devdoc
- `source/HIDLE.json` — RMS/auth base template
