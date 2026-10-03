---
name: signado-competitor-audience-watch
description: Sets up Signado to watch the people engaging with competitors or creators on LinkedIn on a daily or custom schedule, then reports the new warm leads and which sources are worth keeping. Use when the user wants leads from a competitor's or creator's audience, wants to monitor LinkedIn profiles or Company pages, or asks which sources bring useful leads.
license: MIT
compatibility: Requires the Signado MCP server (https://mcp.signado.io/mcp) connected with OAuth sign-in to a Signado workspace.
metadata:
  author: signado
  version: "1.0"
---

# Signado competitor audience watch

Follow the audience of competitors and creators your buyers pay attention to, on a schedule, and keep only the sources that bring useful leads.

## Steps

1. Ask the user for up to three competitors or creators, as LinkedIn profile URLs (`linkedin.com/in/...`) or Company pages (`linkedin.com/company/...`).
2. Ask when to check them:
   - daily: `{"mode": "daily"}`
   - selected weekdays at one local time: `{"mode": "custom", "weekdays": [1, 2, 3, 4, 5], "local_time": "08:00", "timezone": "Europe/London"}` (ISO weekdays, 1 is Monday, and an IANA timezone)
3. For each one, call `add_source` with `source_type` `competitor_creator` for a profile or `company` for a Company page, the `profile_url`, the `schedule`, and `confirm` false. Show the projected credit cost it returns.
4. Only after the user confirms, repeat the call with `confirm` true.
5. To change timing later, call `set_source_schedule` with the `source_id` or exact `source_name`, the new `schedule` and `confirm` false, then true once the user agrees. Changes apply to future runs only.
6. After runs complete, call `get_best_of_run` with each `source_id` and show the new leads with what they commented on and why they look like the user's buyers.
7. Point out sources that keep finding nobody useful. If the user agrees, call `remove_source` to archive them. Their history stays.

## Rules

- Monitoring spends credits on every new warm lead found. Confirm the projected cost before activating any source.
- Creator and Company page sources surface commenters, not reactions.
- Signado never logs into LinkedIn. Never ask for LinkedIn credentials or cookies.
