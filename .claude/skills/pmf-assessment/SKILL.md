---
name: product-market-fit
description: Master frameworks for measuring, achieving, and maintaining product-market fit (PMF). Use when validating new products, assessing readiness to scale, diagnosing retention problems, planning market expansion, measuring "very disappointed" score, implementing PMF engines, or determining if you have permission to grow. Covers Sean Ellis 40% test, Superhuman PMF engine, retention curve analysis, leading/lagging indicators, pre-PMF vs. post-PMF strategies, Lenny's 46-leader principles (reference customer counts, segment-level fit, pull signals, multi-stage PMF), and maintaining fit as markets evolve.
---

# Product-Market Fit

Frameworks for measuring, achieving, and maintaining the critical milestone where your product satisfies strong market demand.

## Overview

Product-Market Fit (PMF) is the degree to which a product satisfies strong market demand - the inflection point where a product becomes a "must-have" for a well-defined market segment.

**Core Principle:** PMF is not a destination, it's a milestone that gives you permission to scale. Maintaining it requires continuous attention to customer needs and market evolution.

**Key Insight:** You can't manufacture PMF through marketing or sales tactics. PMF comes from deeply understanding a specific market segment and building something they desperately need. Scaling before PMF is the number one killer of startups.

**Historical Context:**
- Term coined by Marc Andreessen (2007)
- Operationalized by Sean Ellis with 40% rule (2010)
- Systematized by Rahul Vohra with Superhuman PMF Engine (2017)

## When to Use This Skill

**Auto-loaded by agents**:
- `product-strategist` - For PMF measurement, Sean Ellis survey, and retention analysis

**Use when you need**:
- Measuring product-market fit status
- Running Sean Ellis PMF surveys
- Analyzing retention curves
- Determining readiness to scale
- Diagnosing retention problems
- Planning PMF improvement strategies
- Deciding pre-PMF vs. post-PMF tactics
- Validating market expansion opportunities

---

## Step 0 — Pull from VC Knowledge Hub

Before running any PMF framework, sweep internal sources for existing signals on this company.

```
mcp__vc-knowledge-hub__search("<company name>")
  → find all prior touchpoints: Affinity notes, Granola meeting transcripts, Drive documents

mcp__vc-knowledge-hub__get_company("<company name>")
  → company profile with meeting history and pitch deck content — surface any retention or NPS data shared by the founders

mcp__vc-knowledge-hub__ask("What PMF signals has 42CAP seen for <company name>? Include retention data, NPS scores, churn figures, and any customer reference feedback.")
  → synthesised answer with citations across all sources

mcp__vc-knowledge-hub__get_meeting_feed()
  → catch any recent founder or customer calls with PMF-relevant commentary
```

Cache: any "very disappointed" survey data, retention curve shape, NPS, churn figures, or customer reference quotes already in the pipeline. Use these as the primary evidence base before applying the frameworks below.

If the VC Knowledge Hub returns no results or incomplete data, fall through to Granola, Affinity, Google Drive, and Superhuman individually — those remain the authoritative sources.

---

## Measuring Product-Market Fit

### The Sean Ellis Test (40% Rule)

The definitive method for measuring PMF through a single powerful question.

**The Question:**
> "How would you feel if you could no longer use [product]?"
> - a) Very disappointed
> - b) Somewhat disappointed
> - c) Not disappointed (it isn't really that useful)

**PMF Threshold:**
- **40%+ "Very disappointed" = PMF achieved**
- 25-40% = Close, keep iterating
- <25% = No PMF yet

**Why this works:**
- Measures must-have vs. nice-to-have
- Predictive of retention
- Correlates with organic growth
- Simple to administer
- Actionable results

**Complete survey methodology:** See `assets/sean-ellis-pmf-survey.md` for:
- Full survey template
- When and how to administer
- Sample size requirements
- Analysis framework
- Segment breakdowns

---

### The Superhuman PMF Engine

Systematic framework for measuring and improving PMF score quarter over quarter.

**Philosophy:** PMF is not binary - it's a spectrum you can measure and improve systematically.

**The 5-Step Engine:**
1. **Segment users:** Very disappointed / Somewhat / Not disappointed
2. **Analyze champions:** Who are the "very disappointed" users? What do they have in common?
3. **Find your roadmap:** Different strategies for each segment
4. **Build strategically:** 50% for champions, 50% to convert warm users, 0% for wrong-fit
5. **Measure progress:** Re-survey quarterly, track improvement

**Superhuman's Results:**
```
Q1 2017: 22% → Q2 2018: 58% (18 months)
```

**Complete framework:** See `assets/superhuman-pmf-engine.md` for:
- Detailed 5-step process
- Segment analysis worksheets
- Roadmap allocation strategy
- Progress tracking templates
- Prioritization frameworks

---

### Retention Curves: The Ultimate PMF Test

Retention patterns reveal if your product is truly a must-have.

**Three Patterns:**

**1. Leaky Bucket (No PMF):**
- Continuously declining curve
- Never flattens
- Users leave permanently
- Action: Find PMF before scaling

**2. Flattening Curve (PMF!):**
- Drops initially, then flattens at 30-50%
- Core users retain long-term
- Ready to scale
- Action: Prove acquisition channel, then scale

**3. Smiling Curve (Strong PMF):**
- Usage increases over time
- Network effects or habit formation
- Examples: Social networks, collaboration tools
- Action: Scale aggressively

**Complete analysis:** See `assets/retention-curve-analysis.md` for:
- How to build retention curves
- Diagnosing problems
- Industry benchmarks
- Improving retention by phase

---

## Leading vs. Lagging Indicators

Use both types of indicators to measure PMF comprehensively.

### Leading Indicators (Feel It Now)

Early signals before metrics confirm PMF:

**1. Organic Growth:**
- Word-of-mouth referrals happening
- Unprompted social media mentions
- Inbound signup requests
- Target: >50% of growth organic

**2. User Engagement:**
- High DAU/MAU ratio (stickiness)
- Deep feature adoption
- Long session times
- Target: DAU/MAU >30-40% (B2B), >60% (B2C Social)

**3. Customer Passion:**
- "Don't take this away from me"
- Volunteering to help
- Unsolicited recommendations
- Active community forming

**4. Sales Velocity (B2B):**
- Deals closing faster over time
- Less price resistance
- Shorter sales cycles
- Higher win rates

**5. Struggle to Keep Up:**
- Natural waitlist forming
- Capacity challenges
- Can't hire fast enough
- Good problem to have

### Lagging Indicators (Metrics Confirm It)

Hard metrics that retrospectively validate PMF:

**1. Retention:**
- B2C: <5% monthly churn
- B2B: <2% logo churn
- Cohort curves flattening

**2. Net Promoter Score:**
- NPS >50 (world-class)
- High promoters, low detractors

**3. Unit Economics:**
- LTV:CAC >3:1 (minimum), >5:1 (ideal)
- Payback period <12 months
- Gross margin >70% (SaaS)

**4. Growth Rate:**
- Exponential not linear
- 10%+ month-over-month
- Compounding effects visible

**5. Market Pull:**
- Inbound >50% of new customers
- PR coverage without effort
- Competitive response
- Industry recognition

**Comprehensive guide:** See `references/leading-lagging-indicators.md` for:
- Detailed metrics and benchmarks
- How to use both together
- Early warning systems
- Decision frameworks

---

## Dashboard and Tracking

### The PMF Dashboard

Track PMF through multiple lenses for complete picture.

**Primary Metrics (The Big 3):**
1. Sean Ellis PMF Score (>40% target)
2. Retention Curves (flattening pattern)
3. Net Promoter Score (>50 target)

**Supporting Metrics:**
- Leading indicators (organic growth, engagement, passion)
- Lagging indicators (unit economics, growth rate)
- Segment-specific breakdowns

**Update frequency:**
- Daily: Engagement metrics
- Weekly: Growth metrics
- Monthly: Dashboard review
- Quarterly: Deep-dive + PMF survey

**Complete dashboard:** See `assets/pmf-measurement-dashboard.md` for:
- Full dashboard template
- Metric definitions and benchmarks
- Alert thresholds
- Segment analysis
- Visualization guidelines

---

## Path to Achieving PMF

### Stage 1: Market Understanding

**Activities:**
- Interview 30-50 potential customers
- Understand current alternatives
- Map jobs-to-be-done
- Identify underserved segments

**Timeline:** 2-4 weeks

### Stage 2: Value Hypothesis

**Framework:**
```
For [target segment]
Who [problem/need]
Our [product category]
That [key benefit]
Unlike [alternatives]
We [unique capability]
```

**Validation:** Would 40% be "very disappointed" to lose this?

**Timeline:** 1-2 weeks

**Complete canvas:** See `assets/value-proposition-canvas.md`

### Stage 3: MVP Validation

**Build minimum viable product:**
- Core value only
- Fast to iterate
- Good enough to test hypothesis

**Validation criteria:**
- 10-20 users experiencing value
- Qualitative feedback
- Usage patterns match hypothesis

**Timeline:** 4-8 weeks

### Stage 4: PMF Measurement

**Implement measurement:**
- Sean Ellis survey (after 2-4 weeks of use)
- Minimum 40 responses
- Track % "very disappointed"
- Set improvement targets

**Timeline:** 2-4 weeks to implement

### Stage 5: Systematic Improvement

**Apply Superhuman Engine:**
- Segment by PMF score
- Analyze champions
- Build 50/50 roadmap
- Iterate quarterly

**Timeline:** 6-18 months to reach 40%+

---

## The Three Stages of PMF

### Pre-PMF: Finding Fit (6-24 months)

**Characteristics:**
- High churn, low organic growth
- Sales struggle
- <40% "very disappointed"

**Focus:**
- Rapid iteration
- Customer discovery (10+ interviews/week)
- Small cohorts, extreme learning velocity
- Don't scale yet

**Common mistakes:**
- Premature scaling
- Building too many features
- Ignoring retention data

### At-PMF: Initial Traction (3-6 months)

**Characteristics:**
- 40%+ "very disappointed"
- Retention curves flattening
- Word-of-mouth spreading
- Easier to close deals

**Focus:**
- Prove one acquisition channel works
- Optimize unit economics
- Build for scalability
- Strengthen core value

**Green lights to scale:**
- LTV:CAC >3:1
- Retention curves flat/improving
- One repeatable channel working

### Post-PMF: Scaling (Years)

**Characteristics:**
- Predictable growth
- Multiple channels working
- Strong unit economics
- Efficient go-to-market

**Focus:**
- Scale acquisition
- Geographic expansion
- Adjacent segments
- Product line extensions

**Risk:** Losing PMF through feature bloat, serving wrong customers, losing focus

**Detailed guide:** See `references/pmf-stages-guide.md` for:
- Complete stage breakdowns
- Strategies for each stage
- Transition criteria
- Common mistakes and solutions

---

## Maintaining PMF Over Time

### Why PMF Gets Lost

**Internal factors:**
- Feature bloat dilutes core value
- Serving wrong customers
- Slow iteration speed
- Technical debt blocks innovation

**External factors:**
- Market evolution (needs change)
- New competitors (better alternatives)
- Technology shifts (new capabilities)
- Economic conditions (budget priorities)

### Maintenance Strategies

**1. Continuous Customer Contact:**
- Never stop interviewing (10-20 per week)
- Watch usage data constantly
- Monitor NPS and PMF scores quarterly
- Teresa Torres' weekly touchpoints

**2. Core Value Protection:**
- Resist feature bloat (80% strengthen core, 20% new)
- Maintain product focus
- Protect speed and simplicity
- Regular feature pruning

**3. Segment Discipline:**
- Don't chase every customer
- Say no to wrong-fit deals
- Maintain ICP (ideal customer profile)
- Measure PMF by segment

**4. Regular PMF Surveys:**
- Quarterly Sean Ellis surveys
- Track score by segment
- Watch for declining scores
- Act on early warnings

**5. Competitive Monitoring:**
- Track new alternatives
- Monitor customer switching
- Stay ahead on innovation
- Evolve value proposition

**Complete guide:** See `references/maintaining-pmf-guide.md` for:
- Why PMF degrades
- Detailed maintenance strategies
- Warning signs checklist
- Recovery playbook

---

## Case Studies

Learn from real-world PMF journeys:

**Superhuman: Systematic PMF Improvement**
- 22% → 58% in 18 months
- Data-driven PMF engine
- Methodical quarterly improvement

**Slack: Maintaining PMF Through Evolution**
- Strong initial PMF with tech startups
- Expanded while protecting core value
- Multiple segment expansion successful

**Quibi: Cautionary Tale of No PMF**
- $1.75B raised, complete failure
- Built 18 months without validation
- Ignored user feedback, iterated too slowly

**Figma: Remote Work Inflection Point**
- 5 years to PMF (patient technology building)
- COVID accelerated PMF dramatically
- Right product, right time

**Detailed case studies:** See `references/pmf-case-studies.md` for:
- Complete journey narratives
- Metrics and timelines
- Key lessons from each
- What worked and what didn't

---

## Lenny's Framework: Signals from 46 Product Leaders

Distilled from Lenny Rachitsky's research across 46 experienced product leaders. Use these principles to contextualize and triangulate the quantitative metrics above.

### Core Principles

**PMF is obvious when you have it**
> *"If there's doubt, you likely don't have it."* — Matt MacInnis

Authentic PMF feels unmistakable: the market actively pulls the product from you. If you need to persuade every customer, you don't have it yet.

**Retention is the ultimate single metric**
> *"Product market fit has one metric. Retention."* — Uri Levine

Retention curves that flatten — especially a "smile curve" where engagement rises over time — are the strongest structural signal. Before retention data matures, the Sean Ellis 40% test is the leading proxy.

**PMF is not static — it can be lost**
> *"You might fall out of product market fit in a year or five years if you're not continually making your product better."* — Casey Winters

Markets shift, competitors improve, technology evolves. Re-survey quarterly and watch for declining scores.

**Reference customers validate PMF**
> *"I want 6–8 references for B2B, 15–25 for B2C as an indication of PMF."* — Christian Idiodi

Customers willing to advocate publicly — not just use the product — demonstrate genuine conviction. Build a reference customer pipeline before claiming PMF.

**PMF exists in segments, not universally**
> *"Do we have the fit in specific segments?"* — Karri Saarinen

Find your strongest fit in one segment first, measure PMF score per segment, then expand. Aggregate scores can mask deep fit in a beachhead alongside no fit elsewhere.

**PMF requires distribution, not just retention**
> *"If you have a product that retains well and you can't find more users for it, I don't think that's product market fit."* — Casey Winters

Both retention and a scalable, repeatable acquisition channel are required before claiming PMF. Retention without growth is product-channel fit, not market fit.

**PMF is multi-stage, not binary** (Todd Jackson's 4 levels)

| Stage | Customer Count | Priority |
|-------|---------------|----------|
| Nascent | 3–5 | Satisfaction |
| Developing | 5–25 | Demand |
| Strong | 25–100 | Efficiency |
| Extreme | 100+ | Scale |

**Customer "pull" signals**
> *"We felt the questions change — 'How are you pricing this? When can we start a POV?' That's real intent."* — Raaz Herzberg

Watch for customers driving the next steps: asking about pricing, timelines, and integration. Polite interest ≠ pull.

**Outrage during outages signals PMF**
> *"If customers weren't furious during downtime, that signals no product market fit."* — Jeff Weinstein

Mission-critical products generate strong reactions when unavailable. If customers shrug at downtime, the product is nice-to-have.

### Diagnostic Questions (Lenny's Framework)

Use these in customer interviews and reference calls to triangulate PMF beyond the survey score:

1. "If you could no longer use this product, how would you work around it?" *(low effort = no PMF)*
2. "Have you recommended this to a colleague? Why / why not?"
3. "What would you lose if this went away tomorrow?"
4. "When did you last use it — and what triggered that session?"
5. "Is there a segment of your team or workflow where this is truly indispensable?"

### Common PMF Mistakes (from 46 leaders)

- **Confusing launch spikes with PMF** — Product Hunt traffic or press coverage doesn't mean PMF. Look for sustained organic growth post-launch.
- **Listening to "somewhat disappointed" users** — Focus on what makes the "very disappointed" segment love the product, not what would make lukewarm users slightly happier.
- **Scaling too early** — Paid growth before PMF burns cash and can damage brand reputation with the wrong-fit customers you acquire.
- **Conflating TAM with PMF** — A large market opportunity says nothing about whether you have fit within it.
- **Ignoring segment-level retention** — Aggregate retention can look acceptable while a small "champion" segment carries the whole cohort.

---

## PMF Best Practices

**DO:**
- Measure PMF systematically (40% rule)
- Survey quarterly to track progress
- Focus on champions (double down on "very disappointed")
- Protect core value as you scale
- Maintain customer proximity always
- Use retention curves as ultimate test
- Say no to wrong-fit customers
- Iterate rapidly before PMF
- Be patient (can take 6-24 months)

**DON'T:**
- Scale before achieving PMF (leaky bucket)
- Ignore retention for acquisition
- Build for everyone (niche down)
- Assume PMF is permanent (keep measuring)
- Stop talking to customers (ever)
- Add features constantly (bloat)
- Chase every deal (segment discipline)
- Rush the process (systematic > fast)

---

## Related Skills

- `user-research-techniques` - Interview methods, research synthesis (understanding users)
- `validation-frameworks` - Problem/solution validation and MVP testing
- `market-sizing-frameworks` - Market opportunity assessment


---

## Language and Tone

- Use expert terminology appropriate to the context (VC due diligence, financial analysis, competitive intelligence)
- Avoid superfluous prose, self-references, expert advice disclaimers, and apologies
- No em dashes or en dashes; use commas, parentheses, or rewrite the sentence instead
- Lead with data and specific findings; interpretation follows the evidence
- No superlatives (world-class, revolutionary, best-in-class, cutting-edge)

