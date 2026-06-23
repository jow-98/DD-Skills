# Competitive Analysis Excel — Reference Guide

Detailed guidance for producing a rigorous, sourced competitive analysis. Covers how to write each column, fill in frameworks, and use internal intelligence correctly.

---

## General Principles

1. **Internal sources first.** AlphaSight calls, Granola transcripts, and expert emails contain proprietary intelligence that generic web research cannot replicate. Every expert opinion must appear in the Expert View column and must have shaped the Key Difference column.
2. **No fabrication.** Unknown = blank. This is a DD document. Guessing fills the file with noise.
3. **Target startup is always row 3**, highlighted yellow. Key Difference always describes how a *competitor* differs from the *target startup* — not the other way around.
4. **Be specific.** "Focuses on enterprise" is not a Key Difference. "Targets Fortune 500 procurement teams with a 9-month sales cycle vs. [Target]'s 30-day mid-market PLG motion" is a Key Difference.

---

## Sheet 1: Competitor Overview

### Column-by-Column Guide

#### Company Name
Full legal or trading name. No abbreviations unless the brand is universally known (e.g. SAP, not "SAP SE").

#### URL
Homepage URL only. No tracking parameters. Format: `https://company.com`

#### Location
ISO-2 country code of headquarters. Examples: `DE`, `US`, `FR`, `GB`, `IL`. Do not write city names here.

#### Headcount
Number only. Source from Specter, LinkedIn, or company website. Do not write "~45" — write `45`. If a range is given (e.g. LinkedIn "11–50"), use the midpoint (`30`) and note it as estimated.

#### Founded
Four-digit year only. `2021` not "Founded in 2021".

#### Product Description
≤200 characters. One sentence: what the product does, for whom, and the key output or value.

**Good**: "API platform for LLM evaluation — lets AI teams run automated red-teaming before production deployment."
**Bad**: "AI-powered platform that leverages machine learning to help companies optimize their workflows."

Rules:
- No adjectives (powerful, robust, leading, best-in-class)
- No vague verbs (leverage, enable, empower)
- Must name the customer type and the outcome

#### Last Funding
Format: `€Xm\nRound Year` (two lines in the cell). Examples:
- `€12m\nSeed 2024`
- `$45m\nSeries B 2023`
- `Acquired\nby Cisco 2024`
- `Incumbent` (for large established players)
- `N/A` (bootstrapped or unknown)

Use local currency if known; USD if not.

#### Investors
≤60 characters. List the most recognisable investors, comma-separated. If too many, list the lead(s) only.

#### Key Difference
≤200 characters. Describes how **this competitor** differs from **the target startup** — always relative, never absolute.

**Framework for writing Key Difference:**
1. Pick the most important axis of differentiation (market segment, product approach, pricing model, distribution, moat)
2. State the competitor's position on that axis
3. State the implication for the target startup

**Good examples:**
- "Targets Fortune 500 compliance teams (ACV >€150k) via direct sales — opposite segment to [Target]'s mid-market PLG; limited overlap today but could expand down-market."
- "Open-source with hosted tier; strong developer adoption but no enterprise SLA or support — different buyer and motion."
- "Acquired by ServiceNow (2024); now bundled into enterprise platform — channel advantage but innovation velocity likely slowed post-acquisition."

**Bad examples:**
- "Also uses AI to solve similar problems"
- "Competitor with more funding"
- "Focuses on the enterprise segment"

#### Category
One of three values only:
- **Direct** — same ICP, same problem, similar approach
- **Adjacent** — overlapping ICP or problem, different core use case
- **Incumbent** — legacy player or large platform offering a feature that competes

Order in the sheet: Direct first, then Adjacent, then Incumbent.

#### Feature / Product Dimensions (5–8 columns)

**Choosing dimensions**: Pick dimensions that actually differentiate in this specific market. Avoid generic dimensions that every competitor will score identically.

Good dimension examples:
- "Self-hosted deployment" (binary — relevant if data residency matters)
- "Real-time monitoring" (binary — relevant if post-production use case is key)
- "EU AI Act compliance module" (binary — relevant for regulated industries)
- "Model agnostic" (binary — relevant for LLM tooling)
- "Sensor type" (descriptive — relevant for hardware)
- "Integration depth" (descriptive — native / API / webhook)

**Two dimension types:**
- **Binary**: use `x` (confirmed present), `(x)` (partial or in beta), or blank (absent or unknown). Centre-align.
- **Descriptive**: short text value. Left-align, wrap text.

It's fine to mix both types. Label the section header accordingly: "Features" (software capabilities), "Product" (hardware/deep tech attributes), or "Category" (market segment groupings).

**Filling dimensions:**
- Base on Specter intelligence, product pages, Granola transcripts, expert emails
- Blank means unknown — do not guess
- `(x)` = mentioned on roadmap, in beta, or confirmed but limited functionality

#### Expert View (conditional column)

Include this column only if at least one expert opinion was found in Step 1.

Format: `[Expert type, date if known]: 'summary or direct quote'`

Examples:
- `AlphaSight call, Apr 2025: 'Considered best-in-class for enterprise red-teaming but too expensive for growth-stage companies — deal sizes above €80k are rare outside F500.'`
- `Network expert (ex-VP Engineering, Tier-1 bank), Mar 2025: 'We evaluated this and chose [Target] — their onboarding was 10× faster.'`
- `Founder reference call, May 2025: 'They positioned against us but couldn't match our latency benchmarks.'`

Rules:
- Keep each entry to 1–2 sentences
- Attribute to source type, not necessarily name (AlphaSight call, expert email, reference call, advisor)
- If no expert opinion exists for a competitor, leave the cell blank — do not write "No expert data"
- Do not average or synthesise multiple expert opinions into one — keep them separate if they differ

---

## Sheet 2: Strategic Frameworks

### Porter's Five Forces — Scoring Guide

Score each force 1–5 where:
- **1** = Very Low threat/power (favourable for incumbents/target)
- **5** = Very High threat/power (unfavourable; structural challenge)

| Force | What to assess | Signals that raise the score |
|-------|---------------|------------------------------|
| **Threat of New Entrants** | How easy is it for new players to enter this market? | Low capital requirements, no regulatory moat, open-source alternatives, no network effects |
| **Bargaining Power of Suppliers** | How much leverage do key input providers have? | Single-source cloud dependency (one AI model provider), proprietary data held by a third party, scarce specialist talent |
| **Bargaining Power of Buyers** | How much leverage do customers have? | Concentrated buyer base, low switching costs, commoditised alternatives, annual contracts with easy exit |
| **Threat of Substitutes** | Can customers solve the problem a different way? | Manual processes, internal build options, adjacent tools that partially solve the problem, open-source DIY |
| **Competitive Rivalry** | How intense is competition among existing players? | Many funded competitors, low differentiation, price competition, high churn industry |

**Common mistakes:**
- Scoring all forces as 3 (uninformative "average" outputs) — force yourself to take a view
- Ignoring the Internal Source column — every score should be traceable to an expert call, analyst report, or Affinity note
- Writing implications in the abstract — tie each implication to a specific strategic action the startup should take

**Overall Attractiveness**: 1–2 sentences synthesising the five scores. Include a verdict:
- Average score < 2.5 = Highly attractive market structure
- Average score 2.5–3.5 = Moderate; specific forces need monitoring
- Average score > 3.5 = Structurally challenging; startup needs a specific moat to survive

---

### SWOT — Writing Guide

**Strengths and Weaknesses** = internal to the target startup (things they control)
**Opportunities and Threats** = external (market, competition, regulation, macro)

This is the most commonly filled incorrectly. Rules:

#### Strengths — what actually constitutes a structural strength

**Good Strengths (structural, hard to replicate):**
- "Proprietary adversarial prompt dataset (10k+ examples) built over 18 months — not replicable without similar research investment"
- "Founding team has published 12 papers on LLM safety — credibility accelerates enterprise sales cycles"
- "Deep integration with [Platform X] — embedded in customer CI/CD pipeline creates high switching cost"

**Bad Strengths (features, not structural advantages):**
- "Good product UI"
- "Fast onboarding"
- "Experienced team" (too vague)
- "First-mover advantage" (only valid with a clear moat mechanism)

#### Weaknesses — be honest

A memo with no weaknesses signals poor analysis. Common real weaknesses for early-stage startups:
- Single geographic focus (not yet tested in other markets)
- Founder-dependent sales (no repeatable motion yet)
- No enterprise security certification (SOC 2, ISO 27001)
- Limited integrations vs. incumbents
- Small team with key-person risk

#### Opportunities — specific and time-bound

**Good Opportunities:**
- "EU AI Act enforcement (Aug 2026) creates mandatory compliance requirement — buying trigger for their audit module"
- "OpenAI's GPT-5 release is increasing LLM adoption by mid-market companies — expanding the ICP beyond early adopters"

**Bad Opportunities:**
- "Growing AI market"
- "Increasing enterprise demand for AI"

#### Threats — honest and specific

**Good Threats:**
- "Anthropic or OpenAI could bundle evaluation tools into their API offerings — reduces standalone value"
- "Category crowding: 3 well-funded US competitors (>€20m raised each) entering European market in 2025"

**Bad Threats:**
- "Competition from large companies"
- "Market could change"

---

### Competitive Positioning Map — Scoring Guide

Two axes: **Price** (1=cheapest, 10=most expensive) and **Features** (1=most basic, 10=most comprehensive).

These are relative scores within the competitive set — not absolute. Calibrate:
- The cheapest player in the set gets a 1–2
- The most expensive gets an 8–10
- The target startup gets positioned relative to the set

The Positioning Note (≤100 chars) explains the quadrant logic:
- Low price, high features = "Aggressive land-and-expand; margin risk at scale"
- High price, high features = "Enterprise positioning; long sales cycle"
- Low price, low features = "Entry-level / freemium; PLG motion"
- High price, low features = "Niche / specialist; limited growth potential"

---

## Sheet 3: Marketing Intel

### Messaging Matrix — Column Guidance

Fill from competitor websites, marketing pages, and expert/Granola insights. Scrape the actual tagline — do not summarise.

| Dimension | Where to find it | What to write |
|-----------|-----------------|---------------|
| **Tagline / Headline** | Homepage H1 | Copy verbatim, in quotes |
| **Core Value Proposition** | Hero section or About page | One sentence, verbatim or paraphrase |
| **Primary Audience** | "For teams like..." or "Built for..." | Job title + company type |
| **Key Differentiator Claim** | Comparison page, "Why us" section, G2 reviews | What they claim sets them apart |
| **Tone / Voice** | Overall website tone | Technical / Casual / Enterprise / Founder-led |
| **Proof Points Used** | Customer logos, case study stats, awards | Named customers, specific metrics, certification logos |
| **Category Framing** | How do they define the market? | "AI evaluation platform" vs. "LLM testing suite" vs. "AI governance" |
| **Primary CTA** | Homepage button | "Book a demo" / "Start free trial" / "Get API key" |

**The Category Framing row is strategically important.** If all competitors use the same category label, the target startup should consider whether to adopt it (legitimacy) or coin a new one (differentiation). Note this in the Expert Intelligence column if an expert commented on it.

### Channel Coverage — Scoring Guide

Use four levels consistently:

| Level | Meaning | Fill colour |
|-------|---------|------------|
| **Strong** | Active, high-volume, clearly part of GTM strategy | Light green `C6EFCE` |
| **Present** | Exists but not a primary channel; inconsistent cadence | Yellow `FFEB9C` |
| **Weak** | Minimal presence; last updated >6 months ago | Orange `FFCC99` |
| **None** | No presence found | Light red `FFC7CE` |

**How to assess each channel:**

- **Blog / SEO**: Check if blog exists, when last published, whether posts rank (search `site:[domain] blog`)
- **Case Studies**: Count available on their website; note if gated
- **Whitepapers / Ebooks**: Search `site:[domain] whitepaper OR ebook OR guide filetype:pdf`
- **Webinars / Events**: Check their events page or Eventbrite/LinkedIn for upcoming sessions
- **Video**: YouTube channel subscriber count and upload frequency
- **G2 / Review Sites**: Number of reviews on G2, Capterra, or Trustpilot; average rating
- **Paid Ads**: Use Google `site:[domain]` + ad transparency tools; or note if they appear in paid results for key terms
- **Community / Forum**: Slack community, Discord, Reddit presence, Stack Overflow engagement

---

## Data Quality Standards

| Data point | Minimum standard |
|------------|-----------------|
| Company count | Named source URL — no estimates without flagging |
| Expert opinion | Attributable to source type and approximate date |
| Key Difference | Specific to this competitor vs. target; ≤200 chars |
| Porter's scores | Each score tied to a specific factor, not just a number |
| SWOT items | Structural, not surface-level — see guide above |
| Taglines | Verbatim from source (quoted) |
| Channel scores | Based on actual observation, not assumption |

---

## Red Flags in Competitive Analysis

Flag these explicitly to the user or in the Notes section:

1. **No Direct competitors found** — the market may be earlier than thought, or the ICP is too narrow. Check whether incumbents handle this need with internal tools.
2. **A competitor has 10× more funding** — assess whether they can outspend the target in GTM and R&D. Flag as a threat in SWOT.
3. **Expert calls negative on the target** — surface this honestly. Do not bury it. A good DD memo acknowledges what could go wrong.
4. **Competitor is backed by a strategic investor** (e.g. Salesforce Ventures, Google Ventures in same space) — indicates potential acquirer or existential threat if the strategic decides to build in-house.
5. **All competitors use the same tagline / category framing** — category is commoditising. Target startup needs a differentiated position or the market is being won on distribution, not product.
6. **No expert opinion available** — note this gap explicitly. Consider whether an Alphasights call should be scheduled before IC.
