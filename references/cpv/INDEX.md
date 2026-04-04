# CPV Code Hierarchy — Public Procurement Vocabulary

Navigate by division for detailed codes.

## Divisions

| Code | Label | File |
|------|-------|------|
| 30 | Machines de bureau et informatique | [divisions/30.md](divisions/30.md) |
| 33 | Équipements médicaux | [divisions/33.md](divisions/33.md) |
| 44 | Construction matériaux | [divisions/44.md](divisions/44.md) |
| 45 | Travaux de construction | [divisions/45.md](divisions/45.md) |
| 48 | Logiciels et systèmes informatiques | [divisions/48.md](divisions/48.md) |
| 50 | Réparation et maintenance | [divisions/50.md](divisions/50.md) |
| 71 | Architecture et ingénierie | [divisions/71.md](divisions/71.md) |
| 72 | Services informatiques | [divisions/72.md](divisions/72.md) |
| 79 | Services aux entreprises | [divisions/79.md](divisions/79.md) |
| 85 | Santé et action sociale | [divisions/85.md](divisions/85.md) |
| 90 | Environnement | [divisions/90.md](divisions/90.md) |

Use `lookup_cpv_tree` for interactive navigation of ALL divisions.

## Usage
```
→ search_tenders(cpv_codes=["72"])          // All IT services
→ search_tenders(cpv_codes=["72200000"])    // Programming only
→ lookup_cpv_tree(level="divisions")         // Browse all
→ lookup_cpv_tree(level="codes", prefix="72") // Drill into IT
```
