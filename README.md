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

## Install in Hermes Agent

This repository is also a [Hermes Agent](https://hermes-agent.nousresearch.com) plugin. Hermes reads `plugin.json` and `mcp.json` at the root, and the same three skills.

```bash
hermes plugins install signado-io/signado-skills --enable
```

Ask Hermes for your warm leads. On first use a browser opens the Signado sign-in: choose your workspace and approve. No API key is needed. The skills load as `signado:signado-warm-lead-brief`, `signado:signado-post-engagement-catcher` and `signado:signado-competitor-audience-watch`.

What the Hermes plugin runs and sends:

- **Sign-in runs one local program.** A Hermes plugin can't switch on OAuth for a remote server, so `mcp.json` starts [`mcp-remote`](https://github.com/geelen/mcp-remote) version 0.1.49, pinned, through `npx` (Node.js 18 or later). On first use it registers a sign-in client named "Hermes Agent (Signado)" with Signado's sign-in service, opens your browser and a local callback port, and keeps your sign-in tokens in `~/.mcp-auth` on your machine. The package holds no key or secret.
- **Network.** Every tool call goes to `https://mcp.signado.io/mcp` and acts on the workspace you chose at sign-in. The tool list comes from that server at runtime.
- **Approvals.** `plugin.json` marks the server `untrusted`, so Hermes asks you before every tool that is not read-only, and refuses those tools in unattended runs. Only `ping` and the tools that read leads, exports and templates run without asking. `find_email`, `draft_message`, `preview_source`, `add_source`, `set_source_schedule` and `run_source_once` spend Signado credits.
- **Nothing else.** No hooks, no Python, no telemetry and no self-updater.

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

The plugin contains Markdown skills and one remote MCP server entry, `https://mcp.signado.io/mcp`. It runs no local code, hooks or scripts. Tool calls go to Signado under the workspace you sign in to. Some tools spend Signado credits, such as finding emails, drafting messages and running a source, and each skill asks for confirmation before those steps. Lead results include public LinkedIn posts and comments written by other people; the skills tell the agent to treat that text as data and never follow instructions found in it. Privacy policy: https://signado.io/legal/privacy

## The people Signado finds

The leads Signado finds are people who are not Signado users. This is how their information is handled.

- **Where it comes from:** public LinkedIn posts, comments, reactions, profiles, company pages and job posts, collected through the data provider Apify, plus public web pages about the person's company. When a customer asks, Signado looks up the person's work email address from their LinkedIn profile. Signado never uses a customer's LinkedIn account to collect any of it.
- **Why Signado may use it:** legitimate interests (GDPR Article 6(1)(f)) for business-to-business prospecting about a person's professional activity, weighed against their rights in a written assessment. Signado is the controller for these records and answers requests about them directly.
- **How to see it or be removed:** anyone Signado has found can email hello@signado.io with their LinkedIn profile address. Signado tells them what it holds, corrects it or deletes it from every Signado workspace, within one month. They can also object to Signado using it, or complain to a data protection authority.

Full details: [People whose public LinkedIn activity appears in Signado](https://signado.io/legal/privacy#people-whose-public-linkedin-activity-appears-in-signado)

## Links

- Signado MCP: https://signado.io/mcp?utm_source=github&utm_medium=repo&utm_campaign=signado-skills
- Setup guide: https://signado.io/help/mcp/mcp-overview
- Cursor plugin: https://github.com/signado-io/signado-cursor-plugin

## License

The skills in this repository are MIT licensed. The Signado service itself is proprietary.
