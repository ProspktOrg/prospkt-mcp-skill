---
name: prospkt-mcp-server
description: Install and use the Prospkt MCP Server to access French B2B intelligence — 12M+ companies, business signals, public tenders, LinkedIn data, and contact enrichment — directly from Claude, Cursor, or any MCP-compatible AI. Use when the user wants to search French companies, find business signals, explore tenders, enrich contacts, or set up B2B prospecting workflows.
license: Proprietary
compatibility: Requires Node.js 18+ and an active Prospkt account (https://prospkt.fr)
metadata:
  author: prospkt
  version: "2.1.0"
  category: b2b-intelligence
---

# Prospkt MCP Server — French B2B Intelligence for AI

Prospkt MCP gives your AI direct access to the most comprehensive French B2B database:
- **12M+ companies** (INSEE SIRENE + LinkedIn)
- **2.5M+ business signals** (hiring, fundraising, legal changes...)
- **230K+ public tenders** (BOAMP/TED)
- **330K+ LinkedIn profiles** matched to SIREN
- **274K+ contract attributions**

## Setup

### Step 0: Check if Prospkt MCP is already installed

Before doing anything, verify if the MCP server is already configured:

**For Claude Desktop/Code**: check if `~/.config/claude/claude_desktop_config.json` contains a `"prospkt"` entry.
**For Cursor**: check if `.cursor/mcp.json` contains a `"prospkt"` entry.

If it's already configured, try calling `list_signal_types` to verify the connection works.
If it works → skip to "Available Tools" below. If it fails or isn't configured → continue with Step 1.

### Step 1: Get Your API Key

1. Go to **https://app.prospkt.fr/pages/account-settings-api-keys**
2. Click **"Nouvelle clé"**
3. Enter a name (e.g., "Claude Desktop", "Cursor")
4. **IMPORTANT**: Copy the `psk_...` key immediately — it's shown only once!

> **Interactive flow**: Ask the user: "Do you already have a Prospkt API key (starts with psk_)?"
> - If yes → ask them to paste it
> - If no → guide them to https://app.prospkt.fr/pages/account-settings-api-keys, wait, then ask them to paste the key

### Step 2: Configure Your AI

**Claude Desktop / Claude Code** — add to `~/.config/claude/claude_desktop_config.json`:
```json
{
  "mcpServers": {
    "prospkt": {
      "command": "npx",
      "args": ["-y", "@prospkt/mcp-server"],
      "env": { "PROSPKT_API_KEY": "psk_YOUR_KEY_HERE" }
    }
  }
}
```

**Cursor / Windsurf** — add to MCP settings (`.cursor/mcp.json` or IDE settings):
```json
{
  "prospkt": {
    "command": "npx",
    "args": ["-y", "@prospkt/mcp-server"],
    "env": { "PROSPKT_API_KEY": "psk_YOUR_KEY_HERE" }
  }
}
```

**SSE (HTTP)** — for custom apps/n8n:
```bash
npm install -g @prospkt/mcp-server
PROSPKT_API_KEY=psk_YOUR_KEY MCP_TRANSPORT=sse MCP_PORT=3200 prospkt-mcp
# Connect via http://localhost:3200/sse
```

### Step 3: Verify Installation

After configuration, **restart your AI client** and verify:
1. Call `list_signal_types` — should return 30 signal types
2. Call `search_companies` with `{"limit": 1}` — should return 1 company

If you get "Invalid API key", double-check the key starts with `psk_` and has no extra spaces.

## Available Tools (16)

### Free (all tiers, 0 credits)
| Tool | Description |
|------|-------------|
| `search_companies` | Search 12M+ companies by NAF, city, size, revenue |
| `search_signals` | Search business signals (hiring, fundraising, legal) |
| `search_tenders` | Search public tenders by CPV, department, budget |
| `describe_data` | Schema documentation for all views/columns |
| `lookup_naf_tree` | Navigate NAF/APE activity code hierarchy |
| `lookup_cpv_tree` | Navigate CPV procurement vocabulary |
| `list_signal_types` | Catalog of 30 signal types |

### Starter (1 credit each)
| Tool | Description |
|------|-------------|
| `get_company` | Full company profile (directors, financials, establishments) |
| `get_company_signals` | Signal history for a company |

### Pro (requires Pro plan)
| Tool | Credits | Description |
|------|---------|-------------|
| `enrich_email` | 1 | Find professional email |
| `enrich_phone` | 10 | Find phone number |
| `get_company_linkedin` | 1 | LinkedIn data + job postings |
| `query_data` | 1/query | Custom SQL on read-only views |

### Business (requires Business plan)
| Tool | Credits | Description |
|------|---------|-------------|
| `get_tender_history` | 1 | Contract attribution history |
| `search_attributions` | 0 | Search contract awards |
| `icp_score` | 1 | Score companies against your ICP |

## Pricing

| Tier | Price | Rate Limit | Keys |
|------|-------|-----------|------|
| Starter | Free | 100/hr | 2 |
| Pro | 250€/mois | 500/hr | 2 |
| Business | 400€/mois | 2000/hr | 5 |

## Use Cases

See [references/USE-CASES.md](references/USE-CASES.md) for 7 detailed examples:
1. **Prospecting** — Find target companies by industry/size/location
2. **Market intelligence** — Competitive landscape analysis
3. **Business signals** — Real-time intent detection
4. **Public tenders** — Find procurement opportunities
5. **ICP scoring** — Qualify leads against your profile
6. **Custom SQL** — Complex joins across all tables
7. **Contact enrichment** — Find emails and phones

## Reference Data (loaded on demand)

Code hierarchies are organized as directories — only the relevant branch is loaded:

- **NAF codes**: [references/naf/INDEX.md](references/naf/INDEX.md) → sections/ → divisions/
  - Example: [references/naf/sections/J.md](references/naf/sections/J.md) (IT & Communication)
  - Example: [references/naf/divisions/62.md](references/naf/divisions/62.md) (Programmation)
- **CPV codes**: [references/cpv/INDEX.md](references/cpv/INDEX.md) → divisions/
  - Example: [references/cpv/divisions/72.md](references/cpv/divisions/72.md) (IT services)
- **DB Schema**: [references/SCHEMA.md](references/SCHEMA.md) — Column docs for query_data
- **Troubleshooting**: [references/TROUBLESHOOTING.md](references/TROUBLESHOOTING.md) — Errors & how MCP works

## Quick Reference

**Department codes**: 75=Paris, 69=Lyon, 13=Marseille, 31=Toulouse, 33=Bordeaux, 59=Lille, 92/93/94=Petite couronne

**Size codes**: 11=10-19, 12=20-49, 21=50-99, 22=100-199, 31=200-249, 32=250-499, 41=500-999, 51=2000-4999, 53=10000+

**Legal forms**: 5710=SAS, 5720=SASU, 5499=SARL, 5498=EURL, 5599=SA, 1000=EI

**Tips**: Use `lookup_naf_tree` before searching (don't guess NAF codes). Use `describe_data` before `query_data`. Combine search → get_company → signals for full intelligence.
