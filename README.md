# Oxmaint MCP

Public, docs-only wrapper for the hosted **Oxmaint** remote MCP server. No code or secrets live here.

| | |
|---|---|
| **MCP Server URL (frozen)** | `https://oxmaint-portal-autonomous-units.vercel.app/api/mcp` |
| Health | `https://oxmaint-portal-autonomous-units.vercel.app/api/mcp?health=1` |
| Alias (when DNS healthy) | `https://mcp.oxmaint.ai/api/mcp` → same server |
| Transport | Streamable HTTP (MCP protocol `2025-06-18`, `2025-03-26`, `2024-11-05`) |
| Auth | `Authorization: Bearer <OXMAINT_MCP_API_KEY>` (required on POST) |
| Mode hint | Optional header `X-Oxmaint-Mode: sandbox` \| `demo` \| `production` |

**Contract:** 36-tool catalog (8 packs) is frozen for customer bind/explore — see Oxmaint Development freeze note `oxmaint-mcp-public-freeze-t733u-2026-10-25.md`. Live `tool_count` may lag until mount completes; URL and tool names do not change when the durable backend switches.

Sandbox is a **tenant flag** on this same URL (not a second host). Production tenant keys are issued **after purchase** only. Prospects use the demo key on this surface.

## Quick start

Set `OXMAINT_MCP_API_KEY` (request a demo key from Oxmaint), then use the included [`.mcp.json`](./.mcp.json).

```bash
# Discovery (no key needed)
curl https://oxmaint-portal-autonomous-units.vercel.app/api/mcp

# tools/list (key required)
curl -X POST https://oxmaint-portal-autonomous-units.vercel.app/api/mcp \
  -H "Authorization: Bearer $OXMAINT_MCP_API_KEY" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -H "X-Oxmaint-Mode: sandbox" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

Without a bearer token, POST returns `401 Unauthorized`.

## Support

https://oxmaint.ai · contact@oxmaint.com
