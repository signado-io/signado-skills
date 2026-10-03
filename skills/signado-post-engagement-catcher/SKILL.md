---
name: signado-post-engagement-catcher
description: Turns one LinkedIn post into a list of people worth contacting by running a one-time Signado scan of its commenters, reactors or both, then shows who they are and what the commenters wrote. Use when the user shares a LinkedIn post URL and wants the people engaging with it, or asks who commented on or reacted to a post.
license: MIT
compatibility: Requires the Signado MCP server (https://mcp.signado.io/mcp) connected with OAuth sign-in to a Signado workspace.
metadata:
  author: signado
  version: "1.0"
---

# Signado post engagement catcher

Scan the people engaging with one LinkedIn post, once, and turn them into warm leads with context.

## Steps

1. Get the LinkedIn post URL from the user and ask whether they want its commenters, its reactors, or both (default both).
2. Call `add_source` with `source_type` `post`, the `post_url`, `post_engagement_scope` (`commenters`, `reactors` or `both`) and `confirm` false. Show the user the plan it returns.
3. When the user agrees, call `add_source` again with the same values and `confirm` true. Post sources are saved paused and run once only. They never take a schedule.
4. Optionally call `preview_source` with the new `source_id`, `source_type` `post` and a new `idempotency_key` to check the post has public engagement. A preview has a small fixed cost, so ask first.
5. Agree a maximum credit amount with the user, then call `run_source_once` with the `source_id`, that amount as `max_credits`, and `confirm` true. Scanning both commenters and reactors needs room for at least two leads.
6. The run finishes in the background. When the user checks back, call `list_warm_leads` with `discovered_after` set to the time the run started.
7. For each person, show their name, role, company and, for commenters, exactly what they wrote. Reactors have no comment text.
8. Ask which people deserve a message, then follow the same paid-step rules as the warm lead brief: confirm the cost, then `draft_message` or `find_email` with their `contact_ids`.

## Rules

- Never run a paid step without the user's confirmation of the cost or credit ceiling.
- If the run needs to stop, call `cancel_source_run`. Leads already found stay and unspent credits are returned.
- Signado never logs into LinkedIn. Never ask for LinkedIn credentials or cookies.
