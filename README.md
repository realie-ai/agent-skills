# Realie plugin

Official [Realie](https://www.realie.ai) plugin for **Cursor** and **Grok Bot**. It connects the agent to Realie's hosted MCP server for live U.S. property data: parcel records, owners, sales history, mortgages, liens, tax assessments, building details, and automated value estimates (AVM) for 164M+ parcels across all 50 states and DC.

## What's included

- **MCP server** (`mcp.json`): `https://app.realie.ai/api/mcp`, Streamable HTTP. Sign in with your Realie account (OAuth) the first time a tool runs. No API key goes in the config.
- **Skill** (`skills/realie-property-research`): how to pick the right tool, reuse parcel IDs, and page results.

## Tools

All tools are read-only.

| Tool | What it does |
| --- | --- |
| `address_lookup` | Look up a property by street address and state |
| `parcel_lookup` | Look up a property by APN (+ state, county) or by `realieParcelId` |
| `property_search` | Search a state by county, city, ZIP, address, use code, foreclosure status, or recent sale date |
| `location_search` | Find properties near a latitude/longitude (radius up to 2 miles) |
| `comparables` | Comparable sales near a subject property, filtered by size, beds, baths, price, type, and time frame |
| `owner_search` | Properties held by a named owner |

List tools return up to 10 properties per call and page with a cursor or offset.

## Install

Realie is not in the Cursor Marketplace yet; the listing is waiting for Cursor's review. Grok Bot uses the same marketplace. Until it's listed:

- **Cursor (plugin, with the skill):** clone this repo into `~/.cursor/plugins/local/realie` (a real folder, not a symlink), then restart Cursor or run **Developer: Reload Window**. Call any Realie tool; Cursor opens Realie sign-in on first use.
- **Cursor (MCP server only):** add this to `~/.cursor/mcp.json`:

  ```json
  { "mcpServers": { "realie": { "url": "https://app.realie.ai/api/mcp" } } }
  ```

- **Grok Bot (MCP server only):** in the desktop app, open **Connect apps** and choose **Add a custom connector**. Set **Type** to **Remote HTTPS** and **Server URL** to `https://app.realie.ai/api/mcp`, then choose **Add plugin**.

Other clients: see the [Realie MCP Server docs](https://docs.realie.ai/realie-mcp-server).

## Account and billing

Sign in with your Realie account when prompted; new accounts are created in the same window. A Free account gets 25 tool calls a month with no card. Add a payment method at [app.realie.ai](https://app.realie.ai/billing) for more. Each successful call uses one token from your plan's monthly allowance; empty results and errors are not billed. See [Plans and Pricing](https://docs.realie.ai/api-reference/pricing).

Realie is not a consumer reporting agency. Realie data may not be used to decide an individual's eligibility for credit, insurance, employment, or housing. See the [Terms](https://www.realie.ai/terms) and [Privacy Policy](https://www.realie.ai/privacy).

## Support

[support@realie.ai](mailto:support@realie.ai) · [docs.realie.ai](https://docs.realie.ai)

## License

MIT
