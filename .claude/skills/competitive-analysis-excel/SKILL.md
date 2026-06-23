---
name: competitive-analysis-excel
description: Research competitors for a startup being evaluated in due diligence and generate an Excel (.xlsx) Competitor Overview file matching the standard DD format. Use when asked to create a competitor Excel, build a competitive analysis spreadsheet, or generate a comp overview file.
---

# Competitive Analysis — Excel Output Skill

Research a startup's competitive landscape using every available knowledge source and produce a `.xlsx` Competitor Overview file in the standard DD format.

## Input

Invoked as `/competitive-analysis-excel <Startup Name>` or with a company name / URL in args.
Optional: user may pass a target output path. Default: `./<StartupName>__Competitor_Overview.xlsx`

## Workflow

Give the user a brief one-line status update as each step completes.

---

### Step 1 — Internal knowledge sweep (run all in parallel)

Pull everything the firm already knows before touching any external source. Internal intelligence takes precedence over external data.

**1a. Affinity CRM**
- `mcp__Affinity__search_companies` with the startup name to find the CRM record and its ID.
- If found, call `mcp__Affinity__get_notes_for_entity` (entity_type=1) to get all analyst notes on the startup.
- Call `mcp__Affinity__get_company_list_entries` to check which pipeline lists it's on and what stage.
- Run `mcp__Affinity__semantic_search` with _"competitors to [startup name] in [inferred market]"_ to surface related companies already tracked in the CRM.
- For any CRM-tracked competitors found, also pull their notes with `get_notes_for_entity`.

**1b. Granola meeting notes**
- `mcp__Granola__query_granola_meetings` with:
  - _"[startup name] — what did the founders say about their competitors?"_
  - _"[startup name] — product differentiation, market positioning"_
  - _"[startup name] — risks, competitive threats, concerns raised"_
- Call `mcp__Granola__get_meetings` on any relevant meeting IDs for full summaries and private notes.
- Extract any competitor names, market observations, or founder quotes about competition.

**1c. Email (Superhuman)**
- `mcp__Superhuman__query_email_and_calendar`:
  - _"What emails do we have about [startup name]?"_
  - _"Any analyst reports, market research, or competitor comparisons related to [startup name] or [inferred market]?"_
- `mcp__Superhuman__list_threads` with `subject_contains` or `body_contains` = startup name to find pitch emails, forwarded research, follow-ups.
- Extract competitor mentions, market intel, and any analyst commentary.

**1d. Google Drive**
- `mcp__Google_Drive__search_files` with `fullText contains '[startup name]'` to find memos, decks, prior analyses.
- `mcp__Google_Drive__search_files` with `title contains 'Competitor'` or `title contains 'Landscape'` to find prior competitive analyses.
- Call `mcp__Google_Drive__read_file_content` on relevant files (investment memos, pitch decks, competitor overviews).
- Extract: competitor names mentioned, feature comparisons already done, analyst observations.

Consolidate Step 1 findings. Note which competitors were already known internally and any views/scores already attached to them.

---

### Step 2 — Resolve the target company via Specter

- `mcp__Specter__find_company` with the startup name or URL.
- `mcp__Specter__get_company_profile` + `mcp__Specter__get_company_intelligence` on the resolved ID.

Extract and cache:
- Full name, website, HQ country (ISO-2 code), headcount (number), founding year (number)
- Product description (≤200 chars)
- Last funding: format as `$Xm\nRound Year` (e.g. `$12M\nSeries A 2024`); `N/A` if unknown
- Investor names (≤60 chars, comma-separated)
- Primary product category / market segment

---

### Step 3 — Identify competitors

Triangulate from three sources:

**3a. Specter**
- `mcp__Specter__find_similar_companies` on the resolved company ID.

**3b. Evertrace talent signals**
- `mcp__Evertrace__search_companies` for the startup name to get its entity ID (exe_* format).
- `mcp__Evertrace__list_signals` with `past_companies=[entity_id]` — surfaces people from the startup's talent pool and which other companies they've worked at, revealing the real competitive talent ecosystem.
- Repeat for 2–3 named competitors to map cross-company talent flows.

**3c. Web search**
- `WebSearch`: _"[startup name] competitors 2024 2025"_
- `WebSearch`: _"best alternatives to [startup name]"_
- `WebSearch`: _"[market category] competitive landscape [year]"_

Merge all sources. Prioritise companies appearing in multiple sources or flagged in Step 1 internal intel.
Target 10–20 competitors:
- **Direct** — same product, same buyer, same stage
- **Adjacent** — same problem, different approach or segment
- **Incumbent** — established players being displaced

---

### Step 4 — Research each competitor

For each competitor, collect data from multiple sources:

| Source | What to call |
|--------|-------------|
| Specter | `find_company` → `get_company_profile` for core data |
| Affinity | `search_companies` → if found, `get_notes_for_entity` for internal analyst notes |
| Granola | `query_granola_meetings` — any calls with this competitor? |
| Superhuman | `query_email_and_calendar` — any email intel about this competitor? |
| Web | `WebSearch` or `WebFetch` their site to fill remaining gaps |

Collect for each:
- Company name, URL, HQ (ISO-2), headcount (number), founding year (number)
- Product description (≤200 chars)
- Last funding string (format: `$Xm\nRound Year`; `Acquired` / `Incumbent` / `N/A` as appropriate)
- Investor names (≤60 chars)
- **Key Difference** from the target startup (≤200 chars — specific: *how* they differ in approach, focus, customer, not just "less advanced")
- Category: Direct / Adjacent / Incumbent

---

### Step 5 — Determine feature dimensions

Based on the target startup's product, pick 5–8 binary dimensions that best differentiate competitors in this market. Use Step 1 internal intel and product research to choose dimensions the founders and analysts have actually discussed.

Examples by domain:
- **Procurement / spend**: Supplier data & discovery | Spend analytics & intelligence | Procurement automation | AI-driven automation | Real-time insights | Supplier-data enrichment | Workflow automation
- **Robotics / safety**: HW / SW | Robot type | Use case focus | State (static/dynamic) | Human/AMR interaction | Human behavior prediction
- **Voice AI**: Model type | Tool/Func. call | State transitions | SIP/Telephony | REST API | Agent builder | Orch. SDK | Self-hosting | Multilingual | Open source | Inbound calls | Outbound calls
- **AI / cybersecurity**: Category sub-segment | Open source | Runtime detection | Red-teaming | NHI/secrets mgmt

---

### Step 6 — Score feature dimensions

For each company (target + all competitors), mark each feature dimension:
- `x` = confirmed present
- `(x)` = partial or limited
- leave blank = absent or unknown

Base on: Specter intelligence, company websites, product pages, Granola founder call notes (what founders claimed), email intel. Do not guess — blank if uncertain.

---

### Step 7 — Generate the Excel file

Install openpyxl if needed:
```bash
pip install openpyxl -q
```

Write and execute a Python script to generate the file. Use this structure exactly:

#### Excel layout

**Row 1 — Section header row:**
- A1: empty
- B1: `Overview` (merged across Overview columns B–J)
- K1: `Features` (merged across all feature columns, if >1)

**Row 2 — Column headers:**
`Company Name | URL | Location | Headcount | Founded | Product Description | Last Funding | Investors | Key Difference | Category | [Feature 1] | [Feature 2] | …`

**Row 3 — Target startup** (always first data row)

**Rows 4+ — Competitors** (Direct first, then Adjacent, then Incumbents)

#### Styling
- Row 1 section headers: bold, `BFBFBF` fill
- Row 2 column headers: bold, `D9D9D9` fill, thin borders, wrap text
- Row 3 target startup: `FFFACD` (light yellow) fill on all cells
- Feature cells: centre-aligned
- Freeze rows 1–2 (freeze pane at A3)
- Auto-fit column widths, capped at 60 characters
- Sheet name: `Tabellenblatt1`

#### Python script template

```python
import openpyxl
from openpyxl.styles import Font, PatternFill, Alignment, Border, Side
from openpyxl.utils import get_column_letter

wb = openpyxl.Workbook()
ws = wb.active
ws.title = "Tabellenblatt1"

# --- populate these from your research ---
feature_cols = [...]   # list of feature dimension names
companies = [
    {
        "name": "...", "url": "...", "location": "...",
        "headcount": ..., "founded": ..., "description": "...",
        "funding": "...", "investors": "...", "key_diff": "...",
        "category": "...",
        "features": {"Feature 1": "x", "Feature 2": None, ...}
    },
    # ... remaining companies
]

overview_cols = [
    "Company Name", "URL", "Location", "Headcount", "Founded",
    "Product Description", "Last Funding", "Investors", "Key Difference", "Category"
]
all_cols = overview_cols + feature_cols
n_ov = len(overview_cols)
n_ft = len(feature_cols)
total = len(all_cols)

# Row 1: section headers
ws.cell(1, 2).value = "Overview"
ws.cell(1, 2).font = Font(bold=True)
ws.cell(1, 2).fill = PatternFill("solid", fgColor="BFBFBF")
ws.merge_cells(start_row=1, start_column=2, end_row=1, end_column=n_ov)
if n_ft > 0:
    ws.cell(1, n_ov + 1).value = "Features"
    ws.cell(1, n_ov + 1).font = Font(bold=True)
    ws.cell(1, n_ov + 1).fill = PatternFill("solid", fgColor="BFBFBF")
    if n_ft > 1:
        ws.merge_cells(start_row=1, start_column=n_ov+1, end_row=1, end_column=total)

# Row 2: column headers
thin = Side(style="thin")
border = Border(left=thin, right=thin, top=thin, bottom=thin)
for ci, name in enumerate(all_cols, 1):
    c = ws.cell(2, ci, name)
    c.font = Font(bold=True)
    c.fill = PatternFill("solid", fgColor="D9D9D9")
    c.alignment = Alignment(wrap_text=True, vertical="top")
    c.border = border

# Data rows
for ri, co in enumerate(companies, 3):
    vals = [co["name"], co["url"], co["location"], co["headcount"], co["founded"],
            co["description"], co["funding"], co["investors"], co["key_diff"], co["category"]]
    for ci, v in enumerate(vals, 1):
        ws.cell(ri, ci, v)
    for fi, fname in enumerate(feature_cols, n_ov + 1):
        c = ws.cell(ri, fi, co["features"].get(fname))
        c.alignment = Alignment(horizontal="center")
    if ri == 3:  # target startup highlight
        for ci in range(1, total + 1):
            ws.cell(ri, ci).fill = PatternFill("solid", fgColor="FFFACD")

# Column widths
for ci in range(1, total + 1):
    maxw = max(
        (len(str(ws.cell(r, ci).value)) for r in range(1, ws.max_row + 1) if ws.cell(r, ci).value),
        default=8
    )
    ws.column_dimensions[get_column_letter(ci)].width = min(maxw + 2, 60)

ws.freeze_panes = "A3"

output_path = "..."   # fill in
wb.save(output_path)
print(f"Saved: {output_path}")
```

Fill all data variables with real research, then execute with `Bash`.

---

### Step 8 — Verify the output

```bash
ls -lh <output_path>
python3 -c "
import openpyxl
wb = openpyxl.load_workbook('<output_path>')
ws = wb.active
print(f'{ws.max_row} rows x {ws.max_column} cols — OK')
"
```

---

### Step 9 — Commit and push

```bash
git add <output_path>
git commit -m "Add competitor overview: <Startup Name>"
git push -u origin <current-branch>
```

---

## Report to user when done

- File path and size
- Number of competitors included (broken down by tier: Direct / Adjacent / Incumbent)
- Feature dimensions used
- Which internal sources contributed (Affinity notes, Granola meetings, emails, Drive files) — list specific documents or meeting titles where useful
- Git commit short hash
- Any companies where data was unavailable or estimated

---

## Data format rules

| Field | Format |
|-------|--------|
| Location | ISO-2 country code (e.g. `GE`, `US`, `UK`, `FR`) |
| Headcount | Number only, no string (`45` not `"~45"`) |
| Founded | Numeric year (`2021` not `"2021"`) |
| Last Funding | `$Xm\nRound Year` · `Acquired` · `Incumbent` · `N/A` |
| Key Difference | How competitor differs *from target startup*, ≤200 chars |
| Feature marks | `x` confirmed · `(x)` partial · blank unknown/absent |

## Important notes

- Target startup is always row 3 (first data row) — the baseline for all Key Difference comparisons.
- Internal sources (Affinity notes, Granola founder call quotes, email threads, Drive memos) enrich Key Difference and feature scores — use them.
- Do not fabricate data. Unknown = blank. This is a DD document — accuracy over completeness.
- If a company appears in both internal and external sources and data conflicts, note the discrepancy in Key Difference or a comment cell.
