# DD-Skills

A curated library of Claude AI skills for venture capital due diligence and investment analysis. Skills live in `.claude/skills/` and are automatically available in Claude Code sessions.

## Structure

```
.claude/skills/              ← Claude loads skills from here
├── pmf-assessment/
├── measuring-pmf-lenny/
├── startup-idea-validation/
├── market-sizing/
├── market-sizing-analysis/
├── market-sizing-excel/
├── competitive-analysis/
├── competitive-analysis-excel/
├── competitive-landscape/
├── competitive-intelligence-playbook/
├── sector-research/
├── market-environment-analysis/
├── marketing-competitive-analysis/
├── business-analyst/
└── startup-analyst/

noways__Competitor_Overview.xlsx   ← Example DD output
```

## Skills

### Product-Market Fit

| Skill | What it does |
|-------|-------------|
| `pmf-assessment` | Sean Ellis 40% test + Superhuman PMF engine. Includes retention curve analysis, survey templates, PMF stage guides, and case studies. |
| `measuring-pmf-lenny` | PMF framework distilled from 46 product leaders. Covers disappointment surveys, retention curves, reference customer counts, and segment-level fit. |

### Due Diligence

| Skill | What it does |
|-------|-------------|
| `startup-idea-validation` | 9-dimension GO/NO-GO scorecard: problem severity, market size, defensibility, founder-market fit, feasibility, GTM, and risk. |
| `startup-analyst` | Startup evaluation framework for investment screening and DD — market opportunity, unit economics, team, metrics. |
| `business-analyst` | Business analysis framework for evaluating startup fundamentals and KPI dashboards. |

### Market Research

| Skill | What it does |
|-------|-------------|
| `market-sizing` | TAM/SAM/SOM — core market sizing skill (built-in). |
| `market-sizing-excel` | Generates Excel market sizing output (built-in). |
| `market-sizing-analysis` | TAM/SAM/SOM via top-down, bottom-up, and value-theory with a SaaS worked example. |
| `sector-research` | Frameworks for doing sector research from scratch. Useful when covering a new vertical. |
| `market-environment-analysis` | Macro and industry trend methodology with indicator references. |

### Competitive Analysis

| Skill | What it does |
|-------|-------------|
| `competitive-analysis` | Landscape mapping, feature matrices, win/loss analysis (built-in). |
| `competitive-analysis-excel` | Generates Excel competitor overview output (built-in). |
| `competitive-landscape` | Competitive landscape mapping with Porter's Five Forces and positioning. |
| `competitive-intelligence-playbook` | Deep competitive intelligence methodology — thorough, structured analysis. |
| `marketing-competitive-analysis` | Competitive marketing strategy — positioning, messaging, GTM differentiation. |

## Usage

Skills are invoked with `/skill-name` in a Claude Code session, or Claude will trigger them automatically based on your request.

## Sources

Skills sourced from [luisschmitzheadline/VC-Skills.md](https://github.com/luisschmitzheadline/VC-Skills.md) and custom-built for 42cap DD workflows.
