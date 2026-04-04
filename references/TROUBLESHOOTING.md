# Prospkt MCP — Troubleshooting

## Common Errors

### "Invalid API key"
- Verify key starts with `psk_`
- Check it's active at https://app.prospkt.fr/pages/account-settings-api-keys
- No extra spaces when copying
- Key may have been revoked

### "Rate limit exceeded"
- Starter: 100 req/hr, Pro: 500/hr, Business: 2000/hr
- Space out searches
- Cache results when possible
- Upgrade plan for higher limits

### "Tier error — requires Pro/Business"
- `enrich_email`, `enrich_phone`, `get_company_linkedin`, `query_data` → Pro
- `get_tender_history`, `search_attributions`, `icp_score` → Business
- Check tier at Settings > Facturation
- Upgrade at https://prospkt.fr/pricing

### "Query blocked" (query_data)
- Only SELECT queries allowed (no INSERT/UPDATE/DELETE)
- Only whitelisted tables (mv_company_basic, signals, boamp, etc.)
- System tables (pg_shadow, information_schema) are blocked
- 5-second timeout — add WHERE filters for large tables
- LIMIT enforced: Pro=100 rows, Business=500 rows

### Tool returns empty results
- Broaden search criteria (fewer filters)
- Check NAF code with `lookup_naf_tree` — don't guess
- Check CPV code with `lookup_cpv_tree`
- Use `describe_data` to verify column names for `query_data`

### "Company not found"
- Verify SIREN is exactly 9 digits
- Company may be closed (etatAdministratifUniteLegale = 'F')
- Try searching by name instead of SIREN

### MCP server won't start
- Check Node.js version (18+ required)
- Verify `PROSPKT_API_KEY` env variable is set
- Check `DATABASE_URL` if running locally
- Try `npx -y @prospkt/mcp-server` to get latest version

## How MCP Works

MCP (Model Context Protocol) is a standard created by Anthropic for connecting AI models to external data sources:

1. **Your AI** (Claude, Cursor...) connects to the MCP server at startup
2. The server exposes **tools** that the AI can call
3. When you ask a question, the AI decides which tool(s) to call
4. The tool queries the Prospkt database and returns structured JSON
5. The AI formats the response for you

```
User → AI → MCP Server → Prospkt Database
                ↑              ↓
         Tool calls      Structured JSON
```

Two transport modes:
- **stdio** (default): MCP server runs as a child process of your AI client
- **SSE**: MCP server runs as an HTTP service, useful for multi-client setups
