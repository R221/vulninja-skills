---
name: vulninja-pr-review
description: Review pending Infrastructure-as-Code changes in the current git branch through vul.ninja's security scanners before merging or pushing. Use when the user is about to push, merge, or open a PR with infrastructure changes (terraform, cloudformation, ARM, Bicep), or wants to validate IaC changes for security issues before they ship.
---

Security-review pending IaC changes before the user ships them.

1. Determine the base branch:
   - Try "main" first.
   - If not present, try "master".
   - If neither, use `git symbolic-ref refs/remotes/origin/HEAD`.

2. Run `git diff --name-only <base>..HEAD` to list changed files.

3. Filter to IaC files only:
   - .tf, .tf.json, .hcl → terraform
   - .yaml/.yml/.json with CloudFormation shape → cloudformation
   - .bicep, .arm.json or .json with ARM shape → ARM/Bicep
   - Skip everything else.

4. If no IaC files changed: tell the user there's nothing to review and
   they're clear to push from a cloud-security perspective. Stop.

5. For each IaC file, read the post-change contents and call:
   - `assess_iac_change(iac_code, iac_format)` for terraform/cloudformation
   - `assess_arm_template(template, template_format)` for ARM/Bicep

6. Aggregate findings across all files. Group by severity. For each:
   file:line, rule, severity, short_description,
   remediation_available.

7. Summarize:
   - No critical/high → diff is clear to ship.
   - Findings at critical/high → list, recommend addressing pre-merge.
     Offer `get_remediation(finding_id)` for any the user wants to fix
     inline.

8. Don't block the push. Produce the review; the merge decision is the
   user's.
