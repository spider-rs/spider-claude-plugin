---
name: spider
description: >-
  Overview and tool-selection guide for Spider Cloud — crawl, scrape, search the
  web, extract structured data with AI, and drive remote browsers with anti-bot
  bypass. Use whenever "Spider" or "Spider Cloud" is mentioned, or when the task
  is to get web content (scrape a page, crawl a site), search the web, extract
  structured data from pages, or automate a real browser, and you want to pick
  the right Spider tool for the job.
---

# Spider Cloud

Spider is a high-throughput web data platform for AI agents: crawl sites, scrape
pages, search the web, extract structured data with AI, and control remote
browsers — all through the hosted Spider MCP server with built-in anti-bot
bypass, proxy rotation, and stealth escalation.

Use this skill as the starting point for any Spider task: it maps the request to
the right tool. The individual capabilities each have a deeper skill —
`spider-crawl-scrape` for fetching/searching content and `spider-browser` for
interactive browser sessions.

## Setup

The Spider MCP server is hosted at `https://mcp.spider.cloud/mcp` and uses OAuth:
on first connection the client prompts for browser authorization — no API key to
paste (a `SPIDER_API_KEY` Bearer token from https://spider.cloud/api-keys works as
an alternative). Core tools run on pay-per-use credits; the AI tools require an
[AI subscription](https://spider.cloud/ai/pricing).

## Picking the right tool

**Need web content?**
- One page → `spider_scrape`
- Multiple pages / follow links → `spider_crawl`
- Behind bot protection → `spider_unblocker`
- Already have HTML locally → `spider_transform`

**Need to find something?**
- Web search → `spider_search` (set `fetch_page_content: true` to get full content)
- Links on a known page → `spider_links`

**Need AI-driven extraction?** (requires AI subscription, each takes a plain-English `prompt`)
- Structured data from a page → `spider_ai_scrape`
- Crawl guided by intent → `spider_ai_crawl`
- Search with ranking/filtering → `spider_ai_search`
- Link discovery by description → `spider_ai_links`
- Multi-step browser task in English → `spider_ai_browser`

**Need interactive browser control?** → the `spider_browser_*` tools (see the `spider-browser` skill)

**Need account info?**
- Credit balance → `spider_get_credits` (check before large operations)

## Common patterns

- **Search then scrape** — `spider_search` to find URLs, then `spider_scrape` each, or set `fetch_page_content: true` to do it in one call.
- **Scrape then transform** — `spider_scrape` with `return_format: "raw"` for HTML, then `spider_transform` to reformat without extra requests.
- **Bypass bot protection** — try `spider_scrape` first; if blocked, escalate to `spider_unblocker`; for interactive sites use the browser tools with higher `stealth`.
- **Budget guard** — `spider_get_credits` before a large crawl.
