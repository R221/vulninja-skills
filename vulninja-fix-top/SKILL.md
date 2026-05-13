---
name: vulninja-fix-top
description: Pick the highest-priority cloud security finding from vul.ninja with available remediation, propose the fix in the user's repo, validate it, and open a PR. Use when the user asks to fix the top finding, fix the most urgent issue, close the loop on their highest-severity cloud risk, or address criticals from the latest scan.
---

Close the loop on the user's single highest-priority vul.ninja finding —
find, fix, validate, PR.

1. Call `list_dashboard_findings` with severity_min="critical", limit=20.
   If empty, retry with severity_min="high".

2. Handle errors as in vulninja-triage.

3. If empty at high+, tell the user the cloud is currently clean. Stop.

4. Deduplicate findings before picking the top. If multiple entries
   share the same `(resource, rule)` pair, keep only the most recent
   (highest `created_at`) and discard the rest. Then identify the top
   finding from the deduplicated set.

5. Identify the top finding with `remediation_available: true`. If none
   of the returned findings has remediation available, pick the top
   severity and tell the user you'll walk through it manually using its
   description and references.

6. Call `get_dashboard_finding(scan_id, finding_id)` for full context.

7. Call `get_remediation_for_dashboard_finding(scan_id, finding_id)`.

8. Present:
   - What's wrong (rule, evidence)
   - Why it matters (severity)
   - The proposed fix (diff if code)

9. Locate the relevant file in the user's repo. Search by resource name
   or file pattern.

10. Before writing:
    - Confirm with the user that the proposed change matches their setup.
    - Show the exact diff that would be applied.

11. Apply on a fresh git branch. Never commit to main.

12. Validate using `assess_iac_change` against the new file contents.
    Confirm the original finding's rule is no longer flagged.

13. If validation passes, propose opening a PR via `gh pr create`. The
    PR body should reference the original finding ID, include the diff,
    and note the validation result. Wait for user approval before
    running gh pr create.

14. Never bypass git hooks (--no-verify) or push --force. If anything
    fails, stop and surface the error.
