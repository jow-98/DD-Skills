# Market Sizing Excel — Reference Guide

Detailed guidance for producing a rigorous, sourced, bottom-up market sizing. Always default to bottom-up as the primary methodology. Top-down is a sanity check only.

---

## Default Approach: Bottom-Up First

Always build the bottom-up model first. The core formula is:

```
TAM = # ICP Companies × ACV
SAM = TAM × ICP Fit %
SOM = SAM × Market Share %
```

Do not start with a top-down number and work backwards — that leads to unfounded precision. Build up from company counts and a justified ACV, then cross-check against analyst reports.

---

## Step 1: Define the ICP Precisely

The ICP definition is the most important input. A loose ICP produces a useless market size. Be specific on all four axes:

| Axis | Questions to answer | Example |
|------|--------------------|----|
| **Company size** | Minimum employees or revenue? | >250 employees |
| **Industry / vertical** | NACE code or NAICS code? | NACE 64.1 (monetary institutions) |
| **Geography** | Which countries? | DACH + Benelux + Nordics (home market) |
| **Tech/budget signal** | Must they have a specific tech stack, spend threshold, or role? | Must have an ML/AI team or dedicated data function |

**Source of truth for ICP**: Internal context takes precedence. Check in this order:
1. Granola founder call transcripts — how does the founder describe their target customer?
2. Alphasights / expert call notes — how do practitioners describe the buyer profile?
3. Google Drive pitch deck — ICP slide
4. Affinity notes — analyst observations on who the early customers are
5. Product website / Specter — infer from customer logos and case studies

---

## Step 2: Company Counts — Sources by Region

Use the most granular, authoritative source available. Never estimate counts without a URL.

### Europe

| Source | Best for | URL pattern |
|--------|----------|-------------|
| **Eurostat Structural Business Statistics** | NACE code + employee size class | `ec.europa.eu/eurostat/web/structural-business-statistics` |
| **Bureau van Dijk / Orbis** | Any industry + revenue/employee filter | `bvdinfo.com` |
| **Amadeus** | EU company counts by country and sector | `amadeus.bvdinfo.com` |
| **National statistics offices** | Country-level granularity | `destatis.de` (DE), `insee.fr` (FR), `ons.gov.uk` (UK) |

Search template: `"[NACE code] enterprises [country/region] [size class] eurostat"`

### USA

| Source | Best for | URL pattern |
|--------|----------|-------------|
| **Census County Business Patterns (CBP)** | NAICS code + employee band | `census.gov/programs-surveys/cbp.html` |
| **Census SUSB** | Revenue-based filters | `census.gov/programs-surveys/susb.html` |

Search template: `"[NAICS code] [state/US] [employee band] census.gov"`

### Global

For markets outside EU/USA, use:
- **World Bank Enterprise Surveys** — `enterprisesurveys.worldbank.org`
- **OECD SDBS** — `stats.oecd.org`
- **Dun & Bradstreet** — `dnb.com`

### When no authoritative count exists

If no official count is available, use a proxy and flag it:
1. LinkedIn company search (filter by industry + employee range) — note that LinkedIn undercounts SMBs
2. Specter `find_similar_companies` — useful for tech company counts
3. Industry association membership numbers — check trade association websites
4. Analyst report company count claims — cite the report and date

In the Source column, write: `"Estimated via LinkedIn search [date] — no official source found"` and flag in the Notes block.

---

## Step 3: ACV — Scenarios and Sources

### Source priority (always use internal sources first)

1. **Expert call transcripts (Granola)** — willingness-to-pay data from practitioners. Use as the conservative (entry) scenario.
2. **Alphasights briefs (Superhuman)** — buyer budget ranges from the expert network.
3. **Pitch deck pricing slide (Google Drive)** — founder's ACV claim. Use as mid or optimistic scenario.
4. **Founder emails / Affinity notes** — pricing discussed informally.
5. **Public pricing page** — search `"[startup name]" pricing`.
6. **Category benchmarks** — see table below.

### Scenario design rules

Model **2–3 scenarios** per region block. Name them meaningfully:

| Scenario | When to use | ACV basis |
|----------|-------------|-----------|
| **V1 — Entry / Expert WTP** | Always | Expert call WTP or 40th percentile of benchmarks |
| **V2 — Mid / Founder Claim** | Always | Founder's stated ACV or pitch deck price |
| **V3 — Upside / Category Ceiling** | When market is nascent or pricing is unclear | Top quartile benchmark or analogous category |

If internal sources conflict (e.g. expert WTP of €15k vs. founder claim of €40k), model both as named scenarios and explain the gap in the Notes block. Do not average — the gap itself is a finding.

### ACV benchmarks by category

Use only when internal sources are absent or need external validation.

| Category | Entry ACV | Mid ACV | High ACV | Source |
|----------|-----------|---------|----------|--------|
| Early-stage SaaS, SMB | €3–8k | €10–20k | €30k+ | OpenView SaaS Benchmarks |
| Mid-market SaaS | €15–30k | €40–80k | €100k+ | KeyBanc SaaS Survey |
| Enterprise SaaS | €50–100k | €150–300k | €500k+ | Bessemer State of Cloud |
| AI / ML tooling | €20–50k | €80–150k | €300k+ | a16z AI survey, Dealroom |
| Fintech / compliance | €30–80k | €100–250k | €500k+ | Industry benchmarks |
| DevTools / infrastructure | €5–20k | €25–60k | €100k+ | OpenView PLG survey |
| Healthcare / MedTech | €20–60k | €100–250k | €500k+ | KLAS, Black Book surveys |
| Marketplace take rate | 3–8% | 10–15% | 20–30% | Category-specific |

Always cite the specific benchmark source (report name, year, URL) in the Notes block.

---

## Step 4: ICP Fit % — How to Justify It

ICP Fit % = the share of the total company count that genuinely matches the full ICP profile. This is always < 100%. Conservative assumptions build credibility.

### Typical ranges

| ICP definition tightness | Typical ICP Fit % |
|--------------------------|-------------------|
| Very broad (all companies in NACE code) | 5–15% |
| Moderate (size + industry filter) | 20–40% |
| Tight (size + industry + tech/budget signal) | 40–65% |
| Very tight (named ideal profile, specific use case) | 60–80% |

### How to determine ICP Fit %

1. Ask: of all companies in the count, how many have the tech stack, budget, or operational profile to be a buyer today?
2. Cross-check against founder's existing customer base — what % of the target universe do their current customers represent?
3. Use expert call intelligence — did practitioners give a view on addressable share?
4. Cite your reasoning explicitly in the Notes block.

**Do not write just "30%" — always write "30% — [reason]" in Notes.**

---

## Step 5: Market Share % — How to Justify It

Market Share % = the realistic share the startup can capture within the SAM at a 3–5 year horizon for SOM. This is always a small number for early-stage companies.

### Typical ranges

| Stage | Beachhead Market Share | Notes |
|-------|----------------------|-------|
| Pre-seed / Seed | 0.5–2% | Single region, single segment |
| Series A | 1–5% | Multi-segment or first international |
| Series B+ | 3–10% | Multi-regional, scaled GTM |

### Sanity check

Implied customer count = SOM ÷ ACV. Does this number pass a sense check?

For example: SOM of €5m at €30k ACV implies 167 customers. For a Seed company, is 167 customers achievable in 3–5 years? If yes, the model is consistent. If it implies 5,000 customers, your market share assumption is too high.

Always include this sanity check in the Notes block.

---

## Notes Block — Required Content

The Notes block is mandatory. Every reviewer will look here first. Include:

1. **ICP definition** — full description, all four axes, and source
2. **Company count source** — URL or publication name and date for each region
3. **ACV basis** — which internal source anchored each scenario; if external benchmark used, cite it
4. **ICP Fit % rationale** — one sentence per region explaining the percentage
5. **Market Share % rationale** — one sentence explaining the assumption
6. **Data vintage** — year of statistics used (e.g. "Eurostat 2022 data")
7. **Internal sources consulted** — list Granola meetings, Superhuman threads, and Drive docs reviewed
8. **Conflicts** — if internal sources disagreed, note what the disagreement was and how it was resolved in the model
9. **Sanity check** — implied customer count at SOM and whether it passes a common-sense test

---

## Top-Down Sheet — Purpose and Rules

The Top-Down sheet is a **cross-check only**. It does not drive the model.

Rules:
- Aim for 3–5 independent top-down sources from at least two tiers (analyst firm + market research publisher)
- Always include the founder's own TAM claim as one row (labeled "Founder claim – [source]")
- Include expert market size opinions if any were given in calls (labeled "Expert estimate – [name/role, date]")
- Include your bottom-up SAM (V2 mid scenario) as the anchor row for comparison
- If top-down and bottom-up diverge by more than 3×, investigate and explain in Notes

### Divergence interpretation

| Relationship | Interpretation |
|-------------|---------------|
| Bottom-up SAM ≈ top-down market (within 1–2×) | Consistent. Use bottom-up as primary. |
| Bottom-up SAM << top-down (>3× smaller) | Your ICP or ACV is conservative. Check if you're undersizing the opportunity. |
| Bottom-up SAM >> top-down (>3× larger) | Your ICP Fit % or Market Share % may be too aggressive. Revisit assumptions. |
| Founder TAM >> all other sources | Flag as "Founder optimism" — common; model conservatively. |

---

## Value-Theory Sheet — When to Use It

Skip the Value-Theory sheet by default. Add it only when:

- The startup is creating a new category where no ACV benchmark exists
- The startup is displacing significant existing spend (e.g. replacing a manual process costing €200k/yr per company)
- Bottom-up and top-down diverge by >5× and you need a third anchor

When adding it, the WTP % assumption (how much of the value created the product captures as price) is the most important number. Typical range: 10–30% of value created. Cite precedent from comparable categories.

---

## Common Mistakes to Avoid

| Mistake | Correction |
|---------|-----------|
| Using the full industry universe without ICP filtering | Always apply employee/revenue/tech filters; justify ICP Fit % |
| Taking the founder's TAM at face value | Model it in Top-Down as "Founder claim" — build your own bottom-up independently |
| Using round ICP Fit % numbers (e.g. exactly 50%) | Justify every percentage; round numbers signal guesswork |
| Sourcing company counts from Statista without checking their original source | Trace Statista numbers back to the original source (usually Eurostat or Census) |
| Omitting the Notes block | The Notes block is mandatory — all assumptions must be explained |
| Building only one ACV scenario | Always model at least 2 scenarios; 3 is better |
| Currency inconsistency (mixing € and $) | Pick one and use it throughout; note the conversion rate and date if mixed sources are used |
| Market Share % > 10% for early-stage | This is unrealistic for Seed; flag and investigate if the model requires it to justify the investment |
