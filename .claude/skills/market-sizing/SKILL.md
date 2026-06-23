---
name: market-sizing
description: Research and output a bottom-up market sizing for a startup being evaluated in due diligence. Produces a cited markdown report with TAM/SAM/SOM tables by region, key assumptions, and a top-down sanity check. Use when asked for a market sizing, addressable market analysis, or TAM/SAM/SOM for a startup.
---

# Market Sizing Skill (Co Work / Markdown Output)

Produce a rigorous bottom-up market sizing for a startup being evaluated in a DD process, with full source citations for every key number.

## Input

Invoked as `/market-sizing <Startup Name>` or with a company name / URL in args.

The user may optionally provide extra context in the args, e.g.:
- Target customer segment (e.g. "mid-market manufacturing companies")
- Pricing model / ACV if already known
- Geographies to focus on

## Workflow

Work through each step in order. Keep the user updated with one-line status messages.

### 0. Gather all internal DD context first

Before doing any external research, pull everything already in the pipeline about this startup. Run all of these in parallel:

**Granola — meeting notes & expert calls:**
Use `mcp__Granola__query_granola_meetings` with the startup name to find all recorded meetings (founder calls, expert calls, partner discussions). For each result call `mcp__Granola__get_meeting_transcript` to get the full transcript.
Extract and cache:
- Any ICP descriptions the founders or experts gave
- Willingness-to-pay signals (specific price points mentioned, budget ranges, buyer personas)
- Market size numbers the founders claimed (their own TAM/SAM assumptions)
- Customer names or segments mentioned
- Any expert pushback on market assumptions

**Superhuman — email threads:**
Use `mcp__Superhuman__query_email_and_calendar` with the startup name to find all relevant email threads.
Also search for related terms: `"<startup name>" Alphasights`, `"<startup name>" expert`, `"<startup name>" customer`.
Extract and cache:
- Alphasights / expert network briefs sent or received — these contain ICP definitions and willingness-to-pay data
- Any founder emails describing their pricing model or customer pipeline
- Expert replies with market commentary or competitor callouts
- Any forwarded decks or attachments referenced

**Google Drive — decks and documents:**
Use `mcp__Google_Drive__search_files` with the startup name to find pitch decks, market sizing docs, or DD memos already on file.
For each relevant file call `mcp__Google_Drive__read_file_content` or `mcp__Google_Drive__download_file_content`.
Extract and cache:
- The startup's own TAM/SAM/SOM figures and methodology
- Any pricing slides (ACV, pricing tiers, take-rate assumptions)
- ICP slides (customer profile, target company size, industries)
- Any market research slides with cited sources

**Affinity — CRM notes:**
Use `mcp__Affinity__search_companies` with the startup name to find the company in the CRM.
Then call `mcp__Affinity__get_notes_for_entity` and `mcp__Affinity__get_meetings_for_entity` to retrieve all notes and meeting logs.
Extract: any ICP, pricing, or market size commentary logged by the team.

**Synthesise internal context:**
After gathering all internal data, compile a "known facts" summary:
- Confirmed ICP (from founders, experts, or Alphasights briefs)
- Confirmed or signalled ACV / pricing (from deck, emails, expert calls)
- Founder's own market size claim (to compare with your model)
- Any willingness-to-pay data from expert calls or customer conversations
- Flag any contradictions between sources (e.g. founder claims $500K ACV but expert says $100K is the ceiling)

Use this internal context to anchor the model. Only use web research to fill gaps or validate numbers that are absent from internal sources.

### 1. Resolve the target company

Use `mcp__Specter__find_company` with the startup name or URL.
Then call `mcp__Specter__get_company_profile` + `mcp__Specter__get_company_intelligence` on the resolved ID.

Extract and cache:
- Full name, website, HQ, industry/sector
- Product description: what it does, for whom, how it is priced (per seat, per usage, % of spend, flat fee, etc.)
- Current known customers / customer type (ICP)
- Pricing signals: any public pricing, customer contract sizes, ARR multiples mentioned
- Stage and known ARR / revenue if available

### 2. Define the target customer segment (ICP)

Based on the product, define the Ideal Customer Profile precisely:
- Company size (employees, revenue thresholds, or spend thresholds)
- Industry / vertical(s)
- Geography (which regions are realistically addressable in the next 3–5 years)
- Buying signal (what triggers them to buy this product)

Document your ICP definition explicitly — it becomes the denominator for the bottom-up count.

Use `WebSearch` to validate the ICP definition if unclear:
- `"<startup name>" customers case study`
- `"<startup name>" target market enterprise mid-market`

### 3. Count target accounts by region

This is the core of the bottom-up. For each relevant region (typically EU, USA, and any other key geographies):

a. Find the total universe of companies matching the ICP size/industry criteria.
   Use `WebSearch` with queries like:
   - `number of [industry] companies [region] >250 employees eurostat`
   - `number of [industry] enterprises [region] statistics`
   - `[industry] company count [region] site:eurostat.eu OR site:census.gov OR site:statista.com`

   Preferred authoritative sources (in order):
   - EU: Eurostat (eurostat.eu) — search structural business statistics by NACE code
   - USA: Census Bureau (census.gov) — County Business Patterns, or NAM for manufacturing
   - Global: World Bank, OECD, industry trade associations
   - Fallback: Statista, IBISWorld, industry reports

b. Apply an ICP fit % — the fraction of those companies that truly match (right size tier, right use case pain, right buying readiness). Be conservative. Typical range: 20–50%. Document your reasoning.

c. Record the source URL for each company count.

### 4. Determine ACV (Annual Contract Value)

Use internal DD context first (Step 0), then fill gaps with external research.

Priority order for ACV inputs:
1. **Expert call transcripts** (Granola) — willingness-to-pay signals from customers or industry experts are the strongest signal
2. **Alphasights / expert network briefs** (Superhuman) — often contain explicit budget range data from practitioners
3. **Startup's own pitch deck** (Google Drive) — founder's claimed ACV or pricing model
4. **Founder emails / CRM notes** (Superhuman / Affinity) — pricing discussed in context
5. **Public pricing pages** — search `"<startup name>" pricing`
6. **Category SaaS benchmarks** — search `<category> SaaS average contract value benchmark`

If sources conflict (e.g. founder claims higher ACV than experts suggest is achievable), model both as separate scenarios and flag the divergence explicitly.

Always cite the source or reasoning for every ACV figure used.

### 5. Calculate TAM, SAM, SOM

For each region and scenario:

```
TAM  = # ICP companies (total universe) × ACV
SAM  = TAM × ICP Fit %          (realistically serviceable given go-to-market focus)
SOM  = SAM × Market Share %     (obtainable within 5 years given competitive dynamics)
```

Market Share % guidance:
- Beachhead / early-stage: 0.5–2%
- Post-PMF with strong GTM: 3–10%
- Category leader in niche: 10–30%

Document the reasoning for each % applied.

### 6. Project forward

Apply an appropriate CAGR to estimate market size in 5 years (current year + 5):
- Source the CAGR from industry forecasts (search: `<industry> market CAGR 2025 2030 forecast`)
- Use the TAM CAGR from a credible source (Gartner, IDC, Grand View Research, McKinsey)

Calculate:
- Current year SAM
- 5-year SAM (with CAGR)

### 7. Top-down sanity check

Find 1–2 top-down market size reports for the broader category:
- Search: `<category> total addressable market size 2024 2025 billion`
- Sources: Gartner, IDC, Grand View Research, MarketsandMarkets, Statista

Compare the bottom-up SAM to what the top-down implies. Note whether the bottom-up is conservative, aligned, or aggressive vs. top-down. Explain any large divergence.

### 8. Compile assumptions & caveats

List every key assumption made:
- ICP definition rationale
- ICP fit % and why
- ACV source / benchmark
- Market share % and why
- CAGR source
- Geographies excluded and why (and their rough size if material)

Note what could expand the market (e.g. APAC, SMB segment, adjacent verticals not modelled).

## Output format

```markdown
# Market Sizing: [Startup Name]
_Generated [date] · Bottom-up methodology · Sources: Granola, Superhuman, Google Drive, Specter, web search_

## 0. Internal DD Context
_What we already know from the pipeline — meetings, emails, decks, expert calls_

| Source | Key Finding | Implication for Model |
|---|---|---|
| Granola – [meeting/date] | [e.g. "Founder: ACV €400–600K mid-market"] | Used as ACV anchor for V2 scenario |
| Superhuman – [email/Alphasights brief] | [e.g. "Expert: buyers budget €200–300K initially"] | Entry ACV set conservatively at €250K |
| Google Drive – [deck name] | [e.g. "Pitch deck claims €1.5B TAM, 2,500 accounts"] | Cross-checked against bottom-up |
| Affinity | [e.g. "CIO buyer, budget from IT infra line"] | ICP fit % set to 40% |

_Note "No data found" for any source with no results — do not skip._

## 1. Company & Product Summary
[2–3 sentences: what it does, for whom, how priced]

## 2. ICP Definition
[Precise description of target customer: size, industry, geography, buying trigger]

## 3. Bottom-Up Market Sizing

### Assumptions
| Parameter | Value | Source / Reasoning |
|---|---|---|
| ACV (entry / mid / enterprise) | €X / €Y / €Z | [source] |
| ICP fit % | X% | [reasoning] |
| Market share % (beachhead) | X% | [reasoning] |
| CAGR | X% | [source] |

### TAM / SAM by Region — [Scenario: e.g. "Mid ACV"]

| Region | # ICP Companies | ACV | ICP Fit % | TAM | Market Share % | SAM | Source |
|---|---|---|---|---|---|---|---|
| EU | X | €Y | Z% | €A | B% | €C | [URL] |
| USA | X | €Y | Z% | €A | B% | €C | [URL] |
| **Total** | | | | **€A** | | **€C** | |

_Repeat table for each ACV scenario if multiple_

### 5-Year Projection

| Metric | Current Year | +5 Years (X% CAGR) |
|---|---|---|
| Total SAM | €X | €Y |
| SOM (beachhead) | €X | €Y |

## 4. Top-Down Sanity Check

[Paragraph comparing bottom-up result to top-down industry reports]

| Source | Market Size (current) | Market Size (5yr) | CAGR |
|---|---|---|---|
| [Source name + URL] | €X | €Y | Z% |

Bottom-up vs. top-down: [aligned / conservative / aggressive — explain]

## 5. Key Assumptions & Caveats

| Assumption | Value | Sensitivity |
|---|---|---|
| [assumption 1] | [value] | [what changes if wrong] |
| ... | | |

**Markets not included:** [e.g. APAC, SMB — add rough size if known]

## 6. Sources

- [Source 1 — description + URL]
- [Source 2 — description + URL]
- ...
```

## Important notes

- **Every number needs a source.** If you cannot find a primary source, say so explicitly and flag it as an estimate.
- This is a DD context — be conservative and transparent. Investors will stress-test every assumption.
- Use bottom-up as the primary analysis. Top-down is validation only.
- If the startup targets a niche within a broader industry, do not use the full industry count — apply ICP fit % carefully.
- Never cite ChatGPT, Claude, or AI-generated content as a source. Only cite primary data (statistics agencies, analyst reports, company filings, reputable trade associations).
- If Specter returns no useful data, rely on WebSearch — but always fetch and read the actual source page to verify the numbers.
- Distinguish clearly between TAM (total universe if everyone bought), SAM (realistic serviceable subset), and SOM (obtainable share).
