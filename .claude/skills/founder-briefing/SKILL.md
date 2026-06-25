---
name: founder-briefing
description: "Draft a founder briefing email explaining the AlphaSights expert call process. Can output in English or German — specify the language in the args (default: English). Pulls company context and founder name from Affinity and Granola. Use when asked to write, draft, or prepare a founder briefing for AlphaSights expert calls."
---

# Founder Briefing Skill

Draft a ready-to-send email to the founder(s) explaining the AlphaSights expert call process and what is expected of them. Can output in **English or German** — the user specifies which in the args or the conversation.

---

## Intake

Invoked as `/founder-briefing <Company Name>` or with a company name in args.

Confirm or ask the user for:
- **Company name**
- **Founder name(s)** — first name(s) for the salutation
- **Number of expert calls** (default: 3-4)
- **Deadline date** for completing all calls
- **Language**: English (default) or German — ask if not specified

---

## Step 1 — Pull internal context

Run in parallel.

**VC Knowledge Hub:**
```
mcp__vc-knowledge-hub__search("<company name>")
mcp__vc-knowledge-hub__get_company("<company name>")
```

Extract: company name (canonical form), product description, any known sales / product deck availability.

**Affinity:**
```
mcp__Affinity__search_companies(name="<company name>")
mcp__Affinity__get_notes_for_entity(entity_id, entity_type="company")
mcp__Affinity__search_persons(name="<founder name if known>")
mcp__Affinity__get_person_info(person_id)
```

Extract: founder first name(s), any context about the founder's communication style or prior interactions that would affect tone.

**Granola:**
```
mcp__Granola__list_meetings(search="<company name>")
```

Extract: confirm founder name(s) from meeting participants if not already known.

**Superhuman:**
```
mcp__Superhuman__query_email_and_calendar(query="<company name> founder")
```

Extract: any prior email exchanges with the founder that indicate whether a product / sales deck already exists.

---

## Step 2 — Compose the email

Use the appropriate template below based on the requested language.

Fill all bracketed placeholders before presenting the draft. Do not leave any placeholder unfilled.

---

### ENGLISH TEMPLATE

Subject: AlphaSights Expert Calls | [Company Name]

---

Hi [Founder First Name(s)],

I just reached out to AlphaSights to schedule [Number] expert calls with you. Bad news — it's a bit of work. Good news — there's a chance to win a client. We would like to get this done until [Deadline Date].

Do you already have a product / sales deck that I could share with the experts prior to the call with you? I wrote a briefing, but any additional material helps and contributes to your discussion.

The format of the calls is as follows:
- 35 minute call between the experts and [Company Name], consisting of a 10-15 minute product demo and 20-25 minutes Q&A. This is followed by 25 minutes of feedback with the experts and 42CAP.
- As explained, the basic idea is to validate your value proposition and simulate a customer call that matches your ICP. So please have a product demo prepared. The Zoom calls are scheduled for 1 hour, but we only need you for the first 30-35 minutes.
- Please take the lead for the first few minutes, set the context of what [Company Name] does, and then move into a product demo. The experts have received a short summary of what you do, but have not seen your platform yet.
- There should be enough time for 15 minutes of Q&A between you and the expert at the end of your 30-35 minutes. I will join after the first 30-35 minutes and collect the feedback.

As a next step, it would be great if you could share your availability until [Deadline Date] so I can schedule the expert calls directly. Or feel free to send me your Calendly / Vimcal link to coordinate calls directly.

Let me know if you have any questions!

Best,
Johannes

---

### GERMAN TEMPLATE

Subject: AlphaSights Expertengespräche | [Company Name]

---

Hi [Founder First Name(s)],

ich habe AlphaSights angeschrieben, um [Number] Expertengespräche mit euch zu vereinbaren. Die schlechte Nachricht: Es ist etwas Arbeit. Die gute Nachricht: Es besteht die Chance, einen neuen Kunden zu gewinnen. Wir würden das gerne bis [Deadline Date] abschließen.

Habt ihr bereits ein Produkt- oder Sales-Deck, das ich den Experten vorab teilen könnte? Ich habe ein Briefing verfasst, aber zusätzliches Material hilft und gibt eurer Diskussion mehr Substanz.

Das Format der Gespräche ist wie folgt:
- 35-minütiger Call zwischen den Experten und [Company Name], bestehend aus einer 10-15-minütigen Produktdemo und 20-25 Minuten Q&A. Daran schließen sich 25 Minuten Feedback mit den Experten und 42CAP an.
- Wie besprochen, geht es darum, eure Value Proposition zu validieren und einen Kundencall zu simulieren, der eurem ICP entspricht. Bitte bereitet daher eine Produktdemo vor. Die Zoom-Calls sind auf 1 Stunde angesetzt, wir brauchen euch aber nur die ersten 30-35 Minuten.
- Bitte übernehmt den Call in den ersten Minuten, setzt den Kontext zu [Company Name] und steigt dann direkt in die Produktdemo ein. Die Experten haben vorab eine kurze Zusammenfassung zu euch erhalten, aber eure Plattform noch nicht gesehen.
- Am Ende eurer Zeit sollte noch genügend Zeit für 15 Minuten Q&A zwischen euch und dem Experten bleiben. Ich stoße danach dazu und sammle das Feedback ein.

Als nächsten Schritt wäre es super, wenn ihr mir eure Verfügbarkeit bis [Deadline Date] mitteilen könntet, damit ich die Calls direkt einplanen kann. Oder schickt mir gerne euren Calendly- oder Vimcal-Link zur direkten Koordination.

Meldet euch gerne, falls ihr Fragen habt!

Gruß,
Johannes

---

## Choosing singular vs. plural (German only)

If there is one founder, use singular forms throughout ("du", "dein", "hast du"). If there are two or more founders, use plural forms ("ihr", "euer", "habt ihr") as shown in the template above. Adjust the salutation accordingly: "Hi [Name]," for one founder; "Hi [Name1] und [Name2]," for two.

---

## Quality Checklist

Before presenting the draft, verify:

- [ ] Language matches the user's request (English or German)
- [ ] Founder first name(s) filled in salutation
- [ ] Company name filled consistently throughout
- [ ] Number of expert calls filled (not left as a placeholder)
- [ ] Deadline date filled with the correct date
- [ ] German template: singular/plural forms consistent with the number of founders
- [ ] Format bullets are unchanged from the template — do not shorten or paraphrase
- [ ] Sign-off: "Best, Johannes" (English) or "Gruß, Johannes" (German)
- [ ] Subject line filled

---

## Language and Tone

- Match the warmth and directness of the examples: conversational but professional
- "Bad news / Good news" framing is intentional — keep it
- No superlatives, no filler phrases
- No em dashes or en dashes — use commas or rewrite
- German: informal "du/ihr" throughout — never "Sie"
