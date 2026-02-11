# Prototypes
Clickable HTML prototypes for product discovery and design exploration.

Each prototype is a self-contained HTML file viewable via GitHub Pages.

---

## Handoffs NextGen — Decision-Time Visibility (Candidate A)

**Prototype:** [handoffs-decision-visibility.prototype.html](https://bougie153.github.io/Prototypes/handoffs-decision-visibility.prototype.html)

**Project:** [Handoffs NextGen - Discovery Only](https://linear.app/adasupport/project/handoffs-nextgen-discovery-only-7958fcbfb5b5)

### Problem

Handoff behaviour is fragmented across blocks, channel-specific implementations, and Solutions-owned GitHub configuration with limited visibility. AI Managers can't understand what will happen before they publish changes to handoff routing — leading to misroutes, broken escalations, and slow iteration.

### Goal

Help customers understand *what will happen before they publish changes*. Preview effective routing and prerequisite logic, and simulate outcomes for representative inputs with "because…" explanations.

### Prototype Screens

| Screen | What it shows |
|--------|---------------|
| **Handoffs** | Overview of all integrations with metrics, failure rates, and a pending changes banner |
| **Routing Rules** | Zendesk Chat detail with two views: an AI-generated visual flowchart (from Napkin Notes idea) and a rules table with conditions, destinations, prerequisites, and edit status |
| **Route Preview** | Interactive simulator — select conversation attributes (language, country, tier, topic, email) and see the predicted destination with a step-by-step "because..." explanation trail. Supports testing with or without pending changes |
| **Pending Changes** | Diff-style review of 3 unpublished edits with before/after values, estimated routing impact per queue, and a publish confirmation modal |

### Key interactions to try

- Click "Preview route" on Route Preview with default values (English, Canada, VIP) to see routing to NA Priority Queue
- Toggle "With pending changes" and set email to "No" — the VIP prerequisite fails and routing changes
- Set topic to "Billing" with pending changes — the new billing rule kicks in
- On Routing Rules, nodes with gold outlines indicate pending changes (matching the Napkin Notes concept)

---

## Handoffs NextGen — Run-Time Visibility (Candidate B)

**Prototype:** [handoffs-runtime-visibility.prototype.html](https://bougie153.github.io/Prototypes/handoffs-runtime-visibility.prototype.html)

**Project:** [Handoffs NextGen - Discovery Only](https://linear.app/adasupport/project/handoffs-nextgen-discovery-only-7958fcbfb5b5)

### Problem

When handoffs fail, AI Managers have no way to diagnose what happened. Failures are black boxes requiring Datadog log access and Ada support involvement to troubleshoot. There's no visibility into handoff status, failure reasons, or who changed configuration and when.

### Goal

Help customers diagnose *what happened after a handoff ran*. Surface handoff status, destination, and timestamps. Expose customer-meaningful failure reasons. Provide audit-only change history.

### Prototype Screens

| Screen | What it shows |
|--------|---------------|
| **Handoff Activity** | Dashboard with metrics (total handoffs, success rate, failures, latency), filters by status/channel/destination, and a table of recent handoff events |
| **Handoff Detail (Success)** | Execution trace showing routing decision, payload sent, API response, and ticket created |
| **Handoff Detail (Failed)** | Execution trace highlighting where it broke, plain-language failure reason, and suggested remediation actions |
| **Alerts** | Active failure patterns grouped by root cause with occurrence counts and "how to fix" guidance |
| **Change History** | Audit log of handoff configuration changes with who, what, when, and before/after diffs |

### Key interactions to try

- On Handoff Activity, click a red "Failed" row (e.g. `conv_6c12bf04`) to see the full failure trace with root cause and remediation steps
- On the failed detail screen, walk the execution trace step-by-step: trigger → routing → payload → API failure (401) → retry → refresh token expired → handoff failed
- Click a green "Succeeded" row (e.g. `conv_8f3a2e91`) to compare — see the happy path with ticket created and latency
- On Alerts, click any active alert card to open the detail modal with "what happened", "how to fix" steps, and affected conversations
- Switch between "Active Alerts" and "Resolved" tabs to see historical patterns
- On Change History, click "View diff" on any row to see before/after changes — note how the "Removed order_id" change correlates with the "Payload rejected" alert on the Alerts screen

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
