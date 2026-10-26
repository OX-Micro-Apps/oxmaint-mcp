# Oxmaint MCP

Public, docs-only wrapper for the hosted **Oxmaint** remote MCP server. No code or secrets live here.

| | |
|---|---|
| **MCP Server URL (primary)** | `https://mcp.oxmaint.ai/api/mcp` |
| Health | `https://mcp.oxmaint.ai/api/mcp?health=1` |
| Same gateway | `https://oxmaint-portal-autonomous-units.vercel.app/api/mcp` |
| Transport | Streamable HTTP (MCP protocol `2025-06-18`, `2025-03-26`, `2024-11-05`) |
| Live surface | **22 tools** @ `v0.2.0` pack `core+robot+gse` |
| Auth | `Authorization: Bearer <OXMAINT_MCP_API_KEY>` (required on POST) |
| Mode hint | Optional header `X-Oxmaint-Mode: sandbox` \| `demo` \| `production` |
| Logo (interim) | [assets/oxmaint-logo.png](./assets/oxmaint-logo.png) |

**GET** `/api/mcp` returns the full live tool list + descriptions **without** Bearer. POST `initialize` / `tools/list` needs a Bearer key (tenant `MCP_API_KEY` or rate-limited demo `MCP_DEMO_API_KEY`).

Sandbox is a **tenant flag** on this same URL (not a second host). Production tenant keys are issued **after purchase** only. Prospects use the demo key on this surface. Demo/SAMPLE tenant.

## Marketplace reviewer testing

1. `curl -sS 'https://mcp.oxmaint.ai/api/mcp?health=1'` — expect `healthy`, `version` `0.2.0`, `tool_count` `22`, pack `core+robot+gse`.
2. `curl -sS https://mcp.oxmaint.ai/api/mcp` — lists all 22 tools (no Bearer).
3. Request a marketplace demo Bearer key from **contact@oxmaint.com**.
4. Prompt: *List my open work orders in Oxmaint.* → `list_my_work_orders`.
5. Prompt: *Create a high-priority maintenance request for asset CVG-PWR-0035: abnormal vibration on the drive end.* → `create_maintenance_request`.
6. Prompt: *Stage a robot command to send AU-01 to the inspection bay, then list recent robot commands.* → `stage_robot_command` + `list_robot_commands`.

### Live tools (22 @ v0.2.0)

`list_my_work_orders`, `get_work_order_detail`, `create_maintenance_request`, `list_gse_assets`, `get_planned_maintenance_month`, `list_work_orders_by_stage`, `get_parts_hold_queue`, `check_material_stock`, `create_purchase_requisition`, `get_asset_hierarchy`, `list_rca_summaries`, `list_shift_handovers`, `create_shift_handover`, `list_vendors`, `list_inspection_templates`, `check_compliance_status`, `generate_audit_report`, `get_weekly_wo_projection`, `assign_work_order`, `stage_robot_command`, `run_quality_inspection`, `list_robot_commands`.

## Quick start

Set `OXMAINT_MCP_API_KEY` (request a demo key from Oxmaint), then use the included [`.mcp.json`](./.mcp.json).

```bash
# Discovery (no key needed)
curl https://mcp.oxmaint.ai/api/mcp

# tools/list (key required)
curl -X POST https://mcp.oxmaint.ai/api/mcp \
  -H "Authorization: Bearer $OXMAINT_MCP_API_KEY" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -H "X-Oxmaint-Mode: sandbox" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

Without a bearer token, POST returns `401 Unauthorized`.

## Support

https://oxmaint.com · contact@oxmaint.com
