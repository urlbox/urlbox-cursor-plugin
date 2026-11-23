---
name: screenshot-url
description: Take a screenshot of a web page with Urlbox. Use when the user asks to screenshot, capture or preview a URL, including full-page, mobile or device-sized, dark mode, or ad-free and cookie-banner-free captures.
---

# Screenshot a URL

## When to use

- "Screenshot https://example.com"
- "Show me what this page looks like on a phone / in dark mode"
- "Capture the whole page without the cookie banner"

## Steps

1. Call the Urlbox MCP tool `render_screenshot` with:
   - `url`: the page to capture.
   - `intent`: one sentence (10 to 500 characters) saying why the render is
     needed. Every Urlbox tool call requires it.
2. Add the options the request calls for:

   | The user wants | Option |
   |---|---|
   | The whole page, not just the first screen | `full_page: true` |
   | A specific viewport | `width` and `height` in pixels (default is desktop, 1280 by 1024) |
   | A phone | `width: 390`, `height: 844`, plus a mobile `user_agent` |
   | Dark mode | `dark_mode: true` |
   | No ads | `block_ads: true` |
   | No cookie banners | `hide_cookie_banners: true`, and `click_accept: true` if one remains |
   | A sharper image | `retina: true` |
   | JPEG or WebP instead of PNG | `format: "jpeg"` or `format: "webp"` |
   | One element only | `selector` with a CSS selector |
   | A slow page | `delay` in milliseconds, or `wait_for` a CSS selector |

   Common options are fields on the tool. Put any other option in the tool's
   `options` object. If unsure of an option's name or allowed values, call
   `describe_options` with a keyword (for example "cookie") first.
3. Show the user the returned image or its URL.
4. If the result says the render is still running, call `get_render` with the
   returned `renderId` until it finishes.

## Notes

- For several URLs, or one URL at several sizes, use `render_batch` and collect
  the results with `get_render`.
- Renders go to the default project chosen at sign-in. Pass `project` (an id
  or exact name from `list_projects`) to use another.
- Each render counts against the user's Urlbox plan. `get_usage` shows what is
  left.
- If the screenshot does not match the live page, offer `report_render`.
