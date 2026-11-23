---
name: render-html
description: Render HTML or Markdown to an image or PDF with Urlbox. Use when the user wants to turn an HTML snippet, a template, a local HTML file or Markdown into a PNG, JPEG, WebP or PDF, for example social cards, invoices, reports or email previews.
---

# Render HTML or Markdown to an image or PDF

## When to use

- "Turn this HTML into a PNG"
- "Make an Open Graph image from this template"
- "Render this Markdown file as a PDF"

## Steps

1. Get the HTML to render.
   - HTML: use it as given. If it is a file in the workspace, read the file.
   - Markdown: convert it to a complete HTML document first, with a
     `<style>` block for fonts and spacing. Urlbox renders HTML, not Markdown.
   - Inline the CSS. Reference images and fonts by absolute `https://` URLs,
     because the renderer cannot read local files.
2. Pick the tool by the output wanted:
   - an image: `render_screenshot`
   - a PDF: `render_pdf`
3. Call it with `html` set to the document, instead of `url`, and an `intent`
   of 10 to 500 characters saying why the render is needed.
4. Set the size and format:
   - Image: `width` and `height` for the canvas (1200 by 630 for a social
     card), `format` of `png`, `jpeg` or `webp`, and `retina: true` for a
     sharper result. Use `full_page: true` when the content is taller than
     the viewport, or `selector` to crop to one element.
   - PDF: see the `capture-pdf` skill for page size, margins and backgrounds.
5. Show the user the returned file or its URL.

## Notes

- Call `describe_options` with a keyword to look up any other option.
- The reverse direction, a web page to Markdown, is `render_markdown`.
- If the result says the render is still running, call `get_render` with the
  returned `renderId`.
