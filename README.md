# Dockly plugin for Rakazo

Connect a Rakazo bot to [Dockly](https://trydockly.com) — the platform that
simplifies the dock for trucking and logistics. The bot can look up loads by
BOL number, list driver check-ins, and read the dock door schedule.

**Read-only (v1).** The plugin only reads the connected Dockly account's own
data. It cannot create, update, or cancel loads, and the included skill
instructs the bot never to attempt a write.

## Install

1. In Rakazo, open **Integrations**, choose **+** → **From GitHub**, and
   paste this repository's URL. Choose **Preview**, then **Connect**.
2. In the **Skills** section, choose **Add skills** to add the `dockly`
   skill.
3. With a bot selected, choose **Use in this bot** for the Dockly server.

## Connect your Dockly account (API key)

The MCP server needs your Dockly API key — it is never published here.

1. In Dockly, go to **Settings → API Keys**, create a key (name it e.g.
   "Rakazo"), and copy the `dk_…` value. It is shown once.
2. In Rakazo, open **Integrations** → **+** → **Manage MCP servers**, edit
   the `dockly` server, and expand **Advanced**.
3. Set **Header name** to `X-API-Key` and **Header value** to your key.
   (Alternatively, put the key in **Access token** — the server also
   accepts it as a Bearer token.)
4. Ask the bot `/dockly-today` or "where is load DKL-2026-ABC123?" to
   verify it works.

API access is a Dockly Pro / Enterprise feature. Revoke the key any time
in Dockly Settings; it stops working immediately. Keys are scoped to your
account: a key only ever sees its owner's loads, check-ins, and doors.

## Tools

| Tool | What it does |
|---|---|
| `list_loads` | Page through your loads, newest first; filter by status or BOL |
| `get_load` | One load by UUID, with its pickup/delivery stops |
| `get_load_by_bol` | One load by BOL number, with its stops |
| `list_check_ins` | Driver check-ins on your loads, newest first |
| `list_dock_doors` | Dock door assignments for a date (default today, UTC) |

## Commands

- `/dockly-load-status <BOL>` — status, stops, and latest check-in for a load.
- `/dockly-today [YYYY-MM-DD]` — today's dock schedule and recent check-ins.

## How it works

`mcp.json` points at Dockly's hosted MCP server (Streamable HTTP, MCP
2025-06-18) at
`https://owoxqgzrkoqwbeakynfg.supabase.co/functions/v1/dockly-mcp`.
The server is a Supabase edge function; its source lives in the Dockly
repository under `supabase/functions/dockly-mcp/`. Rakazo reads only the
files in this repository — it never runs code from it.

Rate limit: 60 requests per minute per API key.

## Links

- Dockly: https://trydockly.com
- Dockly Public API v1 (REST + OpenAPI): `docs/public-api-v1.md` in the
  Dockly repository; OpenAPI document at https://trydockly.com/openapi.json
