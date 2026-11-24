---
name: dockly-today
description: Show today's Dockly dock door schedule and any recent driver check-ins.
---

Show what is happening at the dock today.

1. Call `list_dock_doors` with no arguments (defaults to today, UTC).
   If `$ARGUMENTS` contains a date in `YYYY-MM-DD` form, use it instead.
2. Report the assignments grouped by door number, in order, with each
   load's BOL, carrier, consignee, and status.
3. Call `list_check_ins` with `limit` 10 and report the most recent
   driver check-ins, newest first.
4. Read-only: never attempt to change any assignment. If the user asks
   for a change, report the current schedule and ask what they want to do.
