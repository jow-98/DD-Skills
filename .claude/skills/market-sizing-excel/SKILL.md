---
name: market-sizing-excel
description: "Research a startup in due diligence and generate an Excel (.xlsx) Market Sizing file with bottom-up TAM/SAM/SOM tables by region, scenarios, top-down validation, and source citations. Use this over market-sizing when the output must be .xlsx. Use when asked to create a market sizing Excel, build a market sizing spreadsheet, or generate a TAM/SAM file."
---

# Market Sizing — Excel Output Skill

Research a startup's addressable market and produce a `.xlsx` file with a bottom-up market sizing in the standard DD format, including sourced data, scenario tables, and a top-down sanity check sheet.

**Default methodology: Bottom-Up.** Always build the bottom-up model first (ICP company count × ACV × ICP Fit % × Market Share %). Top-down is a sanity check only — never drive the model from a top-down number. See `references/market-sizing-guide.md` for ICP definition standards, ACV benchmarks by category, and guidance on justifying ICP Fit % and Market Share %.

## Input

Invoked as `/market-sizing-excel <Startup Name>` or with a company name / URL in args.
Optional: the user may pass a target output path. Default: `./<StartupName>__Market_Sizing.xlsx`

## Workflow

Keep the user updated with one-line status messages.

### 0. Gather all internal DD context first

Before any external research, pull everything already in the pipeline. Run all of these in parallel:

**VC Knowledge Hub — unified sweep (run first):**
Use `mcp__vc-knowledge-hub__search` with the startup name to find all prior touchpoints across Affinity, Granola, and Drive in one pass.
Then call `mcp__vc-knowledge-hub__get_company` for the full company profile with notes and pitch deck content extracted.
Call `mcp__vc-knowledge-hub__ask("What market size figures, ICP definitions, and pricing assumptions has 42CAP collected for <startup name>?")` for a synthesised summary with source citations.
Call `mcp__vc-knowledge-hub__search_research_findings("<startup name>")` to retrieve any prior market sizing or sector research already saved.
Call `mcp__vc-knowledge-hub__search_operator_playbooks("<sector>")` to surface any operator playbooks with ICP or pricing benchmarks for this vertical.
Call `mcp__vc-knowledge-hub__get_similar_companies` to surface comparable companies in 42CAP deal flow — use as ICP and pricing benchmarks.
Extract and cache: any TAM/SAM claims, ICP definitions, ACV or pricing signals, founder market size assertions.

After completing the sizing, save for future reuse:
`mcp__vc-knowledge-hub__save_research_analysis("<startup name>", summary="<bottom-up TAM/SAM/SOM summary with key assumptions>")`.

If the VC Knowledge Hub returns no results or incomplete data, fall through to the individual connectors directly (Granola, Superhuman, Google Drive, Affinity, Specter, Evertrace, and any other relevant connector) — those remain the authoritative sources.

**Granola — meeting notes & expert calls:**
Use `mcp__Granola__query_granola_meetings` with the startup name. For each result call `mcp__Granola__get_meeting_transcript`.
Extract:
- ICP descriptions given by founders or experts
- Willingness-to-pay / budget range signals (specific figures are gold — use as ACV anchor)
- Founder's own market size claims
- Expert pushback on assumptions

**Superhuman — email threads:**
Use `mcp__Superhuman__query_email_and_calendar` with the startup name. Also search `"<startup name>" Alphasights`, `"<startup name>" expert`, `"<startup name>" customer`.
Extract:
- Alphasights / expert network briefs — these contain ICP definitions and buyer budget data
- Founder emails describing pricing model or customer pipeline
- Expert commentary on market size or competitors
- Any attached or linked decks

**Google Drive — decks and documents:**
Use `mcp__Google_Drive__search_files` with the startup name. For each relevant file call `mcp__Google_Drive__read_file_content` or `mcp__Google_Drive__download_file_content`.
Extract:
- The startup's own TAM/SAM/SOM figures and sources
- Pricing slides (ACV, take-rate, tiers)
- ICP slides (target company profile, size, industry, geography)

**Affinity — CRM notes:**
Use `mcp__Affinity__search_companies` to find the company, then `mcp__Affinity__get_notes_for_entity` and `mcp__Affinity__get_meetings_for_entity`.
Extract: any ICP, pricing, or market size commentary from team notes.

**Evertrace — company signals & discovery:**
Use `mcp__Evertrace__search_companies` with the startup name and relevant ICP keywords to find:
- Similar companies or competitors that have been screened/tracked
- Market signal data (funding events, growth signals) that imply market size
- Company counts within a defined segment (use as a cross-check on Eurostat/Census counts)
Also call `mcp__Evertrace__get_signals_metadata` to understand what signal types are available for this market.

**Synthesise before modelling:**
Compile a "known facts" cache:
- Confirmed ICP (from experts / Alphasights / founders)
- ACV range signalled internally — use as the primary anchor for scenario design
- Founder's own market claim (include as a row in the Top-Down sheet for comparison)
- Any contradictions between sources — model them as separate scenarios and flag in Notes

Only use web research to fill gaps that internal sources do not cover.

### 1. Resolve the target company

Use `mcp__Specter__find_company` with the startup name or URL.
Then call `mcp__Specter__get_company_profile` + `mcp__Specter__get_company_intelligence` on the resolved ID.

Cache:
- Full name, website, HQ, industry/sector
- Product description (what it does, for whom, how priced)
- ICP: customer type, size, industry, geography
- Pricing signals: any known ACV, pricing tiers, contract sizes

### 2. Define the ICP and scope

Based on the product, define precisely:
- Target company size (e.g. >250 employees, >$100M revenue, specific spend threshold)
- Industry / NACE code(s)
- Regions to model (typically EU + USA as primary; add others if material)

Use `WebSearch` to confirm ICP if unclear:
- `"<startup name>" customers target market ICP`

### 3. Research company counts by region

For each region, find the number of companies matching ICP criteria.

Use `WebSearch` with queries like:
- `number of [industry] companies [region] >250 employees eurostat`
- `[NACE/NAICS code] enterprises [region] size class statistics`
- `[industry] large enterprise count [region] site:eurostat.eu OR site:census.gov`

**Authoritative sources — company counts (use in priority order):**

| Priority | Region | Source | Notes |
|---|---|---|---|
| 1 | EU | **Eurostat Structural Business Statistics** — `ec.europa.eu/eurostat/web/structural-business-statistics` | Filter by NACE code + size class (>250 employees). Most granular EU source. |
| 1 | EU | **Bureau van Dijk / Orbis** — search `site:bvdinfo.com` or ask via WebSearch | Pan-European company database; ~500M companies. Best for precise NACE + revenue filters. |
| 2 | EU | **Amadeus database** — `amadeus.bvdinfo.com` | EU-focused subset of Orbis; often cited in academic/industry papers — search `amadeus database [industry] [country] enterprise count` |
| 1 | USA | **US Census Bureau County Business Patterns (CBP)** — `census.gov/programs-surveys/cbp.html` | NAICS code + employee size band. Gold standard for US counts. |
| 2 | USA | **US Census Bureau Statistics of US Businesses (SUSB)** — `census.gov/programs-surveys/susb.html` | Alternative Census table; use for revenue-based filters. |
| 2 | USA | **NAM Manufacturers' Outlook** — `nam.org` | For manufacturing ICP specifically. |
| 1 | UK | **ONS Business Population Estimates** — `ons.gov.uk` | Annual publication; filter by SIC code and employee band. |
| 2 | Global | **World Bank Enterprise Surveys** — `enterprisesurveys.worldbank.org` | Useful for emerging markets or cross-country comparisons. |
| 3 | Global | **OECD Structural and Demographic Business Statistics** — `stats.oecd.org` | Cross-country; searchable by industry and size. |
| 4 | Any | **Dealroom** — search `dealroom.co [industry] [region] companies` | Strong for tech/startup ecosystem counts; also covers traditional enterprises with tech exposure. |
| 4 | Any | **PitchBook** — search `pitchbook.com [industry] [region] company count` | Good for VC-backed company counts; use for tech-adjacent ICP. |
| 5 | Any | **Dun & Bradstreet / Hoovers** — `dnb.com` | Broad commercial database; search `site:dnb.com [industry] [region] company count` |
| 5 | Any | **Statista, IBISWorld** | Use as fallback or cross-check only; cite original source they reference |

**Sector-specific trade association sources (preferred over generic databases when available):**
- Financial services / banking: EBF (European Banking Federation), ABA (American Bankers Assoc.)
- Insurance: Insurance Europe, ACLI (American Council of Life Insurers)
- Telecom: ETNO (European Telecoms), CTIA (US wireless), GSMA (global)
- Manufacturing: Eurofer, NAM, VDA (Germany)
- Retail: EuroCommerce, NRF (US)
- Logistics / supply chain: ECG, GS1

Record the exact source URL for each region's count. Do not estimate counts without a source.

### 4. Determine ACV scenarios

Use internal DD context (Step 0) as the primary source — external research fills gaps only.

Priority order:
1. **Expert call transcripts** (Granola) — willingness-to-pay / budget ceiling from practitioners
2. **Alphasights briefs** (Superhuman) — buyer budget ranges from expert network
3. **Pitch deck pricing slide** (Google Drive) — founder's ACV claim
4. **Founder emails / CRM notes** — pricing discussed informally
5. **Public pricing page** — search `"<startup name>" pricing`
6. **Category benchmarks** — search `<category> SaaS ACV benchmark average contract value`

**External ACV benchmarks — use when internal sources are absent or need validation:**

*SaaS / software benchmarks:*
- **OpenView SaaS Benchmarks** — `openviewpartners.com/reports` — annual survey; ACV by ARR band and go-to-market motion
- **KeyBanc Capital Markets SaaS Survey** — search `keybanc saas survey [year] average contract value` — public summary published annually
- **Bessemer Cloud Index / State of the Cloud** — `bvp.com/atlas/state-of-the-cloud` — ACV / ARR benchmarks for cloud businesses
- **SaaStr Annual** — search `saastr average contract value [category] [year]` — blog posts with benchmark data from practitioners
- **Tomasz Tunguz benchmarks** — search `tomtunguz.com ACV [category]` — data-driven VC blog with SaaS metrics

*Category-specific pricing benchmarks (search these when relevant):*
- `<category> enterprise SaaS average contract value benchmark [year]`
- `<category> pricing model per seat per usage [year]`
- `<competitor name> pricing page` — infer ACV from publicly listed tiers
- `<competitor name> ARR customers` — revenue ÷ customer count = implied ACV

Model 2–3 scenarios:
- If expert WTP data exists: use it as the conservative (entry) scenario; use founder's claimed ACV as the optimistic scenario
- If only founder data exists: use it as mid; build conservative (-40%) and optimistic (+50%) variants
- If no internal data: build 3 scenarios from benchmarks and flag as estimated

If sources conflict, model each as a separate named scenario (e.g. "Expert WTP" vs "Founder Claim") and explain the gap in the Notes block.

### 5. Calculate TAM / SAM / SOM

For each region × ACV scenario:

```
TAM = # ICP companies (total universe) × ACV
SAM = TAM × ICP Fit %
SOM = SAM × Market Share %
```

ICP Fit % = fraction of total universe that truly matches (right size, right pain, right readiness). Be conservative (20–50%).
Market Share % = realistic obtainable share given stage and GTM. Be conservative (0.5–5% for beachhead).

### 6. Find top-down market reports

Aim for **3–5 independent top-down references** across at least two source tiers. Search in parallel:
- `<category> total addressable market 2025 2030 billion forecast`
- `<category> market size CAGR site:gartner.com OR site:idc.com OR site:forrester.com`
- `<category> market report 2025 Grand View Research OR MarketsandMarkets OR Mordor Intelligence`

**Tier 1 — Analyst firms (most credible; full reports paywalled but press releases/summaries are free):**
- **Gartner** — `gartner.com/en/newsroom` — search press releases for forecast numbers
- **IDC** — `idc.com/getdoc.jsp` or search `IDC worldwide [category] forecast [year]`
- **Forrester** — `forrester.com/research` — search for market size reports; summaries often indexed
- **McKinsey Global Institute** — `mckinsey.com/mgi` — look for industry deep-dives with market size data

**Tier 2 — Market research publishers (widely cited; methodology varies):**
- **Grand View Research** — `grandviewresearch.com` — strong CAGR data; search `[category] market size`
- **MarketsandMarkets** — `marketsandmarkets.com` — detailed segment breakdowns
- **Mordor Intelligence** — `mordorintelligence.com` — often has niche sub-segment reports
- **Precedence Research** — `precedenceresearch.com` — good for emerging tech categories
- **Straits Research** — `straitsresearch.com` — alternative estimate for cross-checking
- **IMARC Group** — `imarcgroup.com` — useful for APAC and global figures

**Tier 3 — Investment & ecosystem reports (strong for tech/startup markets):**
- **a16z** — `a16z.com` — annual state-of-market reports for key tech categories (AI, fintech, bio, etc.)
- **Bessemer / BVP Atlas** — `bvp.com/atlas` — cloud and SaaS market reports
- **Dealroom State of European Tech** — `dealroom.co/reports` — EU market sizes and growth
- **CB Insights** — `cbinsights.com/research` — market maps and sizing for startup categories
- **PitchBook-NVCA Venture Monitor** — `pitchbook.com/news/reports` — sector investment data implies market size

**Tier 4 — Industry bodies and government (use for regulated / traditional industries):**
- BIS (Bank for International Settlements) — for fintech/banking
- EBA / ECB — for European banking/payments market data
- GSMA Intelligence — for telecoms
- IEA / Eurostat Energy — for energy/utilities
- WHO / EMA / FDA — for healthcare/pharma

Record: market name, current size, forecast year + size, CAGR, exact source URL. Always fetch and read the actual source page — do not cite a number you haven't verified.

### 7. Generate the Excel file

Install openpyxl if needed: `pip install openpyxl`

Write and execute a Python script to produce the `.xlsx` file with **two sheets**:

---

#### Sheet 1: "Bottom-Up" — one block per ACV scenario

**Structure per scenario block:**

Row 1 of block: Scenario label (e.g. "V1 – Initial ACV") — bold, merged across all columns, darker background.

Row 2 of block: Column headers:
`Region | # ICP Companies | ACV (€) | ICP Fit % | TAM (€) | Market Share % | SAM (€) | Source`

Data rows: one per region, then a **Total** row (summing TAM and SAM).

After the last scenario block: a **Notes / Assumptions** text block explaining:
- ICP definition and source (founder description, Alphasights brief, expert call, or inferred)
- ACV basis: which internal source anchored each scenario (expert WTP, Alphasights brief, pitch deck, or external benchmark)
- Reasoning for ICP Fit % values
- Reasoning for Market Share % values
- Data vintage (year of statistics used)
- Internal sources consulted: list Granola meetings, Superhuman threads, and Google Drive docs used
- Any conflicts between internal sources and how they were resolved in the model

---

#### Sheet 2: "Top-Down" — validation table

Column headers:
`Category | Unit | Current Year Amount | Forecast Year Amount | CAGR | Source`

Rows:
- One row per external top-down market report (Gartner, MarketsandMarkets, etc.)
- One row for the **founder's own TAM claim** (sourced from pitch deck or Granola/Superhuman — label it "Founder claim – [source]")
- One row for the **expert / Alphasights implied market** if any expert gave a market size opinion
- Bottom row: your bottom-up SAM (V2 / mid scenario) as the cross-reference anchor

---

#### Styling rules (match existing DD files exactly):

- **Header rows**: bold, light grey background `D9D9D9`, thin borders on all sides
- **Scenario label rows**: bold, medium grey background `BFBFBF`, merged across all data columns
- **Total rows**: bold, light blue background `DDEEFF`
- **Notes section**: italic, no border, light yellow background `FFFDE7`, merged across columns
- **Source column**: blue font `0000FF`, underlined to indicate hyperlink
- Freeze top row of each sheet (`A2`)
- Auto-fit column widths (cap at 80 chars for Source, 40 for others)
- Sheet names: `Bottom-Up` and `Top-Down`

---

#### Python script template:

```python
import openpyxl
from openpyxl.styles import Font, PatternFill, Alignment, Border, Side, colors
from openpyxl.utils import get_column_letter

wb = openpyxl.Workbook()

# ── SHEET 1: Bottom-Up ──────────────────────────────────────────────
ws1 = wb.active
ws1.title = "Bottom-Up"

startup_name = "..."
generated_date = "..."

headers = ["Region", "# ICP Companies", "ACV (€)", "ICP Fit %",
           "TAM (€)", "Market Share %", "SAM (€)", "Source"]
n_cols = len(headers)

thin = Side(style="thin")
border = Border(left=thin, right=thin, top=thin, bottom=thin)

grey_fill   = PatternFill("solid", fgColor="D9D9D9")
dark_fill   = PatternFill("solid", fgColor="BFBFBF")
blue_fill   = PatternFill("solid", fgColor="DDEEFF")
yellow_fill = PatternFill("solid", fgColor="FFFDE7")

def write_header_row(ws, row, headers):
    for col, h in enumerate(headers, 1):
        c = ws.cell(row, col, h)
        c.font = Font(bold=True)
        c.fill = grey_fill
        c.border = border
        c.alignment = Alignment(wrap_text=True, vertical="top", horizontal="center")

# scenarios: list of dicts with keys: label, regions
# each region: {name, count, acv, icp_fit_pct, market_share_pct, source}
scenarios = [
    {
        "label": "V1 – Initial ACV",
        "regions": [
            {"name": "Europe", "count": 0, "acv": 0, "icp_fit_pct": 0.40,
             "market_share_pct": 0.007, "source": "https://..."},
            {"name": "USA",    "count": 0, "acv": 0, "icp_fit_pct": 0.40,
             "market_share_pct": 0.005, "source": "https://..."},
        ]
    },
    # add more scenarios...
]

current_row = 1
for scenario in scenarios:
    # Scenario label row
    c = ws1.cell(current_row, 1, scenario["label"])
    c.font = Font(bold=True)
    c.fill = dark_fill
    ws1.merge_cells(start_row=current_row, start_column=1,
                    end_row=current_row, end_column=n_cols)
    current_row += 1

    # Header row
    write_header_row(ws1, current_row, headers)
    current_row += 1

    first_data_row = current_row
    for r in scenario["regions"]:
        row_data = [
            r["name"],
            r["count"],
            r["acv"],
            r["icp_fit_pct"],
            None,  # TAM — formula written below
            r["market_share_pct"],
            None,  # SAM — formula written below
            r["source"],
        ]
        for col, val in enumerate(row_data, 1):
            cell = ws1.cell(current_row, col, val)
            cell.border = border
            if col == 8:  # Source
                cell.font = Font(color="0000FF", underline="single")
            if col in (4, 6):  # percentages
                cell.number_format = "0%"
            if col in (5, 7):  # currency
                cell.number_format = '#,##0 "€"'
        # TAM formula: =B*C*D  (count × ACV × ICP fit %)
        tam_cell = ws1.cell(current_row, 5)
        tam_cell.value = f"=B{current_row}*C{current_row}*D{current_row}"
        tam_cell.number_format = '#,##0 "€"'
        tam_cell.border = border
        # SAM formula: =E*F  (TAM × market share %)
        sam_cell = ws1.cell(current_row, 7)
        sam_cell.value = f"=E{current_row}*F{current_row}"
        sam_cell.number_format = '#,##0 "€"'
        sam_cell.border = border
        current_row += 1

    last_data_row = current_row - 1
    # Total row — SUM formulas so totals recalculate when inputs change
    total_row_data = ["Total", "", "", "", None, "", None, ""]
    for col, val in enumerate(total_row_data, 1):
        cell = ws1.cell(current_row, col, val)
        cell.font = Font(bold=True)
        cell.fill = blue_fill
        cell.border = border
        if col in (5, 7):
            cell.number_format = '#,##0 "€"'
    ws1.cell(current_row, 5).value = f"=SUM(E{first_data_row}:E{last_data_row})"
    ws1.cell(current_row, 5).number_format = '#,##0 "€"'
    ws1.cell(current_row, 5).font = Font(bold=True)
    ws1.cell(current_row, 5).fill = blue_fill
    ws1.cell(current_row, 5).border = border
    ws1.cell(current_row, 7).value = f"=SUM(G{first_data_row}:G{last_data_row})"
    ws1.cell(current_row, 7).number_format = '#,##0 "€"'
    ws1.cell(current_row, 7).font = Font(bold=True)
    ws1.cell(current_row, 7).fill = blue_fill
    ws1.cell(current_row, 7).border = border
    current_row += 2  # blank row between scenarios

# Notes block
notes_text = (
    "Notes & Assumptions\n"
    "ICP Definition: ...\n"
    "ICP Fit %: ...\n"
    "Market Share %: ...\n"
    "Data vintage: ..."
)
c = ws1.cell(current_row, 1, notes_text)
c.font = Font(italic=True)
c.fill = yellow_fill
c.alignment = Alignment(wrap_text=True, vertical="top")
ws1.merge_cells(start_row=current_row, start_column=1,
                end_row=current_row + 4, end_column=n_cols)
ws1.row_dimensions[current_row].height = 80

ws1.freeze_panes = "A2"

# ── SHEET 2: Top-Down ────────────────────────────────────────────────
ws2 = wb.create_sheet("Top-Down")
td_headers = ["Category", "Unit", "Current Year Amount", "Forecast Year Amount", "CAGR", "Source"]

write_header_row(ws2, 1, td_headers)

top_down_rows = [
    # {"category": "...", "unit": "...", "current": 0, "forecast": 0, "cagr": 0.0, "source": "https://..."},
]
for row_idx, td in enumerate(top_down_rows, start=2):
    row_data = [td["category"], td["unit"], td["current"],
                td["forecast"], td["cagr"], td["source"]]
    for col, val in enumerate(row_data, 1):
        cell = ws2.cell(row_idx, col, val)
        cell.border = border
        if col == 5:
            cell.number_format = "0%"
        if col == 6:
            cell.font = Font(color="0000FF", underline="single")

ws2.freeze_panes = "A2"

# Auto-fit widths
for ws in [ws1, ws2]:
    for col in ws.columns:
        max_len = 0
        col_letter = get_column_letter(col[0].column)
        for cell in col:
            if cell.value:
                for line in str(cell.value).split("\n"):
                    max_len = max(max_len, len(line))
        cap = 80 if col[0].column == n_cols else 40
        ws.column_dimensions[col_letter].width = min(max_len + 2, cap)

output_path = "..."
wb.save(output_path)
print(f"Saved: {output_path}")
```

Fill all data variables with actual research results, then execute.

### 7b. Optional Sheet 3: "Value-Theory"

**Skip by default.** Add this sheet only when: (a) the startup is creating a new category with no ACV benchmark, (b) it displaces significant existing spend (e.g. replacing a manual process costing >€100k/yr per company), or (c) bottom-up and top-down diverge by >5×. See `references/market-sizing-guide.md` for when to use Value-Theory and how to set the WTP % assumption.

#### Sheet 3 layout

Column headers: `Input | Value | Source / Reasoning`

Rows (one per assumption):
1. Problem cost per customer (€/yr)
2. % of problem solved by product
3. Value created per customer (= row 1 × row 2)
4. Willingness-to-pay % (10–30%)
5. Implied ACV (= row 3 × row 4)
6. Total ICP customers
7. TAM — Value Theory (= row 5 × row 6)
8. ICP Fit % (same as Bottom-Up sheet)
9. SAM — Value Theory (= row 7 × row 8)
10. Market Share % (same as Bottom-Up sheet)
11. SOM — Value Theory (= row 9 × row 10)

After the table: a comparison row showing Value-Theory ACV vs. Bottom-Up ACV and whether they are aligned (within 2–3×), with a 2–3 sentence interpretation.

Styling: same grey headers, blue Total rows. Sheet name: `Value-Theory`.

```python
# Only add if value_theory_applicable is True
value_theory_applicable = True  # set False to skip

if value_theory_applicable:
    ws3 = wb.create_sheet("Value-Theory")

    vt_headers = ["Input", "Value", "Source / Reasoning"]
    for ci, h in enumerate(vt_headers, 1):
        c = ws3.cell(1, ci, h)
        c.font = Font(bold=True); c.fill = grey_fill
        c.border = border; c.alignment = Alignment(wrap_text=True)

    # Fill these from research:
    problem_cost     = 0      # €/yr per customer
    pct_solved       = 0.40   # 40%
    value_per_cust   = problem_cost * pct_solved
    wtp_pct          = 0.20   # 20%
    implied_acv      = value_per_cust * wtp_pct
    total_icp        = 0      # from Step 2 ICP count
    icp_fit_pct      = 0.35
    market_share_pct = 0.01

    tam_vt = implied_acv * total_icp
    sam_vt = tam_vt * icp_fit_pct
    som_vt = sam_vt * market_share_pct

    vt_rows = [
        ("Problem cost per customer (€/yr)",   f"€{problem_cost:,.0f}",   "[source]"),
        ("% of problem solved by product",      f"{pct_solved:.0%}",       "[reasoning]"),
        ("Value created per customer",          f"€{value_per_cust:,.0f}", "= row 1 × row 2"),
        ("Willingness-to-pay %",               f"{wtp_pct:.0%}",          "Typical SaaS: 10–30%"),
        ("Implied ACV",                         f"€{implied_acv:,.0f}",    "= row 3 × row 4"),
        ("Total ICP customers",                 f"{total_icp:,}",          "[source]"),
        ("TAM — Value Theory",                  f"€{tam_vt:,.0f}",         "= ACV × ICP count"),
        ("ICP Fit %",                           f"{icp_fit_pct:.0%}",      "Same as Bottom-Up"),
        ("SAM — Value Theory",                  f"€{sam_vt:,.0f}",         "= TAM × ICP Fit %"),
        ("Market Share %",                      f"{market_share_pct:.0%}", "Same as Bottom-Up"),
        ("SOM — Value Theory",                  f"€{som_vt:,.0f}",         "= SAM × Market Share %"),
    ]
    for ri, (label, val, src) in enumerate(vt_rows, 2):
        is_total = "TAM" in label or "SAM" in label or "SOM" in label
        for ci, v in enumerate([label, val, src], 1):
            c = ws3.cell(ri, ci, v)
            c.border = border
            c.alignment = Alignment(wrap_text=True, vertical="top")
            if is_total:
                c.font = Font(bold=True); c.fill = blue_fill

    # Comparison note
    note_row = len(vt_rows) + 3
    note = (
        f"Value-Theory ACV: €{implied_acv:,.0f} vs. Bottom-Up ACV: €[bottom_up_acv]. "
        "Divergence: [X×]. Interpretation: [aligned / pricing risk / problem cost overestimated]."
    )
    c = ws3.cell(note_row, 1, note)
    c.font = Font(italic=True); c.fill = yellow_fill
    c.alignment = Alignment(wrap_text=True, vertical="top")
    ws3.merge_cells(start_row=note_row, start_column=1, end_row=note_row + 2, end_column=3)

    ws3.freeze_panes = "A2"
    for col in ws3.columns:
        maxw = max((len(str(c.value)) for c in col if c.value), default=10)
        ws3.column_dimensions[get_column_letter(col[0].column)].width = min(maxw + 2, 60)
```

---

### 8. Verify the file

```bash
ls -lh <output_path>
python3 -c "
import openpyxl
wb = openpyxl.load_workbook('<output_path>')
for name in wb.sheetnames:
    ws = wb[name]
    print(f'{name}: {ws.max_row} rows x {ws.max_column} cols')
"
```

Expected: `Bottom-Up`, `Top-Down`, and optionally `Value-Theory`.

Then send the file to the user using the `SendUserFile` tool so they can download it directly from Claude.

### 9. Commit and push

Do **not** commit or push the generated `.xlsx` file — it is a per-deal output, not part of the skills repo.

## Output to user

When done, report:
- File path and size
- Scenarios modelled (label + total SAM for each)
- Top-down sources used
- Git commit short hash
- Any data gaps / estimates flagged

## Important notes

- **Every company count must have a source URL in the Source column.** If no authoritative source found, write `Estimated – [brief reason]` and flag it in the notes.
- **ACV must be justified.** Cite public pricing, a benchmark, or the founder's own materials.
- **ICP Fit % and Market Share % must be explained in the Notes block.** Do not just put a number.
- Do not use ChatGPT, Claude, or AI tools as sources. Only primary data.
- Bottom-up is always primary. Top-down sheet is a sanity check only.
- If the startup targets a niche (e.g. pharma manufacturers only), apply NACE/SIC filtering in the ICP count — do not use the full industry universe.
- Currency: use €  if the startup is EU-based, $ if US-based. Be consistent across the file.
- The file format must match the existing DD Excel examples: two sheets, scenario blocks, sourced rows.


---

## Language and Tone

- Use expert terminology appropriate to the context (VC due diligence, financial analysis, competitive intelligence)
- Avoid superfluous prose, self-references, expert advice disclaimers, and apologies
- No em dashes or en dashes; use commas, parentheses, or rewrite the sentence instead
- Lead with data and specific findings; interpretation follows the evidence
- No superlatives (world-class, revolutionary, best-in-class, cutting-edge)

