# Oxmaint MCP

Public, docs-only wrapper for the hosted **Oxmaint** remote MCP server. No code or secrets live here.

| | |
|---|---|
| **MCP Server URL** | `https://mcp.oxmaint.ai/api/mcp` |
| Fallback (same server, until DNS propagates) | `https://oxmaint-portal-autonomous-units.vercel.app/api/mcp` |
| Transport | Streamable HTTP (MCP protocol `2025-06-18`, `2025-03-26`, `2024-11-05`) |
| Auth | `Authorization: Bearer <OXMAINT_MCP_API_KEY>` (required on POST) |

## Tools (v1)

- `stage_robot_command` — stage a command for an autonomous unit (staged for human approval, not executed blindly)
- `run_quality_inspection` — run a quality inspection on an asset/unit and return findings
- `list_robot_commands` — list staged/recent robot commands and their status

## Quick start

Set `OXMAINT_MCP_API_KEY` (request a key from Oxmaint), then use the included [`.mcp.json`](./.mcp.json) in any MCP client that supports remote HTTP servers.

```bash
# Discovery (no key needed)
curl https://mcp.oxmaint.ai/api/mcp

# tools/list (key required)
curl -X POST https://mcp.oxmaint.ai/api/mcp \
  -H "Authorization: Bearer $OXMAINT_MCP_API_KEY" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

Without a bearer token, POST returns `401 {"error":"Unauthorized"}`.

## Example prompts for reviewers

1. "List the recent robot commands in Oxmaint." → calls `list_robot_commands`, returns command ids, target units, and status.
2. "Run a quality inspection on unit AU-01." → calls `run_quality_inspection`, returns inspection result/findings.
3. "Stage a robot command to send AU-01 to the inspection bay." → calls `stage_robot_command`, returns a staged command id awaiting approval.

## Support

https://oxmaint.ai
