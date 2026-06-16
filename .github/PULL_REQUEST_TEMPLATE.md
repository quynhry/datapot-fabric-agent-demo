## What does this PR do?
<!-- New report? Model/measure change? Theme/docs fix? -->

## Report folder affected
`reports/NN-name/` (or `shared/`, `docs/`)

## Validation gate (CONVENTIONS.md §6) — all must pass
- [ ] `tmdl-validate` passes on every changed `.tmdl`
- [ ] `python validate_pbip.py reports/NN-name/pbip --no-pbir-cli` → 0 errors
- [ ] `jq empty` passes on every changed PBIR JSON
- [ ] TMDL is LF / UTF-8 / no BOM; page & visual folders have **no** `.Page`/`.Visual` suffix

## Data & secrets
- [ ] No real/personal data added (synthetic only)
- [ ] No hardcoded workspace IDs, item IDs, connection strings, or absolute paths

## Registry
- [ ] If a new report: added a row to `reports/README.md`
