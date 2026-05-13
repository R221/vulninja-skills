# Changelog

## v0.1.1 — 2026-05-12

### Fixed
- vulninja-triage and vulninja-fix-top now deduplicate dashboard
  findings by `(resource, rule)` before presenting. Defensive
  mitigation for a known dashboard pipeline issue where duplicate
  findings can appear.
