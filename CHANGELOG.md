# Changelog

## v0.1.2 — 2026-05-12

### Fixed
- Dedup key changed from `(resource, rule)` to `(short_description, rule)`.
  In current dashboard payloads `resource` carries the scan label, not
  the cloud resource identity, so the v0.1.1 key over-collapsed distinct
  findings into one. `short_description` carries the actual identity.

## v0.1.1 — 2026-05-12

### Fixed
- vulninja-triage and vulninja-fix-top now deduplicate dashboard
  findings by `(resource, rule)` before presenting. Defensive
  mitigation for a known dashboard pipeline issue where duplicate
  findings can appear.
