---
name: competitive-analysis-excel
description: Research competitors for a startup being evaluated in due diligence and generate an Excel (.xlsx) Competitor Overview file matching the standard DD format. Use when asked to create a competitor Excel, build a competitive analysis spreadsheet, or generate a comp overview file.
---

# Competitive Analysis — Excel Output Skill

Research a startup's competitive landscape and produce an `.xlsx` file in the standard DD Competitor Overview format.

## Input

Invoked as `/competitive-analysis-excel <Startup Name>` or with a company name / URL in args.
Optional: the user may also pass a target output path. Default path: `./<StartupName>__Competitor_Overview.xlsx`

## Workflow

Keep the user updated with one-line status messages as you go.

### 1. Resolve the target company

Use `mcp__Specter__find_company` with the startup name or URL.
Then call `mcp__Specter__get_company_profile` + `mcp__Specter__get_company_intelligence` on the resolved ID.

Extract and cache:
- Full name, website, HQ country code (2-letter ISO), headcount, founding year
- Product description (max 200 chars)
- Last funding (format: `$Xm\nRound Year`, e.g. `$12M\nSeries A 2024`; write `N/A` if unknown)
- Investor names (comma-separated, max ~60 chars)
- Primary product category / market segment

### 2. Identify competitors

Use `mcp__Specter__find_similar_companies` on the resolved company ID.

Supplement with `WebSearch`:
- `"<startup name>" competitors 2024 2025`
- `best alternatives to "<startup name>"`

Build a deduplicated list of 10–20 competitors across:
- **Direct** – same product, same customer, same stage
- **Adjacent** – same problem, different approach or segment
- **Incumbent** – established players the startup displaces

### 3. Research each competitor

For each competitor call `mcp__Specter__find_company` → `mcp__Specter__get_company_profile`.

Collect for each:
- Company name, URL, HQ country code, headcount (number), founding year (number)
- Product description (max 200 chars)
- Last funding string (format as above; `Acquired` or `Incumbent` for those cases)
- Investor names (max ~60 chars)
- Key Difference from the target startup (max 200 chars — specific, not vague)
- Category (e.g. "Direct Competitor", "Adjacent", "Incumbent")

Use `WebSearch` to fill gaps when Specter returns no result.

### 4. Determine feature dimensions

Analyse the target company's product and decide on 5–8 binary feature/capability dimensions that matter in this market. These become the feature-check columns in the Excel.

Examples by domain:
- **Procurement / spend analytics**: Supplier data & discovery | Spend analytics & intelligence | Procurement automation | AI-driven automation | Real-time insights | Supplier-data enrichment | Workflow automation
- **Robotics / safety**: HW / SW | Robot type | Use case focus | State (static/dynamic) | Human/AMR interaction | Human behavior prediction
- **Voice AI**: Model type | Tool/Func. call | State transitions | SIP/Telephony | REST API | Agent builder | Orch. SDK | Self-hosting | Multilingual | Open source | Inbound calls | Outbound calls
- **Cybersecurity / identity**: Category (sub-segment) | Open source

Use domain knowledge and the target company's feature set to decide the right dimensions. Aim for dimensions where competitors meaningfully differ.

### 5. Score feature dimensions

For each company (target + competitors), mark each dimension:
- `x` = feature present / confirmed
- `(x)` = partial or limited support
- leave blank / `None` = feature absent or unknown

Base scores on Specter intelligence, company websites, and web search results. Do not guess — if unknown leave blank.

### 6. Generate the Excel file

Write a Python script and execute it with `Bash` to create the `.xlsx` file.

#### Excel structure (match exactly):

**Row 1 — Section header row:**
- A1: empty
- B1: `Overview` (merged across Overview columns)
- Next group start: `Category` (merged if needed)
- Next group start: `Features` (merged if needed)

**Row 2 — Column header row:**
`Company Name | URL | Location | Headcount | Founded | Product Description | Last Funding | Investors | Key Difference | Category | [feature col 1] | [feature col 2] | ...`

**Row 3 — Target startup (always first data row)**

**Rows 4+ — Competitors** (Direct first, then Adjacent, then Incumbents)

#### Styling:
- Header row (row 2): bold, light grey background (`D9D9D9`), thin borders
- Section header row (row 1): bold, slightly darker grey (`BFBFBF`)
- Target startup row: light yellow background (`FFFACD`) to highlight it
- Feature columns: centre-align, use a checkmark-friendly font
- Freeze top 2 rows
- Auto-fit column widths (cap at 60 chars)
- Sheet name: `Tabellenblatt1` (to match existing files)

#### Python script template:

```python
import openpyxl
from openpyxl.styles import Font, PatternFill, Alignment, Border, Side
from openpyxl.utils import get_column_letter

wb = openpyxl.Workbook()
ws = wb.active
ws.title = "Tabellenblatt1"

# --- data ---
startup_name = "..."
feature_cols = [...]  # list of feature dimension names
companies = [
    {
        "name": "...", "url": "...", "location": "...", "headcount": ...,
        "founded": ..., "description": "...", "funding": "...",
        "investors": "...", "key_diff": "...", "category": "...",
        "features": {"Feature 1": "x", "Feature 2": None, ...}
    },
    # ... more companies
]

overview_cols = ["Company Name","URL","Location","Headcount","Founded",
                 "Product Description","Last Funding","Investors","Key Difference","Category"]
all_cols = overview_cols + feature_cols
n_overview = len(overview_cols)
n_feature = len(feature_cols)
total_cols = len(all_cols)

# Row 1: section headers
ws.cell(1, 1, None)
ws.cell(1, 2, "Overview")
ws.merge_cells(start_row=1, start_column=2, end_row=1, end_column=n_overview)
if n_feature > 0:
    ws.cell(1, n_overview + 1, "Features")
    if n_feature > 1:
        ws.merge_cells(start_row=1, start_column=n_overview+1, end_row=1, end_column=total_cols)

# Row 2: column headers
for col_idx, col_name in enumerate(all_cols, start=1):
    cell = ws.cell(2, col_idx, col_name)
    cell.font = Font(bold=True)
    cell.fill = PatternFill("solid", fgColor="D9D9D9")
    cell.alignment = Alignment(wrap_text=True, vertical="top")

# Data rows
for row_idx, company in enumerate(companies, start=3):
    ws.cell(row_idx, 1, company["name"])
    ws.cell(row_idx, 2, company["url"])
    ws.cell(row_idx, 3, company["location"])
    ws.cell(row_idx, 4, company["headcount"])
    ws.cell(row_idx, 5, company["founded"])
    ws.cell(row_idx, 6, company["description"])
    ws.cell(row_idx, 7, company["funding"])
    ws.cell(row_idx, 8, company["investors"])
    ws.cell(row_idx, 9, company["key_diff"])
    ws.cell(row_idx, 10, company["category"])
    for feat_idx, feat_name in enumerate(feature_cols, start=n_overview+1):
        ws.cell(row_idx, feat_idx, company["features"].get(feat_name))
    # Highlight target startup row
    if row_idx == 3:
        for col_idx in range(1, total_cols + 1):
            ws.cell(row_idx, col_idx).fill = PatternFill("solid", fgColor="FFFACD")

# Auto-fit column widths
for col_idx in range(1, total_cols + 1):
    max_len = 0
    for row in ws.iter_rows(min_col=col_idx, max_col=col_idx):
        for cell in row:
            if cell.value:
                max_len = max(max_len, len(str(cell.value)))
    ws.column_dimensions[get_column_letter(col_idx)].width = min(max_len + 2, 60)

# Freeze top 2 rows
ws.freeze_panes = "A3"

output_path = "..."
wb.save(output_path)
print(f"Saved: {output_path}")
```

Fill in all the data variables with the actual research, then execute the script.

### 7. Verify the file

After running the script, verify it was created:
```bash
ls -lh <output_path>
python3 -c "import openpyxl; wb=openpyxl.load_workbook('<output_path>'); ws=wb.active; print(f'{ws.max_row} rows x {ws.max_column} cols')"
```

### 8. Commit and push

Stage and commit the generated Excel file on the current branch:
```bash
git add <output_path>
git commit -m "Add competitor overview: <Startup Name>"
git push -u origin <current-branch>
```

## Output to user

When done, report:
- File path and size
- Number of competitors included
- Feature dimensions used
- Git commit reference (short hash)
- Any companies where data was unavailable / estimated

## Important notes

- The target startup is always row 3 (first data row). This is the baseline for Key Difference comparisons.
- Key Difference = how each competitor differs FROM the target startup, not a generic description.
- Funding format must match: `$12M\nSeries A 2024`. For incumbents write `Incumbent`. For acquired write `Acquired`. For unknown write `N/A`.
- Location = 2-letter ISO country code (e.g. `GE` for Germany, `US`, `UK`, `FR`).
- Headcount = numeric value only (e.g. `45`), not a string.
- Founded = numeric year (e.g. `2021`), not a string.
- Do not fabricate data. Mark unknown fields as `None` / blank rather than guessing.
- If `openpyxl` is not installed, run `pip install openpyxl` first.
