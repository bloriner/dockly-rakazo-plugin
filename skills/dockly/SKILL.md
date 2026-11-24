---
name: dockly
description: Read a Dockly account's loads, driver check-ins, and dock door schedule. Use when the user asks where a load is, what its stops are, whether a driver has checked in, or what the dock schedule looks like today. Read-only: the Dockly tools never change anything.
---

# Dockly

Dockly is a platform that simplifies the dock for trucking and logistics.
It tracks loads (identified by BOL number), their pickup/delivery stops,
driver check-ins, and dock door assignments. The connected Dockly account
is the user's own company — the tools only ever return that account's data.

## Tools (all read-only, v1)

- `list_loads` — page through the account's loads, newest first. Optional
  filters: `status` (e.g. `pending`, `assigned`, `in_transit`, `delivered`,
  `canceled`, `bidding`), `bol_number`, `page`, `limit` (default 20, max 100).
- `get_load` — one load by UUID, with its `load_stops`.
- `get_load_by_bol` — one load by BOL number, with its `load_stops`. Use
  this whenever the user gives you a BOL number (e.g. `DKL-2026-ABC123`).
- `list_check_ins` — driver check-ins on the account's loads, newest first.
  Optional filters: `driver_id`, `load_id`, `bol_number`, `page`, `limit`.
- `list_dock_doors` — dock door assignments for a date (`date` as
  `YYYY-MM-DD`; defaults to today, UTC). Each assignment includes the
  assigned load's BOL, carrier, consignee, and status.

Tool results are JSON: list responses are `{data, page, limit, total}`;
single-load responses are `{data: {...load, load_stops: [...]}}`.

## Rules

1. **Read-only.** These tools never change anything in Dockly. There are
   no write tools in v1. If the user asks you to create, update, cancel,
   or check in a load, say plainly that the Dockly integration is
   read-only for now and offer to look up the current state instead. Never
   attempt a write through any other path, and never claim a change was made.
2. **Ask before acting on what you read.** Looking a load up is free;
   anything the user might do with the answer (calling a driver, moving a
   door, reassigning a load) is their call — report the facts and ask.
3. **BOL first.** When the user mentions a BOL number, call
   `get_load_by_bol` with it before anything else; do not guess a load
   from a partial number. If the tool returns `not_found`, say so — the
   load may belong to a different account or the number may be mistyped.
4. **Dates are UTC.** `list_dock_doors` with no date returns today (UTC).
   If the user means a local date, confirm which date they mean.
5. **Auth errors are the user's to fix.** If a tool returns
   `missing_api_key`, `invalid_api_key`, or `revoked_api_key`, tell the user
   to check the Dockly API key in Rakazo (Integrations → Manage MCP servers
   → dockly → Advanced) or create a fresh key in Dockly Settings → API Keys.
   If it returns `plan_required`, API access needs a Dockly Pro or
   Enterprise plan. Never ask the user to paste their key into chat.

## Answering

- When reporting a load, lead with its BOL number, status, carrier, and
  where it is in its stops; then the pickup/delivery addresses and dates.
- When reporting check-ins, lead with driver name, status, and time.
- When reporting the dock schedule, group by door number in order.
