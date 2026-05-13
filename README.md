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

```bash
git clone https://github.com/R221/vulninja-skills ~/.claude/skills/vulninja
```

That's it. Restart Claude Code or start a new session. Type `/` and the
three vulninja skills appear in the picker.

## Update

```bash
cd ~/.claude/skills/vulninja && git pull
```

## Uninstall

```bash
rm -rf ~/.claude/skills/vulninja
```

## License

MIT — see LICENSE.
