---
name: cloudflare-account
description: Manage and inspect a Cloudflare account through the Cloudflare API MCP server. Use for DNS, zones, Workers, Pages, D1, R2, KV, Durable Objects, Access, Zero Trust, account inspection, deployments, and other Cloudflare operations.
---

Use the `cloudflare-api` MCP server for Cloudflare account operations.

On first use, if authentication is required, complete the browser-based Cloudflare OAuth flow opened by the MCP bridge. Do not ask the user to paste Cloudflare credentials into chat.

Prefer least-privilege authorization. Before destructive or high-impact changes, inspect the current state first and clearly identify the target account, zone, Worker, database, bucket, namespace, or policy.

The Cloudflare API MCP server normally exposes two code-mode tools: `search` to discover relevant Cloudflare API endpoints and `execute` to call them. Search first, then execute the smallest necessary operation.

For read-only requests, avoid mutations. For write operations, verify the affected resource and summarize the change after execution.
