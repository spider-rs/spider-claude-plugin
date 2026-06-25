# Spider Cloud plugin for Claude Code

The fastest web crawling, scraping, search, and browser automation for AI agents —
delivered through the hosted **Spider Cloud MCP server**, with built-in anti-bot
bypass, proxy rotation, and AI-driven extraction.

This repository is **both a Claude Code plugin and its own marketplace**, so you
can install it in one step.

- **Website:** https://spider.cloud
- **MCP docs:** https://spider.cloud/mcp
- **Get an API key:** https://spider.cloud/api-keys

## Install

```shell
/plugin marketplace add spider-rs/spider-claude-plugin
/plugin install spider@spider-tools
```

Then run `/reload-plugins` to activate it.

## What this plugin ships

- **1 MCP server** — the hosted Spider endpoint at `https://mcp.spider.cloud/mcp`, exposing crawl, scrape, search, links, screenshot, unblocker, transform, credits, AI extraction, and remote browser-automation tools.
- **3 skills** — `spider` (overview + tool-selection guide), `spider-crawl-scrape` (crawling, scraping, search, bot bypass), and `spider-browser` (interactive browser sessions). Namespaced as `/spider:spider-crawl-scrape`, etc.

## Authentication

The Spider MCP server uses **OAuth** — on first connection Claude Code prompts you
to authorize in the browser, with no API key to paste. The bundled
[`.mcp.json`](.mcp.json) just points at the hosted endpoint:

```json
{
  "mcpServers": {
    "spider": {
      "type": "http",
      "url": "https://mcp.spider.cloud/mcp"
    }
  }
}
```

The endpoint advertises OAuth via [protected-resource metadata](https://mcp.spider.cloud/.well-known/oauth-protected-resource/mcp)
with dynamic client registration and PKCE, so MCP clients authorize automatically.

**API-key alternative.** The server also accepts a Bearer API key if you prefer
not to use OAuth — add an `Authorization` header to the config:

```json
"headers": { "Authorization": "Bearer ${SPIDER_API_KEY}" }
```

with `SPIDER_API_KEY` from https://spider.cloud/api-keys.

Core tools run on pay-per-use credits (no subscription required). The AI tools
require an [AI subscription](https://spider.cloud/ai/pricing).

## Network & credentials (for review)

- **Endpoint called:** `https://mcp.spider.cloud/mcp` (the official Spider Cloud hosted MCP server) and its OAuth discovery/authorization endpoints under the same host. No other network endpoints.
- **Credential:** none stored by the plugin. Auth is OAuth (browser authorization, tokens managed by Claude Code) or, optionally, a user-supplied `SPIDER_API_KEY` sent only as the `Authorization: Bearer` header to the endpoint above. The plugin ships no hooks or scripts, reads nothing from the filesystem, and collects no telemetry.

## License

MIT — see [LICENSE](LICENSE).
