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

This server hands all of that to your agent: 31 tools over streamable HTTP,
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
| R | `visibility_summary` | Mentions, citations and recommendations across engines |
| R | `visibility_prompts` | The tracked prompt panel, with per-prompt verdicts |
| R | `visibility_prompt` | One prompt with its per-engine run list |
| R | `visibility_answer` | A stored AI answer, verbatim |
| R | `visibility_sources` | The pages AI answers were built from |
| R | `visibility_gaps` | Where competitors are named and you aren't |
| W | `prompts_track` | Start tracking a buyer prompt |
| ! | `prompts_retire` | Stop tracking one, freeing a slot |

### Proof and research
| | Tool | |
|---|---|---|
| R | `metrics_gsc` | Real Search Console clicks, impressions, CTR, position |
| R | `keywords_list` | The keyword pool, ranked by Opportunity |
| R | `outreach_list` | The off-page coverage ledger |
| R | `workspace_get` | The connected website: engines, locale, publish mode |
| R | `connections_list` | Connected CMS destinations |
| R | `activity_list` | The attributed activity feed |
| R | `guidance_search` | Sightkick's SEO/AEO methodology |

### Articles
| | Tool | |
|---|---|---|
| R | `articles_list` / `articles_get` | Find an article, read its content |
| W | `articles_create_draft` | Write your own draft as semantic HTML |
| W | `articles_update` | Edit an article's content or metadata |
| W | `articles_generate` | Delegate to Sightkick's staged pipeline |
| R | `articles_score` | Grade a draft on five pillars, with fixes |
| W | `articles_queue` / `articles_unqueue` | Put it on the plan, or take it off |

### Shipping
| | Tool | |
|---|---|---|
| R | `calendar_get` | The publishing calendar |
| W | `calendar_reschedule` | Move an article, insert-and-slide |
| W | `autopilot_set_mode` | The writing dial: auto, drafts, manual |
| R | `actions_list` | The to-do list, each row owned by you or the autopilot |
| W | `actions_complete` | Mark one done, with a result note |
| ! | `actions_skip` | Dismiss one (remembered 60 days) |
| W | `actions_request` | Order an article from the pipeline |
| ! | `publish_article` | Publish to the connected CMS |

## Rules

- **One grant, one website.** An agent connected to one workspace cannot read
  or touch another.
- **Every write is attributed.** Each call lands in the workspace's activity
  feed against the OAuth client that made it.
- **Limits are enforced, not documented.** One generation in flight and five
  manual runs a day, 25 scores a day, 20 writes an hour per workspace.
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
