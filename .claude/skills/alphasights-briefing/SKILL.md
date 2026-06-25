---
name: alphasights-briefing
description: "Draft AlphaSights expert-call briefing emails for companies in due diligence. Handles two email types: (1) the AlphaSights outreach email (always English) and (2) the founder briefing email (English or German). Pulls company context, ICP, and WYHTBI theses from Affinity, Granola, Google Drive, and Superhuman. Use when asked to write, draft, or prepare an AlphaSights briefing or a founder briefing for expert calls."
---

# AlphaSights Briefing Skill

Drafts two complementary emails for AlphaSights expert calls in DD:

1. **AlphaSights email** — outreach to AlphaSights (Elon) describing what experts to find and what to discuss. Always in English.
2. **Founder email** — briefing to the founder(s) on call format and what to prepare. English (default) or German.

See `references/email-templates.md` for canonical real-world examples.

---

## Intake

Invoked as `/alphasights-briefing <Company Name>` or with a company name in args.

Ask the user which email(s) they need if not stated. Confirm or ask for:
- **Company name**
- **Which email**: AlphaSights, founder, or both
- **Number of expert calls** (default: 3-4; Johannes typically uses 4-5 for complex DD)
- **Deadline date** for completing all calls
- **Founder name(s)** — needed for the founder email
- **Language for founder email**: English (default) or German

---

## Step 1 — Pull internal DD context

Run all in parallel before drafting.

**VC Knowledge Hub:**
```
mcp__vc-knowledge-hub__search("<company name>")
mcp__vc-knowledge-hub__get_company("<company name>")
mcp__vc-knowledge-hub__ask("What is the ICP, value proposition, WYHTBI thesis, and key bets for <company name>? Include product description, target customer, named competitors, and any traction metrics.")
mcp__vc-knowledge-hub__search_research_findings("<company name>")
mcp__vc-knowledge-hub__get_thesis_themes()
```

If the VC Knowledge Hub returns no results, fall through to individual connectors.

**Affinity:**
```
mcp__Affinity__search_companies(name="<company name>")
mcp__Affinity__get_notes_for_entity(entity_id, entity_type="company")
mcp__Affinity__get_meetings_for_entity(entity_id, entity_type="company")
mcp__Affinity__query_notes(query="<company name> ICP product WYHTBI thesis expert call")
```

Extract: responsible 42CAP partners (for the founder email intro), ICP, product description, WYHTBI theses.

**Granola:**
```
mcp__Granola__list_meetings(search="<company name>")
mcp__Granola__get_meeting_transcript(meeting_id)   // for each relevant meeting
```

Extract: founder names, product description in founder's own words, any ICP or competitor mentions.

**Superhuman:**
```
mcp__Superhuman__query_email_and_calendar(query="<company name>")
mcp__Superhuman__query_email_and_calendar(query="<company name> AlphaSights expert")
```

Extract: any prior AlphaSights briefs for this company (avoid duplicating expert profiles already used), responsible partners mentioned in prior emails.

**Google Drive:**
```
mcp__Google_Drive__search_files(query="<company name> pitch deck")
mcp__Google_Drive__search_files(query="<company name> investment thesis")
mcp__Google_Drive__read_file_content(file_id)   // for each relevant file
```

Extract: product slides, ICP definition, pricing, named customers or target industries.

---

## Step 2 — Synthesise before drafting

Compile from Step 1:
- **Product description**: what it does, for whom, core outcome (5-6 sentences for "In Short" block)
- **ICP**: job titles, industries, named example companies, key pain points the experts should share
- **WYHTBI theses**: convert each conviction statement into a direct discussion question
- **Named competitors**: include in expert profile to help AlphaSights source the right people
- **Responsible 42CAP partners**: for the founder email intro line
- **Founder name(s)**: from Affinity or Granola

---

## Step 3A — AlphaSights Email

Always in English. Salutation is always `Hi Elon,`.

**Structure:**

1. **Opening**: number of experts + company name + deadline
2. **Deck note**: confirm the deck is attached and can be shared with experts (use "Feel free to share it with the experts prior to the call so that they can prepare themselves." — only add a "do not share" instruction if the founder explicitly requested it)
3. **Name disclosure**: confirm 42CAP name and company name can be shared; expert should know a VC is validating the value proposition as a potential customer
4. **"In Short" block**: 5-6 sentence product/company description. Cover: the problem, the solution, the core value proposition, delivery model, traction signal, and which legacy incumbents or categories it competes with
5. **Expert type block**: three subsections:
   - *Potential job titles*: specific titles, not generic ("decision-makers")
   - *Industries*: named industries + named example companies from the ICP; ask AlphaSights to extend to similar companies
   - *Key priorities*: 3-5 bulleted pain points the right expert should recognise
   - Add a note on familiarity with named competitors if relevant (e.g. "it would be beneficial if the expert is familiar with [Competitor A, Competitor B]")
   - If the product has a technical depth worth probing, add a second block for 1-2 technical/domain experts (separate from the customer calls)
6. **Format block**: copy verbatim from the template below — do not paraphrase
7. **Discussion topics**: 5-7 direct questions derived from WYHTBI theses and key bets; framed as questions, not topic headings
8. **Closing**: standard sign-off

**EMAIL TEMPLATE (AlphaSights):**

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
- [Optional: note on competitor familiarity — "It would be beneficial if the expert is familiar with [Competitor A] or [Competitor B]."]

[Optional second expert block for 1-2 technical experts if relevant:]
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

## Step 3B — Founder Email

English (default) or German. Salutation uses first names.

**Structure:**

1. **Intro**: identify Johannes as supporting [Partner1] and [Partner2]; mention AlphaSights outreach and number of calls
2. **Bad news / Good news**: keep this framing — it's intentional
3. **Deadline**: reference the deadline, ideally with context ("as [Partner] mentioned during the call")
4. **ICP / briefing note**: mention the briefing was written based on available materials; invite the founder to correct the ICP description or share a sales deck
5. **Format block**: copy from the template below — do not paraphrase
6. **Business impact tip**: always include the "small tip" paragraph
7. **Availability request**: Calendly or direct availability until deadline; mention founders can split calls or attend together
8. **Closing**: standard sign-off

**EMAIL TEMPLATE (Founder — English):**

Subject: [Company Name] Expert Calls with 42CAP and AlphaSights

---

Hi [Founder First Name(s)],

This is Johannes from 42CAP — I'm supporting [Partner1] and [Partner2] in the due diligence process. I just reached out to AlphaSights to organize [Number] expert calls with you. The bad news: it's a bit of extra work. The good news: there's a real opportunity to win a client. [As [Partner] mentioned during the call, ] we're aiming to wrap this up by [Deadline Date].

I wrote a briefing on your ideal customer profile, based on what you sent us. But if you have any additional information on your ICP, or you realized during the calls that we should change something about the description, please let me know. Also, it would be great if you have a sales deck or any material to share with the experts ahead of the calls.

Importantly, the format of the calls is as follows:
- 35 minute call between the experts and [Company Name], consisting of a 10-15 minute product pitch and 20-25 minutes Q&A. This is followed by 25 minutes of feedback with the experts and 42CAP.
- Please make sure you're ready with a clear, visual way to pitch your product — please prepare a live demo, and optionally benchmarks, slides, etc.
- The objective of these calls is to simulate a real customer interaction with someone from your ICP — and to validate your positioning and how your product is perceived in a decision-making context.
- A small tip from previous calls: it's often not just about technical performance — customers want to understand the business impact: what will they save, streamline, or gain? So if you can bring that angle in, it usually resonates very well.
- The Zoom calls are scheduled for 1 hour, but we only need you for the first 30-35 minutes. Please take the lead for the first few minutes, set the context of what [Company Name] does and then move into a product pitch. The experts received a short summary of what you do, but haven't seen your platform yet.
- There should be enough time for 15 minutes of Q&A between you and the expert at the end of your 30-35 minutes. I will join after the first 30-35 minutes and collect the feedback.

As a next step, it would be great if you could share your availability until [Deadline Date] so I can schedule the expert calls directly. Or do you have a Calendly link I can use to coordinate meetings? You can attend the calls together, or split up — however you prefer!

Let me know if you have any questions!

Best,
Johannes

---

**EMAIL TEMPLATE (Founder — German):**

Subject: [Company Name] Expert Calls with 42CAP and AlphaSights

---

Hi [Founder Vorname(n)],

ich bin Johannes von 42CAP — ich unterstütze [Partner1] und [Partner2] im Due-Diligence-Prozess. Ich habe gerade AlphaSights kontaktiert, um [Anzahl] Expertengespräche mit euch zu organisieren. Die schlechte Nachricht: Es bedeutet etwas Mehraufwand. Die gute Nachricht: Es besteht eine echte Chance, einen Kunden zu gewinnen. [Wie [Partner] im Call erwähnt hat, ] möchten wir das bis [Deadline] abschließen.

Ich habe ein Briefing zu eurem idealen Kundenprofil erstellt, basierend auf dem, was ihr uns geschickt habt. Falls ihr noch zusätzliche Informationen zu eurem ICP habt oder ihr gemerkt habt, dass wir etwas an der Beschreibung ändern sollten, gebt mir gerne Bescheid. Außerdem wäre es super, wenn ihr ein Sales-Deck oder weiteres Material habt, das wir den Experten vorab schicken könnten.

Das Format der Gespräche ist wie folgt:
- 35-minütiger Call zwischen den Experten und [Company Name], bestehend aus einem 10-15-minütigen Produkt-Pitch und 20-25 Minuten Q&A. Daran schließen sich 25 Minuten Feedback mit den Experten und 42CAP an.
- Bitte bereitet euch auf eine klare, visuelle Präsentation eures Produkts vor — eine Live-Demo ist ideal, ergänzt durch Benchmarks oder Slides falls hilfreich.
- Das Ziel dieser Calls ist es, eine echte Kundeninteraktion mit jemandem aus eurem ICP zu simulieren — und eure Positionierung sowie die Wahrnehmung eures Produkts in einem Entscheidungskontext zu validieren.
- Ein kleiner Tipp aus vorherigen Calls: Es geht oft nicht nur um technische Performance — Kunden wollen vor allem den Business Impact verstehen: Was sparen, vereinfachen oder gewinnen sie durch den Wechsel zu [Company Name]? Wenn ihr diesen Winkel einbringt, kommt das erfahrungsgemäß sehr gut an.
- Die Zoom-Calls sind auf 1 Stunde angesetzt, wir brauchen euch aber nur die ersten 30-35 Minuten. Bitte übernehmt den Call in den ersten Minuten, setzt den Kontext zu [Company Name] und steigt dann direkt in den Produkt-Pitch ein. Die Experten haben vorab eine kurze Zusammenfassung erhalten, aber eure Plattform noch nicht gesehen.
- Am Ende eurer Zeit sollte noch genügend Zeit für 15 Minuten Q&A zwischen euch und dem Experten bleiben. Ich stoße danach dazu und sammle das Feedback ein.

Als nächsten Schritt wäre es super, wenn ihr mir eure Verfügbarkeit bis [Deadline] mitteilen könntet, damit ich die Calls direkt einplanen kann. Oder habt ihr einen Calendly-Link, den ich zur Koordination nutzen kann? Ihr könnt die Calls gemeinsam wahrnehmen oder aufteilen — wie es für euch am besten passt!

Meldet euch gerne, falls ihr Fragen habt!

Gruß,
Johannes

---

## Quality Checklist

**AlphaSights email:**
- [ ] Always English
- [ ] Subject: `Expert Calls [Company Name] x 42CAP x AlphaSights`
- [ ] Salutation: `Hi Elon,`
- [ ] Number of experts filled
- [ ] Deadline filled
- [ ] "In Short" block: 5-6 sentences, concrete, specific traction/market signals included
- [ ] Expert types: job titles + industries with named examples + key priorities (three subsections)
- [ ] Format block: verbatim from template, company name substituted throughout
- [ ] Discussion topics: 5-7 direct questions, derived from WYHTBI theses, not generic
- [ ] Sign-off: `Best, Johannes`

**Founder email:**
- [ ] Subject: `[Company Name] Expert Calls with 42CAP and AlphaSights`
- [ ] Salutation uses first names; multiple founders: `Hi [Name1], hi [Name2],` (lowercase second "hi")
- [ ] Intro identifies Johannes and names the responsible 42CAP partners
- [ ] Bad news / Good news framing present
- [ ] Deadline filled
- [ ] ICP / briefing note invites the founder to correct the profile or share a deck
- [ ] "Small tip" paragraph included
- [ ] Calendly option mentioned
- [ ] Sign-off: `Best, Johannes` (English) or `Gruß, Johannes` (German)
- [ ] German: informal `du/ihr` throughout, never `Sie`; singular/plural consistent with number of founders

---

## Language and Tone

- AlphaSights email: always English
- Founder email: English (default) or German; ask if not specified
- Professional but direct — not stiff
- No superlatives (world-class, revolutionary, cutting-edge)
- No em dashes or en dashes — use commas or rewrite
- Specific over generic: named companies, named job titles, named competitors at every opportunity
