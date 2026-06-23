# DD-Skills

A curated library of Claude AI skills for venture capital due diligence and investment analysis. Skills live in `.claude/skills/` and are automatically available in Claude Code sessions.

## Structure

```
.claude/skills/              ← Claude loads skills from here
```

## Core Skills (use these first)

| Skill | What it does | Output |
|-------|-------------|--------|
| `competitive-analysis` | Full DD competitive landscape: pulls from Affinity, Granola (AlphaSight), Superhuman, Specter, Evertrace, Drive + web. Includes Porter's Five Forces, SWOT, positioning map, and marketing/GTM battlecards. | Markdown report |
| `competitive-analysis-excel` | Same research as above, produces `.xlsx` with 3 tabs: competitor overview, strategic frameworks (Porter's/SWOT/positioning map), and marketing intel (messaging matrix, channel coverage). | `.xlsx` file |
| `market-sizing` | Bottom-up TAM/SAM/SOM via Specter + web. Includes optional value theory methodology (willingness-to-pay) for new categories. Top-down sanity check included. | Markdown report |
| `market-sizing-excel` | Same as above, produces `.xlsx` with 3 sheets: Bottom-Up (scenario blocks), Top-Down (validation), optional Value-Theory. | `.xlsx` file |
| `pmf-assessment` | PMF evaluation combining Sean Ellis 40% test, Superhuman PMF engine, retention curve analysis, and Lenny's 46-leader framework (reference customer counts, segment-level fit, pull signals, multi-stage PMF). | Framework + diagnostics |
| `startup-screening` | Three-phase DD screen: Phase 1 = 9-dimension GO/NO-GO scorecard; Phase 2 = fundamentals (market sizing, unit economics, competitive position, team); Phase 3 = business case (KPI framework, return scenarios, investment thesis). | Structured memo |
| `sector-research` | Two-layer market context: Layer 1 = sector/industry brief (TAM, trends, players, whitespace); Layer 2 = macro environment (indices, VIX, risk-on/off, sector rotation, investment climate). | Brief + macro snapshot |

## Supporting Skills

| Skill | What it does |
|-------|-------------|
| `competitive-landscape` | Generic framework skill: Porter's Five Forces, Blue Ocean Strategy, positioning maps, competitive monitoring. Use as a reference when working through frameworks manually. |
| `competitive-intelligence-playbook` | Deep competitive intelligence methodology (SWOT, Porter's, positioning matrix). Thorough structured analysis for when you need a full playbook. |
| `marketing-competitive-analysis` | Competitive marketing strategy — messaging comparison, content gap analysis, battlecard creation, GTM differentiation. |
| `market-sizing-analysis` | Generic TAM/SAM/SOM frameworks with top-down, bottom-up, and value theory methodologies. Reference skill for methodology. |
| `startup-idea-validation` | 9-dimension GO/NO-GO scorecard and validation ladder as a standalone tool (also embedded in `startup-screening` Phase 1). |

## Skill Relationships

```
startup-screening ──Phase 1──► 9-dimension scorecard (startup-idea-validation embedded)
                 ──Phase 2──► market-sizing + competitive-analysis (abbreviated)
                 ──Phase 3──► pmf-assessment + KPI framework

competitive-analysis ──────► competitive-analysis-excel (same research, Excel output)
market-sizing        ──────► market-sizing-excel (same research, Excel output)

sector-research ──Layer 1──► industry brief
                ──Layer 2──► macro environment + investment climate
```

## Usage

Skills are invoked with `/skill-name` in a Claude Code session, or Claude will trigger them automatically based on your request.

**Typical DD workflow:**
1. `/startup-screening <Company>` — quick GO/NO-GO (Phase 1, 15 min)
2. If GO: `/market-sizing-excel <Company>` + `/competitive-analysis-excel <Company>`
3. `/sector-research <vertical>` for macro context and timing
4. `/pmf-assessment` to evaluate product-market fit signals

## Sources

Skills sourced and adapted from [luisschmitzheadline/VC-Skills.md](https://github.com/luisschmitzheadline/VC-Skills.md) and custom-built for 42cap DD workflows.
