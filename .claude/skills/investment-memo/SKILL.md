---
name: investment-memo
description: "Generate a 42CAP Investment Memo in Word (.docx) format. Pulls company data from Affinity (deal details, notes), Granola (meeting transcripts from founder, reference, and expert calls), Specter (company profile, team, competitors), Google Drive (financial model, cap table, tech DD), and Superhuman (email threads). Produces a fully formatted memo matching the 42CAP template: cover table, Investment Criteria, Solution, Market & Competition, Go-To-Market, ESG, Financials, Funding, Team, and Additional Documents. Use when asked to write, draft, or generate an investment memo."
---

# Investment Memo

Generates a 42CAP Investment Memo in Word (.docx) format, matching the firm's standard visual template exactly: dark green cover table, light green Investment Criteria table, underlined section headers, justified body text, and ESG materiality table.

---

## Intake Checklist

Before starting, confirm with the user or pull from Affinity:

- **Company name** and HQ city
- **Tag line** — one-line product description (e.g. "AI simulation and evaluation platform")
- **Round details** — amount on pre-money, stage (e.g. "€2.8m on €9.8m Pre-Money (Seed)")
- **Syndicate** — 42CAP ticket + co-investors (e.g. "€1.9m 42CAP + €0.9m Mozilla, JME, ABAC")
- **Author name** and date
- **IC Members** — default: Alex, Thomas, Moritz, Julian, Pauline, Johannes
- **Stage**: Pre-Seed / Seed / Series A

---

## Step 0 — Pull from VC Knowledge Hub

Run first. The VC Knowledge Hub aggregates Affinity, Granola, and Google Drive in one semantic layer, so a single sweep often surfaces context faster than hitting each source individually.

```
mcp__vc-knowledge-hub__search("<company name>")
  → unified semantic search across Affinity notes, Granola transcripts, and Drive docs
  → use to discover all prior touchpoints and documents before diving into individual sources

mcp__vc-knowledge-hub__get_company("<company name>")
  → full company profile: CRM fields, notes, meeting history, pitch deck content extracted
  → cache: round details, valuation, team names, product description, any thesis drafts

mcp__vc-knowledge-hub__extract_memo_data("<company name>")
  → extract structured data from any prior memo on file — fast-path to populating standard fields

mcp__vc-knowledge-hub__ask("What is 42CAP's current view on <company name>? Summarise the investment thesis, key concerns, and any IC feedback.")
  → synthesised answer with citations from all connected sources

mcp__vc-knowledge-hub__get_thesis_themes()
  → retrieve 42CAP's active thesis themes — use to frame Investment Criteria alignment and key bets

mcp__vc-knowledge-hub__search_research_findings("<company name>")
  → surface any prior research or analysis already saved on this company or sector

mcp__vc-knowledge-hub__search_investor_insights("<company name>")
  → retrieve any saved investor insights relevant to this deal

mcp__vc-knowledge-hub__list_portfolio()
  → retrieve current portfolio for synergy or conflict checks (referenced in Investment Criteria)

mcp__vc-knowledge-hub__get_portfolio_dashboard()
  → portfolio-level context: sector concentrations, stage mix — relevant for IC framing

mcp__vc-knowledge-hub__get_meeting_feed()
  → recent meetings across all portfolio and pipeline companies — catch any recent calls not yet in Affinity

mcp__vc-knowledge-hub__get_similar_companies("<company name>")
  → competitors and comparables already in 42CAP deal flow — use for Market & Competition section
```

Use the output to pre-populate as many memo fields as possible before the per-source steps below. Flag any conflicts between sources (e.g. different round sizes in Affinity vs. a pitch deck in Drive).

**Key signal to look for in Step 0:** if the hub surfaces a competitive analysis doc or market sizing doc already in Drive, note the file ID or name. Step 4 will pull these as the primary source for Market & Competition — no need to re-research externally.

If the VC Knowledge Hub returns no results or incomplete data, fall through to the individual connectors directly (Granola, Superhuman, Google Drive, Affinity, Specter, Evertrace, and any other relevant connector) — those remain the authoritative sources.

---

## Step 1 — Pull from Affinity

Search for the company and extract deal details, notes, and meetings.

```
mcp__Affinity__search_companies(name="<company>")
  → get company_id

mcp__Affinity__get_company_info(company_id)
  → HQ, website, description

mcp__Affinity__get_notes_for_entity(entity_id, entity_type="company")
  → DD notes, investment thesis drafts, IC feedback

mcp__Affinity__get_meetings_for_entity(entity_id, entity_type="company")
  → meeting log, interaction history

mcp__Affinity__query_notes(query="<company> investment thesis key bets")
  → search for prior analysis

mcp__Affinity__search_opportunities(name="<company>")
  → deal stage, valuation, round size, 42CAP check size
```

---

## Step 2 — Pull Granola transcripts

Find all calls with the company and extract content for each memo section.

```
mcp__Granola__list_meetings(search="<company>")
  → identify all relevant meeting IDs

mcp__Granola__get_meeting_transcript(meeting_id)
  → extract for each meeting
```

**What to extract by call type:**

| Call type | Section it feeds |
|-----------|-----------------|
| Founder calls | Summary, Solution, GTM, Team |
| Reference / customer calls | GTM (client quotes), Team (reference findings) |
| Expert calls | Solution (expert validation), Market (market pull signals) |
| IC calls | WYHTBI theses, key bets, investment decision rationale |

Always include direct verbatim quotes with speaker name where they add credibility (e.g. "Qonto's CTO expressed 'uncommon at this stage' optimism...").

---

## Step 3 — Pull Specter data

```
mcp__Specter__find_company(name="<company>")
  → resolve company_id

mcp__Specter__get_company_profile(company_id)
  → founding date, HQ, headcount, website, description

mcp__Specter__get_company_intelligence(company_id)
  → funding rounds, investors, key people, news

mcp__Specter__get_company_financials(company_id)
  → ARR, revenue, growth metrics (if available)

mcp__Specter__find_similar_companies(company_id)
  → competitor list

# For each competitor:
mcp__Specter__find_company(name="<competitor>")
mcp__Specter__get_company_profile(competitor_id)
  → funding, headcount, HQ, founding year for competitor table
```

---

## Step 4 — Pull Google Drive materials

This is the primary source for Market & Competition and Financials. Pull and read all DD documents before drafting any content.

```
# Market & Competition — look for these first
mcp__Google_Drive__search_files(query="<company> competitive analysis")
mcp__Google_Drive__search_files(query="<company> competitor overview")
mcp__Google_Drive__search_files(query="<company> competitive landscape")
mcp__Google_Drive__search_files(query="<company> market sizing")
mcp__Google_Drive__search_files(query="<company> TAM SAM SOM")

# Financials & cap table
mcp__Google_Drive__search_files(query="<company> financial model")
mcp__Google_Drive__search_files(query="<company> cap table")

# Tech DD and other materials
mcp__Google_Drive__search_files(query="<company> tech DD")
mcp__Google_Drive__search_files(query="<company> pitch deck")
mcp__Google_Drive__search_files(query="<company> expert feedback")

# For each found file:
mcp__Google_Drive__read_file_content(file_id)
```

**Priority rule for Market & Competition and Market Sizing:**
If a competitive analysis doc or market sizing doc exists in Drive, use it as the primary source for those sections. Do NOT re-research competitors from Specter or the web if a Drive doc already covers them — extract and summarise from the existing document instead. Only fall back to Specter or web search to fill specific gaps (e.g. a competitor listed in the Drive doc but lacking detail, or a market sizing figure that needs a more recent source).

**Priority rule for Financials:**
The financial model in Drive is the authoritative source for the Financials section. Pull projected ARR, burn, and unit economics from it directly rather than deriving from Specter or web data.

---

## Step 5 — Pull Superhuman email threads

```
mcp__Superhuman__query_email_and_calendar(query="<company>")
  → find relevant email threads

mcp__Superhuman__get_thread(thread_id)
  → read for term sheet details, co-investor context, IC decisions
```

---

## Step 6 — Draft content section by section

Use `references/memo-sections-guide.md` for detailed writing norms per section. Summary of what each section requires:

### Cover block
Pull from Affinity + user confirmation. All 6 fields required before generating.

### Summary
Four sub-elements — all required:
1. **4-bullet overview**: Market, Solution, Go-To-Market, Team (one punchy sentence each)
2. **"Our view" block**: Rate each of the 6 investment criteria in one line — TAM, PMF, CES (Cost-Efficient Scaling), GM% (Gross Margin), AEC (Available External Capital), TEAM
3. **Key bets for the next 18 months** — 4–6 specific, measurable bullets (e.g. "Close first 20 enterprise clients to reach €1m ARR by Q2 2026")
4. **Why-We-Invested / WYHTBI theses** — 3–5 conviction statements. Each must be a falsifiable belief, not a general observation.

### Investment Criteria (7-row table)
One row per criterion: TAM, PMF, Cost-Efficient Scaling, Gross Margin %, Available External Funding, ESG, Team Quality. Assess each honestly in 1–2 sentences. If a criterion is early or unproven, say so.

### Solution
Follow the exact bullet structure:
- Value prop to customer (Theory) and customer acceptance (Practice)
- Quality use cases (Theory) or Status MVP (Practice)
- Details Back-end (Tech stack) and Front-end (CX)
- Product roadmap
- Stickiness
- Key findings from tech deep dive

### Market & Competition
**Source priority: Drive docs first.** If a market sizing file or competitive analysis file was found in Step 4, use it as the primary source for this entire section. Do not re-run external research if Drive already covers it.

- Simple bottom-up model for home market, Top-5 Europe, and Global (pull figures from the market sizing doc in Drive; if no Drive doc exists, build from Specter + web data)
- Ability and effort to scale countries (regulatory, localisation, GTM cost)
- Overview of competition including feature comparison table (extract from the competitive analysis doc in Drive; if no Drive doc exists, build from Specter `find_similar_companies` + web)

Note: if company's market and GTM are tightly coupled, combine into "Market, Go-To-Market & Competition" — set `market_header` accordingly.

### Go-To-Market
- Initial growth hacking approach (first 0→10 customers)
- Mid-term approach by channel: Marketing, Sales, Partner channel
- Clients (Today — named, with ARR/contract type; Pipeline — stage and value)
- Key findings from client reference calls (direct quotes preferred)

### ESG
Use boilerplate from `references/esg-standard-language.md`. Use the exact standard intro paragraph (replace "xxx" with company name). Populate Environmental / Social / Governance rows with bullet points. Close with the standard closing sentence + one positive impact bullet.

### Financials
- Unit economics and quality of top-line (Recurrence, Gross Margin %)
- Growth cost factor (what does it cost to grow €1 of revenue?)
- Current traction (ARR, MRR, customers, growth rate)
- Pricing (model, tiers, ACV)
- Financial plan — simple version with key assumptions (e.g. "NN new customers p.m. at €Xk ACV")
- Bullet structure: top-line targets → cost base today → cost base at 18m → burn → unit economics → P&L placeholder

### Funding
- Past funding rounds (what was raised, what was achieved with that capital)
- Details current round: total size, 42CAP ticket, co-investors, use of funds (what will be proven)
- Future funding needs: next risk step, break-even milestone
- Potential exit channels: IPO, secondary, trade sale — include named acquisition candidates
- Cap table (simple version)

### Team
One paragraph per key hire in this order: CEO (include Fund Raising Power assessment) → CPO → CRO → CFO → Other key hires. Each paragraph must include reference call findings if available.

### Additional documents
Standard bullet list reflecting actual materials in the DD folder.

---

## Step 7 — Generate the Word document

Once all content is confirmed, execute this Python script. Fill all variables with researched content before running.

```python
import os
from docx import Document
from docx.shared import Pt, Cm, RGBColor
from docx.enum.text import WD_ALIGN_PARAGRAPH
from docx.enum.table import WD_ALIGN_VERTICAL, WD_TABLE_ALIGNMENT
from docx.oxml.ns import qn
from docx.oxml import OxmlElement

# ── HELPERS ──────────────────────────────────────────────────────────────────

def set_cell_bg(cell, hex_color):
    tc = cell._tc
    tcPr = tc.get_or_add_tcPr()
    shd = OxmlElement('w:shd')
    shd.set(qn('w:val'), 'clear')
    shd.set(qn('w:color'), 'auto')
    shd.set(qn('w:fill'), hex_color)
    tcPr.append(shd)

def add_section_header(doc, text):
    """Section header with bottom border underline, matching 42CAP style."""
    p = doc.add_paragraph()
    run = p.add_run(text)
    run.font.size = Pt(11)
    run.font.bold = False
    pPr = p._p.get_or_add_pPr()
    pBdr = OxmlElement('w:pBdr')
    bottom = OxmlElement('w:bottom')
    bottom.set(qn('w:val'), 'single')
    bottom.set(qn('w:sz'), '6')
    bottom.set(qn('w:space'), '1')
    bottom.set(qn('w:color'), 'auto')
    pBdr.append(bottom)
    pPr.append(pBdr)
    p.paragraph_format.space_before = Pt(18)
    p.paragraph_format.space_after = Pt(6)
    return p

def add_body(doc, text, justify=True):
    p = doc.add_paragraph(text)
    p.paragraph_format.space_after = Pt(8)
    if justify:
        p.alignment = WD_ALIGN_PARAGRAPH.JUSTIFY
    return p

def add_bullet(doc, text):
    p = doc.add_paragraph(text, style='List Bullet')
    p.paragraph_format.space_after = Pt(3)
    return p

def add_numbered(doc, text):
    p = doc.add_paragraph(text, style='List Number')
    p.paragraph_format.space_after = Pt(3)
    return p

# ── COLOURS ──────────────────────────────────────────────────────────────────
GREEN_DARK  = "1A5C3A"   # Cover table dark rows + criteria/cap table header
GREEN_MID   = "2E7D52"   # Cover table alternate rows
GREEN_LIGHT = "C5E0B4"   # Criteria table body left col + total rows

# ── DATA — replace all placeholders with researched content ──────────────────

company_name      = "Company Name"
company_hq        = "City"
tag_line          = "One-line product description"
details_round     = "€Xm on €Xm Pre-Money (Stage)"
details_syndicate = "€Xm 42CAP + €Xm Co-investor"
author_date       = "Author Name – Month DD, YYYY"
ic_members        = "Alex, Thomas, Moritz, Julian, Pauline, Johannes"

# Summary — 4 sub-elements
summary_overview = [
    # 4 bullets: Market / Solution / GTM / Team (one punchy sentence each)
    "Market: [market size, CAGR, why now]",
    "Solution: [what the product does and the core customer outcome]",
    "Go-To-Market: [ICP, motion, current traction headline]",
    "Team: [founder backgrounds and why they win this market]",
]
summary_our_view = [
    # One line per criterion: TAM | PMF | CES | GM% | AEC | TEAM
    "TAM: [€Xbn market; venture-scale opportunity]",
    "PMF: [early signals / proven / strong — 1 supporting data point]",
    "CES: [LTV:CAC X:1; payback X months]",
    "GM%: [X% today; path to Y% at scale]",
    "AEC: [co-investors, grant funding, strategic interest]",
    "TEAM: [complementary, domain experts, prior exits]",
]
key_bets = [
    # 4–6 specific, measurable targets for the next 18 months
    "Key bet 1 — specific and measurable target",
    "Key bet 2",
    "Key bet 3",
    "Key bet 4",
]

# Investment Criteria — 7 rows
criteria_rows = [
    ("Total Addressable\nMarket",    "TAM size, CAGR, why venture-scale."),
    ("Product-\nMarket-Fit",         "PMF signals: retention, NPS, pull quotes."),
    ("Cost-Efficient\nScaling",      "LTV:CAC, payback, GTM scalability."),
    ("Gross Margin\nPercentage",     "Current GM% and trajectory."),
    ("Available External\nFunding",  "Pre-money, burn, investor interest."),
    ("ESG",                          "One-sentence ESG assessment."),
    ("Team\nQuality",                "Complementarity, domain expertise, track record."),
]

# WYHTBI — numbered conviction statements
wyhtbi_items = [
    "Conviction statement 1.",
    "Conviction statement 2.",
    "Conviction statement 3.",
    "Conviction statement 4.",
]

# Solution — one paragraph per bullet; keep each focused
solution_paragraphs = [
    # Value prop (Theory): what outcome does the customer get?
    # Value prop (Practice): evidence of customer acceptance (quotes, pilots, NPS)
    "Value prop to customer (Theory): [problem + outcome]. Customer acceptance (Practice): [evidence — pilots, quotes, retention data].",
    # Use cases (Theory) or MVP status (Practice)
    "Quality use cases: [named use case 1], [named use case 2]. Status MVP: [GA / beta / pilot — X active customers].",
    # Tech stack — Back-end architecture and Front-end CX
    "Back-end: [tech stack, infra, model type, data pipeline]. Front-end: [CX, interface, API / no-code / embedded].",
    # Product roadmap
    "Roadmap: Phase 1 [milestone, date] → Phase 2 [milestone, date] → Phase 3 [milestone, date].",
    # Stickiness
    "Stickiness: [switching costs, data moat, workflow lock-in, network effects — be specific].",
    # Tech DD key findings
    "Key findings tech deep dive: [architecture strengths]. [Gaps or risks identified]. [Remediation plan if applicable].",
]

# Market sizing table — (region, n_companies, acv, icp_fit, arr_potential, mkt_share, serviceable_arr)
market_sizing_rows = [
    ("Europe",  "X,XXX",  "€XXX,000", "XX%", "€XXXm", "XX%", "€XXXm"),
    ("USA",     "X,XXX",  "€XXX,000", "XX%", "€XXXm", "XX%", "€XXXm"),
    ("Total",   "XX,XXX", "—",        "—",   "€X.Xb", "—",   "€XXXm"),
]
market_header = "Market & Competition"  # change to "Market, Go-To-Market & Competition" if combined

competition_paragraphs = [
    "Competitive landscape paragraph 1: category structure and named competitors.",
    "Competitive landscape paragraph 2: where target company sits and why it wins.",
    "Differentiation: specific moat (data, switching costs, distribution).",
]

# Go-To-Market (omit if combined above)
gtm_paragraphs = [
    # Initial growth hacking approach (first 0→10 customers)
    "Initial growth hacking: [first 0→10 customer approach — direct outreach, community, PLG, partnerships].",
    # Mid-term by channel: Marketing / Sales / Partner channel
    "Mid-term by channel — Marketing: [content, SEO, events, paid]. Sales: [AEs, SDRs, motion, ACV]. Partner channel: [system integrators, resellers, strategic alliances].",
    # Clients today — named, with ARR and contract type
    "Clients today: [Client A — ARR, use case, since when]; [Client B — ARR, use case]; [Client C — ARR, use case].",
    # Pipeline
    "Pipeline: [Company X — stage, value, expected close]; [Company Y — stage, value]; Total pipeline: €Xm.",
    # Key findings from client reference calls
    "Key findings from client reference calls: [direct quote from reference 1 — name/role if shareable]. [Key theme from reference 2]. [Any concern raised and how company addressed it].",
]

# ESG — customize per company; see references/esg-standard-language.md
esg_rows = [
    ("Environmental", [
        "We encourage the company to track their carbon footprint and (if possible) use renewable energy sources.",
    ]),
    ("Social", [
        "The company plans to grow the team significantly. We encourage them to implement a fair and inclusive hiring process.",
        "The company should implement a DEI policy and a Code of Conduct to create an inclusive working environment.",
    ]),
    ("Governance", [
        "We encourage the company to implement a transparent governance structure and practice open communication with all stakeholders.",
    ]),
]
esg_closing = (
    "In addition to the materiality issues outlined above, we identified no red flags "
    "that would have a negative ESG impact."
)

# Financials — all bullets
financials_topline_label = "Top-line development over the next 18 months (Mon YYYY – Mon YYYY)"
financials_topline = [
    "Increase ARR from €Xk to €Xm ARR",
    "X clients with an ACV of €Xk",
    "Win rate: X new clients per month",
]
financials_cost_today_label = "Cost base today:"
financials_cost_today = [
    "€Xk/month = €Xk HC + €Xk non-HC",
    "X HC at €Xk fully-loaded annual cost",
]
financials_cost_target_label = "Cost base at 18 months:"
financials_cost_target = [
    "€Xk/month = €Xk HC + €Xk non-HC",
    "X HC = X GTM + X Product + X founders",
    "€Xk non-HC = €Xk Compute + €Xk MarCom + €Xk Misc",
]
financials_burn_label = "Burn rate over the next 18 months:"
financials_burn = [
    "€Xm net burn",
    "€Xm gross burn",
    "€Xm round size provides X–X months of runway",
]
financials_unit_econ_label = "Key unit economics:"
financials_unit_econ = [
    "LTV:CAC: X:1 (target ≥3:1 at scale)",
    "CAC payback: <X months",
    "Gross margin: X%",
]

# Funding
funding_paragraphs = [
    # Past funding rounds — what was raised, what was achieved
    "Past funding: [Round type, amount, date, lead investor — what milestones were hit with that capital].",
    # Current round — details and use of funds
    "Current round: €Xm total; 42CAP €Xm; [co-investor] €Xm. Use of funds: [Hire X GTM FTEs], [Build Y], [Reach Z ARR]. What will be proven: [specific milestone that de-risks the next raise].",
    # Future funding needs
    "Future funding: Next round expected [date] at [milestone]. Break-even at [ARR/revenue level] by [date].",
    # Exit channels
    "Potential exit: [Trade sale — named candidates e.g. Salesforce, ServiceNow]; [Secondary market]; [IPO at €Xb+ scale]. Comparable exits: [Company A acquired by X at Y multiple].",
]

# Cap table — (shareholder, share_class, shares, pct_undiluted, pct_fd)
cap_table_rows = [
    ("Founder 1",    "Ordinary",           "XX,XXX", "XX.X%", "XX.X%"),
    ("Founder 2",    "Ordinary",           "XX,XXX", "XX.X%", "XX.X%"),
    ("42CAP",        "Seed Preferred",     "XX,XXX", "XX.X%", "XX.X%"),
    ("Co-investor",  "Seed Preferred",     "XX,XXX", "XX.X%", "XX.X%"),
    ("ESOP",         "Options (Phantom)",  "XX,XXX", "—",     "XX.X%"),
    ("TOTAL",        "",                   "XXX,XXX","100.0%","100.0%"),
]

# Team — (name, role, bio_text)
# Order: CEO → CPO → CRO → CFO → Other key hires
# Each bio must include: prior roles + companies, what they bring, ref call findings (if available)
# CEO bio must include Fund Raising Power assessment
team_members = [
    ("First Last", "CEO", "Prior: [Role at Company A], [Role at Company B]. Brings: [domain expertise / network / fundraising track record]. Fund Raising Power: [Strong/Moderate — specific signal]. Ref call findings: [Quote or theme from reference]."),
    ("First Last", "CPO / CTO", "Prior: [Role at Company A]. Brings: [technical depth, product vision]. Ref call findings: [Quote or theme]."),
    ("First Last", "CRO / CCO", "Prior: [Role at Company A]. Brings: [commercial network, GTM execution]. Ref call findings: [Quote or theme]."),
]
team_closing = [
    # Post-round hiring plan
    "Post-round hiring: [X GTM + Y Product + Z Eng FTEs] targeted by [Month YYYY].",
    # Talent pool context
    "Talent pool: [City/ecosystem] provides strong access to [relevant talent type — e.g. AI engineers from ETH/TU Munich].",
]

# Additional documents
additional_docs = [
    "Pitch deck",
    "Expert and customer feedback",
    "Competitor overview",
    "Cap table",
    "Financial model",
    "Tech DD",
    "Sales pipeline",
    "Other elements in the data room",
]

output_path = f"{company_name.replace(' ', '_')}_IMemo.docx"

# ── DOCUMENT BUILD ────────────────────────────────────────────────────────────

doc = Document()

# Page setup — A4
sec = doc.sections[0]
sec.page_width    = Cm(21.0)
sec.page_height   = Cm(29.7)
sec.top_margin    = Cm(2.5)
sec.bottom_margin = Cm(2.0)
sec.left_margin   = Cm(2.5)
sec.right_margin  = Cm(2.5)

# Default font
doc.styles['Normal'].font.name = 'Calibri'
doc.styles['Normal'].font.size = Pt(10)

# ── TITLE ─────────────────────────────────────────────────────────────────────
p = doc.add_paragraph()
p.alignment = WD_ALIGN_PARAGRAPH.CENTER
run = p.add_run("INVESTMENT MEMO")
run.bold = True
run.font.size = Pt(14)
p.paragraph_format.space_before = Pt(12)
p.paragraph_format.space_after  = Pt(12)

# ── COVER TABLE ───────────────────────────────────────────────────────────────
cover_data = [
    ("Company (HQ)",      f"{company_name} ({company_hq})"),
    ("Tag line",          tag_line),
    ("Details round",     details_round),
    ("Details syndicate", details_syndicate),
    ("Author – Date",     author_date),
    ("IC Member",         ic_members),
]
cover_colors = [GREEN_DARK, GREEN_MID, GREEN_DARK, GREEN_MID, GREEN_DARK, GREEN_DARK]

tbl = doc.add_table(rows=len(cover_data), cols=2)
tbl.style = 'Table Grid'
tbl.alignment = WD_TABLE_ALIGNMENT.LEFT
for i, ((label, value), color) in enumerate(zip(cover_data, cover_colors)):
    lc = tbl.rows[i].cells[0]
    rc = tbl.rows[i].cells[1]
    lc.width = Cm(4.5)
    rc.width = Cm(12.0)
    set_cell_bg(lc, color)
    pl = lc.paragraphs[0]
    rl = pl.add_run(label)
    rl.font.color.rgb = RGBColor(0xFF, 0xFF, 0xFF)
    rl.font.size = Pt(10)
    lc.vertical_alignment = WD_ALIGN_VERTICAL.CENTER
    pr = rc.paragraphs[0]
    pr.add_run(value).font.size = Pt(10)
    rc.vertical_alignment = WD_ALIGN_VERTICAL.CENTER

doc.add_paragraph()

# ── SUMMARY ───────────────────────────────────────────────────────────────────
add_section_header(doc, "Summary")
for b in summary_overview:
    add_bullet(doc, b)
doc.add_paragraph()
p = doc.add_paragraph()
p.add_run("Our view on TAM, PMF, CES, GM%, AEC, TEAM:").font.size = Pt(10)
p.paragraph_format.space_after = Pt(3)
for b in summary_our_view:
    add_bullet(doc, b)
doc.add_paragraph()
p = doc.add_paragraph()
p.add_run("Key bets for the next 18 months:").font.size = Pt(10)
p.paragraph_format.space_after = Pt(3)
for bet in key_bets:
    add_bullet(doc, bet)

# ── INVESTMENT CRITERIA ───────────────────────────────────────────────────────
add_section_header(doc, "Investment Criteria")
crit_tbl = doc.add_table(rows=len(criteria_rows) + 1, cols=2)
crit_tbl.style = 'Table Grid'
# Header row
hdr = crit_tbl.rows[0]
for ci, htext in enumerate(["Criteria", "Comment"]):
    cell = hdr.cells[ci]
    set_cell_bg(cell, GREEN_DARK)
    p = cell.paragraphs[0]
    run = p.add_run(htext)
    run.font.color.rgb = RGBColor(0xFF, 0xFF, 0xFF)
    run.font.bold = True
    run.font.size = Pt(10)
    p.alignment = WD_ALIGN_PARAGRAPH.CENTER
# Body rows
for i, (label, comment) in enumerate(criteria_rows):
    row = crit_tbl.rows[i + 1]
    lc = row.cells[0]
    rc = row.cells[1]
    set_cell_bg(lc, GREEN_LIGHT)
    lc.width = Cm(4.5)
    rc.width = Cm(12.0)
    lp = lc.paragraphs[0]
    lp.add_run(label).font.size = Pt(10)
    lp.alignment = WD_ALIGN_PARAGRAPH.CENTER
    lc.vertical_alignment = WD_ALIGN_VERTICAL.CENTER
    rp = rc.paragraphs[0]
    rp.add_run(comment).font.size = Pt(10)
    rp.alignment = WD_ALIGN_PARAGRAPH.JUSTIFY
    rc.vertical_alignment = WD_ALIGN_VERTICAL.CENTER

doc.add_paragraph()
p = doc.add_paragraph()
p.add_run("What you have to believe in / why we invested:").font.size = Pt(10)
p.paragraph_format.space_after = Pt(3)
for item in wyhtbi_items:
    add_numbered(doc, item)

# ── SOLUTION ──────────────────────────────────────────────────────────────────
add_section_header(doc, "Solution")
for para in solution_paragraphs:
    add_body(doc, para)

# ── MARKET & COMPETITION ──────────────────────────────────────────────────────
add_section_header(doc, market_header)

# Market sizing table
mkt_cols = ["Region", "# Companies", "Expected ACV", "ICP Fit %",
            "ARR Potential", "Mkt Share %", "Serviceable ARR"]
mkt_tbl = doc.add_table(rows=len(market_sizing_rows) + 1, cols=len(mkt_cols))
mkt_tbl.style = 'Table Grid'
for ci, h in enumerate(mkt_cols):
    cell = mkt_tbl.rows[0].cells[ci]
    set_cell_bg(cell, GREEN_DARK)
    p = cell.paragraphs[0]
    run = p.add_run(h)
    run.font.color.rgb = RGBColor(0xFF, 0xFF, 0xFF)
    run.font.bold = True
    run.font.size = Pt(9)
for ri, row_data in enumerate(market_sizing_rows):
    row = mkt_tbl.rows[ri + 1]
    is_total = row_data[0] == "Total"
    for ci, val in enumerate(row_data):
        cell = row.cells[ci]
        if is_total:
            set_cell_bg(cell, GREEN_LIGHT)
        p = cell.paragraphs[0]
        run = p.add_run(val)
        run.font.size = Pt(9)
        if is_total:
            run.font.bold = True

doc.add_paragraph()
for para in competition_paragraphs:
    add_body(doc, para)

# ── GO-TO-MARKET (skip if combined with Market above) ────────────────────────
if market_header == "Market & Competition":
    add_section_header(doc, "Go-To-Market")
    for para in gtm_paragraphs:
        add_body(doc, para)

# ── ESG ───────────────────────────────────────────────────────────────────────
add_section_header(doc, "ESG")
esg_intro = (
    f"During the investment process we analyzed {company_name} along ESG criteria, "
    "to assess the status quo of the company, identify potential risks and, if necessary, "
    "formulate an action plan to address these concerns. Below you will find a materiality "
    "assessment, identifying the ESG issues core to the company's financial performance, "
    "long-term success, and stakeholder value over the coming 18–24 months:"
)
add_body(doc, esg_intro)

esg_tbl = doc.add_table(rows=len(esg_rows) + 1, cols=2)
esg_tbl.style = 'Table Grid'
for ci, htext in enumerate(["Section", "ESG Measures"]):
    cell = esg_tbl.rows[0].cells[ci]
    p = cell.paragraphs[0]
    run = p.add_run(htext)
    run.font.bold = True
    run.font.size = Pt(10)
for i, (section_name, bullets) in enumerate(esg_rows):
    row = esg_tbl.rows[i + 1]
    lc = row.cells[0]
    rc = row.cells[1]
    lc.width = Cm(3.5)
    rc.width = Cm(13.0)
    lc.paragraphs[0].add_run(section_name).font.size = Pt(10)
    for j, bullet_text in enumerate(bullets):
        p = rc.paragraphs[0] if j == 0 else rc.add_paragraph()
        p.add_run(f"• {bullet_text}").font.size = Pt(10)
        p.alignment = WD_ALIGN_PARAGRAPH.JUSTIFY

doc.add_paragraph()
add_body(doc, esg_closing)

# ── FINANCIALS ────────────────────────────────────────────────────────────────
add_section_header(doc, "Financials")
add_body(doc, financials_topline_label)
for b in financials_topline:
    add_bullet(doc, b)
p = doc.add_paragraph()
p.add_run(financials_cost_today_label).font.size = Pt(10)
p.paragraph_format.space_after = Pt(3)
for b in financials_cost_today:
    add_bullet(doc, b)
p = doc.add_paragraph()
p.add_run(financials_cost_target_label).font.size = Pt(10)
p.paragraph_format.space_after = Pt(3)
for b in financials_cost_target:
    add_bullet(doc, b)
p = doc.add_paragraph()
p.add_run(financials_burn_label).font.size = Pt(10)
p.paragraph_format.space_after = Pt(3)
for b in financials_burn:
    add_bullet(doc, b)
p = doc.add_paragraph()
p.add_run(financials_unit_econ_label).font.size = Pt(10)
p.paragraph_format.space_after = Pt(3)
for b in financials_unit_econ:
    add_bullet(doc, b)
add_body(doc, "Below you will find the P&L for the next 12 months:")
add_body(doc, "[Insert P&L table or screenshot from financial model in Google Drive]")

# ── FUNDING ───────────────────────────────────────────────────────────────────
add_section_header(doc, "Funding")
for para in funding_paragraphs:
    add_body(doc, para)

# Cap table
cap_headers = ["Shareholder", "Share Class", "Shares", "% Undiluted", "% FD"]
cap_tbl = doc.add_table(rows=len(cap_table_rows) + 1, cols=len(cap_headers))
cap_tbl.style = 'Table Grid'
for ci, h in enumerate(cap_headers):
    cell = cap_tbl.rows[0].cells[ci]
    set_cell_bg(cell, GREEN_DARK)
    p = cell.paragraphs[0]
    run = p.add_run(h)
    run.font.color.rgb = RGBColor(0xFF, 0xFF, 0xFF)
    run.font.bold = True
    run.font.size = Pt(9)
for ri, row_data in enumerate(cap_table_rows):
    row = cap_tbl.rows[ri + 1]
    is_total = row_data[0] == "TOTAL"
    for ci, val in enumerate(row_data):
        cell = row.cells[ci]
        if is_total:
            set_cell_bg(cell, GREEN_LIGHT)
        p = cell.paragraphs[0]
        run = p.add_run(val)
        run.font.size = Pt(9)
        if is_total:
            run.font.bold = True

# ── TEAM ──────────────────────────────────────────────────────────────────────
add_section_header(doc, "Team")
for name, role, bio in team_members:
    p = doc.add_paragraph()
    run_name = p.add_run(f"{name}, {role}: ")
    run_name.font.size = Pt(10)
    run_bio = p.add_run(bio)
    run_bio.font.size = Pt(10)
    p.alignment = WD_ALIGN_PARAGRAPH.JUSTIFY
    p.paragraph_format.space_after = Pt(8)
for b in team_closing:
    add_bullet(doc, b)

# ── ADDITIONAL DOCUMENTS ──────────────────────────────────────────────────────
add_section_header(doc, "Additional documents")
for item in additional_docs:
    add_bullet(doc, item)

# ── SAVE ──────────────────────────────────────────────────────────────────────
doc.save(output_path)
print(f"Saved: {output_path}")
```

---

## Step 8 — Send to user

```python
# After the script above completes:
SendUserFile(files=[output_path], status="normal")
```

Do **not** commit the generated `.docx` to the skills repo — it is a per-deal output.

---

## Step 9 — Finalise in VC Knowledge Hub

After the `.docx` is generated and sent, register the memo and save key findings for future reuse.

```
mcp__vc-knowledge-hub__finalize_memo("<company name>")
  → mark the memo as finalised in the knowledge hub — enables future extract_memo_data calls

mcp__vc-knowledge-hub__save_research_analysis("<company name>", summary="<one-paragraph investment thesis summary>")
  → persist the core thesis and findings so they surface in future search_research_findings queries
```

---

## Quality Checklist

Before sending, verify:

- [ ] Cover: all 6 rows filled with correct deal details, no placeholders
- [ ] Summary — Overview: 4 bullets (Market, Solution, GTM, Team) — all specific, no generic language
- [ ] Summary — Our view: all 6 criteria rated (TAM, PMF, CES, GM%, AEC, TEAM)
- [ ] Summary — Key bets: 4–6 measurable 18-month targets, not aspirations
- [ ] WYHTBI: 3–5 numbered conviction statements — falsifiable, company-specific
- [ ] Investment Criteria: all 7 rows filled; ESG row present
- [ ] Solution: all 6 bullets covered — value prop (theory + practice), use cases/MVP, tech stack (BE + FE), roadmap, stickiness, tech DD findings
- [ ] Market: bottom-up table with home market + Top-5 Europe + Global, sources cited
- [ ] Competition: named competitors with feature comparison, whitespace identified
- [ ] GTM: initial growth hacking → mid-term by channel (Marketing/Sales/Partner) → named clients + pipeline → ref call findings
- [ ] ESG: company name substituted; Environmental/Social/Governance rows with bullets; closing sentence + positive impact
- [ ] Financials: unit economics, growth cost factor, traction, pricing, financial plan with key assumptions (NN p.m.)
- [ ] Funding: past rounds (achievements) → current round (use of funds + what will be proven) → future needs → exit channels with named candidates → cap table
- [ ] Team: CEO (with Fund Raising Power) → CPO → CRO → CFO → other hires; ref call findings included
- [ ] Additional documents: reflects actual materials in DD folder


---

## Language and Tone

- Use expert terminology appropriate to the context (VC due diligence, financial analysis, competitive intelligence)
- Avoid superfluous prose, self-references, expert advice disclaimers, and apologies
- No em dashes or en dashes; use commas, parentheses, or rewrite the sentence instead
- Lead with data and specific findings; interpretation follows the evidence
- No superlatives (world-class, revolutionary, best-in-class, cutting-edge)

