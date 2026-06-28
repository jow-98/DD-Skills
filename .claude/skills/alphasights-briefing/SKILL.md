---
name: alphasights-briefing
description: "Draft the AlphaSights outreach email for a company in due diligence. Always outputs in English. Pulls company context, ICP, and WYHTBI theses from Affinity, Granola, Google Drive, and Superhuman to populate the expert profile list and discussion topics. Use when asked to write, draft, or prepare an AlphaSights briefing."
---

# AlphaSights Briefing Skill

Drafts the outreach email to AlphaSights (Elon) describing what expert profiles to source and what discussion topics to cover. **Always in English.**

See `references/email-templates.md` for a real example.

---

## Intake

Invoked as `/alphasights-briefing <Company Name>` or with a company name in args.

Confirm or ask the user for:
- **Company name**
- **Number of expert calls** (default: 4-5)
- **Deadline date**

---

## Step 1 — Pull internal DD context

Run all in parallel.

**VC Knowledge Hub:**
```
mcp__vc-knowledge-hub__search("<company name>")
mcp__vc-knowledge-hub__get_company("<company name>")
mcp__vc-knowledge-hub__ask("What is the ICP, value proposition, WYHTBI thesis, and key bets for <company name>? Include product description, target customer, named competitors, and traction metrics.")
mcp__vc-knowledge-hub__search_research_findings("<company name>")
mcp__vc-knowledge-hub__get_thesis_themes()
```

If no results, fall through to individual connectors.

**Affinity:**
```
mcp__Affinity__search_companies(name="<company name>")
mcp__Affinity__get_notes_for_entity(entity_id, entity_type="company")
mcp__Affinity__get_meetings_for_entity(entity_id, entity_type="company")
mcp__Affinity__query_notes(query="<company name> ICP product WYHTBI thesis expert call")
```

**Granola:**
```
mcp__Granola__list_meetings(search="<company name>")
mcp__Granola__get_meeting_transcript(meeting_id)   // for each relevant meeting
```

Extract: product description in founder's own words, ICP, competitor mentions.

**Superhuman:**
```
mcp__Superhuman__query_email_and_calendar(query="<company name> AlphaSights expert")
```

Extract: any prior AlphaSights briefs for this company (avoid duplicating expert profiles already used).

**Google Drive:**
```
mcp__Google_Drive__search_files(query="<company name> pitch deck")
mcp__Google_Drive__search_files(query="<company name> investment thesis")
mcp__Google_Drive__read_file_content(file_id)
```

Extract: product slides, ICP, pricing, named customers, named competitors.

---

## Step 2 — Synthesise

Before drafting, compile:
- **"In Short" block**: 5-6 sentences covering the problem, solution, value proposition, delivery model, traction signal, and named incumbents/competitors it displaces
- **Expert profile**: job titles + industries with named example companies + key pain points; add a second block for 1-2 technical experts if the product has meaningful technical depth to probe
- **Discussion topics**: convert each WYHTBI conviction statement into a direct question (5-7 total)
- **Named competitors**: include so AlphaSights can source experts familiar with them

---

## Step 3 — Compose the email

**EMAIL TEMPLATE:**

Subject: Expert Calls [Company Name] x 42CAP x AlphaSights

---

Hi Elon,

I hope you are doing well. We would be interested in talking with [Number] experts as part of our due diligence process on [Company Name]. The timeline to schedule all of the calls is until [Deadline Date].

Attached to this email is [Company Name]'s product deck. It should give you some context on what they do. Feel free to share it with the experts prior to the call so that they can prepare themselves.

Also, you can share our name, as well as the name of the company prior to the call. The expert should be aware that a VC is consulting them to validate [Company Name]'s value proposition as a potential customer.

In short, [5-6 sentence product and company description].

The following types of experts are of interest:
Overall, we would like to set up [Number] expert calls with potential customers / users of [Company Name]. We are looking for:
- Potential job titles of the expert are: [Title 1, Title 2, Title 3, ...]
- Industries: [Industry 1, Industry 2, ...]. For example, their current client list includes [named clients]. Please extend the research and look for similar companies such as [named comparable companies].
- Key priorities:
  - [Pain point 1]
  - [Pain point 2]
  - [Pain point 3]
- [Optional: "It would be beneficial if the expert is familiar with [Competitor A] or [Competitor B]."]

[Optional — add only if technical depth warrants it:]
In addition, we'd also like to run 1-2 calls with technical / domain experts — ideally people who:
- [Criterion 1]
- [Criterion 2]
- [Criterion 3]

Format of the call:
- 35 minute call between the experts and [Company Name] — 10-15 minute product demo and 15-25 minutes Q&A. This is followed by 25-30 minutes feedback with the experts and us.
- As there will be a product demo, the expert has to sit in front of a laptop and should not dial in via phone. Please communicate this very clearly prior to the call. We had some issues here in the past. If the expert indicates that he cannot make it in front of a laptop, we prefer to reschedule the call. This is a technical product and requires a visual demonstration.
- If the founding team has problems kicking-off the conversation, tell the experts that they should specifically ask for a product demo or some slides on the product.
- Expert calls should take place in the coming days, until [Deadline Date] the latest. I will ask the founder of [Company Name] for his/her availability and send you over the times asap.
- The expert does not have to take the lead for the first 10-15 minutes. I will talk to [Company Name] prior to the call and tell them explicitly that they should set the context and have a product demo prepared.
- After the product demo, the experts should take over and lead the conversation to provide us with answers to the questions below.

The following discussion topics are of interest to us:
- [Question 1 — derived from WYHTBI thesis 1]
- [Question 2 — derived from WYHTBI thesis 2]
- [Question 3 — derived from WYHTBI thesis 3]
- [Question 4]
- [Question 5]

Let me know if you want to jump on a call to discuss details or if you have any questions on the company and what they are doing in detail.

Best,
Johannes

---

## Quality Checklist

- [ ] Always English
- [ ] Subject: `Expert Calls [Company Name] x 42CAP x AlphaSights`
- [ ] Salutation: `Hi Elon,`
- [ ] Number of experts and deadline filled — no placeholders
- [ ] "In Short" block: 5-6 sentences, specific traction/market signals, named competitors
- [ ] Expert types: three subsections (job titles / industries with named examples / key priorities)
- [ ] Format block: verbatim from template, company name substituted throughout
- [ ] Discussion topics: 5-7 direct questions derived from WYHTBI theses — not generic
- [ ] Sign-off: `Best, Johannes`

---

## Language and Tone

- Always English
- No superlatives (world-class, revolutionary, cutting-edge)
- No em dashes or en dashes — use commas or rewrite
- Specific over generic: named companies, titles, and competitors throughout
