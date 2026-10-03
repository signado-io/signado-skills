---
name: signado-warm-lead-brief
description: Builds a brief of the best new warm leads from Signado, people posting or commenting about the user's market on LinkedIn, shows exactly what each person said, and drafts first messages for the leads the user approves. Use when the user asks for warm leads, a daily or morning lead brief, who to contact today, or LinkedIn prospects from Signado.
license: MIT
compatibility: Requires the Signado MCP server (https://mcp.signado.io/mcp) connected with OAuth sign-in to a Signado workspace.
metadata:
  author: signado
  version: "1.0"
---

# Signado warm lead brief

Turn the latest Signado discovery runs into a short brief: who to contact, what they said, and a first line that references it.

## Steps

1. Call `get_best_of_run` to load the best leads from the most recent completed runs. Pass `date` (YYYY-MM-DD) when the user asks about a specific day.
2. If it returns nothing, call `list_warm_leads` with `discovered_after` set to 24 hours ago and `limit` 20.
3. For each lead, show:
   - name, role and company
   - the exact post or comment that surfaced them, quoted, and the source that found them
   - the next move Signado suggests (contact today, comment on the post, find the right person, or skip)
   - one suggested opening line that references what they said
4. When the user wants the full story on one person, call `get_warm_lead_detail` with their `contact_id`.
5. Ask which leads are worth contacting before doing anything paid.
6. For approved leads, call `draft_message` with their `contact_ids` (at most 5 per call) and a new `idempotency_key`. It uses the LinkedIn DM template by default; call `list_templates` first if the user wants another template.
7. Only if the user wants email outreach, call `find_email` with the approved `contact_ids` and a new `idempotency_key`.
8. Report the `credits_charged` and `balance_after` values from each paid response.

## Rules

- `draft_message` and `find_email` cost credits per contact. State the cost and wait for the user's confirmation before calling either.
- Quote what each person said exactly. Never invent context they did not write.
- Reuse an `idempotency_key` only to retry a call whose result was uncertain.
- Signado never logs into LinkedIn. Never ask for LinkedIn credentials or cookies.
