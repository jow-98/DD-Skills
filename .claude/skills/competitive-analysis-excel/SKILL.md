---
name: competitive-analysis-excel
description: "Research competitors for a startup in due diligence and generate an Excel (.xlsx) Competitor Overview file. Use this over competitive-analysis when the output must be .xlsx with multiple tabs (competitor overview, Porter's/SWOT, marketing intel). Use when asked to create a competitor Excel, build a competitive analysis spreadsheet, or generate a comp overview file."
---

# Competitive Analysis — Excel Output Skill

Research a startup's competitive landscape using every available knowledge source and produce a `.xlsx` Competitor Overview file in the standard DD format.

**Reference file**: `references/competitive-analysis-guide.md` contains column-by-column writing guidance, Porter's Five Forces scoring rules, SWOT quality standards, messaging matrix instructions, and channel coverage scoring. Consult it for any judgement calls on content quality or framing.

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

If the VC Knowledge Hub returns no results or incomplete data, fall through to the individual connectors below (Affinity, Granola, Specter, Google Drive, Superhuman) — those remain the authoritative sources.

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

### Step 7b — Add Sheet 2: "Strategic Frameworks"

After the main competitor sheet is written, add a second sheet with Porter's Five Forces scoring and the target startup's SWOT.

#### Sheet 2 layout

**Section A — Porter's Five Forces** (rows 1–8)

Row 1: Section header "Porter's Five Forces" — bold, merged, `BFBFBF` fill.
Row 2: Column headers: `Force | Score (1–5) | Key Factors | Implication | Internal Source`
Rows 3–7: One row per force (New Entrants, Supplier Power, Buyer Power, Substitutes, Rivalry). Score sourced from expert intel in Step 1 where possible.
Row 8: "Overall Attractiveness" — merged label with 1–2 sentence summary.

**Section B — SWOT** (rows 10–onward, after a blank row)

Row 10: Section header "SWOT — [Target Startup]" — bold, merged, `BFBFBF` fill.
Row 11: Four column headers: `Strengths | Weaknesses | Opportunities | Threats`
Rows 12+: 3–5 bullet items per quadrant (one bullet per row), left-aligned, wrap text.
- Strengths/Weaknesses: grey fill `F2F2F2`
- Opportunities: light green fill `E2EFDA`
- Threats: light red fill `FCE4D6`

**Section C — Competitive Positioning Map** (after SWOT, blank row separator)

Column headers: `Company | Tier | Price (1-10) | Features (1-10) | Positioning Note`
Target startup row: `FFFACD` (yellow) fill.
Sort: target first, then Direct, Adjacent, Incumbent.

Sheet name: `Strategic Frameworks`

```python
# Add to existing Python script after wb.save() block — create before saving

ws2 = wb.create_sheet("Strategic Frameworks")

# --- Porter's Five Forces ---
forces = [
    ("Threat of New Entrants",        3, "Capital/tech barriers, network effects",    "Moderate barrier; monitor well-funded entrants"),
    ("Bargaining Power of Suppliers",  2, "Multiple cloud providers available",        "Low risk; commodity infrastructure"),
    ("Bargaining Power of Buyers",     4, "Enterprise buyers; concentrated spend",     "Negotiate on pricing; build switching costs"),
    ("Threat of Substitutes",          3, "Manual processes; adjacent products",       "Educate on ROI vs. DIY alternatives"),
    ("Competitive Rivalry",            4, "10+ direct competitors; fast-growing space","Differentiate on [key dimension]"),
]

ws2.cell(1, 1, "Porter's Five Forces").font = Font(bold=True)
ws2.cell(1, 1).fill = PatternFill("solid", fgColor="BFBFBF")
ws2.merge_cells(start_row=1, start_column=1, end_row=1, end_column=5)

p5f_headers = ["Force", "Score (1–5)", "Key Factors", "Implication", "Internal Source"]
for ci, h in enumerate(p5f_headers, 1):
    c = ws2.cell(2, ci, h)
    c.font = Font(bold=True); c.fill = PatternFill("solid", fgColor="D9D9D9")
    c.border = border; c.alignment = Alignment(wrap_text=True)

for ri, (force, score, factors, impl) in enumerate(forces, 3):
    for ci, val in enumerate([force, score, factors, impl, ""], 1):
        c = ws2.cell(ri, ci, val)
        c.border = border; c.alignment = Alignment(wrap_text=True, vertical="top")

ws2.cell(8, 1, "Overall Market Attractiveness:").font = Font(bold=True)
ws2.cell(8, 2, "Moderate — [1-2 sentence summary]")
ws2.merge_cells(start_row=8, start_column=2, end_row=8, end_column=5)

# --- SWOT ---
ws2.cell(10, 1, f"SWOT — {startup_name}").font = Font(bold=True)
ws2.cell(10, 1).fill = PatternFill("solid", fgColor="BFBFBF")
ws2.merge_cells(start_row=10, start_column=1, end_row=10, end_column=4)

swot_headers = ["Strengths", "Weaknesses", "Opportunities", "Threats"]
swot_fills   = ["F2F2F2",    "F2F2F2",     "E2EFDA",        "FCE4D6"]
for ci, (h, fill) in enumerate(zip(swot_headers, swot_fills), 1):
    c = ws2.cell(11, ci, h)
    c.font = Font(bold=True); c.fill = PatternFill("solid", fgColor=fill)
    c.border = border; c.alignment = Alignment(horizontal="center")

swot_data = {
    "Strengths":     ["[Strength 1]", "[Strength 2]", "[Strength 3]"],
    "Weaknesses":    ["[Weakness 1]", "[Weakness 2]", "[Weakness 3]"],
    "Opportunities": ["[Opportunity 1]", "[Opportunity 2]", "[Opportunity 3]"],
    "Threats":       ["[Threat 1]", "[Threat 2]", "[Threat 3]"],
}
max_rows = max(len(v) for v in swot_data.values())
for ri in range(max_rows):
    for ci, (key, fill) in enumerate(zip(swot_headers, swot_fills), 1):
        items = swot_data[key]
        val = items[ri] if ri < len(items) else ""
        c = ws2.cell(12 + ri, ci, val)
        c.fill = PatternFill("solid", fgColor=fill)
        c.border = border; c.alignment = Alignment(wrap_text=True, vertical="top")

# --- Positioning Map ---
pos_start = 14 + max_rows
ws2.cell(pos_start, 1, "Competitive Positioning Map").font = Font(bold=True)
ws2.cell(pos_start, 1).fill = PatternFill("solid", fgColor="BFBFBF")
ws2.merge_cells(start_row=pos_start, start_column=1, end_row=pos_start, end_column=5)

pos_headers = ["Company", "Tier", "Price (1–10)", "Features (1–10)", "Positioning Note"]
for ci, h in enumerate(pos_headers, 1):
    c = ws2.cell(pos_start + 1, ci, h)
    c.font = Font(bold=True); c.fill = PatternFill("solid", fgColor="D9D9D9")
    c.border = border

# Fill positioning rows from companies list (reuse from Sheet 1)
for ri, co in enumerate(companies, pos_start + 2):
    vals = [co["name"], co["category"], "", "", ""]  # fill price/features scores from research
    for ci, val in enumerate(vals, 1):
        c = ws2.cell(ri, ci, val)
        c.border = border; c.alignment = Alignment(wrap_text=True, vertical="top")
    if ri == pos_start + 2:  # target row
        for ci in range(1, 6):
            ws2.cell(ri, ci).fill = PatternFill("solid", fgColor="FFFACD")

ws2.freeze_panes = "A3"
for col in ws2.columns:
    maxw = max((len(str(c.value)) for c in col if c.value), default=8)
    ws2.column_dimensions[get_column_letter(col[0].column)].width = min(maxw + 2, 50)
```

---

### Step 7c — Add Sheet 3: "Marketing Intel"

Add a third sheet with the messaging matrix and content/channel coverage.

#### Sheet 3 layout

**Section A — Messaging Matrix** (rows 1–onward)

Row 1: Section header "Messaging & Positioning" — bold, merged, `BFBFBF` fill.
Row 2: Company names as column headers (target first, then top 3–5 competitors). First column = Dimension label.
Rows 3–10: One row per messaging dimension (Tagline, Value Prop, Primary Audience, Differentiator Claim, Tone, Proof Points, Category Framing, CTA).

**Section B — Channel Coverage** (after blank row)

Row header: "Content & Channel Coverage" — merged, `BFBFBF` fill.
Sub-headers: same company ordering as messaging matrix. First column = Channel/Format.
Rows: Blog/SEO, Case Studies, Whitepapers, Webinars, Video, G2/Reviews, Paid Ads, Community.
Cells: "Strong" / "Present" / "Weak" / "None" — color-coded (green/yellow/orange/red fill).

Sheet name: `Marketing Intel`

```python
ws3 = wb.create_sheet("Marketing Intel")

# Derive competitor names from companies list
all_names = [co["name"] for co in companies[:6]]  # target + top 5

# --- Messaging Matrix ---
ws3.cell(1, 1, "Messaging & Positioning").font = Font(bold=True)
ws3.cell(1, 1).fill = PatternFill("solid", fgColor="BFBFBF")
ws3.merge_cells(start_row=1, start_column=1, end_row=1, end_column=len(all_names) + 1)

for ci, name in enumerate(["Dimension"] + all_names, 1):
    c = ws3.cell(2, ci, name)
    c.font = Font(bold=True); c.fill = PatternFill("solid", fgColor="D9D9D9")
    c.border = border; c.alignment = Alignment(wrap_text=True)

msg_dimensions = [
    "Tagline / Headline", "Core Value Proposition", "Primary Audience",
    "Key Differentiator Claim", "Tone / Voice", "Proof Points Used",
    "Category Framing", "Primary CTA",
]
for ri, dim in enumerate(msg_dimensions, 3):
    ws3.cell(ri, 1, dim).font = Font(bold=True)
    ws3.cell(ri, 1).border = border
    for ci in range(2, len(all_names) + 2):
        c = ws3.cell(ri, ci, "")  # fill from research
        c.border = border; c.alignment = Alignment(wrap_text=True, vertical="top")
    if ri == 3:  # target column highlight
        for ri2 in range(3, 3 + len(msg_dimensions)):
            ws3.cell(ri2, 2).fill = PatternFill("solid", fgColor="FFFACD")

# --- Channel Coverage ---
chan_start = len(msg_dimensions) + 5
ws3.cell(chan_start, 1, "Content & Channel Coverage").font = Font(bold=True)
ws3.cell(chan_start, 1).fill = PatternFill("solid", fgColor="BFBFBF")
ws3.merge_cells(start_row=chan_start, start_column=1, end_row=chan_start, end_column=len(all_names) + 1)

for ci, name in enumerate(["Channel / Format"] + all_names, 1):
    c = ws3.cell(chan_start + 1, ci, name)
    c.font = Font(bold=True); c.fill = PatternFill("solid", fgColor="D9D9D9")
    c.border = border

channels = ["Blog / SEO", "Case Studies", "Whitepapers / Ebooks",
            "Webinars / Events", "Video Content", "G2 / Review Sites",
            "Paid Ads", "Community / Forum"]
coverage_fills = {"Strong": "C6EFCE", "Present": "FFEB9C", "Weak": "FFCC99", "None": "FFC7CE", "": "FFFFFF"}

for ri, chan in enumerate(channels, chan_start + 2):
    ws3.cell(ri, 1, chan).border = border
    for ci in range(2, len(all_names) + 2):
        val = ""  # fill from research: "Strong" / "Present" / "Weak" / "None"
        c = ws3.cell(ri, ci, val)
        c.border = border
        c.fill = PatternFill("solid", fgColor=coverage_fills.get(val, "FFFFFF"))
        c.alignment = Alignment(horizontal="center")

ws3.freeze_panes = "B3"
for col in ws3.columns:
    maxw = max((len(str(c.value)) for c in col if c.value), default=10)
    ws3.column_dimensions[get_column_letter(col[0].column)].width = min(maxw + 2, 40)
```

---

### Step 8 — Verify the output

```bash
ls -lh <output_path>
python3 -c "
import openpyxl
wb = openpyxl.load_workbook('<output_path>')
for name in wb.sheetnames:
    ws = wb[name]
    print(f'{name}: {ws.max_row} rows x {ws.max_column} cols — OK')
"
```

Expected output: three sheets — `Tabellenblatt1`, `Strategic Frameworks`, `Marketing Intel`.

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


---

## Language and Tone

- Use expert terminology appropriate to the context (VC due diligence, financial analysis, competitive intelligence)
- Avoid superfluous prose, self-references, expert advice disclaimers, and apologies
- No em dashes or en dashes; use commas, parentheses, or rewrite the sentence instead
- Lead with data and specific findings; interpretation follows the evidence
- No superlatives (world-class, revolutionary, best-in-class, cutting-edge)

