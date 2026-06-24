---
name: competitive-analysis
description: Research and output a structured competitive analysis for a startup being evaluated in due diligence. Produces a rich markdown report with a competitor table, feature comparison, and positioning summary. Use when asked for a competitive overview, competitive landscape, or comp analysis of a startup.
---

# Competitive Analysis Skill (Co Work / Markdown Output)

Produce a structured competitive landscape analysis for a startup being evaluated in a DD process.
Pull from every available knowledge source — internal memory first, then external data.

## Input

Invoked as `/competitive-analysis <Startup Name>` or with a company name / URL in args.

## Workflow

Work through each step in order. Give the user a brief one-line status update as each step completes.

---

### Step 1 — Internal knowledge sweep (run all in parallel)

Pull everything the firm already knows before touching any external source. Internal intelligence takes precedence over external data throughout the rest of the workflow.

**1a. VC Knowledge Hub** ← start here, it's the fastest unified search across all internal sources
Run all in parallel:
- `mcp__vc-knowledge-hub__search` → _"[startup name]"_ — pulls the firm's full profile: Affinity data, meeting notes, pitch deck content, Drive docs, all in one call
- `mcp__vc-knowledge-hub__get_company` → startup name — detailed company record with notes and meeting history
- `mcp__vc-knowledge-hub__get_similar_companies` → startup name — competitors already seen in the firm's deal flow
- `mcp__vc-knowledge-hub__ask` → _"What do we know about [startup name] and its competitive landscape? Include any expert or AlphaSight call notes, analyst views, and concerns raised."_ — multi-step AI reasoning across all sources
- `mcp__vc-knowledge-hub__ask` → _"What competitors to [startup name] have we seen in our deal flow? Include any companies in similar spaces."_
- `mcp__vc-knowledge-hub__search` → _"[inferred market category] competitors"_ — surface related companies from the deal flow

For each competitor the hub returns, also call `mcp__vc-knowledge-hub__get_company` to pull their full profile including any notes.

Also run:
- `mcp__vc-knowledge-hub__search_research_findings("<startup name>")` — retrieve any prior competitive research already saved
- `mcp__vc-knowledge-hub__check_companies("<competitor names>")` — batch-check key competitors against known firm data
- `mcp__vc-knowledge-hub__search_operator_playbooks("<sector>")` — surface operator playbooks with competitive positioning insights

After completing the analysis, save for future reuse:
`mcp__vc-knowledge-hub__save_research_analysis("<startup name>", summary="<competitive positioning summary>")`.

If the VC Knowledge Hub returns no results or incomplete data, fall through to the individual connectors directly (Granola, Superhuman, Google Drive, Affinity, Specter, Evertrace, and any other relevant connector) — those remain the authoritative sources.

**1b. Affinity CRM** (for data not captured by the hub, e.g. pipeline stage and field values)
- `mcp__Affinity__search_companies` → find the CRM record and ID
- `mcp__Affinity__get_notes_for_entity` (entity_type=1) → all analyst notes (may have richer detail than hub)
- `mcp__Affinity__get_company_list_entries` → pipeline stage
- For CRM-tracked competitors: `mcp__Affinity__get_notes_for_entity` on each

**1c. Granola — founder calls AND expert/AlphaSight calls**
Run all queries in parallel:
- `mcp__Granola__query_granola_meetings` → _"[startup name] founders — product, market, competitive positioning"_
- `mcp__Granola__query_granola_meetings` → _"[startup name] competitors risks threats concerns"_
- `mcp__Granola__query_granola_meetings` → _"AlphaSight [startup name]"_ — expert calls via AlphaSight
- `mcp__Granola__query_granola_meetings` → _"expert call [startup name] [inferred market/industry]"_ — any other expert/advisor calls
- `mcp__Granola__query_granola_meetings` → _"[startup name] due diligence technical commercial"_

For each relevant meeting ID returned, call `mcp__Granola__get_meetings` for the full summary and private notes, and `mcp__Granola__get_meeting_transcript` if you need exact quotes.

Categorise what you find into two buckets:
  - **Founder/team calls** — what the startup's own people said about their market and competitors
  - **Expert calls** — what independent industry experts, AlphaSight consultants, advisors, or reference calls said

Preserve all Granola citation links `[[n]](url)` verbatim — they must appear in the final report.

**1d. Email — expert opinions and analyst research (Superhuman)**
Run all in parallel:
- `mcp__Superhuman__query_email_and_calendar` → _"Expert opinions or analyst views on [startup name] or its competitors"_
- `mcp__Superhuman__query_email_and_calendar` → _"[startup name] — market research, analyst reports, or due diligence material"_
- `mcp__Superhuman__query_email_and_calendar` → _"AlphaSight [startup name]"_ — AlphaSight email summaries or transcripts sent by email
- `mcp__Superhuman__list_threads` with `body_contains` = startup name → pitch emails, forwarded research, follow-ups
- `mcp__Superhuman__list_threads` with `subject_contains` = _"expert"_ or _"AlphaSight"_ or _"reference call"_ or _"advisor"_ and relevant time window

For any threads found, call `mcp__Superhuman__get_thread` to read the full content.

Extract: expert names/titles, their specific opinions on the startup and its competitors, any quantitative claims they made, and any competitive intelligence they shared.

**1e. Google Drive — prior memos, pitch decks, existing analyses**
- `mcp__Google_Drive__search_files` with `fullText contains '[startup name]'`
- `mcp__Google_Drive__search_files` with `title contains 'Competitor'` or `title contains 'Landscape'` or `title contains 'AlphaSight'`
- `mcp__Google_Drive__read_file_content` on relevant files (memos, decks, prior competitor overviews, expert summaries)

Note: pitch deck PDFs attached in Affinity are already searchable via the hub's `search` tool — no need to pull them separately from Drive unless you need full document content.

**Consolidation after Step 1:**
Build an internal intelligence summary noting:
- Competitors already known internally, with any views/scores
- Expert opinions found (by source: AlphaSight call, network expert email, advisor note, etc.)
- Founder claims about the competitive landscape
- Any conflicts between internal views and external data — flag these explicitly

---

### Step 2 — Resolve the target company via Specter

- `mcp__Specter__find_company` → `mcp__Specter__get_company_profile` + `mcp__Specter__get_company_intelligence`

Extract: full name, website, HQ (ISO-2), headcount, founding year, product description, last funding, investors, primary market category.

---

### Step 3 — Identify competitors

Triangulate from three sources in parallel:

**3a. Specter** — `mcp__Specter__find_similar_companies` on the resolved company ID.

**3b. Evertrace talent signals**
- `mcp__Evertrace__search_companies` for the startup name → get its entity ID (exe_* format).
- `mcp__Evertrace__list_signals` with `past_companies=[entity_id]` → reveals the talent ecosystem and which other companies share talent pools.
- Repeat for 2–3 named direct competitors to map cross-company talent flows.

**3c. Web search**
- `WebSearch`: _"[startup name] competitors 2024 2025"_
- `WebSearch`: _"best alternatives to [startup name]"_
- `WebSearch`: _"[market category] competitive landscape [year]"_

Merge all sources. Prioritise companies flagged in Step 1 internal intel.
Target 10–20 competitors across tiers: **Direct** / **Adjacent** / **Incumbent**.

---

### Step 4 — Research each competitor

For each competitor, use all available sources:

| Source | Action |
|--------|--------|
| Specter | `find_company` → `get_company_profile` |
| Affinity | `search_companies` → if found, `get_notes_for_entity` |
| Granola | `query_granola_meetings` — any expert or founder calls mentioning this competitor? |
| Superhuman | `query_email_and_calendar` — any email intel, expert opinions, or analyst views on this competitor? |
| Web | `WebSearch` or `WebFetch` to fill remaining gaps |

For each competitor collect: URL, HQ (ISO-2), headcount, founding year, product description (≤120 chars), last funding + investors, **Key Difference** from target startup (≤150 chars, specific), tier, and any expert opinion attached to this competitor.

---

### Step 5 — Determine comparison dimensions

Based on the target startup's product and market, choose 5–8 dimensions for comparison. These may be:
- **Binary feature checkmarks** (e.g. "Open source", "Self-hosting") — mark with ✓ / (✓) / –
- **Descriptive attributes** (e.g. "Detection Range", "Sensor Technology", "Product Type") — fill with short text values

Use Step 1 intel and the startup's actual product to decide. Pick dimensions that experts and founders have actually discussed, not generic ones.

---

### Step 6 — Build the competitor table

Output a markdown table:

| Company | URL | Location | Headcount | Founded | Product Description | Last Funding | Investors | Key Difference | Tier | [Dim 1] | [Dim 2] | … |

Rules:
- **First data row = target startup** (bold name, mark `← target`)
- Sort: Direct → Adjacent → Incumbents
- Binary dims: ✓ confirmed · (✓) partial · – absent · ? unknown
- Descriptive dims: short text value or `—`
- Rows where expert opinion exists: mark with † and reference the expert section below

---

### Step 7 — Narrative analysis

**Market Overview** — Market size, growth drivers, key dynamics right now. (3–5 sentences)

**Target Company Positioning** — Where the startup sits in the landscape. Distinct strengths. Who it most directly competes with and how it differentiates. (3–5 sentences)

**Key Competitive Threats** — Top 3 companies posing the most risk. Why each is a threat. Reference expert opinions where available. (3–5 sentences)

**Whitespace & Moat** — Defensible differentiation or underserved niche. Signals from talent data, meeting notes, or expert calls that reinforce or challenge the moat thesis. (3–5 sentences)

---

### Step 7b — Strategic Framework Analysis *(run in parallel with Step 7; include in final output)*

Using evidence already collected in Steps 1–6, produce three sub-analyses. Each is a **separate output section** — do not merge them.

**7b-i. Porter's Five Forces**

Score each force 1–5 (1 = low threat/power, 5 = high). Use internal intel and expert quotes where available to justify scores.

| Force | Score (1–5) | Key Factors | Implication |
|-------|-------------|-------------|-------------|
| Threat of New Entrants | | Capital, tech, regulatory barriers; network effects | |
| Bargaining Power of Suppliers | | Supplier concentration, switching costs, alternatives | |
| Bargaining Power of Buyers | | Customer concentration, price sensitivity, switching costs | |
| Threat of Substitutes | | Manual processes, DIY, adjacent tools | |
| Competitive Rivalry | | # of players, growth rate, differentiation, exit barriers | |

Overall Market Attractiveness: **Attractive / Moderate / Challenging** — 1–2 sentences.

**7b-ii. SWOT (target startup only)**

Draw Strengths/Weaknesses from Step 1 internal intel and Specter data; Opportunities/Threats from Steps 3–6 competitive landscape. 3–5 points per quadrant.

| | Strengths | Weaknesses |
|---|---|---|
| **Internal** | [bullet list] | [bullet list + mitigation each] |

| | Opportunities | Threats |
|---|---|---|
| **External** | [bullet list + how to capitalize] | [bullet list + likelihood + mitigation] |

**7b-iii. Competitive Positioning Map**

Rate each company (target first, then competitors) on two axes using Step 4 data:
- **Price** (1 = cheapest, 10 = most expensive)
- **Feature depth** (1 = minimal/simple, 10 = comprehensive/complex)

| Company | Price (1–10) | Features (1–10) | Tier | Positioning Note |
|---------|-------------|-----------------|------|-----------------|
| [Target] ← target | | | | |
| [Competitor A] | | | Direct | |

White space: [1–2 sentences identifying underserved quadrants or differentiation opportunity]

---

### Step 7c — Marketing & GTM Intelligence *(run in parallel with Step 7; include if GTM data found)*

Analyze how the target and top 3–5 competitors position and market themselves. Pull from product pages, blog/content, job postings, review sites (G2, Capterra), and any Superhuman or Granola intel on GTM strategy.

**7c-i. Messaging Matrix**

| Dimension | [Target] | [Comp A] | [Comp B] | [Comp C] |
|-----------|---------|---------|---------|---------|
| Tagline / Headline | | | | |
| Core value proposition | | | | |
| Primary audience | | | | |
| Key differentiator claim | | | | |
| Tone / Voice | | | | |
| Proof points used | | | | |
| Category framing | | | | |
| Primary CTA | | | | |

**7c-ii. Content & Channel Coverage**

| Channel / Format | [Target] | [Comp A] | [Comp B] | Gap / Opportunity |
|-----------------|---------|---------|---------|-------------------|
| Blog / SEO | | | | |
| Case studies | | | | |
| Whitepapers / Ebooks | | | | |
| Webinars / Events | | | | |
| Video | | | | |
| G2 / Capterra reviews | | | | |
| Paid ads | | | | |
| Community presence | | | | |

Content opportunity: [1–2 sentences on biggest gaps or differentiators to amplify]

**7c-iii. GTM Battlecard Summaries**

For each top 3 competitor:

**[Competitor Name]**
- **Their pitch**: how they describe themselves; top 3 claimed differentiators
- **Where they win**: genuine strengths from reviews + expert intel
- **Where they're vulnerable**: consistent complaints (G2/Capterra, Granola notes)
- **Landmine questions** to ask prospects that highlight the target startup's advantage:
  1. …
  2. …
  3. …

---

### Step 8 — Expert Intelligence section (REQUIRED if any expert content found)

This section is mandatory if Step 1 surfaced any AlphaSight calls, expert emails, advisor notes, or reference calls.

Format each expert opinion as a block:

---
**[Expert Name / Title / Source]** · _[date or meeting title if known]_ · [[citation]](url) ← preserve Granola link

> "[Direct quote or close paraphrase of the expert's view]"

**On the target startup:** [summary of what they said]
**On competitors:** [which competitors they named, and what they said about each]
**Red flags raised:** [any concerns, limitations, or risks they identified]
**Validation points:** [anything they confirmed positively]

---

Include one block per expert source. Do not merge or paraphrase across experts — their individual views matter for triangulation.

After the individual blocks, write a 2–3 sentence **Expert Consensus Summary**: where do independent experts agree, where do they diverge, and what open questions remain.

---

### Step 9 — Internal intelligence addendum (if additional CRM/email/Drive content found)

Summarise Affinity analyst notes, email threads, and Drive documents that didn't fit elsewhere.
Flag any conflicts between internal views and external/expert data.

---

## Output format

```
# Competitive Analysis: [Startup Name]
_[date] · Sources: VC Knowledge Hub, Affinity, Granola (AlphaSight + founder calls), Superhuman, Google Drive, Evertrace, Specter, web search_

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

## Strategic Framework Analysis
### Porter's Five Forces
[scored table + overall attractiveness sentence]

### SWOT — [Target Startup]
[SWOT table]

### Competitive Positioning Map
[positioning table + white space insight]

## Marketing & GTM Intelligence
### Messaging Comparison
[messaging matrix]

### Content & Channel Coverage
[coverage table + opportunity summary]

### GTM Battlecards
[battlecard summary per top 3 competitors]

## Expert Intelligence   ← mandatory if any expert content found
[expert blocks]

**Expert Consensus Summary:** …

## Internal Intelligence  ← if additional CRM/email/Drive content
…

## Sources & Notes
…
```

---

## Important notes

- **Expert opinions are primary evidence** — they override generic web research and should be quoted directly, not paraphrased into vague summaries.
- AlphaSight call transcripts and expert emails from the firm's network contain proprietary DD intelligence that no external database has. Surface every usable insight.
- Preserve all Granola citation links `[[n]](url)` verbatim in all sections.
- The target startup is always the first table row — the baseline for all Key Difference comparisons.
- Do not fabricate data. Mark unknown fields `?`. Accuracy over completeness in a DD context.


---

## Language and Tone

- Use expert terminology appropriate to the context (VC due diligence, financial analysis, competitive intelligence)
- Avoid superfluous prose, self-references, expert advice disclaimers, and apologies
- No em dashes or en dashes; use commas, parentheses, or rewrite the sentence instead
- Lead with data and specific findings; interpretation follows the evidence
- No superlatives (world-class, revolutionary, best-in-class, cutting-edge)

