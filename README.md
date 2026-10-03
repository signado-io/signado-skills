# Signado skills

Signado is a lead generation tool for agencies, consultants and B2B teams that sell go-to-market, digital marketing or AI services. It watches the conversations in your market on LinkedIn and tells you who to contact and what to say.

These [Agent Skills](https://agentskills.io) teach an AI agent to run common Signado workflows through the Signado MCP server.

| Skill | What it does |
|---|---|
| [`signado-warm-lead-brief`](skills/signado-warm-lead-brief/SKILL.md) | Brief of the best new warm leads with what each person said, plus first-message drafts for the ones you approve |
| [`signado-post-engagement-catcher`](skills/signado-post-engagement-catcher/SKILL.md) | One-time scan of a LinkedIn post's commenters, reactors or both, turned into warm leads |
| [`signado-competitor-audience-watch`](skills/signado-competitor-audience-watch/SKILL.md) | Scheduled watch on the people engaging with competitors or creators, with source clean-up |

## Requirements

1. A Signado account. Every plan starts with a 7-day free trial.
2. The Signado MCP server connected to your agent with OAuth sign-in. No API key is needed.

Claude Code:

```bash
claude mcp add --transport http signado https://mcp.signado.io/mcp
```

Codex:

```bash
codex mcp add signado --url https://mcp.signado.io/mcp
codex mcp login signado
```

Other clients, including Cursor, ChatGPT and Grok Bot, are covered in the [setup guide](https://signado.io/help/mcp/mcp-overview).

## Install the skills

Copy any folder from `skills/` into your agent's skills directory, for example `~/.claude/skills/` for Claude Code. Each skill asks for confirmation before any step that spends Signado credits.

## Links

- Signado MCP: https://signado.io/mcp?utm_source=github&utm_medium=repo&utm_campaign=signado-skills
- Setup guide: https://signado.io/help/mcp/mcp-overview
- Cursor plugin: https://github.com/signado-io/signado-cursor-plugin

## License

The skills in this repository are MIT licensed. The Signado service itself is proprietary.
