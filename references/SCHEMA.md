# Prospkt MCP — Database Schema Reference

Use `describe_data` tool for the most up-to-date schema. This is a reference for `query_data` SQL.

## Tables Available for SQL (query_data tool)

### mv_company_basic (12M+ rows)
Core company data from INSEE SIRENE + LinkedIn.

| Column | Type | Description |
|--------|------|-------------|
| siren | VARCHAR(9) | Primary key (9-digit) |
| nomAffichage | TEXT | Display company name |
| surnomAffichage | TEXT | Alternative/trade name |
| categorieJuridiqueUniteLegale | VARCHAR(4) | Legal form code (5710=SAS) |
| dateCreationUniteLegale | DATE | Creation date |
| trancheEffectifsUniteLegale | VARCHAR(2) | Size bracket code |
| codeAPE | VARCHAR(6) | NAF/APE activity code |
| descriptionActiviteUniteLegale | TEXT | Activity description |
| communesiege | TEXT | HQ city |
| adresseCompleteSiege | TEXT | Full HQ address (includes postal code) |
| chiffreAffaire | BIGINT | Revenue (euros) |
| resultatNet | BIGINT | Net income |
| homepage_url | TEXT | Website |
| linkedin_url | TEXT | LinkedIn page |
| linkedin_industry | TEXT | Industry from LinkedIn |
| linkedin_company_size | TEXT | Size from LinkedIn |
| linkedin_follower_count | INT | LinkedIn followers |
| has_email | BOOLEAN | Has enriched emails |
| has_phone | BOOLEAN | Has enriched phones |
| rating | FLOAT | Google rating |
| slug | TEXT | URL slug on prospkt.fr |

### signals (2.5M+ rows)
Business signals detected from various sources.

| Column | Type | Description |
|--------|------|-------------|
| id | BIGINT | Primary key |
| signal_type | VARCHAR | Type (hr.hiring_spike, financial.fundraising, etc.) |
| source | VARCHAR | Data source |
| subject_siren | VARCHAR(9) | Company SIREN |
| title | TEXT | Signal title |
| description | TEXT | Signal description |
| payload | JSONB | Raw signal data |
| occurred_at | TIMESTAMP | When the signal occurred |

### boamp (230K+ rows)
Public procurement tenders.

| Column | Type | Description |
|--------|------|-------------|
| idPrincipal | VARCHAR | Primary ID |
| objet | VARCHAR | Tender subject/title |
| nomAcheteur | VARCHAR | Buyer name |
| cpvCode | JSONB | CPV codes (array) |
| codeDepartement | JSONB | Department codes (array) |
| dateParution | DATE | Publication date |
| dateLimiteReponse | DATE | Response deadline |
| estimatedValue | NUMERIC | Estimated amount |
| typeMarche | VARCHAR | Market type |
| urlAvis | VARCHAR | Notice URL |

### contract_attributions + contract_attribution_lots
Public contract awards. JOIN lots for winner SIREN.

| Column (attributions) | Type | Description |
|--------|------|-------------|
| id | BIGINT | Primary key |
| objet | TEXT | Contract subject |
| nom_acheteur | TEXT | Buyer name |
| codecpv | VARCHAR | CPV code |
| montant | NUMERIC | Amount |
| date_notification | DATE | Award date |

| Column (lots) | Type | Description |
|--------|------|-------------|
| titulaire_siren | VARCHAR | Winner SIREN |
| titulaire_nom | TEXT | Winner name |
| montant_ht | NUMERIC | Lot amount |

### dirigeants
Company directors from INPI.

| Column | Type | Description |
|--------|------|-------------|
| siren | VARCHAR(9) | Company SIREN |
| nom | VARCHAR | Last name |
| prenoms | JSON | First names |
| role | VARCHAR | Role (President, etc.) |
| dateDeNaissance | VARCHAR | Date of birth |

### bilans
Annual financial filings.

| Column | Type | Description |
|--------|------|-------------|
| siren | VARCHAR(9) | Company SIREN |
| dateClotureExercice | DATE | Fiscal year end |
| chiffreAffaire | BIGINT | Annual revenue |
| resultatNet | BIGINT | Net income |
| ebe | BIGINT | EBITDA |
| tauxEndettement | FLOAT | Debt ratio |

### unitelegale (source table — NOT in mv_company_basic)
Contains fields like `etatAdministratifUniteLegale` ('A' = active, 'F' = closed).
To filter active companies, JOIN: `JOIN unitelegale u ON u.siren = basic.siren WHERE u."etatAdministratifUniteLegale" = 'A'`
Note: mv_company_basic does NOT contain this column — you must JOIN unitelegale.

### linkedin_companies, linkedin_jobs, linkedin_profiles, etablissement, avisBodacc
Additional tables — use `describe_data` for full column list.
