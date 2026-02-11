# Prototypes
Clickable HTML prototypes for product discovery and design exploration.

Each prototype is a self-contained HTML file viewable via GitHub Pages.

---

## Glossary Support

**Prototype:** [glossary-support.prototype.html](https://bougie153.github.io/Prototypes/glossary-support.prototype.html)

### Problem

Multi-lingual enterprise customers can't manage business-specific terminology at scale. Overloading company descriptions with glossaries doesn't scale past ~20 terms, and custom instructions don't reliably adhere. This blocks multi-lingual expansion for some of Ada's largest accounts.

Beyond translation, customers also need a way to provide **business-specific definitions** that add critical context to the AI Agent's context window for improved retrieval and reasoning. Terms like "cooking" (NYT), "deposit" (Luno), and "shift swap" (Quinyx) are ambiguous without domain context — the agent misinterprets them, gives generic responses, or asks unnecessary clarification questions. There is no scalable mechanism today for customers to ground the AI Agent's understanding in their business vocabulary.

### Customer Cases

| Customer | Need | Capability |
|----------|------|------------|
| Amway | "IBO" / "ABO" must not be translated across 50+ markets | Do-not-translate |
| Luno | "deposit" means crypto deposit, not bank deposit | Business definition |
| NYT | "cooking" refers to NYT Cooking product, not the activity | Business definition |
| Quinyx | "shift swap" is a specific product feature, not generic scheduling | Business definition |
| IKEA | Product names (KALLAX, MALM) must stay untranslated | Do-not-translate |
| Wealthsimple | "TFSA", "RRSP" must remain in source language | Do-not-translate |
| Tier 1 Bank | Financial terms require exact translations per region | Translation pair |
| Doctolib | Medical terms must not trigger generic language detection | Language detection exclusion |
| Multi-market retailers | Brand names detected as foreign language, causing wrong routing | Language detection exclusion |

### Required Capabilities (P0–P2)

- **P0 — Do-not-translate (DNT):** Mark terms that must never be translated (brand names, acronyms, product names)
- **P0 — Translation pairs:** Provide exact source → target translations for specific terms per language
- **P1 — Business definitions:** Provide domain-specific definitions that ground the AI Agent's reasoning (e.g., "cooking" = NYT Cooking subscription product)
- **P1 — Language detection exclusion:** Exclude terms from language detection to prevent misrouting
- **P2 — Bulk import/export:** CSV-based management for customers with 100+ terms
- **P2 — Regional scoping:** Apply glossary rules per region/market

### Prototype Screens

1. **Glossary** — Term list with multi-capability badges, search, and filter
2. **Term Detail** — Drill-through for DNT/translation terms showing languages and regions
3. **Term Definition** — Drill-through for business definitions with adherence testing
4. **Add Term** — Multi-capability form with conditional fields per type
5. **Import / Export** — CSV import with per-capability row schema
6. **Regions** — Regional scoping and market configuration
