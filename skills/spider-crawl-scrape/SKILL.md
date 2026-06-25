---
name: spider-crawl-scrape
description: >-
  Crawl websites, scrape individual pages, search the web, extract links, and
  bypass bot protection with Spider Cloud. Use when the user wants to pull
  content from one URL or many, gather documentation, search the web and fetch
  results, or get past anti-bot blocks — and wants the right parameters for
  clean, LLM-ready output.
---

# Spider: crawling, scraping & search

Tools for fetching web content at scale. All return clean, LLM-ready output when
you ask for `return_format: "markdown"`.

## spider_scrape — one page
Fetch and extract a single page. Cheaper and faster than crawl when you only need
one URL.
- `return_format` — `markdown` (best for context), `text`, `raw` (HTML)
- `readability` — cleaner article extraction
- `root_selector` — scope extraction to a DOM subtree

## spider_crawl — many pages
Crawl a site following links. Best for docs sites, sitemaps, multi-page content.
- `limit` — max pages (`0` = unlimited)
- `depth` — max link depth from the start URL
- `return_format` — `markdown` recommended
- `filter_output_main_only` — strip nav/footer noise

```
spider_crawl: { url: "https://react.dev/reference/react", limit: 50,
                return_format: "markdown", filter_output_main_only: true }
```

## spider_search — web search
Search the web, optionally fetching full page content.
- `search` — the query
- `num` — max results
- `fetch_page_content` — `true` returns full content, not just URLs
- `tbs` — time filter (`qdr:h` hour, `qdr:d` day, `qdr:w` week, `qdr:m` month)

## spider_links — discover URLs
Extract links from a page without fetching their content. Use to decide what to
crawl next.

## spider_unblocker — bot-protected sites
Access content behind anti-bot protection using fingerprinting and proxy
rotation. Costs extra credits on top of a base scrape — try `spider_scrape`
first, escalate only when blocked.

## spider_transform — HTML → markdown/text
Convert HTML you already have to markdown or text without making a web request.

## spider_screenshot — page image
Server-side screenshot via the REST API. Returns base64 PNG; supports full-page
capture and custom viewports.

## AI extraction (requires AI subscription)
Each takes a plain-English `prompt`:
- `spider_ai_scrape` — structured JSON from a page ("Extract product name, price, rating for each item")
- `spider_ai_crawl` — crawl guided by intent ("Find all pricing pages and extract plan details")
- `spider_ai_search` — search with AI ranking/filtering
- `spider_ai_links` — find and categorize links by description

## Tips
- **Search then scrape** in one call with `fetch_page_content: true`.
- **Scrape then transform**: `return_format: "raw"` then `spider_transform` to reformat with no extra API call.
- Call `spider_get_credits` before large crawls.
