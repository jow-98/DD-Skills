---
name: founder-briefing
description: "Draft the founder briefing email explaining the AlphaSights expert call process and what the founder needs to prepare. English (default) or German. Pulls founder name(s) and responsible 42CAP partners from Affinity and Granola. Use when asked to write, draft, or prepare a founder briefing for AlphaSights expert calls."
---

# Founder Briefing Skill

Drafts the email to the founder(s) explaining the AlphaSights expert call format and what is expected of them. **English (default) or German** — ask if not specified.

See `references/email-templates.md` for a real example.

---

## Intake

Invoked as `/founder-briefing <Company Name>` or with a company name in args.

Confirm or ask the user for:
- **Company name**
- **Founder name(s)** — first name(s) for the salutation
- **Number of expert calls** (default: 4-5)
- **Deadline date**
- **Language**: English (default) or German

---

## Step 1 — Pull internal context

Run in parallel.

**VC Knowledge Hub:**
```
mcp__vc-knowledge-hub__search("<company name>")
mcp__vc-knowledge-hub__get_company("<company name>")
```

**Affinity:**
```
mcp__Affinity__search_companies(name="<company name>")
mcp__Affinity__get_notes_for_entity(entity_id, entity_type="company")
mcp__Affinity__get_meetings_for_entity(entity_id, entity_type="company")
```

Extract: founder first name(s), responsible 42CAP partners (for the intro line), any context from prior calls that can personalise the email (e.g. "as Alex mentioned during the call").

**Granola:**
```
mcp__Granola__list_meetings(search="<company name>")
```

Extract: founder name(s) from meeting participants, partner(s) who attended founder calls.

**Superhuman:**
```
mcp__Superhuman__query_email_and_calendar(query="<company name> founder")
```

Extract: confirm founder name(s) and whether a product/sales deck has already been shared.

---

## Step 2 — Compose the email

Use the appropriate template below. Fill all placeholders before presenting the draft.

---

**EMAIL TEMPLATE (English):**

Subject: [Company Name] Expert Calls with 42CAP and AlphaSights

---

Hi [Founder First Name(s)],

This is Johannes from 42CAP — I'm supporting [Partner1] and [Partner2] in the due diligence process. I just reached out to AlphaSights to organize [Number] expert calls with you. The bad news: it's a bit of extra work. The good news: there's a real opportunity to win a client. [As [Partner] mentioned during the call, ]we're aiming to wrap this up by [Deadline Date].

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

**EMAIL TEMPLATE (German):**

Subject: [Company Name] Expert Calls with 42CAP and AlphaSights

---

Hi [Founder Vorname(n)],

ich bin Johannes von 42CAP — ich unterstütze [Partner1] und [Partner2] im Due-Diligence-Prozess. Ich habe gerade AlphaSights kontaktiert, um [Anzahl] Expertengespräche mit euch zu organisieren. Die schlechte Nachricht: Es bedeutet etwas Mehraufwand. Die gute Nachricht: Es besteht eine echte Chance, einen Kunden zu gewinnen. [Wie [Partner] im Call erwähnt hat, ]möchten wir das bis [Deadline] abschließen.

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

## Singular vs. plural (German)

One founder: use `du/dein/hast du` throughout. Salutation: `Hi [Name],`
Two or more founders: use `ihr/euer/habt ihr` as in the template above. Salutation: `Hi [Name1], hi [Name2],` (lowercase second "hi").

---

## Quality Checklist

- [ ] Subject: `[Company Name] Expert Calls with 42CAP and AlphaSights`
- [ ] Salutation uses first names; multiple founders: `Hi [Name1], hi [Name2],` (lowercase second "hi")
- [ ] Intro names the responsible 42CAP partners
- [ ] Bad news / Good news framing present
- [ ] Number of calls and deadline filled — no placeholders
- [ ] "Small tip" paragraph included
- [ ] Calendly option mentioned
- [ ] Sign-off: `Best, Johannes` (English) or `Gruß, Johannes` (German)
- [ ] German: informal `du/ihr` throughout, never `Sie`; singular/plural consistent with number of founders

---

## Language and Tone

- English (default) or German — ask if not specified
- Warm and direct — the "bad news / good news" framing is intentional, keep it
- No superlatives, no filler phrases
- No em dashes or en dashes — use commas or rewrite
- German: informal throughout, never formal "Sie"
