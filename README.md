# Urlbox plugin for Cursor

Urlbox renders any web page the way a real browser sees it. This plugin
connects Cursor to Urlbox's remote MCP server (`https://api.urlbox.com/mcp`),
so the agent can screenshot a URL, render your own HTML to PNG, JPEG, WebP or
PDF, save pages as PDFs, record videos, and turn pages into Markdown, text or
rendered HTML. It can also manage your Urlbox projects, webhooks and usage.

It contains no code: one MCP server and four skills.

## Install

From the Cursor Marketplace, search for **Urlbox** and click **Add to
Cursor**.

To try it from this repository instead:

```bash
git clone https://github.com/urlbox/urlbox-cursor-plugin ~/.cursor/plugins/local/urlbox
```

Then restart Cursor, or run **Developer: Reload Window**.

## Sign in

The plugin uses OAuth, so there is no key to paste.

1. Open Cursor Settings, then MCP, and find the `urlbox` server.
2. Click to sign in. Urlbox opens in your browser.
3. Sign in, choose an organisation, an access level (Full access, Render and
   read, or Read only) and a default project, then approve.

Only organisation owners and admins can connect. You can review and revoke the
connection in the Urlbox dashboard under Organisation settings, AI agents. You
need a Urlbox account; [sign up at urlbox.com](https://urlbox.com).

### Using an API key instead

Where browser sign-in is not possible, the server also accepts an organisation
API key (Organisation settings, API keys in the
[dashboard](https://urlbox.com/dashboard)) as a Bearer token. Add a server to
your own `~/.cursor/mcp.json` and keep the key in an environment variable:

```json
{
  "mcpServers": {
    "urlbox-key": {
      "url": "https://api.urlbox.com/mcp",
      "headers": { "Authorization": "Bearer ${env:URLBOX_API_KEY}" }
    }
  }
}
```

Never commit a key.

## Example prompts

- "Take a full-page screenshot of https://stripe.com/pricing with ads and
  cookie banners hidden."
- "Screenshot our staging homepage at iPhone size in dark mode."
- "Render `templates/og-card.html` to a 1200 by 630 PNG."
- "Convert `CHANGELOG.md` to an A4 PDF."
- "Save https://example.com/terms as a PDF with backgrounds."
- "How many Urlbox renders do I have left this month?"

Every render counts against your Urlbox plan.

## What's included

| Path | What it is |
|---|---|
| `.cursor-plugin/plugin.json` | Plugin manifest |
| `mcp.json` | The Urlbox MCP server (Streamable HTTP, OAuth) |
| `skills/screenshot-url` | Screenshot a URL: full page, viewport, device, dark mode, blocking ads and cookie banners |
| `skills/render-html` | Render HTML or Markdown to an image or PDF |
| `skills/capture-pdf` | Save a page as a PDF |
| `skills/connect-urlbox` | Signing in, API keys and common errors |

## MCP tools

| Tool | What it does |
|---|---|
| `render_screenshot` | PNG, JPEG or WebP screenshot of a page |
| `render_pdf` | Render a page to PDF |
| `render_video` | Record a page as a video |
| `render_markdown` | Convert a page to Markdown |
| `render_text` | Page text and metadata, with optional structured extraction |
| `render_html` | The rendered HTML of a page |
| `render` | General-purpose render for other formats and combinations |
| `render_batch` | Render many URLs, or one URL several ways |
| `get_render`, `list_renders` | Look up renders |
| `list_projects`, `get_project`, `create_project`, `update_project`, `set_project_defaults` | Manage projects |
| `list_webhooks`, `create_webhook`, `update_webhook`, `delete_webhook` | Manage webhooks |
| `get_organisation`, `get_usage` | Organisation details and plan usage |
| `describe_options` | Search the render option reference |
| `report_render` | Report a render that does not look right |

Which tools appear depends on the access level chosen at sign-in.

## Support

- Documentation: https://urlbox.com/docs/agents/mcp
- Email: support@urlbox.com
- Contact: https://urlbox.com/contact
- Issues with this plugin: https://github.com/urlbox/urlbox-cursor-plugin/issues

## License

MIT
