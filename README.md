# claude-seo-skills

Super SEO is a Claude skill for SEO with the data built in: your Search Console, keywords, SERPs and backlinks, through one MCP server that works in Claude, ChatGPT and Cursor.

A skill on its own is instructions. This one runs on live data: it reads your Google Search Console, then ranks every move your site could make this week on one scale, each with the Search Console number behind it, and works the list top-down. It drafts pages, refreshes, on-page fixes and link pitches. Publishing, sending and spending stay with you.

## Install

Two parts: superagnt's hosted MCP server, which carries the data, and the skills.

**1. Connect the server.** **Server URL:** `https://mcp.superagnt.com/mcp` (Streamable HTTP, OAuth). Your client opens superagnt's consent screen in your browser, and approving it creates your superagnt account if you don't have one. There's no API key to copy. The setup guide for every client: https://www.superagnt.com/install?skill=super-seo

- **Claude** (claude.ai, the desktop app and Cowork): Customize, then Connectors, then Add custom connector. Name it superagnt, paste the server URL, add it and approve. One connector covers Claude on the web, the desktop app, mobile, Cowork and Claude Code signed in with your Claude account.
- **ChatGPT:** In ChatGPT on the web, go to Plugins, select the plus button, then Add custom MCP server. Name it superagnt, enter the server URL under Connection, and set Authentication to OAuth. Tick "I understand and want to continue", then select Create as a plugin. Install the plugin, then start a new chat, type @ and select superagnt. ChatGPT takes custom MCP servers on the web only, and OpenAI lists them for Business, Enterprise/Edu and Pro plans. Business and Enterprise/Edu workspaces get every tool, write actions included. On Pro, connected servers are read and fetch only, so saving each run's numbers needs one of those plans in ChatGPT.
- **Cursor:** put [`config/cursor.mcp.json`](config/cursor.mcp.json) in your project as `.cursor/mcp.json`, then click Login for superagnt under Settings, then MCP.
- **Claude Code:** `claude mcp add --scope user --transport http superagnt https://mcp.superagnt.com/mcp`, then `claude mcp login superagnt`.
- **Codex:** `codex mcp add superagnt --url https://mcp.superagnt.com/mcp`, then `codex mcp login superagnt`.
- **VS Code, Gemini CLI, Windsurf, Zed:** each client's file is in [`config/`](config/).

**2. Install the skills.**

- Any agent that reads SKILL.md files (Claude Code, Codex, Cursor and others): `npx skills add superagnt/claude-seo-skills`. It installs Super SEO and `task-agents`, the base skill its optional weekly run follows.
- Claude Code, as a plugin: `/plugin marketplace add superagnt/plugins`, then `/plugin install super-seo@superagnt`, which brings superagnt's base skills with it.

**3. Connect Search Console** once, from the connections page at https://app.superagnt.com. Until you do, the skill runs on third-party estimates and labels every figure that comes from them.

The skills are plain markdown: [skills/super-seo/SKILL.md](skills/super-seo/SKILL.md), with its research library in [skills/super-seo/references/](skills/super-seo/references/00-agent-seo-playbook.md), and [skills/task-agents/SKILL.md](skills/task-agents/SKILL.md). Read them before you install.

## What it does

Say "run my SEO" for the full cycle, or name one lane:

| Mode | You say | What runs |
|---|---|---|
| (bare) | "run my SEO", "what should I do this week" | The Search Console panel, then every candidate move scored on one scale, top items first |
| `measure` | "what does search console say", "why did my clicks drop" | The panel and what moved, saved as a snapshot |
| `audit` | "audit my site", "check my indexing" | A technical and on-page pass, no drafting |
| `write` | "write pages for my gaps" | Drafts for keyword gaps, five at most |
| `fix` | "what do I fix on this page" | A concrete fix list for the pages you name |
| `links` | "find link targets", "who links to my competitors" | Link targets and pitches, three at most |
| `ai` | "why does ChatGPT not mention us" | Your brand's visibility in AI answers |
| `report` | "where are we at", "write the weekly" | The weekly review from saved runs, with no new spend |

Every run keeps its numbers in your superagnt workspace's database, so the next one says what changed. A weekly hosted run is optional.

## What each tool does

| Tool | Actions | What it does |
|---|---|---|
| `connection_google_search_console_analytics_read` | `query` | Your Search Console clicks, impressions, CTR and position, by query, page, country or device. Listed once Search Console is connected. |
| `connection_google_search_console_sites_read` | `list`, `get` | The Search Console properties you can read. |
| `connection_google_search_console_sitemaps_read` | `list`, `get` | Your submitted sitemaps and their status. |
| `connection_google_search_console_url_inspection_read` | `inspect` | Google's index status for one URL. |
| `data_seo_serp` | `google`, `ai_mode`, `intent` | Google's results for a query, Google AI Mode's answer, and the search intent behind a keyword. |
| `data_seo_keywords` | `ideas`, `overview`, `ranked`, `gap` | Keyword ideas, volume and cost per click, the keywords a domain ranks for, and the gap between you and a competitor. |
| `data_seo_domains` | `overview`, `competitors`, `ai_visibility`, `page_audit`, `tech_stack`, `tech_search` | A domain's traffic picture, its organic competitors, its visibility in AI answers, an on-page audit of a URL, and what a site is built with. |
| `data_seo_backlinks` | `summary`, `referring_domains`, `gap` | A domain's backlink profile, the sites linking to it, and the sites that link to your competitors but not to you. |
| `data_web_scrape` | | Reads a page's text, for voice notes and on-page checks. |
| `agnt_db_sql` | `execute_sql`, `apply_migration` | The tables in your workspace's database that keep each run's numbers. |
| `agnt_tools_list_enabled`, `agnt_tools_search`, `agnt_tools_write` | `enable` | Checks which tools your workspace serves and turns on any the skill needs. |
| `agnt_agents_write`, `agnt_schedules_write` | `create`, `deploy` | The optional weekly hosted run: a small agent superagnt runs on a schedule. |
| `agnt_agents_run`, `agnt_agents_read`, `agnt_schedules_list`, `agnt_observability_read` | | How `task-agents` tests that run once and checks that later runs fired. |

The data tools only read. Each call uses credits from your superagnt workspace: https://www.superagnt.com/pricing

## Prompts to try

- "Run my SEO for <your domain>. My competitors are <a> and <b>."
- "Why did my clicks drop last week? Check Search Console."
- "Audit my homepage and pricing page, and give me the fix list."
- "Write the pages for the keywords my competitors rank for and I don't."
- "Find sites that link to my competitors but not to me, and draft three pitches."
- "Does ChatGPT mention us for <category>? Check our AI visibility."

What it does step by step, and every file it installs: https://www.superagnt.com/skills/super-seo

## About superagnt

superagnt is a marketing plugin for Claude and ChatGPT that helps businesses find customers through outbound, SEO and ads. Super SEO is one of its skills; more of them, for leads, competitor research and social listening, are at https://www.superagnt.com/skills

## License

This repo's config and skills are MIT licensed. See [LICENSE](LICENSE). The server is superagnt's hosted service.
