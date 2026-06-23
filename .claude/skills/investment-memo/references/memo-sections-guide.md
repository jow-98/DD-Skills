# 42CAP Investment Memo — Section Writing Guide

This guide defines the expected content, tone, and length for each section of a 42CAP Investment Memo. Use alongside the SKILL.md workflow.

---

## Cover Table (auto-populated, no prose)

Six rows. All values pulled from Affinity or confirmed with user.

| Row | Field | Example |
|-----|-------|---------|
| 1 | Company | Galtea |
| 2 | Tag line | AI simulation and evaluation platform |
| 3 | Round | €2.8m on €9.8m Pre-Money (Seed) |
| 4 | Syndicate | €1.9m 42CAP + €0.9m Mozilla, JME, ABAC |
| 5 | Author | Johannes / Moritz, June 2025 |
| 6 | IC Members | Alex, Thomas, Moritz, Julian, Pauline, Johannes |

---

## Investment Criteria Table

Eight rows with YES / NO / SOMETIMES verdicts. Justify each verdict in 5–15 words.

| Row | Criterion | Typical justification pattern |
|-----|-----------|-------------------------------|
| 1 | Deep Tech / IP moat | Reference proprietary model, patented architecture, or unique dataset |
| 2 | Founders with domain expertise and founder-market fit | Cite PhD, prior company, or years in sector |
| 3 | Technological edge that creates durable competitive advantage | Reference benchmarks, proprietary data, technical moat |
| 4 | Potential for €1B+ exit | TAM must support venture-scale returns |
| 5 | Portfolio synergies | Name relevant portfolio companies if applicable |
| 6 | Fits our investment stage | Pre-Seed, Seed, or Series A |
| 7 | Solves a significant customer pain point | Quote a pain point with cost of inaction |
| 8 | Clear go-to-market path | Channel, ICP, motion |

Do NOT write a paragraph here — keep to a structured table only.

---

## Executive Summary / Deal Brief

**Length**: 3–5 sentences max. This is the first thing an IC member reads — it must be dense and punchy.

**Must include**:
1. What the company does (one sentence — product + customer + job-to-be-done)
2. Why now (regulatory or technical unlock, market timing)
3. Why we're excited (founder quality, traction, differentiation)
4. Deal terms (round, valuation, 42CAP ticket, co-investors)

**Tone**: Confident, analytical. No superlatives ("revolutionary", "game-changing"). No passive voice.

**Example (Galtea)**:
> Galtea builds an AI simulation and evaluation platform that helps LLM developers test and harden their models against adversarial prompts before production. The LLM reliability problem is acute — AI companies spend 30–40% of engineering cycles on manual red-teaming with no systematic tooling. Founded by [names], former AI researchers at [org], the team has shipped an early product used by [X] customers. We are investing €1.9m in a €2.8m Seed round at €9.8m pre-money alongside Mozilla, JME, and ABAC.

---

## Solution

**Length**: 2–4 paragraphs or equivalent bullet structure. Maximum 400 words.

**Must include**:
1. **Problem restatement** — 1–2 sentences. The pain, the cost, the broken status quo.
2. **Product description** — What it does, how it works (architecture if relevant), what the output looks like.
3. **Differentiation / moat** — What makes this hard to replicate. Data flywheel, proprietary model, unique workflow integration.
4. **Stage of product** — MVP, beta, GA, or production-grade. Reference paying customers or active pilots.

**Tone**: Technical enough to be credible but accessible to a non-expert IC member. Avoid vague phrases like "leverages AI" — describe the actual mechanism.

**Source data**: Granola transcripts from founder calls, Specter company profile, Google Drive tech DD notes.

---

## Market & Competition  *(or "Market, Go-To-Market & Competition" for combined sections)*

### Market sub-section

**Length**: 1–2 paragraphs + market sizing table.

**Market sizing table** (required):

| Tier | Value | Source |
|------|-------|--------|
| TAM | €Xbn | Source |
| SAM | €Xm | Source |
| SOM (5yr) | €Xm | Assumption |

**Must include**:
- Clear TAM/SAM/SOM with sources cited
- CAGR and growth trajectory
- "Why now" — regulatory (e.g. EU AI Act), technological (e.g. LLM commoditisation), or behavioural shift
- Any market structure dynamics (fragmented → consolidating, regulated industry, platform shift)

### Competition sub-section

**Length**: 1 paragraph + competitor table.

**Competitor table** (required, 4–5 rows):

| Company | HQ | Stage | Approach | Key Differentiator vs. Target |
|---------|----|-------|----------|-------------------------------|

**Must include**:
- Why the target wins in its chosen segment
- Any whitespace or positioning that incumbents cannot easily copy
- Moat assessment: is the advantage durable or replicable?

**Source data**: Specter `find_similar_companies`, web search, Affinity notes on competitive landscape.

---

## Go-To-Market

*Only used when GTM is a separate section (not merged with Market & Competition).*

**Length**: 1–2 paragraphs. Maximum 250 words.

**Must include**:
1. **ICP** — Who is the first buyer? Title, company size, vertical.
2. **Motion** — PLG / sales-led / partnerships. How do they acquire and close?
3. **Current traction** — ARR/MRR, customer count, logo names if shareable.
4. **Path to scale** — Second segment, expansion play, channel partnerships.

**Tone**: Specific. Avoid generic GTM language. "Enterprise SaaS sales motion targeting VP Engineering at 50–500 person AI-native companies, ACV €30–60k, 30-day POC to close" is better than "we focus on enterprise customers."

**Source data**: Granola founder call transcripts, Affinity deal notes, Superhuman email threads with founders.

---

## ESG

**Length**: Short structured section. See `esg-standard-language.md` for boilerplate.

**Format**: Intro paragraph (2–3 sentences) + ESG materiality table.

**ESG table structure**:

| ESG Factor | Materiality | Notes |
|------------|-------------|-------|
| Environmental | Low / Medium / High | Why |
| Social | Low / Medium / High | Why |
| Governance | Low / Medium / High | Why |
| Data Privacy | Low / Medium / High | Why |
| AI Ethics | Low / Medium / High | Why |

**Tone**: Factual and proportionate. This section is not marketing — it is a risk and impact assessment.

**Key ESG angles by sector**:
- **AI/ML companies**: AI ethics, bias, model transparency, data provenance are usually HIGH materiality
- **Healthcare/biotech**: Clinical safety, data privacy, access equity are HIGH
- **Climate/energy**: Environmental impact is HIGH; supply chain sourcing matters
- **B2B SaaS**: Governance and data privacy are HIGH; environmental is usually LOW
- **Marketplace/fintech**: Consumer protection, financial inclusion, AML/fraud are HIGH

---

## Financials

**Length**: 1–2 paragraphs + financial summary table.

**Financial summary table**:

| Metric | Current | Year 1 | Year 2 | Year 3 |
|--------|---------|--------|--------|--------|
| ARR / Revenue | | | | |
| Gross Margin | | | | |
| Burn / month | | | | |
| Headcount | | | | |
| Runway | | | | |

**Must include**:
- Current ARR or revenue run-rate and growth rate
- Gross margin (benchmark: ≥60% for SaaS)
- Monthly burn and current runway
- Key financial milestones this round is meant to achieve (e.g. "reach €500k ARR, hire 3 engineers")
- Burn multiple comment if relevant (< 1.5× is efficient)

**Source data**: Google Drive financial model, Affinity deal notes, Granola founder call transcripts.

**If financials are not yet available**: State "Financial model received and reviewed separately" and populate the table with what's known. Do not fabricate numbers.

---

## Funding

**Length**: Short structured section. 1 paragraph + cap table.

**Cap table** (standard structure):

| Shareholder | Shares | % Pre-Money | % Post-Money |
|-------------|--------|-------------|--------------|
| Founder A | | | |
| Founder B | | | |
| [Prior investor] | | | |
| ESOP / Pool | | | |
| **42CAP** | | | |
| [Co-investor] | | | |
| **TOTAL** | | **100%** | **100%** |

**Must include**:
- Pre-money valuation and rationale (comparable rounds, revenue multiple, or negotiated)
- Total round size and use of funds (1–3 bullet points)
- 42CAP ticket and pro-rata / follow-on rights if relevant
- Co-investor names and reputation context (e.g. "Mozilla Ventures brings strategic distribution")

**Tone**: Factual. Note if valuation is aggressive or if there are liquidation preferences or protective provisions worth flagging.

---

## Team

**Length**: 1 row per founder in a structured table, then 1–2 paragraphs of narrative.

**Team table**:

| Name | Role | Background | Founder-Market Fit Signal |
|------|------|------------|--------------------------|
| [Name] | CEO | [Prior role, company] | [Specific domain insight] |
| [Name] | CTO | [Prior role, company] | [Technical depth signal] |

**Narrative must include**:
- Why this team over all others for this problem (founder-market fit)
- Any prior exits, academic credentials, or domain access that is hard to replicate
- Gaps in the team and how they plan to address them (if any)
- Working dynamic / co-founder relationship if known

**Source data**: Specter `get_person_profile`, Granola founder call transcripts, Affinity notes, LinkedIn.

---

## Additional Documents

**Length**: Bullet list only. No prose.

List documents reviewed as part of DD:
- Financial model (Google Drive link if available)
- Cap table
- Pitch deck
- Reference call notes
- Expert call notes
- Tech DD notes
- Legal DD (if applicable)

---

## General Tone & Style Rules

1. **No superlatives**: "world-class", "revolutionary", "best-in-class", "cutting-edge" — delete these.
2. **Specificity over vagueness**: Name customers, cite numbers, quote benchmarks.
3. **IC perspective**: Write for a partner who has 5 minutes and zero context. Every section should be self-contained.
4. **Honest risk acknowledgement**: Every good memo flags the key risks. Omitting risks signals weak analysis.
5. **Consistent terminology**: Pick one term (ARR / revenue / GMV) and use it throughout.
6. **Numbers first**: Lead with the data point, not the interpretation. "€2.3m ARR, growing 15% MoM" not "strong growth trajectory."
7. **Maximum length**: Full memo should be 6–10 pages in Word. Compress where possible.
