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
   response sometimes contains duplicate entries — same resource, same
   rule, different finding_ids. If multiple findings share the same
   `(resource, rule)` pair, keep only the most recent one (highest
   `created_at` value) and discard the rest. This is a known temporary
   dashboard pipeline issue; the dedup is defensive.

5. Present findings in priority order (critical first, then high). For
   each, one line: rule, severity, resource. Then the
   `short_description`.

6. Ask which finding the user wants to address first. When they pick:
   - Call `get_dashboard_finding(scan_id, finding_id)` for full context.
   - If they want a fix, call
     `get_remediation_for_dashboard_finding(scan_id, finding_id)` and
     propose changes against their local repo if relevant files exist.

7. Never apply fixes without explicit user approval. Always show the
   diff first.
