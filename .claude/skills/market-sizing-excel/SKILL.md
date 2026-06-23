---
name: market-sizing-excel
description: Research a startup being evaluated in due diligence and generate an Excel (.xlsx) Market Sizing file with bottom-up TAM/SAM/SOM tables by region, scenarios, top-down validation, and full source citations. Use when asked to create a market sizing Excel, build a market sizing spreadsheet, or generate a TAM/SAM file.
---

# Market Sizing — Excel Output Skill

Research a startup's addressable market and produce a `.xlsx` file with a bottom-up market sizing in the standard DD format, including sourced data, scenario tables, and a top-down sanity check sheet.

## Input

Invoked as `/market-sizing-excel <Startup Name>` or with a company name / URL in args.
Optional: the user may pass a target output path. Default: `./<StartupName>__Market_Sizing.xlsx`

## Workflow

Keep the user updated with one-line status messages.

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

Use `WebSearch`:
- `number of [industry] companies [region] >250 employees eurostat`
- `[industry] enterprise count [region] statistics`

Authoritative sources (in priority order):
- **EU**: Eurostat structural business statistics — `site:eurostat.eu`
- **USA**: US Census Bureau County Business Patterns — `site:census.gov` or NAM for manufacturing (`site:nam.org`)
- **UK**: ONS Business population estimates — `site:ons.gov.uk`
- **Global**: World Bank, OECD, trade association reports
- **Fallback**: Statista, IBISWorld, Grand View Research

Record the exact source URL for each region's count. Do not estimate counts without a source.

### 4. Determine ACV scenarios

Model 2–3 ACV scenarios (e.g. Initial / Mid-Market / Full Rollout, or V1 / V2 / Enterprise):
- Use any public pricing data
- Benchmark against comparable SaaS in the same category
- Reference the startup's own pitch materials if available

Search:
- `"<startup name>" pricing`
- `<category> SaaS ACV benchmark average contract value`

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

Search for 2–3 authoritative top-down market size references:
- `<category> total addressable market 2024 2025 billion forecast`
- `<category> market size CAGR Gartner IDC Grand View`

Record: market size (current + forecast year), CAGR, source URL.

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
- ICP definition
- Reasoning for ICP Fit % values
- Reasoning for Market Share % values
- Data vintage (year of statistics used)

---

#### Sheet 2: "Top-Down" — validation table

Column headers:
`Category | Unit | Current Year Amount | Forecast Year Amount | CAGR | Source`

Rows: one per top-down reference / category.
Bottom row: implied addressable market at the startup's take-rate or penetration assumption (cross-reference to bottom-up SAM).

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

    total_tam = 0
    total_sam = 0
    for r in scenario["regions"]:
        tam = r["count"] * r["acv"] * r["icp_fit_pct"]
        sam = tam * r["market_share_pct"]
        total_tam += tam
        total_sam += sam
        row_data = [
            r["name"],
            r["count"],
            r["acv"],
            r["icp_fit_pct"],
            round(tam),
            r["market_share_pct"],
            round(sam),
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
        current_row += 1

    # Total row
    total_row_data = ["Total", "", "", "", round(total_tam), "", round(total_sam), ""]
    for col, val in enumerate(total_row_data, 1):
        cell = ws1.cell(current_row, col, val)
        cell.font = Font(bold=True)
        cell.fill = blue_fill
        cell.border = border
        if col in (5, 7):
            cell.number_format = '#,##0 "€"'
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

### 9. Commit and push

```bash
git add <output_path>
git commit -m "Add market sizing: <Startup Name>"
git push -u origin <current-branch>
```

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
