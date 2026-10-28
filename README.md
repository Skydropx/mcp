<p align="center">
  <img src="assets/logo.png" alt="Skydropx" width="96" height="96">
</p>

<h1 align="center">Skydropx MCP</h1>

<p align="center">
  The official <a href="https://modelcontextprotocol.io">Model Context Protocol</a> server for <a href="https://www.skydropx.com">Skydropx</a>.<br>
  Quote, buy, track and cancel shipments with Mexico's carriers from any AI assistant.
</p>

<p align="center">
  <code>https://mcp.skydropx.com/</code> · Streamable HTTP · OAuth 2.1 · MCP Registry: <code>com.skydropx/mcp</code>
</p>

---

This repository documents how to connect to the hosted Skydropx MCP server. There is nothing to install or run: the server is operated by Skydropx and you sign in with your own Skydropx account.

## What you can do

- **Quote** with the carriers on your account (Estafeta, DHL, FedEx, UPS, Paquetexpress, 99minutos, J&T Express, iMile and more) from just the origin and destination postal codes. Neighborhoods are filled in from the postal-code catalog.
- **Compare** rates by price, delivery time and capabilities (cash on delivery, insurance).
- **Buy labels** with the rate you pick. Purchases are always confirmed before they run.
- **Track** shipments, **schedule or reschedule pickups**, and request **cancellations**.
- **Work with orders** from your connected stores (Shopify, WooCommerce, Tiendanube, Wix, PrestaShop, VTEX, Mercado Libre, Amazon, Walmart, Temu) and create their labels.
- **Use your address book** ("ship it to my usual customer"), checkout shipping rules, balance and transactions.

## Requirements

- An active Skydropx account ([app.skydropx.com](https://app.skydropx.com)). Shipping operations are available for Mexico accounts.
- An MCP client that supports remote servers with OAuth (Claude, ChatGPT, Cursor, VS Code, Gemini CLI, Claude Code and others).

## Connect

Server URL:

```
https://mcp.skydropx.com/
```

The first time a client connects it opens the Skydropx sign-in page. After you approve, the client keeps the connection; you can revoke it at any time with the `disconnect` tool or from the client.

### Claude (claude.ai and desktop)

Settings → Connectors → **Add custom connector** → paste `https://mcp.skydropx.com/` → Connect.

### ChatGPT

Settings → Apps & Connectors → enable **Developer mode** → **Create** → paste `https://mcp.skydropx.com/`, authentication **OAuth** → Create.

### Claude Code

```bash
claude mcp add --transport http skydropx https://mcp.skydropx.com/
```

### Cursor

Add to `~/.cursor/mcp.json` (or `.cursor/mcp.json` in a project), or use the [`.mcp.json`](.mcp.json) in this repository:

```json
{
  "mcpServers": {
    "skydropx": {
      "url": "https://mcp.skydropx.com/"
    }
  }
}
```

### VS Code

Add to `.vscode/mcp.json`:

```json
{
  "servers": {
    "skydropx": {
      "type": "http",
      "url": "https://mcp.skydropx.com/"
    }
  }
}
```

### Gemini CLI

Add to `~/.gemini/settings.json`:

```json
{
  "mcpServers": {
    "skydropx": {
      "httpUrl": "https://mcp.skydropx.com/"
    }
  }
}
```

### Other clients

Any client that supports the Streamable HTTP transport and OAuth 2.1 works. The server publishes its authorization metadata at `https://mcp.skydropx.com/.well-known/oauth-protected-resource` and supports Dynamic Client Registration and Client ID Metadata Documents.

## Permissions and safety

- **Read-only by default.** Write tools are only offered when the client requests the `write` scope.
- **Every purchase or cancellation is confirmed** before it runs. Labels are charged to your Skydropx account balance and payment methods.
- **Your data stays in your account.** Skydropx does not train models with your data. See the [privacy notice](https://mcp.skydropx.com/legal/privacy).

## Example prompts

```
Quote a 2 kg package, 30x20x15 cm, from postal code 06600 to 64000.
Buy the cheapest label that arrives tomorrow and send me the PDF.
Where is my shipment with tracking number 1234567890?
Schedule a pickup tomorrow at my warehouse.
Create the labels for today's Shopify orders.
```

## Tools

49 tools. Write tools need the `write` scope and ask for confirmation.

| Area | Tool | What it does | Access |
|---|---|---|---|
| Account | `whoami` | Who am I | read |
| | `get_events_since` | Get events since cursor | read |
| | `get_credits` | Get credit balance | read |
| | `list_transactions` | List account transactions | read |
| | `list_extra_charges` | List extra charges | read |
| | `disconnect` | Disconnect Skydropx | connection |
| Quotes | `quote_shipment` | Quote a shipment | read |
| | `get_quotation` | Get quotation | read |
| | `rate_shipment` | Rate a shipment | read |
| | `list_carrier_services` | List carrier services | read |
| | `list_packagings` | List packaging types | read |
| | `list_consignment_notes` | List consignment note codes | read |
| | `lookup_postal_code` | Look up postal code | read |
| Shipments | `list_shipments` | List shipments | read |
| | `get_shipment` | Get shipment | read |
| | `create_shipment` | Create shipment (buy label) | write |
| | `cancel_shipment` | Cancel shipment | write |
| | `protect_shipment` | Insure shipment | write |
| | `track_shipment` | Track shipment | read |
| | `push_tracking_event` | Push tracking event | write |
| | `list_office_points` | List drop-off points | read |
| Pickups | `list_pickups` | List pickups | read |
| | `get_pickup` | Get pickup | read |
| | `pickup_coverage` | Check pickup coverage | read |
| | `schedule_pickup` | Schedule pickup | write |
| | `reschedule_pickup` | Reschedule pickup | write |
| | `reprogram_pickup` | Reprogram pickup | write |
| Orders | `list_orders` | List orders | read |
| | `get_order` | Get order | read |
| | `create_order` | Create order | write |
| | `update_order` | Update order | write |
| | `list_order_labels` | List order labels | read |
| | `create_shipment_for_order` | Create shipment for order | write |
| | `list_products` | List products | read |
| Addresses | `list_address_templates` | List saved addresses | read |
| | `get_address_template` | Get saved address | read |
| | `create_address_template` | Save address | write |
| | `update_address_template` | Update saved address | write |
| | `delete_address_template` | Delete saved address | write |
| | `verify_address_by_carriers` | Verify address with carriers | write |
| Stores | `list_merchant_stores` | List connected stores | read |
| | `get_merchant_store` | Get connected store | read |
| | `update_merchant_store` | Update connected store | write |
| | `list_merchant_store_carriers` | List store carriers | read |
| | `list_conditional_tariffs` | List checkout shipping rules | read |
| | `create_conditional_tariff` | Create checkout shipping rule | write |
| External shipments | `create_external_shipment` | Register external shipment | write |
| | `import_external_shipments` | Import external shipments | write |
| | `get_external_shipment_import` | Get external shipment import | read |

## En español

Servidor MCP oficial de Skydropx. Conecta tu cuenta con tu asistente de IA (Claude, ChatGPT, Gemini, Cursor y otros) para cotizar, comprar guías, rastrear envíos y programar recolecciones con las paqueterías de tu cuenta en México, sin salir de la conversación. URL del servidor: `https://mcp.skydropx.com/`. Inicias sesión con tu cuenta de Skydropx; las compras siempre se confirman antes de ejecutarse.

## Links

- Documentation: https://mcp.skydropx.com/docs
- Privacy: https://mcp.skydropx.com/legal/privacy
- Terms: https://mcp.skydropx.com/legal/terms
- Security: https://mcp.skydropx.com/legal/security
- Official MCP Registry: [`com.skydropx/mcp`](https://registry.modelcontextprotocol.io/v0.1/servers?search=com.skydropx)
- Support: hola@skydropx.com

This is the official server operated by Skydropx. Other listings named after Skydropx are not affiliated with it.

## License

The documentation in this repository is released under the [MIT License](LICENSE). The Skydropx name and logo are trademarks of Skydropx and are not covered by the license. The hosted service is governed by its [terms](https://mcp.skydropx.com/legal/terms).
