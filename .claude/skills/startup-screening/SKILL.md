---
name: startup-screening
description: "Three-phase startup screening for early-stage DD. Phase 1 — GO/NO-GO scorecard (9 dimensions: problem, market, timing, moat, unit economics, founder-market fit, feasibility, GTM, risk). Phase 2 — Fundamentals evaluation (market sizing, unit economics, competitive position, team). Phase 3 — Business case (KPI framework, financial model inputs, investment thesis). Use for pre-seed through Series A. Run Phase 1 first; only proceed to Phase 2 if score is CONDITIONAL or GO."
---

# Startup Screening

Three-phase framework for evaluating early-stage startups in a DD context. Designed to be efficient: Phase 1 takes 15–20 minutes; only proceed deeper if the opportunity passes the initial screen.

---

## Phase 1 — GO/NO-GO Scorecard (9 Dimensions)

### Intake Checklist (Ask First)

Before scoring, confirm:
- One-sentence idea + target user + job-to-be-done
- Business model: B2B/B2C, SaaS/usage-based/marketplace/services, ACV/ARPU range
- Geography and any regulatory constraints
- Target outcome: venture-scale, profitable SMB, or thesis-driven R&D
- Current evidence: interviews done, pilots, pre-sales, competitors identified, pricing assumptions

### The 9-Dimension Scorecard

Score each dimension 0–10. Weight × score = weighted score. Sum to 100.

| # | Dimension | Weight | Score (0–10) | Weighted | What to evaluate |
|---|-----------|-------:|:------------:|:--------:|-----------------|
| 1 | Problem severity | 15% | | | Urgency, cost of inaction, current workarounds. Is this a painkiller or vitamin? |
| 2 | Market size | 12% | | | Sufficient demand for target outcome (venture-scale ≥ $1B TAM) |
| 3 | Market timing | 10% | | | Clear "why now" — regulatory shift, technology unlock, behaviour change |
| 4 | Competitive moat | 12% | | | Defensibility over 3–5 years: network effects, data, switching costs, IP, distribution |
| 5 | Unit economics | 15% | | | Profit path: LTV:CAC ≥ 3×, payback < 18m, gross margin ≥ 60% (SaaS) |
| 6 | Founder-market fit | 8% | | | Domain access, expertise, and execution track record in this space |
| 7 | Technical feasibility | 10% | | | Buildable with available talent and technology; no critical single dependencies |
| 8 | GTM clarity | 10% | | | Defined ICP, channels, motion, path to first 10 customers |
| 9 | Risk profile | 8% | | | What kills it and likelihood: regulatory, tech, market, execution |
| | **Total** | **100%** | | **/100** | |

### Evidence Rules

- **Strong evidence** = behavioural commitment with cost (paid pilots, signed LOIs, switching costs, data access granted)
- **Weak evidence** = opinions, hypotheticals, founder claims without corroboration
- Downgrade scores when evidence is weak. A well-articulated story ≠ evidence.
- Triangulate important claims across at least two sources.

### Verdict Thresholds

| Score | Verdict | Action |
|-------|---------|--------|
| 80–100 | **GO** | Proceed to Phase 2 |
| 60–79 | **CONDITIONAL** | Validate riskiest assumption (RAT) first, then Phase 2 |
| 40–59 | **PIVOT** | Identify which dimensions fail; explore adjacent positioning |
| <40 | **NO-GO** | Stop; document reasoning |

### Riskiest Assumption Test (RAT)

For CONDITIONAL verdicts, identify the single assumption that kills the business if wrong, and define the cheapest falsifiable test:

| RAT | Test | PASS threshold | FAIL threshold |
|-----|------|---------------|----------------|
| [Assumption] | [Experiment type: interview / smoke test / concierge / paid pilot] | [Specific metric] | [Specific metric] |

Pre-register thresholds before running the test. Do not move goalposts.

### Validation Ladder (Default)

| Step | Goal | Strong signal |
|------|------|--------------|
| Interviews (≥20) | Validate problem and context | Repeated pain, real workarounds, existing spend |
| Smoke test | Validate demand at price | Qualified conversion with price shown |
| Concierge / WoZ | Validate workflow value | Users complete the job and return |
| Paid pilot | Validate willingness-to-pay | Paid, renewed, or expanded |

### Phase 1 Output Format

```
# Startup Screening — Phase 1: GO/NO-GO
**Company**: [Name]  **Date**: [Date]  **Analyst**: [Name]

## Scorecard
[table above filled in]

## Verdict: [GO / CONDITIONAL / PIVOT / NO-GO]
**Score**: [X/100]

## Rationale
[3–5 sentences: which dimensions drove the verdict; what's strong, what's weak]

## Riskiest Assumption
[If CONDITIONAL]: [Assumption statement + test design + PASS/FAIL thresholds]

## Next Step
[Specific action: "Proceed to Phase 2" / "Run RAT by [date]" / "No-go — file for reference"]
```

---

## Phase 2 — Fundamentals Evaluation

Run only if Phase 1 verdict is GO or CONDITIONAL (RAT passed). Produces a deeper quantitative assessment across four areas.

### 2a. Market Sizing

Apply bottom-up methodology (reference `/market-sizing` skill for full workflow):

```
TAM = # ICP companies × ACV
SAM = TAM × ICP Fit %
SOM = SAM × Market Share % (0.5–5% for beachhead)
```

Document: ICP definition, data sources for company counts, ACV benchmark, ICP Fit % rationale.
Sanity-check with one top-down industry report (CAGR source required).

**Flag if**: TAM < $500M (marginal for venture), or market is winner-take-all with an entrenched leader.

### 2b. Unit Economics

| Metric | Formula | Benchmark | Startup's Value |
|--------|---------|-----------|----------------|
| CAC | Sales + Mktg spend ÷ new customers | Stage-dependent | |
| LTV | ARPA × Gross Margin ÷ Churn Rate | ≥ 3× CAC | |
| LTV:CAC | LTV ÷ CAC | ≥ 3× (min); ≥ 5× (strong) | |
| Payback period | CAC ÷ (ARPA × Gross Margin) | < 18 months | |
| Gross margin | (Revenue − COGS) ÷ Revenue | ≥ 60% SaaS | |
| Burn multiple | Net burn ÷ Net new ARR | < 1.5× | |

If metrics are unavailable, use comparables from public SaaS benchmarks and flag as estimated.

### 2c. Competitive Position

Summarise the competitive landscape in a 5-row table (target + top 4 direct competitors):

| Company | Founded | Funding | Headcount | Key Differentiation | Moat Type |
|---------|---------|---------|-----------|--------------------| ----------|

Assess: Is the target in a crowded market or whitespace? Can the target win in their chosen segment given current resources?

For full competitive analysis, invoke `/competitive-analysis <startup name>`.

### 2d. Team Assessment

| Factor | Assessment | Signal |
|--------|-----------|--------|
| Founder-market fit | [Strong / Moderate / Weak] | Domain experience, network access |
| Technical depth | [In-house / Outsourced / Gap] | Can they build the product? |
| Commercial capability | [Strong / Moderate / Gap] | Sales, BD, GTM experience |
| Prior exits or venture track record | [Yes / No] | Execution credibility |
| Team completeness for stage | [Complete / Missing: X] | Pre-seed vs. Series A needs differ |

### Phase 2 Output Format

```
# Startup Screening — Phase 2: Fundamentals
**Company**: [Name]  **Date**: [Date]

## Market Sizing
TAM: €[X]  SAM: €[X]  SOM: €[X]
[ICP definition + sources]

## Unit Economics
[table filled in]
LTV:CAC: [X×] — [Strong / Acceptable / Weak]

## Competitive Position
[5-row competitor table]
Assessment: [2–3 sentences]

## Team
[team assessment table]
Assessment: [2–3 sentences]

## Phase 2 Verdict
[Proceed to Phase 3 / Conditional on X / No-go]
```

---

## Phase 3 — Business Case

Run only after Phase 2 confirms a genuine investment opportunity. Produces the investment thesis and KPI framework.

### 3a. Investment Thesis (1 page)

Structure:
1. **Market opportunity** — why this market, why now
2. **Product differentiation** — what's defensible and why
3. **Team** — why this team can win
4. **Traction** — evidence of PMF (reference `/pmf-assessment` for full framework)
5. **Financial upside** — path to exit; return scenario at 5–7 years

### 3b. KPI Framework

Define the North Star metric and supporting KPIs by business model:

**SaaS:**
| KPI | Target | Current | Trend |
|-----|--------|---------|-------|
| ARR | | | |
| MRR Growth (MoM) | ≥ 10% | | |
| Churn Rate (logo) | < 2%/mo | | |
| NDR (Net Dollar Retention) | ≥ 120% | | |
| CAC Payback | < 18 mo | | |
| Rule of 40 (Growth % + FCF Margin) | ≥ 40 | | |

**Marketplace:**
| KPI | Target | Current | Trend |
|-----|--------|---------|-------|
| GMV Growth (MoM) | ≥ 15% | | |
| Take Rate | | | |
| Liquidity (supply/demand match %) | ≥ 70% | | |
| Repeat Purchase Rate | ≥ 50% | | |

**Consumer:**
| KPI | Target | Current | Trend |
|-----|--------|---------|-------|
| DAU/MAU | ≥ 30% | | |
| D30 Retention | ≥ 20% | | |
| Virality (K-factor) | ≥ 1 | | |
| ARPU | | | |

### 3c. Return Scenario

| Scenario | Revenue at Exit (5yr) | Multiple | Enterprise Value | Our Ownership | Gross Return |
|----------|-----------------------|----------|-----------------|---------------|-------------|
| Base | | 8–12× ARR | | | |
| Bull | | 15–20× ARR | | | |
| Bear | | 4–6× ARR | | | |

### Phase 3 Output Format

```
# Startup Screening — Phase 3: Business Case
**Company**: [Name]  **Date**: [Date]

## Investment Thesis
[5-point structure above]

## KPI Framework
[business-model-specific KPI table]
Current performance: [assessment]

## Return Scenarios
[3-row scenario table]
Expected return: [Base case at X× in Y years]

## Recommendation
[Invest / Pass / Deepen DD on X]
**Key condition**: [what must be true to invest]
**Key risk**: [what could kill the investment]
```

---

## AI / Automation Notes

If the startup depends on AI (agents, LLMs, automation), validate explicitly:
- **Data rights**: can they legally and reliably access the required training/inference data?
- **Reliability**: what are the failure modes and human fallback mechanisms?
- **Cost-to-serve**: model inference + retrieval + HITL costs at scale — model in Phase 2 unit economics.
- **Competitive durability**: is the AI advantage proprietary (fine-tuned, data moat) or commodity (OpenAI wrapper)?

---

## Stage Calibration

| Stage | Phase 1 Focus | Phase 2 Focus | Phase 3 Focus |
|-------|-------------|--------------|--------------|
| Pre-seed | Problem severity, founder-market fit | Early interviews, smoke tests | Vision + team |
| Seed | PMF signals, GTM clarity | Retention curves, first cohorts | Unit economics path |
| Series A | Proven unit economics | Repeatability, NDR, payback | Scale model + exit |


---

## Language and Tone

- Use expert terminology appropriate to the context (VC due diligence, financial analysis, competitive intelligence)
- Avoid superfluous prose, self-references, expert advice disclaimers, and apologies
- No em dashes or en dashes; use commas, parentheses, or rewrite the sentence instead
- Lead with data and specific findings; interpretation follows the evidence
- No superlatives (world-class, revolutionary, best-in-class, cutting-edge)

