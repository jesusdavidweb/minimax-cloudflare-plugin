# MiniMax Cloudflare Account Plugin

Portable **Agent Plugins 1.0** package that connects compatible coding agents to Cloudflare's official API MCP server.

## Included

- `plugin.json` — Agent Plugins 1.0 manifest.
- `mcp.json` — Streamable HTTP connection to Cloudflare API MCP.

## MCP endpoint

`https://mcp.cloudflare.com/mcp`

## Authentication

Authentication is handled by the MCP client. Cloudflare's MCP server can initiate its authorization flow when the client connects. Grant only the permissions required for your workflows.

## MiniMax Code

In **Import plugin → Import from GitHub**, use:

`https://github.com/jesusdavidweb/minimax-cloudflare-plugin`

The repository root contains both required Agent Plugins files.
