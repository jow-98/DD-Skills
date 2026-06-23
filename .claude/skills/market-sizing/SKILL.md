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

### 2b. Value Theory (optional — use when creating a new category or if no comparable SaaS pricing exists)

Value theory estimates market size from the economic value the product creates, rather than from customer counts × ACV.

**When to use:**
- The startup creates a genuinely new category with no direct pricing comparables
- The startup displaces significant manual labour or existing spend (e.g. replaces a €500k/yr process)
- Top-down and bottom-up are yielding implausibly wide ranges

**Process:**
1. **Quantify the problem cost** — what does the customer currently spend (time × FTE cost, third-party fees, error rates × cost) to handle the problem the startup solves?
   - Search: `"<startup name>" ROI case study` or `<problem category> cost per year survey`
2. **Estimate % of that cost the startup solves** — be conservative (20–60% typical)
3. **Estimate willingness-to-pay** — typically 10–30% of value created
4. **Multiply by the addressable customer base** (from Step 2 ICP)

```
Value per customer       = Problem cost × % solved by product
Price per customer (ACV) = Value × WTP % (10–30%)
TAM                      = Total ICP customers × ACV
SAM / SOM                = apply same ICP Fit % and Market Share % from Step 3–5
```

**Output:** One additional row in the assumptions table and one ACV scenario in the TAM/SAM table labelled "Value Theory ACV".

Cross-check: value-theory ACV vs. bottom-up ACV should be within 2–3×. Large divergence = either the product captures too little value (pricing risk) or the problem isn't as costly as assumed.

---

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

Estimate the expected ACV per customer. Use:
- Any public pricing the startup has disclosed
- Comparable SaaS pricing benchmarks for the category
- Per-unit economics (e.g. price × typical usage volume)
- Investor materials / pitch deck data if provided

If pricing has multiple tiers or scenarios, model 2–3 scenarios (e.g. Entry / Mid / Enterprise ACV).

Search for benchmarks:
- `"<startup name>" pricing`
- `<category> SaaS average contract value benchmark`
- `<comparable company> ARR per customer`

Always cite the source or reasoning for the ACV figure.

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
_Generated [date] · Bottom-up methodology · Sources cited inline_

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
