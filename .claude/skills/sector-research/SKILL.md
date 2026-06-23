---
name: sector-research
description: "Two-layer market context tool for DD. Layer 1 — Sector/Industry Research: TAM/SAM/SOM, trends, drivers, competitive landscape, and founder-level market brief for a specific vertical. Layer 2 — Market Environment Analysis: macro backdrop, global indices, risk-on/risk-off, sector rotation, and investment climate. Use when covering a new vertical, assessing market timing, or building thesis context."
---

# Sector Research & Market Environment Analysis

Combined market context skill for due diligence. Operates in two layers — run both when assessing a new investment; run Layer 1 alone for sector deep-dives; run Layer 2 alone for macro snapshots.

---

## Layer 1 — Sector / Industry Research

Produce a structured market brief for a specific vertical or startup category.

### Output Structure

- **Quick Take** — One line: market size, CAGR, key dynamic.
- **Market Definition** — What's in scope: geography, segment, vertical, exclusions.
- **TAM / SAM / SOM** — Total / serviceable / obtainable; sources and key assumptions cited.
- **Trends & Drivers** — 3–5 trends (technology, regulatory, demand-side) with growth implications.
- **Competitive Landscape** — Key players, market structure (fragmented / consolidated), and whitespace.
- **Implications** — What it means for the company being evaluated: opportunities and risks.

### Workflow

1. Parse the market, segment, and geography from the user's request or the startup's pitch.
2. Run web searches in parallel:
   - `<sector> market size TAM 2024 2025 CAGR Gartner IDC forecast`
   - `<sector> competitive landscape key players 2024`
   - `<sector> regulatory trends technology drivers`
3. Pull internal briefs if VC Knowledge Hub or Google Drive is connected (`mcp__vc-knowledge-hub__search` → sector name; `mcp__Google_Drive__search_files` → sector keywords). If the VC Knowledge Hub returns no results or incomplete data, fall through to the individual connectors directly (Granola, Superhuman, Google Drive, Affinity, Specter, Evertrace, and any other relevant connector) — those remain the authoritative sources.
4. Produce the structured brief per output structure above.
5. For detailed competitive analysis, invoke `/competitive-analysis <startup name>`. For full market sizing, invoke `/market-sizing <startup name>`.

### Data Sources (in priority order)

| Type | Sources |
|------|---------|
| Market size / CAGR | Gartner, IDC, Grand View Research, Statista, IBISWorld |
| Company counts | Eurostat (EU), US Census Bureau, ONS (UK), OECD |
| Regulatory / policy | EUR-Lex, Federal Register, industry trade associations |
| Competitive players | Specter (`find_similar_companies`), Crunchbase, LinkedIn |
| Internal | VC Knowledge Hub, Google Drive, Affinity notes |

---

## Layer 2 — Market Environment Analysis

Assess the macro backdrop and investment climate. Run this when timing matters (e.g. is now a good time to deploy? how does sector rotation affect this vertical?).

### Data to Collect (use web search for live data)

1. **Global equity indices** — S&P 500, NASDAQ, Nikkei 225, DAX, Shanghai Composite, Hang Seng
2. **Forex** — EUR/USD, USD/JPY, major pairs
3. **Commodities** — WTI crude, gold, silver
4. **US Treasury yields** — 2-year, 10-year, 30-year
5. **VIX** — fear gauge; categorise volatility level
6. **Recent macro events** — FOMC decisions, CPI/NFP releases, geopolitical events

### Assessment Framework

| Dimension | Signal | Source |
|-----------|--------|--------|
| Trend direction | Uptrend / Downtrend / Range-bound | Index price action |
| Risk sentiment | Risk-on / Risk-off | VIX + equity/bond flows |
| Volatility | Low (<15) / Normal (15–25) / Elevated (25–35) / Extreme (>35) | VIX level |
| Sector rotation | Where is capital flowing? | Sector ETF flows, analyst commentary |
| Relevant macro events | ⭐⭐⭐ FOMC, NFP, CPI · ⭐⭐ GDP, Retail Sales · ⭐ Reference | Economic calendar |

### VIX Interpretation

| VIX | Label | Implication |
|-----|-------|-------------|
| <15 | Calm | Risk-on; equities bid; growth assets favoured |
| 15–25 | Normal | Balanced; standard risk appetite |
| 25–35 | Elevated | Caution; flight to quality begins |
| >35 | Extreme | Risk-off; safe havens; deployment timing sensitive |

### Output Format (Layer 2)

```
📊 Market Environment — [Date / Time]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Global Indices
  S&P 500:  [price] ([Δ%])   NASDAQ: [price] ([Δ%])
  DAX:      [price] ([Δ%])   Nikkei: [price] ([Δ%])

Risk Sentiment:  [Risk-On / Risk-Off / Neutral]
VIX:            [level] — [label]
10yr Treasury:  [yield]%

Key Events (next 5 days):
  ⭐⭐⭐ [Event] — [Date]
  ⭐⭐  [Event] — [Date]

Sector Rotation:  Capital flowing into [sectors]; out of [sectors].

Investment Climate: [2–3 sentence synthesis — is this a good moment to deploy in growth / deep-tech / SaaS? What risks are priced in?]
```

### Implication for DD

After the environment snapshot, add 2–3 sentences contextualising the macro backdrop for the specific startup being evaluated:
- Does the current risk-on/off environment affect their fundraising timeline?
- Is their sector currently in favour or out of favour with growth investors?
- Are there macro tailwinds or headwinds (interest rates, regulation, commodities) relevant to their model?

---

## Usage Examples

**New vertical deep-dive:**
> "Run sector research on AI-powered legal tech in Europe."
→ Layer 1 only. Produce structured brief with TAM, trends, players, whitespace.

**Macro snapshot before investment decision:**
> "What's the current market environment?"
→ Layer 2 only. Produce live macro snapshot.

**Full DD market context:**
> "Give me market context for a Series A cybersecurity startup."
→ Both layers. Layer 1 for the cybersecurity vertical; Layer 2 for macro backdrop and timing implications.


---

## Language and Tone

- Use expert terminology appropriate to the context (VC due diligence, financial analysis, competitive intelligence)
- Avoid superfluous prose, self-references, expert advice disclaimers, and apologies
- No em dashes or en dashes; use commas, parentheses, or rewrite the sentence instead
- Lead with data and specific findings; interpretation follows the evidence
- No superlatives (world-class, revolutionary, best-in-class, cutting-edge)

