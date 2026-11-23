---
name: connect-urlbox
description: Connect or troubleshoot the Urlbox MCP connector. Use when Urlbox tools are missing, the server needs sign-in, a call fails with 401, 403, 429, InvalidOptions or UsageExceeded, or the user asks how to sign in or where to get a Urlbox API key.
---

# Connect or troubleshoot Urlbox

## When to use

- The Urlbox tools do not appear, or the server shows "Needs authentication"
- A Urlbox tool call returns an error
- The user asks how to sign in or how to use an API key

## Connect by signing in (default)

1. Open Cursor Settings, then MCP (or Customize, then MCP servers), and find
   the `urlbox` server.
2. Click to sign in. Urlbox opens in the browser.
3. Sign in, choose an organisation, an access level (Full access, Render and
   read, or Read only) and a default project, then approve.
4. Back in Cursor, check that the server lists its tools. Call
   `get_organisation` to confirm the connection works.

Only organisation owners and admins can connect a client. There is no key to
create or paste. Connections can be reviewed and revoked in the Urlbox
dashboard under Organisation settings, AI agents.

## Connect with an API key (alternative)

Use this where browser sign-in is not possible.

1. In the Urlbox dashboard (https://urlbox.com/dashboard) open Organisation
   settings, API keys, and create a key. Organisation keys start with
   `ubx_org_`. The access level and default project are chosen at creation.
2. Add a server to the user's own `~/.cursor/mcp.json` that sends the key as a
   header. Keep the key in an environment variable, never in a committed file:

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

A project secret key also works, but only for the render tools, `get_render`
and `describe_options`, and it renders into its own project.

## Common errors

| Error | Cause | Fix |
|---|---|---|
| 401 Unauthorized | Not signed in, or the key is wrong or revoked | Sign in again, or replace the key |
| 403 Forbidden | The account is inactive, the project is disabled, or there is no enabled project | The response says which; fix it in the dashboard |
| 429 or `RateLimited` | Too many requests | Wait the `Retry-After` seconds, then retry |
| A tool is missing | The access level or key does not include it | Reconnect with a higher access level |
| `InvalidOptions` | An argument is missing or wrong, most often `intent` (required, 10 to 500 characters) | Fix the argument; check names with `describe_options` |
| `ProjectNotFound` or `ProjectDisabled` | Wrong or disabled `project` | Call `list_projects` and pick an enabled one |
| `UsageExceeded` | The plan's renders are used up | Call `get_usage`; upgrade or wait for the next period |
| The result says "running" | The render outlasted `wait_seconds` | Call `get_render` with the `renderId` |

If a render finished but looks wrong, call `report_render` with a short
description. For anything else, contact support@urlbox.com.
