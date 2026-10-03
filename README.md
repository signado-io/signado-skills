# Signado skills

Signado is a lead generation tool for agencies, consultants and B2B teams that sell go-to-market, digital marketing or AI services. It watches the conversations in your market on LinkedIn and tells you who to contact and what to say.

These [Agent Skills](https://agentskills.io) teach an AI agent to run common Signado workflows through the Signado MCP server.

| Skill | What it does |
|---|---|
| [`signado-warm-lead-brief`](skills/signado-warm-lead-brief/SKILL.md) | Brief of the best new warm leads with what each person said, plus first-message drafts for the ones you approve |
| [`signado-post-engagement-catcher`](skills/signado-post-engagement-catcher/SKILL.md) | One-time scan of a LinkedIn post's commenters, reactors or both, turned into warm leads |
| [`signado-competitor-audience-watch`](skills/signado-competitor-audience-watch/SKILL.md) | Scheduled watch on the people engaging with competitors or creators, with source clean-up |

You need a Signado account. Every plan starts with a 7-day free trial.

## Install the Claude Code plugin

The plugin bundles the three skills and the Signado MCP server, so there is nothing else to connect.

```bash
claude plugin marketplace add signado-io/signado-skills
claude plugin install signado@signado
```

Then run `/mcp` in Claude Code, select the Signado server and sign in to your workspace. No API key is needed. Ask for your warm leads, or run a skill directly, for example `/signado:signado-warm-lead-brief`.

## Use the skills in other agents

Connect the Signado MCP server to your agent with OAuth sign-in. No API key is needed.

Codex:

```bash
codex mcp add signado --url https://mcp.signado.io/mcp
codex mcp login signado
```

Other clients, including Cursor, ChatGPT and Grok Bot, are covered in the [setup guide](https://signado.io/help/mcp/mcp-overview).

Then copy any folder from `skills/` into your agent's skills directory.

## What the plugin runs and sends

The plugin contains Markdown skills and one remote MCP server entry, `https://mcp.signado.io/mcp`. It runs no local code, hooks or scripts. Tool calls go to Signado under the workspace you sign in to. Some tools spend Signado credits, such as finding emails, drafting messages and running a source, and each skill asks for confirmation before those steps. Privacy policy: https://signado.io/legal/privacy

## Links

- Signado MCP: https://signado.io/mcp?utm_source=github&utm_medium=repo&utm_campaign=signado-skills
- Setup guide: https://signado.io/help/mcp/mcp-overview
- Cursor plugin: https://github.com/signado-io/signado-cursor-plugin

## License

The skills in this repository are MIT licensed. The Signado service itself is proprietary.
