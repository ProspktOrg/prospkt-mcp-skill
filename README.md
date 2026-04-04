# Prospkt MCP Skill — French B2B Intelligence for AI

[![Agent Skills](https://img.shields.io/badge/Agent_Skills-compatible-blue)](https://agentskills.io)

> **Agent Skill** for installing and using the [Prospkt](https://prospkt.fr) MCP Server.
> Gives your AI direct access to **12M+ French companies**, business signals, public tenders, LinkedIn data, and contact enrichment.

## 🚀 Quick Install

Tell your AI:

```
Install the Prospkt MCP skill from https://github.com/ProspktOrg/prospkt-mcp-skill
```

Your AI will:
1. Clone this repo
2. Guide you to create an API key at [app.prospkt.fr](https://app.prospkt.fr/pages/account-settings-api-keys)
3. Configure the MCP server for your environment
4. Start using 16 intelligence tools

## 🤖 What You Can Do

Once installed, ask your AI things like:

- *"Find IT consulting companies in Paris with 50+ employees"*
- *"What companies in Lyon had hiring spikes this month?"*
- *"Show me open public tenders for IT services over 100K€"*
- *"Score this company against my ICP"*
- *"Find the professional email for Jean Dupont at Acme Corp"*

## 📦 What's Included

```
prospkt-mcp-skill/
├── SKILL.md                    # Main instructions (139 lines)
├── references/
│   ├── naf/                    # NAF/APE code hierarchy
│   │   ├── INDEX.md            # 21 sections overview
│   │   ├── sections/           # Per-section files (A-U)
│   │   └── divisions/          # Detailed codes per division
│   ├── cpv/                    # CPV procurement codes
│   │   ├── INDEX.md
│   │   └── divisions/          # Per-division files
│   ├── USE-CASES.md            # 7 detailed examples
│   ├── SCHEMA.md               # Database schema for SQL queries
│   └── TROUBLESHOOTING.md      # Common errors & how MCP works
├── scripts/                    # (future: automation scripts)
└── assets/                     # (future: templates, resources)
```

## 🔧 16 Tools

| Tool | Tier | Credits | Description |
|------|------|---------|-------------|
| `search_companies` | Free | 0 | Search 12M+ companies by NAF, city, size, revenue |
| `search_signals` | Free | 0 | Business signals (hiring, fundraising, legal) |
| `search_tenders` | Free | 0 | Public tenders by CPV, department, budget |
| `describe_data` | Free | 0 | Schema documentation |
| `lookup_naf_tree` | Free | 0 | NAF code hierarchy navigation |
| `lookup_cpv_tree` | Free | 0 | CPV code hierarchy navigation |
| `list_signal_types` | Free | 0 | Catalog of 30 signal types |
| `get_company` | Starter | 1 | Full company profile |
| `get_company_signals` | Starter | 1 | Signal history |
| `enrich_email` | Pro | 1 | Find professional email |
| `enrich_phone` | Pro | 10 | Find phone number |
| `get_company_linkedin` | Pro | 1 | LinkedIn data + jobs |
| `query_data` | Pro | 1 | Custom SQL queries |
| `get_tender_history` | Business | 1 | Contract awards |
| `search_attributions` | Business | 0 | Search who won contracts |
| `icp_score` | Business | 1 | Score against your ICP |

## 💰 Pricing

| Tier | Price | Rate Limit |
|------|-------|-----------|
| Starter | Free | 100 req/hr |
| Pro | 250€/month | 500 req/hr |
| Business | 400€/month | 2000 req/hr |

## 📋 Requirements

- An AI assistant that supports [Agent Skills](https://agentskills.io) (Claude Code, Hermes, etc.)
- A Prospkt account — [Sign up at prospkt.fr](https://prospkt.fr)
- Node.js 18+

## 🔗 Links

- [Prospkt SaaS](https://prospkt.fr)
- [API Key Management](https://app.prospkt.fr/pages/account-settings-api-keys)
- [Agent Skills Specification](https://agentskills.io/specification)

## License

Proprietary — see SKILL.md for details.
