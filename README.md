# DD-Skills

A curated library of Claude AI skills for venture capital due diligence and investment analysis. Each skill is a structured prompt framework that Claude uses to perform a specific analytical task.

## What's here

```
knowledge_skills/
├── due_diligence/          # Core DD frameworks (PMF, validation, memos)
├── market_research/        # Market sizing, competitive analysis, sector research
└── investment_analysis/    # Startup evaluation and business analyst frameworks

noways__Competitor_Overview.xlsx   # Example competitor overview output
```

## Skills

### Due Diligence

| Folder | What it does |
|--------|-------------|
| `skillsmp-product-market-fit` | PMF assessment using Sean Ellis 40% test + Superhuman PMF engine. Assets include retention curve analysis, survey templates, and PMF stage guides. |
| `lenny-measuring-product-market-fit` | PMF framework distilled from 46 product leaders (Lenny's Newsletter). Covers disappointment surveys, retention curves, reference customer counts, and segment-level fit. |
| `vasilyu-startup-idea-validation` | 9-dimension GO/NO-GO scorecard: problem severity, market size, defensibility, founder fit, etc. Good for initial screening. |

### Market Research

| Folder | What it does |
|--------|-------------|
| `antigravity-market-sizing-analysis` | TAM/SAM/SOM via top-down, bottom-up, and value-theory approaches. Includes a SaaS worked example and data source references. |
| `skillsmp-competitive-landscape` | Competitive landscape mapping, feature matrices, and win/loss analysis. |
| `stratarts-competitive-intelligence` | Structured competitive intelligence playbook — 88KB of methodology for deep competitor analysis. |
| `propane-founder-market-research` | Frameworks for sector research from scratch. Useful when covering a new vertical. |
| `openclaw-market-environment-analysis` | Macro and industry trend methodology with indicator references and utility scripts. |
| `propane-marketing-competitive-analysis` | Competitive marketing strategy analysis — positioning, messaging, GTM differentiation. |

### Investment Analysis

| Folder | What it does |
|--------|-------------|
| `antigravity-business-analyst` | Business analyst framework for evaluating startup fundamentals. |
| `antigravity-startup-analyst` | Startup analyst framework for investment screening and DD. |

## How to use

Each skill folder contains a `SKILL.md` — this is the prompt file Claude reads to activate that skill. Supporting files in `assets/`, `references/`, and `examples/` subdirectories give Claude additional context and templates.

Load a skill by pointing Claude at the relevant `SKILL.md`, or use the Claude Code skills system if configured.

## Sources

Skills are adapted from [luisschmitzheadline/VC-Skills.md](https://github.com/luisschmitzheadline/VC-Skills.md), a community-maintained repository of VC-oriented Claude skill frameworks.
