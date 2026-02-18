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

## Glossary Support — P0 Capabilities

**Prototype:** [glossary-support-p0.prototype.html](https://bougie153.github.io/Prototypes/glossary-support-p0.prototype.html)

**Project:** [Glossary Support - Delivery](https://linear.app/adasupport/project/glossary-support-delivery-985f65635f44)

### Problem

Multi-lingual enterprise customers can't ensure terminology accuracy across markets. The agent lacks a scalable way to inject business-specific term knowledge at inference time — resulting in incorrect translations (e.g., "rides" → "carousel" instead of "corsa" for Dott), brand names being translated when they shouldn't be (ShapingNewTomorrow product names), and language detection failures (OnTop brand name triggering wrong language switch). Amway's European migration is stalled without this.

### Goal

P0: Ship exact translation pairs and do-not-translate terms with dynamic retrieval — so AI Managers can define business terminology at scale and the agent reliably applies the right translations per turn, without context window bloat.

### Prototype Screens

| Screen | What it shows |
|--------|---------------|
| **Glossary** | Term list with metrics (153 terms, 20 languages, 3 conflicts), search/filter by language and type, and per-row actions |
| **Add term** | Form to create a new term — toggle between Translation pair and Do-not-translate, with per-language translation rows |
| **Edit term** | Multi-language term editor with inline conflict warning and delete |
| **Import CSV** | 3-step bulk import: upload → column mapping → validation preview with ready/flagged/duplicate row counts |
| **Conflicts** | Detected conflicts between glossary entries (critical) and between glossary and company description (warning) with resolve modal |
| **Conversations** | Conversation list filtered by glossary usage, with a conversation detail view showing exactly which terms were retrieved and highlighted in the transcript |

### Key interactions to try

- On **Glossary**, click any term row to open Edit term — notice the Spanish conflict warning inline on the IBO term with a link to the Conflicts screen
- Click **Add term** and toggle between "Translation pair" and "Do not translate" — the form fields change and a DNT info banner appears explaining language detection exclusion
- Click **Import CSV** and walk through all 3 steps: upload → column mapping (auto-detected) → validation summary with 148 ready, 3 duplicates, 2 flagged rows
- On **Conflicts**, click "Resolve" on the IBO → Spanish conflict to choose which translation to keep
- On **Conversations**, select a conversation from the left list — see the glossary panel showing which terms were applied, and the highlighted terms in the transcript (gold highlighting)

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

### Key interactions to try

- On the **Glossary** screen, notice how terms like "IBO / ABO" show multiple capability badges (Translation + Definition + Lang exclusion) — click it to drill into the **Term Detail** screen with 20 language mappings and regional overrides (e.g., German in Austria differs from Germany)
- Click **"cooking"** or **"deposit"** or **"shift swap"** to open the **Term Definition** screen — these are the P1 business definition use cases where ambiguous terms need domain context
- On the **Term Definition** screen, review the side-by-side **adherence test**: "Without glossary" the agent asks which cooking service the customer means; "With glossary" it immediately recognizes NYT Cooking and takes action
- Click **Add term** and see how the four capability checkboxes (DNT, Translation pair, Definition, Lang exclusion) each reveal different required fields — a single term can have all four enabled simultaneously
- On the **Definition** capability card in Add Term, note the hint: "This context is added to the AI agent's context window for improved retrieval and reasoning"
- Navigate to **Import / Export**, then click the upload zone to jump to the **Preview** tab — notice the per-capability row format (e.g., "Dott" has separate rows for DNT and lang exclusion), warnings for existing terms, and an error row for a missing translation value
- On **Regions**, click any region card (e.g., "Europe — Western") to open the detail modal with locale codes — then review the overrides table below showing how "IBO / ABO" translates differently in Belgium vs France
