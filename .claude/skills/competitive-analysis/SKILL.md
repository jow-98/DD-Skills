---
name: competitive-analysis
description: Research and output a structured competitive analysis for a startup being evaluated in due diligence. Produces a rich markdown report with a competitor table, feature comparison, and positioning summary. Use when asked for a competitive overview, competitive landscape, or comp analysis of a startup.
---

# Competitive Analysis Skill (Co Work / Markdown Output)

Produce a structured competitive landscape analysis for a startup the user is evaluating in a DD process.

## Input

The skill is invoked as `/competitive-analysis <Startup Name>` or with a company name / URL in the args.

## Workflow

Work through each step in order. Keep the user updated with one-line status messages as you go.

### 1. Resolve the target company

Use `mcp__Specter__find_company` with the startup name or URL from args.
Then call `mcp__Specter__get_company_profile` + `mcp__Specter__get_company_intelligence` on the resolved ID.
Note: company ID is a 24-char hex string returned by `find_company`.

Extract and cache:
- Full name, website, HQ location, headcount, founding year
- One-paragraph product description (what it does, for whom, core value prop)
- Last funding round & amount, lead investors
- Primary product category / market segment

### 2. Identify competitors

Use `mcp__Specter__find_similar_companies` on the resolved company ID to get an initial list.

Then use `WebSearch` to supplement:
- Search: `"<startup name>" competitors site:tracxn.com OR site:g2.com OR site:crunchbase.com`
- Search: `best alternatives to "<startup name>" 2024 2025`

Deduplicate and build a list of 10–20 meaningful competitors across these tiers:
- **Direct** – same product, same customer, same stage
- **Adjacent** – same problem, different approach or segment
- **Incumbent** – established players the startup displaces

### 3. Research each competitor

For each competitor, use `mcp__Specter__find_company` → `mcp__Specter__get_company_profile` to pull:
- URL, HQ location, headcount, founding year
- Last funding & amount, key investors
- One-sentence product description
- Key differentiator vs. the target startup (how do they differ in approach, focus, or customer?)

If Specter returns no result for a competitor, use `WebSearch` to fill the gaps.

### 4. Determine feature dimensions

Based on the target company's product category, identify 4–8 binary feature/capability dimensions that matter most for differentiation in this market. Examples:
- For procurement software: Spend analytics, Supplier discovery, AI automation, Workflow automation, Real-time insights
- For robotics safety software: HW vs SW, Dynamic safety, Human behavior prediction, AMR focus
- For voice AI: S2S model, Telephony/SIP, Self-hosting, Open source, Inbound/Outbound calls

Label each dimension clearly.

### 5. Build the competitor table

Output a markdown table with these columns (in order):
| Company | URL | Location | Headcount | Founded | Product Description | Last Funding | Investors | Key Difference | Category | [Feature 1] | [Feature 2] | ... |

Rules:
- First row after header = the target startup (mark it clearly)
- Use `✓` for confirmed feature present, `(✓)` for partial/limited, `–` for absent, `?` for unknown
- Truncate Product Description to ~120 chars
- Truncate Key Difference to ~150 chars focusing on how they differ FROM the target startup
- Sort remaining rows: Direct competitors first, then Adjacent, then Incumbents

### 6. Write the analysis narrative

After the table, write four short sections (3–5 sentences each):

**Market Overview** – Size, growth, key dynamics driving this market right now.

**Target Company Positioning** – Where the startup sits in the landscape. What it does distinctly well. Who it is most directly competing with.

**Key Competitive Threats** – Top 3 companies that pose the most risk. Why.

**Whitespace & Moat** – Where the startup has defensible differentiation or an underserved niche.

### 7. Source notes

List the main sources used (Specter, web searches, company websites) at the end.

## Output format

```
# Competitive Analysis: [Startup Name]
_Generated [date] · Sources: Specter, web search_

## Competitor Overview

[markdown table]

## Market Overview
...

## Target Company Positioning
...

## Key Competitive Threats
...

## Whitespace & Moat
...

## Sources
...
```

## Important notes

- Always put the target startup as the first data row so the reader can use it as a reference baseline.
- If Specter data is unavailable for a company, still include the row and mark data as `?` — do not silently omit.
- Cite your Key Difference observations specifically — avoid vague phrases like "less advanced". Instead write e.g. "focuses on supplier discovery only; lacks internal spend classification".
- This is a DD context: be accurate and conservative. Do not embellish capabilities you have not confirmed.
