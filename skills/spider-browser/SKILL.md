---
name: spider-browser
description: >-
  Drive a Spider cloud browser session step by step: open, navigate, click,
  fill, wait, read the page, run JavaScript, screenshot, close. Use when a task
  needs clicks, typing, a login, a form submit or several steps on one site.
  For reading a page with no interaction, use spider_scrape instead.
---

# Spider: browser sessions

Use a session only when the page needs interaction. A plain read is cheaper
with `spider_scrape` (add `request: "chrome"` if the page needs JavaScript).

## Session rules

1. `spider_browser_open` starts a session and returns a `session_id`. Pass it
   to every other browser tool.
2. Close it with `spider_browser_close` when you are done. An open session is
   billed.
3. A session closes after 5 minutes idle. You can hold at most 5 sessions at
   once. If a tool says the session is gone, open a new one.

## Tools

- `spider_browser_open`: optional `stealth` level. `0` auto (default),
  `1` standard, `2` residential, `3` premium.
- `spider_browser_navigate`: `url`. Retries on failure and detects page load
  itself, so you do not pass wait conditions.
- `spider_browser_click`: `selector`. Waits for the element first. `timeout`
  in ms, default 10000.
- `spider_browser_fill`: `selector` and `value`. Clears the field, then types.
- `spider_browser_wait_for`: `selector` to wait for an element,
  `navigation: true` to wait for the next page load, or neither to wait for the
  network and DOM to settle. `timeout` in ms, default 30000.
- `spider_browser_content`: `format: "text"` for visible text, `"html"` for
  the DOM. The default is `html`. Prefer `text` to save context.
- `spider_browser_evaluate`: `expression`, a JavaScript string run in the page.
  Wrap multi-line code as `(function(){ ... })()`.
- `spider_browser_screenshot`: a base64 PNG of the viewport.
- `spider_browser_close`: ends the session.

## Pattern: submit a form

```
1. spider_browser_open:      {}
2. spider_browser_navigate:  { session_id, url: "https://app.example.com/login" }
3. spider_browser_fill:      { session_id, selector: "input[name='email']", value: "user@example.com" }
4. spider_browser_fill:      { session_id, selector: "input[name='password']", value: "<ask the user>" }
5. spider_browser_click:     { session_id, selector: "button[type='submit']" }
6. spider_browser_wait_for:  { session_id, navigation: true }
7. spider_browser_content:   { session_id, format: "text" }
8. spider_browser_close:     { session_id }
```

Never invent credentials. Ask the user for them, and do not echo a password
back in your reply.

If a page keeps blocking the session, close it and open a new one with
`stealth: 2` or `stealth: 3`.

With an AI subscription, `spider_ai_browser` takes a `url` and a plain English
`prompt` and runs the steps itself. Use it when the user describes the goal and
not the clicks.
