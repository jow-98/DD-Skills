---
name: alphasights-briefing
description: "Draft an AlphaSights expert-call briefing email for a company in due diligence. Always outputs in English. Pulls company context, ICP, and WYHTBI theses from internal sources (Affinity, Granola, Google Drive, Superhuman) to populate the expert-type list and discussion topics. Use when asked to write, draft, or prepare an AlphaSights briefing."
---

# AlphaSights Briefing Skill

Draft a ready-to-send AlphaSights briefing email for a company in active DD. **Always output in English**, regardless of the language the user writes in.

---

## Intake

Invoked as `/alphasights-briefing <Company Name>` or with a company name in args.

Confirm or ask the user for:
- **Company name**
- **Number of experts** (default: 3-4 if not specified)
- **Deadline date** for scheduling all calls
- **Language for the briefing** — always English; ignore any other language request

---

## Step 1 — Pull internal DD context

Run all of these in parallel before drafting anything.

**VC Knowledge Hub:**
```
mcp__vc-knowledge-hub__search("<company name>")
mcp__vc-knowledge-hub__get_company("<company name>")
mcp__vc-knowledge-hub__ask("What is the ICP, value proposition, and WYHTBI thesis for <company name>? Summarise the product, target customer, and key bets.")
mcp__vc-knowledge-hub__search_research_findings("<company name>")
mcp__vc-knowledge-hub__get_thesis_themes()
```

Extract and cache:
- Product description (what it does, for whom, the core problem it solves)
- ICP (ideal customer profile: industry, company size, buyer persona)
- WYHTBI / investment thesis conviction statements — these become the discussion topics
- Key bets for the next 18 months — relevant for framing what experts should validate

If the VC Knowledge Hub returns no results, fall through to individual connectors below.

**Affinity:**
```
mcp__Affinity__search_companies(name="<company name>")
mcp__Affinity__get_notes_for_entity(entity_id, entity_type="company")
mcp__Affinity__get_meetings_for_entity(entity_id, entity_type="company")
mcp__Affinity__query_notes(query="<company name> ICP product value proposition WYHTBI thesis")
```

**Granola:**
```
mcp__Granola__list_meetings(search="<company name>")
mcp__Granola__get_meeting_transcript(meeting_id)   // for each relevant meeting
```

Extract: founder's description of their product, target customer quotes, any expert feedback already gathered.

**Superhuman:**
```
mcp__Superhuman__query_email_and_calendar(query="<company name>")
mcp__Superhuman__query_email_and_calendar(query="<company name> AlphaSights expert")
```

Extract: any prior AlphaSights briefs for this company (to avoid duplication), expert network discussion topics already used.

**Google Drive:**
```
mcp__Google_Drive__search_files(query="<company name> pitch deck")
mcp__Google_Drive__search_files(query="<company name> investment thesis")
mcp__Google_Drive__search_files(query="<company name> ICP customer")
mcp__Google_Drive__read_file_content(file_id)   // for each relevant file
```

Extract: product slides, ICP definition, any market or customer validation slides.

---

## Step 2 — Draft the company description paragraph

Write 5-6 sentences covering:
1. The problem the target customer faces
2. What the product does (the solution)
3. The core value proposition (outcome for the customer)
4. The delivery model (SaaS, API, embedded, etc.)
5. Current traction signal or stage (optional, if available from internal sources)
6. Why the expert call is relevant (validating demand / willingness-to-pay from the ICP)

Be concrete. No superlatives. No generic language.

---

## Step 3 — Define expert types

Based on the ICP, write a bulleted list of expert profiles 42CAP is looking for. These should be potential customers or users of the product, matching the ICP as closely as possible. Typical format:

- **[Buyer persona / title]** at **[company type / size / industry]** who **[have the problem the product solves]**
- Include 2-4 specific profile types
- If the ICP has multiple tiers (e.g. mid-market vs. enterprise), list them separately
- Specify relevant geography if important (e.g. DACH, EU, US)

---

## Step 4 — Define discussion topics

Derive discussion topics directly from the WYHTBI theses and key bets gathered in Step 1. Each topic should be a question the expert call is designed to answer. Format as a numbered list.

Good discussion topics:
- Validate whether the pain point is real and budgeted for
- Probe willingness-to-pay and how they currently solve the problem
- Test the value proposition (would they pay for this?)
- Explore competitive alternatives they already use
- Assess how the product fits into their workflow / buying process
- Identify any blockers to adoption

Frame topics as open-ended questions, not yes/no.

---

## Step 5 — Compose the email

Fill the template below with all gathered content. Do not leave any placeholder unfilled — if a value is unknown, ask the user before sending.

---

**EMAIL TEMPLATE:**

Subject: Expert Calls [Company Name] x 42CAP x AlphaSights

---

Hi everyone,

I hope you are doing well. We would be interested in talking with [Number of Experts] experts as part of our due diligence process on [Company Name]. The timeline to schedule all of the calls is until [Deadline Date].

Attached to this email is [Company Name]'s pitchdeck for the fundraising round. It should give you some context on what they do. Please do not share this deck with the experts. I will share a second product / sales deck with you by the end of the week that can be forwarded to potential experts prior to the call.

Also, you can share our name, as well as the name of the company prior to the call. The expert should be aware that a VC is consulting them to validate [Company Name]'s value proposition as a potential customer.

In short, [5-6 sentence company description from Step 2].

The following types of experts are of interest:

Overall, we would like to set up [Number of Experts] expert calls with potential customers / users of [Company Name]. We are looking for:

[Bulleted expert profile list from Step 3]

Format of the call:
- 35 minute call between the experts and [Company Name] — 10-15 minute product demo and 15-25 minutes Q&A. This is followed by 25-30 minutes of feedback with the experts and us.
- As there will be a product demo, the expert has to sit in front of a laptop and should not dial in via phone. Please communicate this very clearly prior to the call. We had some issues here in the past. If the expert indicates that he cannot make it in front of a laptop, we prefer to reschedule the call. This is a technical product and requires a visual demonstration.
- If the founding team has problems kicking-off the conversation, tell the experts that they should specifically ask for a product demo or some slides on the product.
- Expert calls should take place in the coming days, until [Deadline Date] the latest. I will ask the founder of [Company Name] for his/her availability and send you over the times asap.
- The expert does not have to take the lead for the first 10-15 minutes. I will talk to [Company Name] prior to the call and tell them explicitly that they should set the context and have a product demo prepared.
- After the product demo, the experts should take over and lead the conversation to provide us with answers to the questions below.

The following discussion topics are of interest to us:

[Numbered discussion topics from Step 4]

Let me know if you want to jump on a call to discuss details or if you have any questions on the company and what they are doing in detail.

Best,
Johannes

---

## Quality Checklist

Before presenting the draft, verify:

- [ ] Always in English
- [ ] Company name consistent throughout
- [ ] Number of experts filled (not left as a placeholder)
- [ ] Deadline date filled
- [ ] Company description is 5-6 sentences, concrete, no superlatives
- [ ] Expert types are specific to the ICP — not generic ("executives" or "industry professionals")
- [ ] Discussion topics are derived from WYHTBI theses — not generic questions
- [ ] Format section is unchanged from template (do not paraphrase or shorten it)
- [ ] Subject line format: "Expert Calls [Company Name] x 42CAP x AlphaSights" — no deviations
- [ ] Sign-off: "Best, Johannes"

---

## Language and Tone

- Always English, regardless of the language the user writes in
- Professional but direct — not formal or stiff
- No superlatives (world-class, revolutionary, cutting-edge, game-changing)
- No em dashes or en dashes — use commas or rewrite
- Specific over generic at every opportunity
