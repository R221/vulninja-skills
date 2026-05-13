# vul.ninja skills for Claude Code

Three slash commands that bring vul.ninja's cloud security into your
editor.

- `/vulninja-triage` — walk through current findings in priority order
- `/vulninja-fix-top` — fix the highest-priority finding and open a PR
- `/vulninja-pr-review` — security-review pending IaC changes before merge

## Requirements

- [Claude Code](https://claude.com/claude-code) 2.1+
- A vul.ninja account — sign up at https://vul.ninja
- The vul.ninja MCP server connected to your Claude Code session — see
  setup at https://vul.ninja/mcp/docs

## Install

Clone the repo somewhere stable, then symlink each skill into
`~/.claude/skills/`. Claude Code scans the top level of that
directory — each skill needs to live there directly, but symlinks
work fine and let you keep one git checkout to update from.

```bash
mkdir -p ~/code && git clone https://github.com/R221/vulninja-skills ~/code/vulninja-skills
mkdir -p ~/.claude/skills
ln -s ~/code/vulninja-skills/vulninja-triage ~/.claude/skills/
ln -s ~/code/vulninja-skills/vulninja-fix-top ~/.claude/skills/
ln -s ~/code/vulninja-skills/vulninja-pr-review ~/.claude/skills/
```

Restart Claude Code or start a new session. Type `/` and the three
vulninja skills appear in the picker.

## Update

```bash
cd ~/code/vulninja-skills && git pull
```

The symlinks pick up the new contents — no re-link needed.

## Uninstall

```bash
rm ~/.claude/skills/vulninja-triage ~/.claude/skills/vulninja-fix-top ~/.claude/skills/vulninja-pr-review
rm -rf ~/code/vulninja-skills
```

## License

MIT — see LICENSE.
