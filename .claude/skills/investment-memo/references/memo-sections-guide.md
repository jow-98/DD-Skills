# 42CAP Investment Memo — Section Writing Guide

Detailed content expectations, structure, and tone for every section of a 42CAP Investment Memo. Use alongside SKILL.md.

---

## Cover Block

Six rows, auto-populated from Affinity. No prose — values only.

| Row | Field | Example |
|-----|-------|---------|
| 1 | Company (HQ) | Galtea (Berlin) |
| 2 | Tag line | AI simulation and evaluation platform |
| 3 | Details round | €2.8m on €9.8m Pre-Money (Seed) |
| 4 | Details syndicate | €1.9m 42CAP + €0.9m Mozilla, JME, ABAC |
| 5 | Author – Date | Johannes – June 23, 2025 |
| 6 | IC Members | Alex, Thomas, Moritz, Julian, Pauline, Johannes |

---

## Summary

Four sub-elements. All required.

### 1. Overview bullets (Market / Solution / GTM / Team)

Four bullets, one per topic. Each is a single punchy sentence — lead with the most important number or fact.

**Market**: Start with market size or CAGR. Name the trend driving it.
> "€4.2bn AI testing market growing 38% YoY, driven by LLM adoption in regulated industries."

**Solution**: Product + customer + outcome. No technical jargon.
> "Galtea's simulation platform lets AI teams red-team LLM deployments in hours instead of weeks, cutting pre-production failure rates by >60%."

**Go-To-Market**: ICP + current traction headline.
> "Targeting VP Engineering at AI-native companies ($10m–$1bn ARR); 8 paying customers, €180k ARR, growing 22% MoM."

**Team**: Founder backgrounds and the domain-fit signal.
> "PhD researchers from DeepMind and ETH Zürich; 12 years combined experience in adversarial ML and AI safety."

### 2. Our view on TAM, PMF, CES, GM%, AEC, TEAM

Six bullets, one per criterion. Rating + one supporting sentence. Be honest — if PMF is early, say so.

- **TAM**: [€Xbn] — [why venture-scale; geography scope]
- **PMF**: [Early signals / Emerging / Proven] — [one data point: NPS, retention, "must-have" survey result]
- **CES** (Cost-Efficient Scaling): [LTV:CAC X:1; payback X months; burn multiple X×]
- **GM%**: [X% today; path to Y% at scale by when]
- **AEC** (Available External Capital): [co-investor quality; grant funding; strategic interest from named corporates]
- **TEAM**: [complementarity; domain expertise; prior exits or credentials]

### 3. Key bets for the next 18 months

4–6 bullets. Each must be:
- Specific: name the metric and target
- Time-bound: by what date or milestone
- Measurable: a reader can later assess pass/fail

Examples:
> "Close first 20 enterprise clients to reach €1m ARR by Q2 2026."
> "Ship v2.0 evaluation suite to expand from LLM red-teaming to full AI governance workflows."
> "Hire VP Sales by Q4 2025 to build repeatable outbound motion."

### 4. Why-We-Invested / WYHTBI theses

3–5 numbered conviction statements. These are the beliefs the investment rests on — not observations, but falsifiable theses.

Format: **"We believe [specific claim] because [evidence or logic]."**

Examples:
> "1. We believe the AI reliability problem will grow faster than tooling to solve it — every new LLM deployment creates demand for adversarial testing that manual red-teams cannot scale."
> "2. We believe Galtea's simulation corpus (10k+ adversarial prompts) creates a compounding data moat that commodity open-source tools cannot replicate."
> "3. We believe the founding team's academic background in adversarial ML gives them a 12–18 month technical lead over VC-backed competitors building on top of GPT-4."

---

## Investment Criteria Table

Seven rows. One short paragraph (2–3 sentences) per criterion. Assess honestly.

| Criterion | What to assess |
|-----------|---------------|
| **Total Addressable Market** | TAM size, CAGR, why it's venture-scale (≥$1bn TAM). Name the source. |
| **Product-Market Fit** | PMF evidence: retention curves, NPS, "must-have" %, pull from existing users. Distinguish early signal from proven PMF. |
| **Cost-Efficient Scaling** | LTV:CAC (≥3×), payback period (<18mo), burn multiple (<1.5×), gross margin trajectory. |
| **Gross Margin %** | Current GM% and target at scale. Benchmark: ≥60% SaaS, ≥40% marketplace, ≥70% pure software. |
| **Available External Funding** | Co-investor quality and strategic fit, grant funding secured, corporate interest. Does the company attract smart capital? |
| **ESG** | One-sentence ESG summary — main materiality factor and whether any red flags exist. |
| **Team Quality** | Complementarity of founders, domain expertise, execution track record, network access. |

---

## Solution

Six bullet points converted into prose paragraphs. One paragraph per bullet.

### 1. Value prop to customer — Theory and Practice

**Theory**: What outcome does the customer get? Frame around the job-to-be-done and the cost of not solving it.
**Practice**: Evidence of customer acceptance. Quotes, pilot results, NPS, retention data, expansions.

> "Theory: [Company] eliminates [specific pain] for [ICP], reducing [time/cost/risk] by [X%]. Practice: Of the 8 paying customers, 6 have expanded usage within 90 days; NPS of 72 with three customers describing the product as 'the first tool we've seen that actually works at scale.'"

### 2. Quality use cases (Theory) or Status MVP (Practice)

Name 2–3 specific use cases with the user, workflow, and outcome. If early-stage, describe MVP status: what's built, what's not, what's live.

> "Use case 1: [Company A] uses [product] to [workflow], cutting [X hours] to [Y minutes]. Use case 2: [Company B] deploys [product] for [purpose], reducing [error rate] from X% to Y%."

### 3. Back-end (Tech stack) and Front-end (CX)

**Back-end**: Architecture, infrastructure, model type, data pipeline, key technical choices and why.
**Front-end**: How customers interact — UI, API, embedded, no-code. UX quality signals.

> "Back-end: Python/FastAPI microservices on AWS; fine-tuned Llama 3.1 70B on proprietary adversarial dataset; vector store on Pinecone. Front-end: Web app (React) with a no-code scenario builder; REST API for enterprise integrations; Slack integration for alerting."

### 4. Product roadmap

Three stages. Each with a milestone and approximate date.

> "Phase 1 (Q3 2025): GA launch of evaluation suite with 50+ pre-built adversarial scenarios. Phase 2 (Q1 2026): Multi-model comparison and compliance reporting for EU AI Act. Phase 3 (Q3 2026): Agent monitoring — extend from pre-production to live production LLM monitoring."

### 5. Stickiness

What makes it hard to leave? Be specific — not "users love it" but what switching costs exist.

- Data moat: customer data locked in the platform
- Workflow integration: embedded in CI/CD pipeline
- Network effects: shared benchmarks or community
- High-touch: analyst relationship, custom scenario libraries
- Regulatory: compliance certification tied to platform

### 6. Key findings from tech deep dive

Summarise the tech DD output in 2–4 sentences. Include architecture strengths, any gaps, and remediation plan.

> "Architecture is modular and well-documented; the team can iterate quickly. The main gap is absence of SOC 2 Type II — company is targeting certification by Q1 2026, which is standard for enterprise deals. No critical single-vendor dependencies. Security posture is strong for stage."

---

## Market & Competition

Three required elements.

### 1. Bottom-up market sizing table

| Region | # Companies | Expected ACV | ICP Fit % | ARR Potential | Mkt Share % | Serviceable ARR |
|--------|------------|-------------|-----------|---------------|-------------|----------------|
| [Home market] | X,XXX | €XXX,000 | XX% | €XXXm | X% | €XXm |
| Top-5 Europe | X,XXX | €XXX,000 | XX% | €XXXm | X% | €XXm |
| Global (ex-EU) | XX,XXX | €XXX,000 | XX% | €X.Xbn | X% | €XXXm |
| **Total** | | | | | | **€XXXm** |

**Sources for company counts**: Specter `find_similar_companies`, Eurostat (EU), US Census Bureau, or industry reports.

**ICP Fit %**: The share of total companies that genuinely match the ICP (size, sector, tech stack, budget). Be conservative — 5–20% is typical.

**Mkt Share %**: Beachhead share assumption at 5 years. 0.5–5% is standard for early-stage; justify if higher.

### 2. Ability and effort to scale countries

Address: regulatory requirements per market, localisation effort (language, integrations), GTM cost per new market, and whether the product travels without customisation.

> "The product works in English today; EU expansion requires German and French localisation (estimated 2 engineer-months). Regulatory: EU AI Act applies in all EU markets — the company's compliance module is a GTM asset, not a blocker. US expansion is medium-effort: data residency requirements need an AWS US-East region deployment."

### 3. Competition overview including feature comparison

Named competitors with a feature comparison table. 4–5 rows (target + top 4 direct competitors).

| Company | HQ | Funding | Key Feature | Weakness vs. [Target] |
|---------|----|---------|-------------|----------------------|
| [Target] | | | | — |
| Competitor A | | €Xm | | |
| Competitor B | | €Xm | | |
| Competitor C | | €Xm | | |

Closing paragraph: where the target wins and why that position is defensible.

---

## Go-To-Market

Four elements. Be specific — names, numbers, channels.

### 1. Initial growth hacking approach (0 → 10 customers)

What was the first GTM motion? Cold outreach, community, inbound, partnerships, warm intros from investors. What converted?

> "First 8 customers came from founder's personal network in the LLM developer community (Twitter/X and Hugging Face). Two came inbound via a guest post on The AI Engineer newsletter. Average sales cycle: 3 weeks; no AE required at this stage."

### 2. Mid-term approach by channel

Three channels — Marketing, Sales, Partner channel:

**Marketing**: Content, SEO, events, paid, community. Name 1–2 specific bets.
**Sales**: AE headcount plan, ICP targeting, outbound motion, ACV target, playbook.
**Partner channel**: System integrators, cloud marketplaces (AWS, Azure), strategic alliances with platform vendors.

### 3. Clients (Today and Pipeline)

**Today** — named customers (or anonymised by sector if NDA), ARR, contract type, since when.
**Pipeline** — named opportunities, stage in funnel, expected ARR, close date.

Example:
> "Today: Aleph Alpha (€48k ARR, annual, since Jan 2025); Mistral AI (€36k ARR, annual); [FinTech Co.] (€60k ARR, pilot converting to annual). Pipeline: [Insurance Co.] — POC signed, €120k ARR, close Q3 2025; [Bank] — in evaluation, €80k ARR; Total pipeline: €420k ARR."

### 4. Key findings from client reference calls

Direct quotes preferred. Attribute by role (not necessarily name). Capture both positives and any concerns.

> "'This is the first tool that gave us confidence before shipping to production — we caught 3 critical issues in the first week.' — Head of AI, Series B fintech. Common theme: customers value speed-to-insight (30-min onboarding) and the depth of the adversarial scenario library. One customer flagged limited reporting features for compliance teams — addressed in Q3 roadmap."

---

## ESG

**Standard opening paragraph** (use verbatim, replace "xxx"):

> "During the investment process we analyzed [Company] along ESG criteria, to assess the status quo of the company, identify potential risks and, if necessary, formulate an action plan to address these concerns. Below you will find a materiality assessment, identifying the ESG issues core to the company's financial performance, long-term success, and stakeholder value over the coming 18–24 months:"

**ESG table** — three rows (Environmental / Social / Governance). Bullet points in the right column.

See `esg-standard-language.md` for pre-filled tables by sector.

**Closing sentence** (use verbatim):
> "In addition to the materiality issues outlined above, we identified no red flags that would have a negative ESG impact."

**Positive impact bullet** — add one specific positive externality if relevant:
> "• Positive impact: [Company]'s platform reduces the risk of biased or unsafe AI deployments reaching production, contributing to safer AI ecosystems."

---

## Financials

Five elements. All must use real numbers from the financial model (Google Drive).

### 1. Unit economics and quality of top-line

- Recurrence: % of revenue that is recurring (SaaS ARR vs. one-off services)
- Gross Margin: current GM% and target at scale
- LTV:CAC and payback period

> "88% of revenue is recurring ARR. Current GM% is 71%; target 80%+ at scale as infrastructure costs amortise. LTV:CAC of 4.2:1 at current ACV; CAC payback 9 months."

### 2. Growth cost factor

What does it cost to grow €1 of new ARR? (Net burn ÷ net new ARR = burn multiple)

> "Current burn multiple of 1.4× — efficient for Seed stage. Each €1 of new ARR costs €1.40 in net burn. Target: <1.0× by €1m ARR as fixed costs are spread."

### 3. Current traction

ARR or MRR + growth rate + customer count.

> "€180k ARR; growing 22% MoM over last 3 months. 8 paying customers, average ACV €22.5k. 2 customers have expanded (upsell rate 25% of cohort)."

### 4. Pricing

Model, tiers, and ACV range.

> "Usage-based + platform fee. Tier 1: €12k/yr (up to 100k evaluations/mo). Tier 2: €36k/yr (up to 1m evaluations/mo, SLA). Enterprise: custom pricing, starting €60k/yr. Target ACV: €30–50k at scale."

### 5. Financial plan — simple version with key assumptions

State the key growth assumption (NN new customers per month at €X ACV) and show the high-level projection:

> "Key assumption: 3 new customers per month at €30k ACV average from Q4 2025. Below you will find the P&L for the next 12 months: [insert table from financial model]"

Bullet format for the financial narrative:
- Top-line: Increase ARR from €Xk to €Xm by [date]
- Cost base today: €Xk/month = €Xk HC + €Xk non-HC
- Cost base at 18 months: €Xk/month = €Xk HC + €Xk non-HC
- Burn: €Xm net burn over 18 months; round provides X–Y months runway

---

## Funding

Four elements.

### 1. Past funding rounds (what was raised and what was achieved)

List chronologically. For each round: amount, type, date, lead investor, and what milestones were hit.

> "Pre-seed: €400k (convertible note, 20% discount, Apr 2024) — built MVP, signed first 3 LOIs. Grants: €150k EXIST grant (Jul 2024) — used for compute and initial team."

### 2. Current round — details and use of funds

Total round, 42CAP ticket, co-investors, and what will be proven.

> "€2.8m Seed; 42CAP leading with €1.9m; Mozilla Ventures (€500k), JME (€250k), ABAC (€150k) co-investing. Use of funds: hire 3 GTM FTEs (€900k), product development (€800k), compute and infra (€300k), runway buffer (€800k). What will be proven: reach €1m ARR with 30+ enterprise customers, validate repeatable outbound sales motion with <18mo CAC payback."

### 3. Future funding needs

Next round milestone and break-even path.

> "Series A targeted at €1m ARR; expected raise in 18–24 months at €15–20m pre-money. Break-even at €2.5m ARR (approximately 36 months post-Seed at current cost structure)."

### 4. Potential exit channels

Name specific acquirer candidates and comparable transactions.

> "Primary exit path: trade sale to AI infrastructure players (Weights & Biases, Anthropic, Google DeepMind, Palantir) or enterprise software consolidators (ServiceNow, Datadog). Comparable exits: Arthur AI acquired by [X]; Robust Intelligence acquired by Cisco (2024) at ~€100m — validates strategic value of AI safety tooling. IPO possible at €200m+ ARR scale."

### Cap table

| Shareholder | Share Class | Shares | % Undiluted | % FD |
|-------------|------------|--------|-------------|------|
| Founder A | Ordinary | | | |
| Founder B | Ordinary | | | |
| [Prior investor] | CLA converted | | | |
| ESOP / Option pool | Options | — | — | |
| **42CAP** | Seed Preferred | | | |
| [Co-investor] | Seed Preferred | | | |
| **TOTAL** | | | **100%** | **100%** |

---

## Team

### Structure

One paragraph per key person in this order: **CEO → CPO → CRO → CFO → Other key hires**

Each paragraph must include:
1. Name and role
2. 2–3 prior companies or roles (specific, not generic)
3. What they uniquely bring to this company
4. **Fund Raising Power** — for CEO only: track record raising capital, investor network, ability to tell the story
5. Reference call findings (if available) — direct quote or theme

### CEO paragraph

The CEO paragraph is the most important. It must answer: can this person raise a Series A, hire talent, and close enterprise deals?

> "**[Name], CEO**: Previously [Role] at [Company A] (acquired by X for €Ym in 20XX) and [Role] at [Company B]. At [Company A] he built the enterprise sales team from 0 to €8m ARR and led the Series B. Brings deep relationships with ML engineering leaders at European fintechs and automotive OEMs — the core 42CAP ICP. Fund Raising Power: strong — closed this Seed in 6 weeks with Mozilla Ventures as strategic co-investor; clear ability to articulate technical differentiation to non-technical investors. Reference: 'One of the sharpest product-market thinkers I've worked with — he identified our core pain before we could articulate it ourselves.' — VP Engineering, Series C fintech."

### CPO / CTO paragraph

Focus on technical depth, prior product experience, and ability to attract engineers.

### CRO / CCO paragraph

Focus on commercial track record, domain network, and enterprise sales experience.

### CFO paragraph (if present)

Focus on financial rigour, fundraising process management, and investor relations.

### Team closing bullets

- Post-round hiring plan: roles, count, and target date
- Talent pool context: why this city/ecosystem is an asset for recruitment

---

## Additional Documents

Bullet list only. No prose. Reflect what is actually in the DD folder.

Standard list:
- Pitch deck
- Expert and customer feedback
- Competitor overview
- Cap table
- Financial model
- Tech DD
- Sales pipeline
- [Any other materials in the data room]

---

## Style Rules

1. **No superlatives** — delete "world-class", "revolutionary", "best-in-class", "cutting-edge".
2. **Numbers before narrative** — lead with the data point, then the interpretation.
3. **Named companies and people** — anonymise only when NDA requires; otherwise name customers, competitors, and reference sources.
4. **Honest on weak areas** — every good memo flags the main risk. Omitting risks signals weak analysis.
5. **IC perspective** — write for a partner who has 5 minutes and zero prior context. Each section must be self-contained.
6. **Maximum length** — full memo should be 6–10 pages in Word. Compress where possible.
7. **Consistent terminology** — pick one term (ARR / revenue / GMV) and use it throughout.
