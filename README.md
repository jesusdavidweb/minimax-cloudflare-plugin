# MiniMax Cloudflare Account Plugin

Portable **Agent Plugins 1.0** package for MiniMax Code and other compatible agents. It connects the agent to Cloudflare's official API MCP server and Cloudflare's documentation MCP server.

## What it provides

### `cloudflare-api`

Connects to:

`https://mcp.cloudflare.com/mcp`

Cloudflare's API MCP exposes the Cloudflare API through the token-efficient Code Mode `search` and `execute` tools. It can manage the Cloudflare resources permitted by the OAuth scopes you grant, including DNS, zones, Workers, Pages, D1, R2, KV, Durable Objects, Queues, Workflows, Hyperdrive, Access / Zero Trust, WAF, tunnels, certificates, analytics, account configuration, and other API-backed Cloudflare products.

### `cloudflare-docs`

Connects to:

`https://docs.mcp.cloudflare.com/mcp`

This gives the agent current Cloudflare documentation instead of relying only on model knowledge.

### `cloudflare-account` skill

Adds operational guidance for discovering API endpoints, inspecting state before writes, validating changes, using live Cloudflare documentation, and handling destructive operations carefully.

## Why the OAuth configuration is explicit

`mcp-remote` falls back to the generic OAuth scopes `openid email profile` when it cannot discover usable resource scopes. Those are not valid scopes for the Cloudflare API MCP authorization handler and Cloudflare returns `invalid_scope`.

This plugin therefore explicitly starts OAuth with Cloudflare's valid bootstrap scopes:

```text
user:read offline_access account:read
```

Cloudflare's authorization UI can then let you choose the permissions you actually want to grant, including its read-only and full-access presets.

## Authentication state

The plugin isolates `mcp-remote` OAuth state under the Agent Plugins writable data directory using:

```text
${PLUGIN_DATA}/cloudflare-api-auth
```

This avoids sharing credentials with unrelated `mcp-remote` integrations using the default `~/.mcp-auth` directory.

## MiniMax Code installation

Use **Import plugin → Import from GitHub** and import:

```text
https://github.com/jesusdavidweb/minimax-cloudflare-plugin
```

After updating from an older version, remove the previous plugin from MiniMax and import it again so MiniMax refreshes `mcp.json` and the bundled skill.

## OAuth permissions

For normal inspection and development work, start with the smallest useful permission set.

If you intentionally want MiniMax to manage the account broadly, select **Full access** in Cloudflare's authorization screen. The actual operations available to the MCP server are still bounded by the permissions Cloudflare grants to the OAuth token and by the products available to your account.

## Why only two MCP servers are enabled by default

Cloudflare also publishes specialized MCP servers for Workers bindings, builds, observability, Radar, Browser Rendering, Logpush, AI Gateway, AI Search, audit logs, DNS analytics, DEX, CASB, GraphQL, containers, and Agents SDK documentation.

They are not all enabled here by default because each authenticated server creates an independent MCP/OAuth connection. Loading all of them in MiniMax would add redundant tools and can trigger multiple authorization sessions. The Cloudflare API MCP already exposes the full public Cloudflare API surface; the documentation MCP covers current reference material.

## Runtime dependency

MiniMax must provide Node.js / `npx`. The plugin pins the stdio-to-remote bridge to:

```text
mcp-remote@0.1.38
```

Pinning avoids unexpected behavior changes from installing an arbitrary future `latest` version during startup.

## Troubleshooting

### `invalid_scope`

Make sure MiniMax has re-imported the latest plugin version. The current `mcp.json` must contain:

```text
user:read offline_access account:read
```

and must not contain:

```text
openid email profile
```

### Plugin loads but MCP tools are missing

Confirm MiniMax can execute `npx`. The Agent Plugins MCP configuration uses a local stdio bridge because MiniMax may not expose remote Streamable HTTP MCP entries directly as plugin tools.

### OAuth state from an old installation

Remove the plugin and import it again. Current versions keep their OAuth state inside the plugin data directory rather than the global `~/.mcp-auth` directory.
