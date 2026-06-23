---
name: competitive-analysis
description: Research and output a structured competitive analysis for a startup being evaluated in due diligence. Produces a rich markdown report with a competitor table, feature comparison, and positioning summary. Use when asked for a competitive overview, competitive landscape, or comp analysis of a startup.
---

# Competitive Analysis Skill (Co Work / Markdown Output)

Produce a structured competitive landscape analysis for a startup being evaluated in a DD process.
Pull from every available knowledge source — internal memory (CRM, meetings, email, Drive) first, then external data.

## Input

Invoked as `/competitive-analysis <Startup Name>` or with a company name / URL in args.

## Workflow

Work through each step in order. Give the user a brief one-line status update as each step completes.

---

### Step 1 — Internal knowledge sweep (run all in parallel)

Before touching any external source, pull everything the firm already knows. This internal intelligence takes precedence and will surface context no database has.

**1a. Affinity CRM**
- `mcp__Affinity__search_companies` with the startup name to find the CRM record.
- If found, call `mcp__Affinity__get_notes_for_entity` (entity_type=1) to retrieve all analyst notes.
- Also call `mcp__Affinity__get_company_list_entries` to see which pipeline lists it sits on and what stage it's at.
- Run `mcp__Affinity__semantic_search` with a description like _"competitors to [startup name] in [inferred market]"_ to surface any related companies already tracked in the CRM.
- Repeat `get_notes_for_entity` for any CRM-tracked competitors found.

**1b. Granola meeting notes**
- `mcp__Granola__query_granola_meetings` with queries:
  - _"[startup name] — what did the founders say about their competitors?"_
  - _"[startup name] — product, market, differentiation"_
  - _"[startup name] — risks, concerns, competitive threats"_
- If relevant meeting IDs surface, call `mcp__Granola__get_meetings` for the full summary and private notes.
- Preserve all inline citation links `[[n]](url)` from Granola responses — include them verbatim in the final report.

**1c. Email (Superhuman)**
- `mcp__Superhuman__query_email_and_calendar` with:
  - _"What do we know about [startup name] from emails?"_
  - _"Any analyst reports, competitor comparisons, or market research about [startup name] or its market?"_
- `mcp__Superhuman__list_threads` with `subject_contains` or `body_contains` set to the startup name to find pitch emails, follow-ups, forwarded research.

**1d. Google Drive**
- `mcp__Google_Drive__search_files` with `fullText contains '[startup name]'` to find any existing memos, decks, or prior analyses.
- For each relevant file (investment memos, competitive analyses, pitch decks), call `mcp__Google_Drive__read_file_content`.
- Specifically look for any prior competitor overview files (titles matching "Competitor", "Competitive", "Landscape").

Consolidate everything from 1a–1d into an internal intelligence summary before proceeding. Note which competitors were already known internally and any analyst views already captured.

---

### Step 2 — Resolve the target company via Specter

- `mcp__Specter__find_company` with the startup name or URL.
- Call `mcp__Specter__get_company_profile` + `mcp__Specter__get_company_intelligence` on the resolved ID.

Extract:
- Full name, website, HQ country (ISO-2), headcount, founding year
- Product description (what it does, for whom, core value prop)
- Last funding round, amount, lead investors
- Primary product category / market segment

---

### Step 3 — Identify competitors

Triangulate from three sources:

**3a. Specter**
- `mcp__Specter__find_similar_companies` on the resolved company ID.

**3b. Evertrace talent signals**
- `mcp__Evertrace__search_companies` for the startup name to get an entity ID.
- Use `mcp__Evertrace__list_signals` with `past_companies` set to that entity ID — this surfaces people who previously worked there or at competitors, revealing the talent ecosystem and which companies share talent pools.
- Also search for 2–3 named direct competitors from step 3a to map cross-company talent movement.

**3c. Web search**
- `WebSearch`: _"[startup name] competitors 2024 2025"_
- `WebSearch`: _"best alternatives to [startup name]"_
- `WebSearch`: _"[market category] landscape startups [year]"_

Merge all sources. Prioritise companies that appear in multiple sources or were already known from internal intel (Step 1). Aim for 10–20 competitors across tiers:
- **Direct** — same product, same buyer, same stage
- **Adjacent** — same problem, different approach or segment
- **Incumbent** — established players being displaced

---

### Step 4 — Research each competitor

For each competitor:
- `mcp__Specter__find_company` → `mcp__Specter__get_company_profile` for core data.
- `mcp__Affinity__search_companies` — check if already in the CRM; if so pull notes.
- `mcp__Granola__query_granola_meetings` — check if any calls with this competitor exist.
- `mcp__Superhuman__query_email_and_calendar` — check for any email intel on this competitor.
- Fill remaining gaps with `WebSearch` or `WebFetch` of their homepage/product page.

For each competitor collect:
- URL, HQ (ISO-2), headcount (number), founding year (number)
- Product description (≤120 chars)
- Last funding string and investors
- **Key Difference** — how they differ *from the target startup* (≤150 chars, specific not vague — e.g. "focuses on supplier discovery only; lacks internal spend classification" not "less advanced")
- Tier: Direct / Adjacent / Incumbent

---

### Step 5 — Determine feature dimensions

Analyse the target startup's product and pick 5–8 binary dimensions that matter most for differentiation in this specific market. Label them clearly.

Examples by domain:
- Procurement: Supplier data & discovery | Spend analytics | AI automation | Workflow automation | Real-time insights | Supplier enrichment
- Robotics safety: HW vs SW | Robot type | Dynamic safety | Human/AMR interaction | Human behavior prediction
- Voice AI: S2S model | Telephony/SIP | Self-hosting | Open source | Inbound calls | Outbound calls | Agent builder
- AI security: Category sub-segment | Open source | Runtime detection | Red-teaming | NHI/secrets

---

### Step 6 — Build the competitor table

Output a markdown table:

| Company | URL | Location | Headcount | Founded | Product Description | Last Funding | Investors | Key Difference | Tier | [Feature 1] | [Feature 2] | … |

Rules:
- **First data row = target startup** (bold the name, mark it as `← target`)
- Sort remaining rows: Direct first, then Adjacent, then Incumbents
- `✓` = confirmed present · `(✓)` = partial/limited · `–` = absent · `?` = unknown
- Flag rows where data came from internal sources with a ✦ symbol and explain in the notes section

---

### Step 7 — Narrative analysis

Write four sections (3–5 sentences each):

**Market Overview** — Market size, growth, key dynamics. What's driving activity in this space right now.

**Target Company Positioning** — Where the startup sits. What it does distinctly well. Who it most directly competes with and how it differentiates.

**Key Competitive Threats** — Top 3 companies posing the most risk. Why each is a threat. Any context from internal intel (e.g. "founders mentioned X in the 12 May call").

**Whitespace & Moat** — Where the startup has defensible differentiation or an underserved niche. Any signals from talent data or meeting notes that reinforce or challenge the moat thesis.

---

### Step 8 — Internal intelligence addendum

If Step 1 surfaced meaningful proprietary context (analyst notes, founder call quotes, email threads, prior memos), include a dedicated section:

**Internal Intelligence**
- Summarise key observations from Affinity notes, Granola meeting transcripts, email threads, and Drive documents.
- Quote directly where useful (with Granola citation links preserved).
- Note any conflicts between internal views and external data.

---

## Output format

```
# Competitive Analysis: [Startup Name]
_[date] · Sources: Specter, Affinity, Granola, Superhuman, Google Drive, Evertrace, web search_

## Competitor Overview

[markdown table]

## Market Overview
…

## Target Company Positioning
…

## Key Competitive Threats
…

## Whitespace & Moat
…

## Internal Intelligence  ← only if Step 1 returned useful content
…

## Sources & Notes
…
```

---

## Important notes

- Internal sources (CRM, meetings, email, Drive) take priority — they carry proprietary context no external database has.
- Always show the target startup as the first table row so it serves as the comparison baseline.
- Key Difference must describe how a competitor differs *from the target*, not a generic description.
- Preserve all Granola citation links `[[n]](url)` verbatim — do not paraphrase them away.
- If a company appears in multiple sources, reconcile discrepancies and note them.
- Do not fabricate data. Mark unknown fields `?`. This is a DD context — accuracy over completeness.
