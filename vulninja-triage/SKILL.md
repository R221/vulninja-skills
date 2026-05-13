---
name: vulninja-triage
description: Walk through current cloud security findings from vul.ninja in severity order, starting with the highest-priority. Use when the user asks about vul.ninja findings, security triage, what is broken in their cloud, current open criticals, or recent security issues.
---

Help the user triage cloud security findings from their connected
vul.ninja dashboard.

1. Call the `list_dashboard_findings` tool (from the vulninja MCP server)
   with severity_min="high" and limit=10.

2. Handle errors:
   - `error: "dashboard_unavailable"` → tell the user the vul.ninja MCP
     server isn't fully configured and link to https://vul.ninja/mcp/docs.
     Stop.
   - `error: "unauthorized"` → tell the user their vk_live_* API key may
     need rotation; link to https://dashboard.vul.ninja/mcp-setup. Stop.
   - Other errors → surface verbatim.

3. If the response is empty: tell the user their cloud is currently
   clean at high+ severity. Suggest re-running with severity_min="medium"
   for a fuller view.

4. Deduplicate findings before presenting. The `list_dashboard_findings`
   response sometimes contains duplicate entries with different
   finding_ids. If multiple findings share the same
   `(short_description, rule)` pair, keep only the most recent one
   (highest `created_at` value) and discard the rest. This is a known
   temporary dashboard pipeline issue; the dedup is defensive. Note:
   `(resource, rule)` is not a reliable key today — `resource` often
   carries the scan label, not the actual cloud resource identity,
   which lives in `short_description`.

5. Group findings by `resource_type_display` before presenting:
   - For each resource_type group, sort the group internally by
     severity (critical > high > medium > low).
   - Sort groups by total severity weight descending — the group
     containing a critical wins over a group of highs, etc.
   - Show the display name as a header with the count, e.g.
     `Key Vault (1)` or `Container Registry (2)`. Under each header,
     list findings as one line each: severity, resource (or
     `short_description` if `resource` is just the scan label), then
     the `short_description`.
   - If `resource_type_display` is missing on tool responses (older
     MCP server) all findings collapse into a single "Other" group —
     just list them flat in severity order in that case.
   - If everything lands in one group anyway, skip the headers and
     present a flat severity-sorted list.
   - The "Other" group always lands at the bottom of the grouped
     list, prefixed with a soft note like: "These findings don't have
     a classifiable resource type — usually means the upstream cloud
     API didn't include one."

6. Ask which finding the user wants to address first. When they pick:
   - Call `get_dashboard_finding(scan_id, finding_id)` for full context.
   - If they want a fix, call
     `get_remediation_for_dashboard_finding(scan_id, finding_id)` and
     propose changes against their local repo if relevant files exist.

7. Never apply fixes without explicit user approval. Always show the
   diff first.
