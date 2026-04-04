# Prospkt MCP — Use Cases

## 1. Prospecting — Find Target Companies

**Prompt**: "Find IT consulting companies in Paris with 50+ employees and revenue over 5M€"

```
→ search_companies(naf_codes=["62.01Z"], cities=["Paris"], size_min="21", revenue_min=5000000)
```

**Then dig deeper** on each result:
```
→ get_company(siren="443061841")          // Full profile
→ get_company_linkedin(siren="443061841") // LinkedIn + jobs
→ get_company_signals(siren="443061841")  // Recent signals
```

## 2. Market Intelligence — Competitive Landscape

**Prompt**: "SAS companies in cybersecurity in Île-de-France created after 2020 with LinkedIn"

```
→ search_companies(
    naf_codes=["62.01Z", "62.02A"],
    departments=["75", "92", "93", "94", "91", "77", "78", "95"],
    created_after="2020-01-01",
    has_linkedin=true,
    legal_forms=["5710"]
  )
```

## 3. Business Signals — Real-Time Intent

**Prompt**: "Companies in Lyon with hiring spikes or fundraising in the last 30 days"

```
→ search_signals(
    signal_types=["hr.hiring_spike", "financial.fundraising"],
    departments=["69"],
    since="2024-03-01"
  )
```

**Follow-up**: Get company details for each signal match.

## 4. Public Tenders — Find Opportunities

**Prompt**: "Open IT service tenders over 100K€"

```
→ search_tenders(cpv_codes=["72"], status="open", amount_min=100000, sort_by="amount", sort_order="desc")
```

**Check competitors** who won similar contracts:
```
→ search_attributions(cpv_codes=["72"], min_amount=100000, since="2023-01-01")
```

## 5. ICP Scoring — Qualify Leads

**Prompt**: "Score company 443061841 against my ICP: tech SAS, 50+ employees, Paris, LinkedIn"

```
→ icp_score(siren="443061841", naf_codes=["62.01Z"], departments=["75"], size_min="21", legal_forms=["5710"], has_linkedin=true)
```

Returns 0-100 score with grade (A/B/C/D) and breakdown by criteria.

## 6. Custom SQL — Power Queries

**Prompt**: "Companies with the highest revenue growth between their last 2 fiscal years"

```
→ query_data(sql="
    SELECT b1.siren, c.\"nomAffichage\",
           b1.\"chiffreAffaire\" as ca_recent,
           b2.\"chiffreAffaire\" as ca_precedent,
           ROUND((b1.\"chiffreAffaire\"::numeric - b2.\"chiffreAffaire\"::numeric) / NULLIF(b2.\"chiffreAffaire\"::numeric, 0) * 100, 1) as growth_pct
    FROM bilans b1
    JOIN bilans b2 ON b1.siren = b2.siren
    JOIN mv_company_basic c ON c.siren = b1.siren
    WHERE b1.\"dateClotureExercice\" > b2.\"dateClotureExercice\"
      AND b1.\"chiffreAffaire\" > 0 AND b2.\"chiffreAffaire\" > 0
    ORDER BY growth_pct DESC LIMIT 20
  ")
```

**Available tables for SQL**: mv_company_basic, mv_company_size, signals, boamp, dirigeants, bilans, etablissement, linkedin_companies, linkedin_jobs, linkedin_profiles, contract_attributions, contract_attribution_lots, avisBodacc

## 7. Contact Enrichment

**Prompt**: "Find the email for Jean Dupont at Acme Corp"

```
→ enrich_email(first_name="Jean", last_name="Dupont", company_name="Acme Corp")
→ enrich_phone(first_name="Jean", last_name="Dupont", company_name="Acme Corp")
```

Note: Enrichment returns cached results instantly or queues a waterfall lookup (30-120s).
