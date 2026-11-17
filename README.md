# AeonProxy MCP server

Residential and mobile proxies for AI agents. Connect Claude, ChatGPT, Cursor, VS Code, Claude Code, Windsurf or any other [Model Context Protocol](https://modelcontextprotocol.io) client to [AeonProxy](https://aeonproxy.com) and the assistant can look up proxy locations and prices, make ready proxy lines from your account and load web pages through residential or mobile IPs in the country you choose.

```
https://aeonproxy.com/mcp
```

Remote server, Streamable HTTP. Nothing to install. Public tools work without an account; account tools ask you to sign in (OAuth) or take your API key.

Full guide: **https://aeonproxy.com/docs/setup/mcp-server**

## Tools

| Tool | What it does | Sign-in |
| --- | --- | --- |
| `list_proxy_locations` | Countries, regions, cities, ISPs and mobile carriers, each with its login key | No |
| `get_proxy_prices` | Price per GB by volume, ISP and datacenter prices per IP, unlimited plans, the free trial | No |
| `quote_proxy_price` | The exact price of an amount of traffic or a number of IPs | No |
| `get_ip_stock` | Countries where ISP or datacenter IPs can be ordered now, with the number in stock | No |
| `check_site_access` | Whether the gateway blocks a site or port by default | No |
| `get_service_status` | Live state of the gateway and networks | No |
| `test_proxies` | Checks proxy lines from any provider: answers or not, latency, exit IP and country | No |
| `search_aeonproxy_docs` | Finds the setup guide or docs section for a tool, error or feature | No |
| `get_account` | Balance, traffic left, sub-users, active orders and your proxy logins | Yes |
| `generate_proxy_lines` | Up to 100 ready lines with country, region, city, ISP or carrier targeting, rotating or sticky up to 6 hours | Yes |
| `fetch_via_proxy` | Loads a page through your residential or mobile proxy in the chosen country and returns Markdown, text or HTML | Yes |
| `get_traffic_usage` | Your traffic per day over the last 1–30 days | Yes |
| `list_orders`, `get_order` | Your orders, and every IP of an ISP or datacenter order with its login | Yes |

No tool buys anything, tops up or changes your account. `fetch_via_proxy` uses your paid traffic and reports the KB each page used.

## Connect

**Claude** (web and desktop): Customize → Connectors → Add custom connector → `https://aeonproxy.com/mcp`. When a tool needs your account, Claude shows **Connect**: sign in to AeonProxy and choose **Allow**.

**ChatGPT**: turn on developer mode (Settings → Apps → Advanced settings), create an app with `https://aeonproxy.com/mcp` and OAuth.

**Claude Code**

```bash
claude mcp add --transport http aeonproxy https://aeonproxy.com/mcp
```

Then `/mcp` → AeonProxy → sign in in the browser.

**Cursor** (`~/.cursor/mcp.json`)

```json
{
  "mcpServers": {
    "aeonproxy": { "url": "https://aeonproxy.com/mcp" }
  }
}
```

**VS Code** (`.vscode/mcp.json`)

```json
{
  "servers": {
    "aeonproxy": { "type": "http", "url": "https://aeonproxy.com/mcp" }
  }
}
```

**Windsurf** (`mcp_config.json`)

```json
{
  "mcpServers": {
    "aeonproxy": { "serverUrl": "https://aeonproxy.com/mcp" }
  }
}
```

**Clients that only start local servers**, through [`mcp-remote`](https://www.npmjs.com/package/mcp-remote) (Node.js):

```json
{
  "mcpServers": {
    "aeonproxy": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://aeonproxy.com/mcp"]
    }
  }
}
```

## Sign-in or API key

- **Sign-in (OAuth 2.1 with PKCE).** The app opens a page on aeonproxy.com that names the app and what it may do. After **Allow** the connection appears in Settings → Security as "AeonProxy MCP · <app>" and can be ended there. The token works on the MCP server only.
- **API key.** For scripts and clients that let you set headers: copy the key from Settings → API and send `Authorization: Bearer ap_live_…` (or `X-API-Key`). It is the key of the [AeonProxy REST API](https://aeonproxy.com/docs/api). Keys in the URL are not accepted.

```json
{
  "mcpServers": {
    "aeonproxy": {
      "url": "https://aeonproxy.com/mcp",
      "headers": { "Authorization": "Bearer ap_live_YOUR_KEY" }
    }
  }
}
```

## Limits

- 120 requests a minute per account (shared with the REST API key), 300 a minute per address without sign-in.
- `fetch_via_proxy`: one GET request per call, up to 5 redirects, no JavaScript, 5 MB and 30 seconds per page, 20 pages a minute and 3 at a time per account.
- `generate_proxy_lines`: up to 100 lines per call. `test_proxies`: 10 lines per call without sign-in, 50 with it.

## Privacy

The server does not log the addresses of the pages you load, and it stores nothing about your conversations. Account data is read only for the tools you call. AeonProxy's [Privacy Policy](https://aeonproxy.com/privacy) and [Terms of Service](https://aeonproxy.com/terms) apply; use of the proxies is subject to the [Acceptable Use Policy](https://aeonproxy.com/acceptable-use).

## Support

Chat on [aeonproxy.com](https://aeonproxy.com) or [aeonproxy.com/contact](https://aeonproxy.com/contact).

## Registry

Listed in the official MCP Registry as `com.aeonproxy/mcp` ([server.json](server.json)).
