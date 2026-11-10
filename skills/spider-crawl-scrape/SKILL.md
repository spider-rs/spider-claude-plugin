---
name: spider-crawl-scrape
description: >-
  Parameters and call patterns for Spider's fetch tools: spider_scrape,
  spider_crawl, spider_search, spider_links, spider_transform,
  spider_screenshot and the spider_ai_* extraction tools. Use when you are
  about to call one of them and need the right arguments for clean markdown,
  a bounded crawl, a search with page content, or a page that came back
  blocked or empty.
---

# Spider: fetch tool parameters

Every fetch tool takes `url`. Comma-separate several URLs to fetch them in one
call.

## Shared options

- `return_format`: `markdown` for reading, `text` for plain text, `raw` for
  HTML. The default is `raw`.
- `request`: `smart` (default) picks HTTP or a headless browser per page.
  `http` is the fastest. `chrome` renders JavaScript.
- `filter_output_main_only: true` drops nav, aside and footer.
- `readability: true` gives cleaner article text.
- `root_selector` keeps one part of the page, such as `"#main-content"`.
  `exclude_selector` drops a part.
- `metadata: true` adds the title, description and keywords.
- `return_page_links: true` adds the links found on each page.
- `country_code` (for example `"gb"`) fetches from that country.

## spider_scrape: one page

Use it when the user gives one URL, or a short list of URLs, and wants those
pages only. It does not follow links.

```
spider_scrape: { url: "https://example.com/pricing", return_format: "markdown",
                 filter_output_main_only: true }
```

## spider_crawl: many pages from one site

Use it when the user wants a section or a whole site.

- `limit`: max pages. The default is `0`, no cap. Always set it.
- `depth`: max link depth from the start URL. The default is `25`.
- `whitelist` or `blacklist`: URL path patterns to keep or skip, regex allowed.
- `budget`: pages per path, for example `{"*": 100, "/blog": 20}`.
- `sitemap: true` adds pages from sitemap.xml.
- `subdomains: true` follows subdomains.

```
spider_crawl: { url: "https://react.dev/reference/react", limit: 50,
                return_format: "markdown", filter_output_main_only: true }
```

## spider_search: find pages first

Use it when the user has a topic or question and no URL.

- `search`: the query.
- `num`: how many results to return.
- `fetch_page_content: true` returns each result page's content. Set
  `limit: 1` with it, or each result URL is crawled as a whole site.
- `tbs`: time range. `qdr:h` hour, `qdr:d` day, `qdr:w` week, `qdr:m`
  month, `qdr:y` year.
- `country`, `language`, `location`: narrow the results.

```
spider_search: { search: "rust async runtime comparison", num: 5,
                 fetch_page_content: true, limit: 1, return_format: "markdown" }
```

## spider_links: list URLs without content

Returns the links on a page. Use it to choose what to scrape or crawl. `limit`
caps the number of links.

## spider_transform: HTML you already have

Converts HTML to markdown or text with no web request.

## spider_screenshot: page image

Returns a base64 PNG. Supports full page capture and custom viewports.

## A page came back blocked or empty

1. Retry `spider_scrape` with `request: "chrome"` so the page renders
   JavaScript.
2. If it is still blocked, add `proxy: "residential"`.
3. If the page needs a click, a cookie banner closed or a login, switch to the
   browser tools in the `spider-browser` skill.

Do not loop retries on the same settings. Report the status to the user after
the steps above.

## AI extraction (needs an AI subscription)

Each takes `url` (or `search`) and a plain English `prompt`:

- `spider_ai_scrape`: structured JSON from a page.
- `spider_ai_crawl`: a crawl guided by the prompt, with `limit`.
- `spider_ai_search`: search with AI ranking and filtering.
- `spider_ai_links`: links matched to a description.
- `spider_ai_browser`: a multi-step browser task described in words.

If a call says the subscription is missing, tell the user and fall back to the
core tools.

## Credits

`spider_get_credits` returns the balance. Check it before a large crawl.
