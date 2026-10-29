# PuraSIM MCP server — travel eSIM plans for AI assistants

Remote [Model Context Protocol](https://modelcontextprotocol.io) server for **[PuraSIM](https://purasim.com/en)**, a travel eSIM store. It lets Claude, ChatGPT, Cursor, VS Code and any MCP client search PuraSIM's prepaid data plans for 187 countries and territories, recommend a plan for a trip, check coverage and mobile networks, and return a link where the user can buy the plan on purasim.com.

```
https://api.purasim.com/mcp
```

- **Transport:** Streamable HTTP (stateless, JSON responses)
- **Authentication:** none. It only reads the public catalog, the same data shown on purasim.com
- **Read-only:** no tool places an order, takes a payment or touches an account
- **Data:** the live PuraSIM catalog (about 3,400 plans), rebuilt daily from the store and the network provider

## Connect

**Claude (claude.ai and Claude Desktop):** Settings → Connectors → Add custom connector → paste `https://api.purasim.com/mcp`.

**Claude Code:**

```bash
claude mcp add --transport http purasim https://api.purasim.com/mcp
```

**ChatGPT (developer mode):** create a custom connector with this URL and authentication *None*.

**Cursor** (`~/.cursor/mcp.json`), **Windsurf**, and other clients that take a JSON config:

```json
{
  "mcpServers": {
    "purasim": { "url": "https://api.purasim.com/mcp" }
  }
}
```

**VS Code** (`.vscode/mcp.json`):

```json
{
  "servers": {
    "purasim": { "type": "http", "url": "https://api.purasim.com/mcp" }
  }
}
```

Clients that only speak stdio can use [`mcp-remote`](https://www.npmjs.com/package/mcp-remote):

```json
{
  "mcpServers": {
    "purasim": { "command": "npx", "args": ["-y", "mcp-remote", "https://api.purasim.com/mcp"] }
  }
}
```

## Tools

| Tool | What it does |
|---|---|
| `recommend_plan` | Best plan(s) for a trip from its length and expected usage (light, medium, heavy). Accepts one destination or a list of countries for a multi-country trip. |
| `search_plans` | Plans for a country, region or global, cheapest first. Filters: data, validity, price, daily plans. |
| `check_coverage` | "Does it work in Peru?": every destination that covers a country, with its mobile networks. |
| `get_destination_info` | Countries, operators, 4G/5G, validity options and entry price of a destination. |
| `get_plan` | Full detail of one plan: data, validity, speed after the daily allowance, networks per country. |
| `list_destinations` | All destinations (countries, regions, global) with entry prices. |
| `create_checkout_link` | The purasim.com page of a plan with the plan preselected. No order is placed. |

Every tool accepts `locale` (en, es, de, fr, it, pt, nl, pl, cs, da, no, sv, ro, tr, ar, he, id, ja, ko, th, zh) for the language of the store pages and country names, and `currency` (USD, EUR, GBP) for prices. Destinations can be given in any of those languages, as ISO codes ("JP") or as regions ("Europe", "Balkans", "Global").

## Example questions

- "I'm going to Japan for 10 days and use a lot of maps and Instagram. Which eSIM should I get?"
- "Two weeks in Spain, Portugal and Morocco. Is there one eSIM that covers all three?"
- "Does a PuraSIM eSIM work in Peru, and on which networks?"
- "Cheapest eSIM for the USA with at least 5 GB for a week, in euros."

## What the plans are

Prepaid, data-only eSIMs (no phone number), delivered by email with a QR code and installed on any eSIM-compatible, carrier-unlocked phone. [Check if your phone supports eSIM](https://purasim.com/en/pages/compatibilidad). Prices on purasim.com may be shown in the shopper's local currency at checkout.

## Privacy

The server needs no account and receives no personal data: only the arguments of each tool call (destination, trip length, filters). Links carry a source tag so PuraSIM can count visits that come from AI assistants.

## Contact

[purasim.com](https://purasim.com/en) · team@purasim.com
