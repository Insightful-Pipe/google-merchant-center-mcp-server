# Google Merchant Center MCP Server by Insightful Pipe

[![MCP Compatible](https://img.shields.io/badge/MCP-Compatible-blue)](https://insightfulpipe.com/mcp-servers/google-merchant-center)
[![Insightful Pipe](https://img.shields.io/badge/Insightful_Pipe-MCP_Servers-purple)](https://insightfulpipe.com/mcp-servers)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

> **Connect Google Merchant Center to AI assistants: product feeds, inventory, promotions and Shopping performance.**

Part of the [Insightful Pipe MCP Server Collection](https://insightfulpipe.com/mcp-servers) — use Google Merchant Center from Claude, ChatGPT, Cursor, and other AI assistants through the Model Context Protocol (MCP).

<img src="images/google-merchant-center-icon.svg" alt="Google Merchant Center MCP Server" width="64" height="64">

## MCP Server URL

```
https://google-merchant-center.insightfulmcp.com/
```

## What is Google Merchant Center MCP?

Google Merchant Center MCP is a **remote Model Context Protocol server** hosted by InsightfulPipe. Manage product feeds, inventory, promotions, and return policies, and query Shopping performance through the Merchant API.

## Installation

### Claude

1. Copy the MCP Server URL: `https://google-merchant-center.insightfulmcp.com/`
2. Open [Claude Connectors Settings](https://claude.ai/settings/connectors)
3. Scroll to the bottom and click **Add custom connector**
4. Paste the URL and click **Add**
5. Click **Connect** on the connector to start authorization
6. Click **Authorize access** in the browser to complete the connection

### ChatGPT

Custom MCP servers are added through ChatGPT's **Developer mode**. Availability depends on your ChatGPT plan, and workspace admins may need to allow it.

1. Turn on **Developer mode** in ChatGPT settings
2. Create a new app for a remote MCP server and paste the URL: `https://google-merchant-center.insightfulmcp.com/`
3. Authorize with your InsightfulPipe account

See OpenAI's guide: [Developer mode and MCP apps in ChatGPT](https://help.openai.com/en/articles/12584461-developer-mode-and-mcp-apps-in-chatgpt)

### Claude Code

```bash
claude mcp add --transport http google-merchant-center https://google-merchant-center.insightfulmcp.com/
```

### Cursor

Add the server to `~/.cursor/mcp.json` (all projects) or `.cursor/mcp.json` (one project):

```json
{
  "mcpServers": {
    "google-merchant-center": {
      "url": "https://google-merchant-center.insightfulmcp.com/"
    }
  }
}
```

Then authorize the connection when Cursor prompts you.

## Available Actions

44 actions: 22 read, 22 write.

### Read Actions (22)

| Action | Description |
|--------|-------------|
| `get_account` | Get the Merchant Center account information |
| `get_business_info` | Get business identity info (address, phone, customer service) |
| `get_datasource` | Get a single data source by its numeric ID (datasource_id path param) |
| `get_product` | Get a single product by its full product ID (contentLanguage~feedLabel~offerId, e.g. en~US~SKU123) |
| `get_product_status` | Get a product's approval status, issues, and destinations (returns the product with its productStatus) |
| `get_program_review` | Get a program's participation state and review eligibility (program_id path param, e.g. 'shopping-ads'); returns the program resource |
| `get_promotion` | Get a single promotion by ID (promotion_id path param) |
| `get_region` | Get one region by ID (region_id path param) |
| `get_return_policy` | Get a single return policy by ID (policy_id path param) |
| `get_shipping_settings` | Get shipping settings (services, rate groups, delivery times) |
| `get_user` | Get a single user by email (user_email path param) |
| `list_account_issues` | List account-level issues (policy, missing settings, etc.) |
| `list_datasources` | List all data sources (primary, supplemental, file-based, API-based) |
| `list_local_inventories` | List local (in-store) inventories for a product (product_id path param) |
| `list_products` | List all products in the Merchant Center account |
| `list_programs` | List participation in Merchant Center programs (Shopping Ads, Free Listings, etc.) |
| `list_promotions` | List all promotions in the Merchant Center |
| `list_regional_inventories` | List regional inventories for a product (product_id path param) |
| `list_regions` | List all regions configured on the account |
| `list_return_policies` | List online return policies attached to this account |
| `list_users` | List users with access to this Merchant Center account |
| `query_report` | Run a Merchant API Reports query (SQL-like dialect) |

### Write Actions (22)

| Action | Description |
|--------|-------------|
| `add_user` | Add a user to the account by email with the given access rights |
| `create_local_inventory_feed` | Create a scheduled-fetch LOCAL INVENTORY data source (file input, as Google requires) |
| `create_primary_feed` | Create an API-based primary product data source (use its id as data_source_id for insert_product) |
| `create_promotion_feed` | Create an API-based PROMOTION data source (use its id as data_source_id for insert_promotion) |
| `create_region` | Create a region defined by postal codes or geotarget ids |
| `create_regional_inventory_feed` | Create an API-based REGIONAL INVENTORY data source for a feed label |
| `create_return_policy` | Create an online return policy from flat fields |
| `create_supplemental_feed` | Create an API-based supplemental product data source (optionally linked to a primary feed) |
| `delete_datasource` | Delete a data source (datasource_id path param) |
| `delete_product` | Delete a product input |
| `delete_region` | Delete a region (region_id path param) |
| `delete_return_policy` | Delete a return policy (policy_id path param) |
| `fetch_datasource` | Trigger an immediate fetch of a FILE-based data source (datasource_id path param) |
| `insert_product` | Insert or update a product (upsert) |
| `insert_promotion` | Insert or update a promotion (needs a promotion-type data_source_id) |
| `remove_user` | Remove a user from the account (user_email path param) |
| `update_business_info` | Patch business identity (address and/or phone) |
| `update_local_inventory` | Insert/update local (in-store) inventory for a product (store_code must match a Business Profile store) |
| `update_product` | Update a product (upsert via productInputs:insert) |
| `update_region` | Patch a region (region_id path param); updateMask is derived from the fields you pass |
| `update_regional_inventory` | Insert/update regional inventory for a product (needs a configured region_id via create_region) |
| `update_shipping_settings` | REPLACE shipping with a single flat-rate service (wipes existing services) |

## Control What Your AI Can Do

You decide what AI agents can do with each connected account:

- **Turn individual actions on or off** for every connected account, so agents only see the actions you allow.
- **Connect as Read-only or Read & Write.** A read-only connection can only enable read actions.
- **Destructive actions stay off by default.** Actions such as deletes are disabled until an admin enables them.
- **Team access per account.** Restricted team members only use the accounts they are granted, with the read actions enabled on them.

## Usage Examples

```
"Which products have issues or are disapproved?"
```

```
"Show Shopping performance by product for the last 30 days"
```

```
"List my return policies"
```

## Pricing

The Google Merchant Center MCP server is included in every InsightfulPipe plan, together with all other MCP servers and the CLI. Plans start at $29.99/month with a 7-day free trial. See [insightfulpipe.com/pricing](https://insightfulpipe.com/pricing) for current plans.

## Explore More MCP Servers by Insightful Pipe

Visit **[insightfulpipe.com/mcp-servers](https://insightfulpipe.com/mcp-servers)** to discover our full collection of MCP servers.

- [Shopify MCP](https://insightfulpipe.com/mcp-servers/shopify)
- [WooCommerce MCP](https://insightfulpipe.com/mcp-servers/woocommerce)
- [Magento MCP](https://insightfulpipe.com/mcp-servers/magento)

**[View All MCP Servers →](https://insightfulpipe.com/mcp-servers)**

## Resources

- [Documentation](https://insightfulpipe.com/docs)
- [Video Tutorial](https://www.youtube.com/playlist?list=PLJNzvjxzI5Xwe__BJJLAelSF0ewO3mEFk)
- [InsightfulPipe Blog](https://insightfulpipe.com/blog)

## Support

- **Documentation**: [insightfulpipe.com/docs](https://insightfulpipe.com/docs)
- **All MCP Servers**: [insightfulpipe.com/mcp-servers](https://insightfulpipe.com/mcp-servers)
- **Email**: support@insightfulpipe.com

---

**[Insightful Pipe](https://insightfulpipe.com)** — AI-powered marketing analytics through MCP servers. [Explore all integrations →](https://insightfulpipe.com/mcp-servers)

## License

MIT License - see [LICENSE](LICENSE) for details.
