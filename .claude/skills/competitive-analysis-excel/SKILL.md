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

Pull everything the firm already knows before touching any external source. Internal intelligence takes precedence throughout.

**1a. VC Knowledge Hub** ← start here, unified semantic search across Affinity + Granola + Drive + pitch deck PDFs in one call
Run all in parallel:
- `mcp__vc-knowledge-hub__search` → _"[startup name]"_ — full firm profile: Affinity data, meeting notes, pitch deck content, Drive docs
- `mcp__vc-knowledge-hub__get_company` → startup name — detailed company record with all notes and meeting history
- `mcp__vc-knowledge-hub__get_similar_companies` → startup name — competitors already in the firm's deal flow
- `mcp__vc-knowledge-hub__ask` → _"What do we know about [startup name] and its competitive landscape? Include any expert or AlphaSight call notes, analyst views, and concerns raised."_
- `mcp__vc-knowledge-hub__ask` → _"What competitors to [startup name] have we seen in our deal flow?"_
- `mcp__vc-knowledge-hub__search` → _"[inferred market category] competitors"_ — related companies from deal flow

For each competitor the hub returns, call `mcp__vc-knowledge-hub__get_company` to pull their full profile.

**1b. Affinity CRM** (for pipeline stage and field values not in the hub)
- `mcp__Affinity__search_companies` → find the CRM record and ID for the startup.
- `mcp__Affinity__get_notes_for_entity` (entity_type=1) → all analyst notes on the startup.
- `mcp__Affinity__get_company_list_entries` → pipeline stage and lists.
- For any CRM-tracked competitors: `get_notes_for_entity` to pull their notes.

**1d. Granola — founder calls AND expert/AlphaSight calls**
Run all queries in parallel:
- `mcp__Granola__query_granola_meetings` → _"[startup name] founders — product, market, competitive positioning"_
- `mcp__Granola__query_granola_meetings` → _"[startup name] competitors risks threats concerns"_
- `mcp__Granola__query_granola_meetings` → _"AlphaSight [startup name]"_ — expert calls via AlphaSight
- `mcp__Granola__query_granola_meetings` → _"expert call [startup name] [inferred market/industry]"_ — any other expert/advisor calls
- `mcp__Granola__query_granola_meetings` → _"[startup name] due diligence technical commercial"_

For each relevant meeting ID, call `mcp__Granola__get_meetings` for full summary and private notes.
Get transcripts with `mcp__Granola__get_meeting_transcript` when you need exact quotes.

Separate findings into:
- **Founder/team calls** — what the startup said about their market and competitors
- **Expert calls** — independent industry experts, AlphaSight consultants, advisors, reference calls

**1e. Email — expert opinions and analyst research (Superhuman)**
Run all in parallel:
- `mcp__Superhuman__query_email_and_calendar` → _"Expert opinions or analyst views on [startup name] or its competitors"_
- `mcp__Superhuman__query_email_and_calendar` → _"[startup name] — market research, analyst reports, due diligence material"_
- `mcp__Superhuman__query_email_and_calendar` → _"AlphaSight [startup name]"_
- `mcp__Superhuman__list_threads` with `body_contains` = startup name
- `mcp__Superhuman__list_threads` with `subject_contains` = _"expert"_ / _"AlphaSight"_ / _"reference call"_ / _"advisor"_

Call `mcp__Superhuman__get_thread` on relevant threads to read full content.

Extract: expert names/titles, opinions on the startup and its competitors, any quantitative claims, red flags raised, validation points.

**1f. Google Drive — prior memos, pitch decks, existing analyses**
- `mcp__Google_Drive__search_files` → `fullText contains '[startup name]'`
- `mcp__Google_Drive__search_files` → `title contains 'Competitor'` or `title contains 'Landscape'` or `title contains 'AlphaSight'`
- `mcp__Google_Drive__read_file_content` on relevant files.

**Consolidate Step 1:**
- List of competitors already known internally with any analyst views
- Expert opinions by source (AlphaSight call, network expert email, advisor)
- Founder claims about the competitive landscape
- Conflicts between internal views and external data

---

### Step 2 — Resolve target company via Specter

- `mcp__Specter__find_company` → `mcp__Specter__get_company_profile` + `mcp__Specter__get_company_intelligence`

Extract: full name, website, HQ (ISO-2), headcount (number), founding year (number), product description (≤200 chars), last funding (format: `$Xm\nRound Year`), investor names (≤60 chars), primary market category.

---

### Step 3 — Identify competitors

Triangulate from all sources in parallel:

**3a. Specter** — `mcp__Specter__find_similar_companies`

**3b. Evertrace talent signals**
- `mcp__Evertrace__search_companies` → get the startup's entity ID (exe_* format)
- `mcp__Evertrace__list_signals` with `past_companies=[entity_id]` → talent ecosystem mapping
- Repeat for 2–3 named direct competitors

**3c. Web search**
- `WebSearch`: _"[startup name] competitors 2024 2025"_
- `WebSearch`: _"best alternatives to [startup name]"_

Merge all sources. Prioritise companies appearing in multiple sources or flagged in Step 1.
Target 10–20 competitors: **Direct** / **Adjacent** / **Incumbent**.

---

### Step 4 — Research each competitor

For each competitor, use all available sources:

| Source | Action |
|--------|--------|
| Specter | `find_company` → `get_company_profile` |
| Affinity | `search_companies` → if found, `get_notes_for_entity` |
| Granola | `query_granola_meetings` — expert or founder calls mentioning this competitor? |
| Superhuman | `query_email_and_calendar` — email intel, expert opinions, analyst views on this competitor? |
| Web | `WebSearch` / `WebFetch` to fill gaps |

Collect for each:
- Company name, URL, HQ (ISO-2), headcount (number), founding year (number)
- Product description (≤200 chars)
- Last funding string (`$Xm\nRound Year` · `Acquired` · `Incumbent` · `N/A`)
- Investor names (≤60 chars)
- **Key Difference** from target startup (≤200 chars — specific, not vague; incorporate expert language where available)
- Category: Direct / Adjacent / Incumbent
- Expert opinion summary for this competitor (if any found) — used in the Expert View column

---

### Step 5 — Determine comparison dimensions

Based on the target startup's product and market, choose 5–8 dimensions. These may be:

**Binary/checkmark dimensions** (for capability comparisons):
- Mark: `x` = confirmed · `(x)` = partial · blank = absent/unknown
- Examples: Open source, Self-hosting, Real-time insights, Workflow automation, Inbound calls, Dynamic safety

**Descriptive dimensions** (for technical/product attribute comparisons):
- Fill with short text values (not checkmarks)
- Examples: Sensor Technology, Detection Range, Product Type, Unit Cost Tier, Model Type, Use Case Focus
- Used when the Alethos/noways-style "Product" section is more appropriate than binary features

Choose dimension type based on what best differentiates competitors in this specific market. It's fine to mix both types.

Also add an **Expert View** column if any expert opinions were found in Step 1. This column sits after the feature/product columns and contains a 1–2 sentence summary of what independent experts said about each company (or blank if no expert opinion found).

---

### Step 6 — Score / fill dimensions

For each company (target + competitors):
- Binary dims: `x`, `(x)`, or blank
- Descriptive dims: short text value or blank
- Expert View: 1–2 sentence expert opinion summary (with source noted), or blank

Base on: Specter intelligence, product pages, Granola transcripts, email intel. Do not guess — blank if uncertain.

---

### Step 7 — Generate the Excel file

Install openpyxl if needed: `pip install openpyxl -q`

Write and execute a Python script. Use this structure exactly:

#### Excel layout

**Row 1 — Section header row:**
- A1: empty
- B1: `Overview` (merged B1 through the last Overview column)
- Next group header: `Features` OR `Product` OR `Category` depending on what fits the market — choose the label that best describes the comparison columns
- If an Expert View column is added, add a separate `Expert Intelligence` header merged over it

**Row 2 — Column headers:**
`Company Name | URL | Location | Headcount | Founded | Product Description | Last Funding | Investors | Key Difference | Category | [Dim 1] | [Dim 2] | … | Expert View ← if expert data found`

**Row 3 — Target startup** (always first data row, highlighted yellow)

**Rows 4+ — Competitors** (Direct first, then Adjacent, then Incumbents)

#### Styling
- Row 1 section headers: bold, `BFBFBF` fill
- Row 2 column headers: bold, `D9D9D9` fill, thin borders, wrap text
- Row 3 target startup row: `FFFACD` (light yellow) fill on all cells
- Binary feature cells: centre-aligned
- Descriptive cells: left-aligned, wrap text
- Expert View column (if present): `EBF5EB` (light green) fill on all data cells, wrap text
- Freeze rows 1–2 (freeze pane at A3)
- Auto-fit column widths, capped at 60 chars (Expert View column capped at 80 chars)
- Sheet name: `Tabellenblatt1`

#### Python script template

```python
import openpyxl
from openpyxl.styles import Font, PatternFill, Alignment, Border, Side
from openpyxl.utils import get_column_letter

wb = openpyxl.Workbook()
ws = wb.active
ws.title = "Tabellenblatt1"

# --- populate from research ---
feature_section_label = "Features"  # or "Product" or "Category" — choose based on market
has_expert_col = True  # set False if no expert opinions found

feature_cols = [...]       # list of dimension names
companies = [
    {
        "name": "...", "url": "...", "location": "...",
        "headcount": ..., "founded": ..., "description": "...",
        "funding": "...", "investors": "...", "key_diff": "...",
        "category": "...",
        "dims": {"Dim 1": "x", "Dim 2": None, "Descriptive Dim": "some text", ...},
        "expert_view": "Industry expert (AlphaSight, May 2025): 'Considered best-in-class for X but weak on Y.'"
        # or None if no expert opinion
    },
    # ...
]

overview_cols = [
    "Company Name", "URL", "Location", "Headcount", "Founded",
    "Product Description", "Last Funding", "Investors", "Key Difference", "Category"
]
expert_cols = ["Expert View"] if has_expert_col else []
all_cols = overview_cols + feature_cols + expert_cols

n_ov = len(overview_cols)
n_ft = len(feature_cols)
n_ex = len(expert_cols)
total = len(all_cols)

thin = Side(style="thin")
border = Border(left=thin, right=thin, top=thin, bottom=thin)

# Row 1: section headers
ws.cell(1, 2).value = "Overview"
ws.cell(1, 2).font = Font(bold=True)
ws.cell(1, 2).fill = PatternFill("solid", fgColor="BFBFBF")
ws.merge_cells(start_row=1, start_column=2, end_row=1, end_column=n_ov)

if n_ft > 0:
    ws.cell(1, n_ov + 1).value = feature_section_label
    ws.cell(1, n_ov + 1).font = Font(bold=True)
    ws.cell(1, n_ov + 1).fill = PatternFill("solid", fgColor="BFBFBF")
    if n_ft > 1:
        ws.merge_cells(start_row=1, start_column=n_ov+1, end_row=1, end_column=n_ov+n_ft)

if n_ex > 0:
    ws.cell(1, n_ov + n_ft + 1).value = "Expert Intelligence"
    ws.cell(1, n_ov + n_ft + 1).font = Font(bold=True)
    ws.cell(1, n_ov + n_ft + 1).fill = PatternFill("solid", fgColor="C6E0B4")  # green header

# Row 2: column headers
for ci, name in enumerate(all_cols, 1):
    c = ws.cell(2, ci, name)
    c.font = Font(bold=True)
    c.fill = PatternFill("solid", fgColor="D9D9D9")
    c.alignment = Alignment(wrap_text=True, vertical="top")
    c.border = border

# Data rows
expert_fill = PatternFill("solid", fgColor="EBF5EB")
target_fill = PatternFill("solid", fgColor="FFFACD")

for ri, co in enumerate(companies, 3):
    vals = [co["name"], co["url"], co["location"], co["headcount"], co["founded"],
            co["description"], co["funding"], co["investors"], co["key_diff"], co["category"]]
    for ci, v in enumerate(vals, 1):
        ws.cell(ri, ci, v)

    for fi, fname in enumerate(feature_cols, n_ov + 1):
        v = co["dims"].get(fname)
        c = ws.cell(ri, fi, v)
        c.alignment = Alignment(horizontal="center" if v in ("x", "(x)", None) else "left",
                                wrap_text=True)

    if n_ex > 0:
        ev_col = n_ov + n_ft + 1
        c = ws.cell(ri, ev_col, co.get("expert_view"))
        c.fill = expert_fill
        c.alignment = Alignment(wrap_text=True, vertical="top")

    # Target startup highlight (row 3)
    if ri == 3:
        for ci in range(1, total + 1):
            ws.cell(ri, ci).fill = target_fill

# Column widths
for ci in range(1, total + 1):
    col_name = all_cols[ci - 1]
    cap = 80 if "expert" in col_name.lower() else 60
    maxw = max(
        (len(str(ws.cell(r, ci).value)) for r in range(1, ws.max_row + 1) if ws.cell(r, ci).value),
        default=8
    )
    ws.column_dimensions[get_column_letter(ci)].width = min(maxw + 2, cap)

ws.freeze_panes = "A3"

output_path = "..."  # fill in
wb.save(output_path)
print(f"Saved: {output_path}")
```

Fill all data variables with actual research, then execute with `Bash`.

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
- Number of competitors (Direct / Adjacent / Incumbent breakdown)
- Feature/product dimensions used, and whether section label is "Features", "Product", or "Category"
- Expert sources incorporated — list each by name/type (e.g. "2 AlphaSight calls", "1 network expert email from [name/role]")
- Which internal sources contributed (Affinity, Granola, email, Drive)
- Git commit short hash
- Any companies where data was missing or estimated

---

## Data format rules

| Field | Format |
|-------|--------|
| Location | ISO-2 country code (`GE`, `US`, `UK`, `FR`, etc.) |
| Headcount | Number only (`45` not `"~45"`) |
| Founded | Numeric year (`2021`) |
| Last Funding | `$Xm\nRound Year` · `Acquired` · `Incumbent` · `N/A` |
| Key Difference | How competitor differs *from target startup*, ≤200 chars; incorporate expert language if available |
| Binary dims | `x` confirmed · `(x)` partial · blank unknown/absent |
| Descriptive dims | Short text value, or blank |
| Expert View | `[Expert name/type, date]: 'quote or summary'` — blank if no expert opinion |

## Important notes

- **Expert opinions are primary evidence.** AlphaSight calls and expert emails from the firm's network contain proprietary DD intelligence. Every usable insight must be surfaced in the Expert View column and must have shaped the Key Difference column.
- Do not merge or average expert opinions — keep them attributable (name, title, source, date where known).
- Target startup is always row 3. Key Difference always describes how a competitor differs *from the target*.
- The third section label (`Features` / `Product` / `Category`) should reflect the market: use `Product` for hardware/deep-tech comparisons (like Alethos radar), `Features` for software capability comparisons, `Category` for market segment groupings.
- Do not fabricate data. Unknown = blank. This is a DD document.
