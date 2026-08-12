---
name: cloudflare-account
description: Manage, inspect, troubleshoot, and deploy Cloudflare resources through the official Cloudflare API MCP server, with live Cloudflare documentation available for reference. Use for DNS, zones, Workers, Pages, D1, R2, KV, Durable Objects, Queues, Workflows, Hyperdrive, AI, Access, Zero Trust, WAF, rulesets, tunnels, certificates, analytics, logs, account settings, and other Cloudflare operations.
---

# Cloudflare Account

Use `cloudflare-api` for operations against the user's Cloudflare account and `cloudflare-docs` for current documentation, API semantics, limits, compatibility details, and product guidance.

## Authentication

The `cloudflare-api` MCP bridge uses Cloudflare OAuth. The initial OAuth request intentionally asks only for the valid bootstrap scopes `user:read`, `offline_access`, and `account:read`. Cloudflare's own authorization page can then present its permission presets and permission selector. Grant only what is needed for the task; choose Cloudflare's **Full access** preset only when the user explicitly wants broad account management.

Never ask the user to paste Cloudflare passwords, Global API Keys, OAuth tokens, or other credentials into chat.

## API workflow

Cloudflare API MCP normally exposes two Code Mode tools:

- `search`: discover the correct endpoint and request shape from Cloudflare's OpenAPI catalog.
- `execute`: call the selected Cloudflare API endpoint.

For account operations:

1. Identify the correct account and, when relevant, zone before changing anything.
2. Use `search` before `execute` when the endpoint or schema is not already established in the current interaction.
3. Read the existing resource before mutating it when practical.
4. Make the smallest change required.
5. Re-read or otherwise verify the resulting state after a write.

The API MCP covers the full public Cloudflare API surface exposed by the server, including DNS, Workers, storage, security, networking, Zero Trust, analytics, GraphQL analytics, and developer-platform products, subject to the scopes the user grants and the features available on the user's Cloudflare plan.

## Documentation workflow

Use `cloudflare-docs` when current product behavior matters, especially for:

- Wrangler configuration and compatibility dates.
- Workers runtime APIs and bindings.
- Product limits and plan-specific behavior.
- Recently changed or deprecated APIs.
- Cloudflare security and Zero Trust configuration.
- Deployment architecture and recommended platform patterns.

Prefer current Cloudflare documentation over remembered API details.

## Safety

For read-only requests, do not mutate resources.

Before destructive or high-impact operations such as deleting zones, databases, buckets, Workers, DNS records, Access applications, tunnels, certificates, rulesets, or account-level security settings, clearly identify the target and inspect the current state first.

Do not weaken security controls merely to make a deployment work. Prefer narrowly scoped fixes and preserve existing WAF, Access, TLS, DNSSEC, authentication, and authorization controls unless the requested task explicitly requires changing them.

When a write succeeds, summarize what changed and which account/zone/resource was affected.
