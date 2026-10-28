---
name: skydropx
description: Use the Skydropx MCP tools to quote, buy, track and cancel shipments in Mexico, schedule pickups and manage store orders for the signed-in Skydropx account. Use when the user mentions Skydropx, shipping labels (guías), carriers like Estafeta, DHL, FedEx, UPS, Paquetexpress, or asks to quote, ship, track or pick up a package in Mexico.
---

# Skydropx

Shipping for the authenticated Skydropx merchant through the `skydropx` MCP server.

## Happy path

1. `quote_shipment` — a postal code per address usually suffices; missing neighborhoods
   are completed from the postal-code catalog. Rates come back `pending`.
2. `get_quotation` — poll until `is_completed` is true (a few seconds), then show the
   rates (price, carrier, delivery days) and let the user choose.
3. `create_shipment` with the chosen `rate_id`. Resolve `consignment_note` via
   `list_consignment_notes` and `package_type` via `list_packagings` from the user's plain
   words. Phone and email are required for both parties.
4. `track_shipment` / `cancel_shipment` / `schedule_pickup` as needed.

## Rules

- Prefer calling a tool with the data you have over asking first. If the tool needs more,
  it answers `needs_input` naming the exact fields; ask only for those, in one question.
- When the user references a known person or place ("my usual customer", "my warehouse"),
  check `list_address_templates` before asking for an address.
- A result with `status: confirmation_required` means the action was NOT executed: show
  the user what it will do (cost, carrier, addresses), get an explicit yes, then call the
  same tool again with `_confirm: true`.
- On errors, follow `code` and `suggested_fix` (e.g. `RATE_EXPIRED` → quote again,
  `insufficient_scope` → the connection lacks write access, `ENDPOINT_UNAVAILABLE` → the
  capability is not available in this environment).
- Never guess IDs, rates or postal codes. Amounts are MXN.
