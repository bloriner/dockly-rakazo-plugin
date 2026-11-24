---
name: dockly-load-status
description: Look up a Dockly load by BOL number and report its status, stops, and check-ins.
---

Look up the Dockly load for the BOL number in `$ARGUMENTS` and report where
it stands.

1. Call `get_load_by_bol` with `bol_number` set to `$ARGUMENTS` (trimmed).
   If `$ARGUMENTS` is empty, ask the user for the BOL number first.
2. Report the load's BOL number, status, carrier, shipper/consignee, and
   its pickup/delivery stops in order, with scheduled dates.
3. If the load exists, also call `list_check_ins` with the same
   `bol_number` and report the latest driver check-in, if any.
4. If the tool returns `not_found`, say the load was not found under that
   BOL in the connected Dockly account — do not guess another load.
5. Read-only: never attempt to change the load. If the user asks for a
   change, report the current state and ask what they want to do.
