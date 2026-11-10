# Spider plugin for Claude Code and Cursor

This plugin connects your coding agent to Spider's hosted MCP server. The agent
gets tools to scrape a page, crawl a site, search the web, list links, take
screenshots, turn HTML into markdown and drive a cloud browser session. Three
skills tell the agent which tool fits the task, so it scrapes one page instead
of crawling a whole site, and searches only when you have not given it a URL.

The same repository is a Claude Code plugin, its own Claude Code marketplace and
a Cursor plugin.

## Install in Claude Code

```shell
/plugin marketplace add spider-rs/spider-claude-plugin
/plugin install spider@spider-tools
```

Run `/reload-plugins` afterwards. The first Spider tool call opens your browser
so you can sign in to Spider.

## Install in Cursor

Install Spider from the Cursor marketplace, or add only the MCP server with this
link:

[![Add Spider MCP server to Cursor](https://cursor.com/deeplink/mcp-install-dark.svg)](cursor://anysphere.cursor-deeplink/mcp/install?name=spider&config=eyJ1cmwiOiJodHRwczovL21jcC5zcGlkZXIuY2xvdWQvbWNwIn0=)

The link adds this server entry to Cursor:

```json
{
  "mcpServers": {
    "spider": {
      "url": "https://mcp.spider.cloud/mcp"
    }
  }
}
```

## Sign in

The server uses OAuth. Your MCP client finds the authorization server from the
[protected resource metadata](https://mcp.spider.cloud/.well-known/oauth-protected-resource/mcp),
registers itself and asks you to sign in. You paste no key and the plugin stores
no credential. Your client keeps the OAuth token.

If your client cannot do OAuth, the server also accepts a Spider API key as a
Bearer token. The [connect guide](https://spider.cloud/mcp/connect/?utm_source=spider-plugin-readme&utm_medium=directory&utm_campaign=spider-plugin-readme-listing)
shows how to set that up.

Core tools spend credits from your Spider account. The `spider_ai_*` tools need
an [AI subscription](https://spider.cloud/ai/pricing?utm_source=spider-plugin-readme&utm_medium=directory&utm_campaign=spider-plugin-readme-listing).

## What is in the plugin

- One MCP server: `https://mcp.spider.cloud/mcp`, over streamable HTTP.
- Skill `spider`: picks the tool. One URL means scrape, many pages from one
  site means crawl with a page limit, a topic with no URL means search.
- Skill `spider-crawl-scrape`: the arguments for scrape, crawl, search, links,
  transform, screenshot and the AI extraction tools, plus what to do when a page
  comes back blocked or empty.
- Skill `spider-browser`: browser session rules and the open, navigate, click,
  fill, wait, read and close steps.

In Claude Code the skills show up as `/spider:spider`, `/spider:spider-crawl-scrape`
and `/spider:spider-browser`.

## Tools

The main tools the server exposes:

- `spider_scrape`, `spider_crawl`, `spider_search`, `spider_links`,
  `spider_screenshot`, `spider_transform`, `spider_get_credits`
- `spider_browser_open`, `spider_browser_navigate`, `spider_browser_click`,
  `spider_browser_fill`, `spider_browser_wait_for`, `spider_browser_content`,
  `spider_browser_evaluate`, `spider_browser_screenshot`, `spider_browser_close`
- `spider_ai_scrape`, `spider_ai_crawl`, `spider_ai_search`, `spider_ai_links`,
  `spider_ai_browser`, with an AI subscription

## Network and data

- The plugin talks to one host, `mcp.spider.cloud`: the MCP endpoint and its
  OAuth endpoints.
- Each tool call sends its arguments to that server: the URLs, search queries,
  selectors, form values and JavaScript the agent passes. Spider then fetches the
  pages you asked for and returns their content to your agent.
- The plugin has no hooks, scripts or binaries. It reads no local files, reads
  no environment variables and sends no telemetry.
- Spider's [privacy policy](https://spider.cloud/privacy/?utm_source=spider-plugin-readme&utm_medium=directory&utm_campaign=spider-plugin-readme-listing)
  and [terms](https://spider.cloud/eula/?utm_source=spider-plugin-readme&utm_medium=directory&utm_campaign=spider-plugin-readme-listing)
  cover what the server does with that data.

## Help

- [MCP docs](https://spider.cloud/mcp/?utm_source=spider-plugin-readme&utm_medium=directory&utm_campaign=spider-plugin-readme-listing)
- [Support](https://spider.cloud/support/?utm_source=spider-plugin-readme&utm_medium=directory&utm_campaign=spider-plugin-readme-listing)
- Email support@spider.cloud

## License

MIT. See [LICENSE](LICENSE).
