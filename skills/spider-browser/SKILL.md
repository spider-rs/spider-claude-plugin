---
name: spider-browser
description: >-
  Drive a real remote browser with Spider Cloud — open a session, navigate,
  click, fill forms, run JavaScript, screenshot, and read page content, with
  smart retry, browser switching, and stealth escalation. Use for multi-step
  browser automation: logging into a site, filling and submitting forms,
  scraping JS-rendered pages, or anything needing an interactive session.
---

# Spider: browser automation

Interactive browser sessions running in Spider's cloud (via spider-browser) with
smart retry, automatic browser switching, anti-bot protection, proxy rotation,
and stealth auto-escalation. Supports CDP and BiDi.

## Session lifecycle

1. `spider_browser_open` → start a session, returns a `session_id`
2. Use the `session_id` with every browser tool below
3. `spider_browser_close` → **always** close when done to stop billing

Sessions auto-close after 5 minutes idle. Max 5 concurrent sessions.

## Tools

- **`spider_browser_open`** — `browser`: `auto` (recommended), `chrome`, `chrome-new`, `firefox`; `stealth`: `0` auto-escalate, `1` standard, `2` residential, `3` premium.
- **`spider_browser_navigate`** — navigate with smart retry and automatic browser switching on failure. No need to specify wait conditions; load detection is internal.
- **`spider_browser_click`** — waits for the element, then clicks. `timeout` ms (default 10000).
- **`spider_browser_fill`** — clears the field and types the new value.
- **`spider_browser_screenshot`** — base64 PNG of the current viewport; just pass `session_id`.
- **`spider_browser_content`** — `format: "html"` (full DOM) or `format: "text"` (visible text).
- **`spider_browser_evaluate`** — run a JavaScript string expression in page context. Use `(function(){ ... })()` for multi-line; only string expressions, no arrow functions.
- **`spider_browser_wait_for`** — `selector` (wait for a CSS selector), `navigation: true` (wait for next load), or neither (wait for network idle + DOM stability). `timeout` ms (default 30000).
- **`spider_browser_close`** — close the session and release resources.

## Pattern: form submission

```
open → navigate → fill (each field) → click submit → wait_for navigation → content or screenshot → close
```

```
1. spider_browser_open:     { browser: "auto" }
2. spider_browser_navigate: { url: "https://app.example.com/login" }
3. spider_browser_fill:     { selector: "input[name='email']", value: "user@example.com" }
4. spider_browser_fill:     { selector: "input[name='password']", value: "..." }
5. spider_browser_click:    { selector: "button[type='submit']" }
6. spider_browser_wait_for: { navigation: true }
7. spider_browser_screenshot: {}
8. spider_browser_close:    {}
```

For heavily protected sites, open with `stealth: 2` or `stealth: 3`. Prefer the
`spider_ai_browser` tool when you'd rather describe the whole task in plain
English than script each step.
