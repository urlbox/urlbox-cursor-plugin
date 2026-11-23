---
name: capture-pdf
description: Save a web page as a PDF with Urlbox. Use when the user asks to save, export, print or archive a URL as a PDF, or needs control over page size, orientation, margins or backgrounds.
---

# Capture a page as PDF

## When to use

- "Save https://example.com/pricing as a PDF"
- "Export this article as an A4 PDF without the cookie banner"
- "Archive this page as a PDF"

## Steps

1. Call the Urlbox MCP tool `render_pdf` with:
   - `url`: the page to save.
   - `intent`: one sentence (10 to 500 characters) saying why the render is
     needed.
2. Add PDF options as needed. Call `describe_options` with the keyword "pdf"
   to see every PDF option with its type and allowed values. The usual ones:

   | The user wants | Option |
   |---|---|
   | A paper size | `pdf_page_size` (for example `A4` or `Letter`) |
   | Landscape | `pdf_orientation: "landscape"` |
   | Margins | `pdf_margin` |
   | Background colours and images kept | `pdf_background: true` |
   | The on-screen layout rather than the print stylesheet | `media: "screen"` |

   Options that are not fields on the tool go in its `options` object.
3. Clean the page the same way as a screenshot: `block_ads: true`,
   `hide_cookie_banners: true`, and `delay` or `wait_for` for slow pages.
4. Give the user the link to the PDF.
5. If the result says the render is still running, call `get_render` with the
   returned `renderId` until it finishes.

## Notes

- To make a PDF from HTML or Markdown instead of a URL, pass `html` in place
  of `url`. See the `render-html` skill.
- For many pages at once, use `render_batch` and collect each result with
  `get_render`.
