---
name: spider
description: >-
  Pick the right Spider tool for a web task. Use when the user mentions Spider,
  or asks to read a web page, pull content from a whole site, search the web,
  list the links on a page, take a screenshot, turn HTML into markdown, or
  click and type on a live page. Covers when to use spider_scrape,
  spider_crawl, spider_search, spider_links and the browser tools.
---

# Spider: pick the tool

Spider's tools come from the hosted MCP server at `https://mcp.spider.cloud/mcp`.
The first call asks the user to sign in to Spider in the browser (OAuth).
Core tools spend credits from the user's Spider account. The `spider_ai_*`
tools need an AI subscription (https://spider.cloud/ai/pricing).

## Decide in this order

1. **You have a URL and need that one page.** Use `spider_scrape`. It does not
   follow links, so it is the cheapest and fastest call.
2. **You need many pages from one site**, such as a docs section, a blog
   archive or every product page. Use `spider_crawl` with a `limit`. Never
   leave `limit` unset on an unknown site: the default is `0`, which means no
   page cap.
3. **You have no URL yet, only a question or topic.** Use `spider_search`.
   Read the result URLs, then scrape the ones you need. To get page content
   in the same call, set `fetch_page_content: true` and `limit: 1`, or each
   result is crawled as a whole site.
4. **You need to know what is on a site before choosing pages.** Use
   `spider_links` on the start page, then scrape or crawl the URLs you pick.
5. **The page needs clicks, typing, a login or several steps.** Use the
   `spider_browser_*` tools. The `spider-browser` skill has the session rules.
6. **You already have the HTML.** Use `spider_transform`. It makes no web
   request.
7. **You need an image of the page.** Use `spider_screenshot`.

Scrape versus crawl is the common mistake. If the user names one URL and asks
about that page, scrape it. If they say "the docs", "the whole site", "all the
pages under /blog", crawl it with a limit that matches the ask.

Search versus scrape is the other one. If the user gives a URL, do not search
for it. If they give a topic, search first.

## Defaults that save tokens

- Ask for `return_format: "markdown"`. The default is raw HTML.
- Set `filter_output_main_only: true` to drop nav, aside and footer.
- Use `readability: true` for articles.
- Use `root_selector` when you know the part of the page you need.
- Call `spider_get_credits` before a crawl of more than a few hundred pages.

## Structured data

For JSON fields from a page, prefer `spider_ai_scrape` with a plain English
`prompt`, if the user has an AI subscription. Without one, scrape as markdown
and extract the fields yourself, or pass `css_extraction_map`.

The `spider-crawl-scrape` skill lists the parameters for each fetch tool.
