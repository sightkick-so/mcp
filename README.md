# Sightkick MCP server

> **This repo is metadata and documentation, not the server's source.** Sightkick
> is a hosted product; the server runs at `https://app.sightkick.so/mcp` and there
> is nothing here to install or self-host. What you'll find is [`server.json`](server.json)
> — the [MCP Registry](https://registry.modelcontextprotocol.io) entry — plus the
> connection instructions and tool catalog below.

Sightkick is an SEO/AEO autopilot. It tracks how ChatGPT, Gemini, Google AI
Overviews and AI Mode answer your buyers' questions, researches the keywords
worth winning, writes and publishes articles on a daily calendar, and proves
outcomes with Google Search Console data.

This server hands all of that to your agent: 42 tools over streamable HTTP,
authenticated with OAuth 2.1. Agent access is included in every paid plan —
it is never a separate tier.

## Endpoint

```
https://app.sightkick.so/mcp
```

There is no API key. The first call opens a sign-in, and one OAuth grant is
bound to one workspace (one website) — the consent screen carries a workspace
selector.

## Connect

**Claude Code**

```sh
claude mcp add --transport http sightkick https://app.sightkick.so/mcp
```

**Claude** (desktop and claude.ai) — Settings → Connectors → "Add custom
connector", paste the endpoint.

**ChatGPT** — Settings → Connectors, turn on Developer mode, "Add", paste the
endpoint.

**Cursor** — Settings → MCP → "Add server", pick streamable HTTP, paste the
endpoint.

**VS Code** — run "MCP: Add Server" from the command palette, pick HTTP, paste
the endpoint.

**Anything else that speaks MCP over streamable HTTP** — add a custom or remote
server and paste the same endpoint.

There is also a [skill](https://github.com/sightkick-so/skill) that teaches an
agent how to use these tools well, rather than just what they are:

```sh
npx skills add sightkick-so/skill
```

## Tools

`R` read-only · `W` write · `!` destructive, requires explicit confirmation.

### AI visibility
| | Tool | |
|---|---|---|
| R | `get_visibility_summary` | Mentions, citations and recommendations across engines |
| R | `list_tracked_prompts` | The tracked prompt panel, with per-prompt verdicts |
| R | `get_tracked_prompt` | One prompt with its per-engine run list |
| R | `get_ai_answer` | A stored AI answer, verbatim |
| R | `list_cited_sources` | The pages AI answers were built from |
| R | `list_visibility_gaps` | Where competitors are named and you aren't |
| W | `track_prompt` | Start tracking a buyer prompt |
| ! | `retire_prompt` | Stop tracking one, freeing a slot |

### Proof and research
| | Tool | |
|---|---|---|
| R | `get_search_metrics` | Real Search Console clicks, impressions, CTR, position |
| R | `list_keywords` | The keyword pool, ranked by Opportunity |
| R | `research_keywords` | Keyword ideas and long-tail around a topic — on the monthly research allowance |
| R | `competitor_keywords` | What any domain ranks for, with positions |
| R | `gap_keywords` | What a rival ranks for that this site doesn't |
| R | `get_serp` | One keyword's live top-10, People Also Ask, and the AI Overview's cited pages |
| W | `save_keywords` | Add keywords to the pool — graded and scored right away |
| R | `list_outreach` | The off-page prospect ledger |
| R | `list_backlinks` | Every watched link and its verdict: live, dropped, checking, not found |
| R | `get_workspace` | The connected website: engines, locale, publish mode |
| R | `list_connections` | Connected CMS destinations |
| R | `list_activity` | The attributed activity feed |
| R | `search_guidance` | Sightkick's SEO/AEO methodology |

### Articles
| | Tool | |
|---|---|---|
| R | `list_articles` / `get_article` | Find an article, read its content |
| W | `create_article_draft` | Write your own draft as semantic HTML |
| W | `update_article` | Edit an article's content or metadata |
| W | `generate_article` | Delegate to Sightkick's staged pipeline |
| R | `score_article` | Grade a draft on four pillars, with fixes |
| W | `queue_article` / `unqueue_article` | Put it on the plan, or take it off |

### Backlinks
| | Tool | |
|---|---|---|
| W | `approve_prospect` | Find the editorial contact and draft the pitch — nothing sent |
| ! | `send_prospect` | Book the pitch into the send queue (a real email from the user's inbox) |
| ! | `dismiss_prospect` | Decline a prospect for good — its whole site goes on the Blocklist |
| W | `run_prospect_discovery` | Run the daily prospect discovery now |
| W | `add_prospect` | Add a page by URL as a prospect, verified and scored like a found one |

### Shipping
| | Tool | |
|---|---|---|
| R | `get_calendar` | The publishing calendar |
| W | `reschedule_article` | Move an article, insert-and-slide |
| W | `set_autopilot_mode` | The writing dial: autopilot, drafts, manual — moving it up needs `confirm: true` |
| R | `list_actions` | The owner's to-do list: what only a person with the keys can do |
| W | `complete_action` | Mark one done, with a result note |
| ! | `skip_action` | Dismiss one (remembered 60 days) |
| W | `request_action` | Order an article from the pipeline |
| ! | `publish_article` | Publish to the connected CMS |

## Rules

- **One grant, one website.** An agent connected to one workspace cannot read
  or touch another.
- **Every write is attributed.** Each call lands in the workspace's activity
  feed against the OAuth client that made it.
- **Limits are enforced, not documented.** One generation in flight and the
  plan's monthly article budget, 25 scores a day, 20 writes an hour per
  workspace.
- **Publishing needs `confirm: true`**, unless the workspace already runs on
  full auto. It is meant to represent real user approval, never an automatic
  retry.

## Links

- [Server page](https://sightkick.so/mcp) — the human version of this document
- [Agent guide](https://sightkick.so/llm-info) — the same catalog, written for LLM retrieval
- [Server card](https://app.sightkick.so/.well-known/mcp/server-card.json)
- [Skill](https://github.com/sightkick-so/skill) — how to use the tools well
- [sightkick.so](https://sightkick.so) · [Privacy](https://sightkick.so/privacy)

MIT © Sprike LLC (Sightkick)
