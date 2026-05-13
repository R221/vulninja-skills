# Changelog

## v0.1.3 — 2026-05-12

### Added
- vulninja-triage groups findings by resource type (e.g. "Key Vault",
  "SQL Database") for cleaner triage when findings span multiple
  resource types. The "Other" group lands at the bottom with a soft
  note that the upstream cloud API didn't surface a usable type.
- vulninja-fix-top includes resource type in the finding summary.

### Requires
- vul.ninja MCP server with `resource_type` / `resource_type_display`
  on dashboard findings tool responses. If your server is older,
  skills still work but won't show grouping (all findings collapse
  into one "Other" group).

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
