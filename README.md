# Sightkick MCP server

> **This repo is metadata and documentation, not the server's source.** Sightkick
> is a hosted product: the server runs at `https://app.sightkick.so/mcp` and there
> is nothing here to install or self-host. What you'll find is [`server.json`](server.json),
> the [MCP Registry](https://registry.modelcontextprotocol.io) entry, plus the
> connection steps and the tool catalog below.

Sightkick is the agentic SEO and GEO platform your website runs on, and your
own AI agent uses it to rank on Google and in AI search. It tracks how AI
models answer your buyers' questions, researches the keywords worth winning,
publishes pages on a schedule, improves the live ones one measured change at a
time, runs outreach to the pages AI answers cite, and proves outcomes with
Google Search Console data.

This server hands all of it to your agent: 37 tools over streamable
HTTP, authenticated with OAuth 2.1. Agent access comes with both packages
(Standard and Pro), never as an add-on.

## Endpoint

```
https://app.sightkick.so/mcp
```

Connecting and listing the tools need no sign-in. The first tool call asks
for one (HTTP 401 with the OAuth challenge), and one OAuth grant is bound to
one workspace (one website): the consent screen carries a workspace selector.
An agent that runs on its own sends a workspace API key instead (below).

## Connect

**Claude Code**

```sh
claude mcp add --transport http sightkick https://app.sightkick.so/mcp
```

When the first tool call asks for the sign-in: `/mcp`, pick sightkick and
approve it.

**Codex**

```sh
codex mcp add sightkick --url https://app.sightkick.so/mcp
codex mcp login sightkick
```

**Claude** (desktop and claude.ai): Settings → Connectors → "Add custom
connector", paste the endpoint.

**ChatGPT**: Settings → Connectors, turn on Developer mode, "Add", paste the
endpoint.

**Anything else that speaks MCP over streamable HTTP**: add a remote server
and paste the same endpoint. Every client's steps, written for an agent to
follow: [sightkick.so/connect/agent.md](https://sightkick.so/connect/agent.md).

**No MCP client** (a cron job, a CI step, a cloud agent): the same tools run
on a workspace API key through the CLI, `npx -y sightkick`
([guide](https://sightkick.so/connect/cli.md)), and the HTTP API at
`https://api.sightkick.so/v1` ([guide](https://sightkick.so/connect/api.md),
[OpenAPI 3.1](https://api.sightkick.so/v1/openapi.json)).

The playbook ships inside the server: its instructions reach every client on
connect, telling a connected agent how to use these tools well, not just what
they are, and `get_guidance` returns Sightkick's SEO and GEO method on demand.
There is no separate skill to install.

## Tools

Each tool is one thing an agent works on, verb first. A read takes a `view`, a
change an `action` (`check_web` takes `check`, `research_keywords` `from`,
`research_backlinks` `lookup`), and each tool's description lists them with
their fields.

`R` read-only, runs unasked in most apps · `W` changes something or spends an
allowance, so the app asks first · `!` has an action that deletes something,
takes a page down or sends an email (destructiveHint).

### Start

| | Tool | What it does |
|---|---|---|
| R | `get_workspace` | Get the workspace |
| R | `get_guidance` | Get SEO and GEO guidance |
| R | `get_activity` | Get the activity. `view`: log, events, report, receipt |

### AI visibility and search

| | Tool | What it does |
|---|---|---|
| R | `get_visibility` | Get AI visibility. `view`: summary, gaps, gap_candidates, competitors, sources, mentions, rival_pages |
| R | `get_prompts` | Get tracked prompts. `view`: list, prompt, answer, suggestions, rechecks, models |
| W | `update_prompts` | Change tracked prompts. `action`: track, retire, restore, models |
| W | `update_competitors` | Change tracked competitors. `action`: track, untrack, set_domain |
| W | `ask_ai` | Ask AI models now. `action`: ask, batch, recheck, cancel_recheck |
| R | `get_search_console` | Get Search Console. `view`: metrics, query, detail, export, sitemaps, searches |
| W | `check_web` | Check the web now. `check`: rank, index, serp, page, crawl |

### Keywords

| | Tool | What it does |
|---|---|---|
| R | `get_keywords` | Get keywords. `view`: keywords, topics |
| W | `research_keywords` | Research keywords. `from`: seed, competitor, gap |
| W | `update_keywords` | Change keywords. `action`: save, move, set_main, remove, link |

### Pages

| | Tool | What it does |
|---|---|---|
| R | `get_pages` | Get pages. `view`: list, page, check_topic, revisions, receipt, shipped, calendar, schedule, images |
| W | `score_page` | Score a page |
| ! | `update_page` | Write and change pages. `action`: create, edit, delete, register, unregister, log_change |
| ! | `update_page_images` | Change a page's images. `action`: list, upload, alt, featured, position, remove |
| W | `schedule_page` | Schedule pages. `action`: get, place, unplace, set_slots |
| ! | `publish_page` | Publish pages. `action`: publish, republish, take_down, undo_edit |
| W | `generate_page` | Have Sightkick write a page |

### Improvements

| | Tool | What it does |
|---|---|---|
| R | `get_improvements` | Get improvements. `view`: suggested, other_pages, issues, health, todos, results, candidates |
| W | `update_improvements` | Act on improvements. `action`: approve, skip, unqueue, restore, undo, redo, decline_issue, restore_issue, complete_todo, skip_todo, scan, indexnow |

### Backlinks and outreach

| | Tool | What it does |
|---|---|---|
| R | `get_backlinks` | Get backlinks. `view`: ledger, profile, mentions, directories, kit |
| W | `research_backlinks` | Research backlinks. `lookup`: profile, gap, metrics |
| ! | `update_backlinks` | Change backlinks. `action`: add, remove, directory, prepare_directory, save_kit, kit_image, start_directories, rebuild_kit |
| R | `get_outreach` | Get outreach. `view`: prospects, prospect, threads, thread, follow_up, sequences, sequence, blocklist, unmatched |
| W | `update_outreach` | Work the outreach. `action`: add, discover, find_contact, set_contact, lead_with, prepare, write_pitch, draft_reply, answer_offer, handled, discard_reply, edit_message, edit_follow_up, pause_follow_ups, resume_follow_ups, won, lost, dismiss, unblock, link_mail, ignore_mail |
| ! | `send_outreach` | Send outreach email. `action`: pitch, reply, follow_up |

### Reports

| | Tool | What it does |
|---|---|---|
| R | `get_reports` | Get reports |
| ! | `update_reports` | Change reports. `action`: create, edit, share, rotate_link, remove |

### Settings

| | Tool | What it does |
|---|---|---|
| R | `get_settings` | Get settings. `view`: brand, writing, images, offerings, markets, sites, destinations, search_console, inbox, sequences, agents, jobs, usage, models, webhook, plan, notifications |
| ! | `update_brand` | Change the brand profile. `action`: profile, name, add_offering, edit_offering, remove_offering, offering_image, remove_offering_image, market, timezone |
| W | `update_writing` | Change the writing rules. `action`: author, remove_author, voice, key_facts, rules, images |
| ! | `update_connections` | Change connections. `action`: connect_wordpress, connect_shopify, connect_framer, connect_webhook, edit, test, remove, destination, add_destination, remove_destination, search_console_link, search_console_properties, pick_property, disconnect_search_console, invite_agent, webhook, test_webhook |
| ! | `update_outreach_setup` | Change the outreach setup. `action`: signature, daily_limit, warm_up, send_window, opt_out, postal_address, mailbox_link, disconnect_inbox, managed_domains, check_domain, create_managed_inbox, save_sequence, duplicate_sequence, activate_sequence, split_test, end_split_test, archive_sequence, unarchive_sequence, remove_sequence |
| W | `update_jobs` | Change Sightkick's job settings. `action`: job, floor, warmup |
| ! | `update_account` | Change the plan, notifications and brands. `action`: preview_plan, change_plan, undo_downgrade, pause_plan, resume_plan, cancel_plan, notifications, create_brand, delete_brand |

## Rules

- **The agent is a user.** Whatever the owner can do in the app, their agent
  can do, at any time: every reply, an offer's answer, the listing kit,
  directory submissions. Sightkick's job settings (Auto, Manual, Off) and its
  own rules (waits after a page goes live, the warm-up, the pitch floor, the
  duplicate check) only govern what Sightkick does by itself. No call takes a
  `confirm`: what an agent may do is its connection's permissions.
- **Pages go live on the schedule.** Every written page on it goes live on
  its day, exactly as written, whoever wrote it. A page without a day waits in
  Unscheduled until the owner or an agent gives it one. Each page says whether
  Sightkick writes it (`sightkickWrites`): an agent's own page is the agent's
  to write before its day, or it goes back to Unscheduled as missed.
- **One grant, one website.** An agent connected to one workspace cannot read
  or touch another.
- **Every write is attributed.** Each call lands in the workspace's activity
  feed against the client that made it.
- **Limits are enforced, not documented.** One generation in flight and the
  plan's monthly pages, 25 scores a day, and a runaway guard of 1,000
  writes an hour per workspace. A metered call's answer says what it used
  and what's left.
- **Live pages are safe to hand over.** An edit to a live page needs the hash
  of the copy it was made on, and every agent edit is kept and can be undone,
  at any age.

## Links

- [Server page](https://sightkick.so/mcp): the human version of this document
- [Agent reference](https://sightkick.so/llm-info): the same catalog, written for LLM retrieval
- [Connect your agent](https://sightkick.so/connect/agent.md) · [llms.txt](https://sightkick.so/llms.txt)
- Server cards: [SEP-2127](https://app.sightkick.so/mcp/server-card) ·
  [with the tools](https://app.sightkick.so/.well-known/mcp/server-card.json)
- [sightkick.so](https://sightkick.so) · [Privacy](https://sightkick.so/privacy)

MIT © Sprike LLC (Sightkick)
