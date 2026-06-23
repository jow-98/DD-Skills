# 42CAP ESG Standard Language

This file provides boilerplate ESG text and customisation rules for the investment memo ESG section. Select and adapt the appropriate blocks based on the company's sector and business model.

---

## Standard ESG Intro Paragraph

Use this as the opening paragraph for the ESG section. Replace `[Company]` and `[sector]`.

> As part of our standard due diligence process, 42CAP evaluates ESG factors relevant to each investment. [Company] operates in the [sector] space, where [primary ESG theme] is the most material consideration. We have assessed the following factors and do not identify any ESG-related blockers to investment.

---

## ESG Materiality Table — Template

| ESG Factor | Materiality | Notes |
|------------|-------------|-------|
| Environmental | | |
| Social | | |
| Governance | | |
| Data Privacy & Security | | |
| AI Ethics & Bias | | |

Materiality levels: **Low** / **Medium** / **High**

---

## Pre-filled Tables by Sector

### AI / ML / LLM Companies

| ESG Factor | Materiality | Notes |
|------------|-------------|-------|
| Environmental | Low | Software-only business; compute footprint is a known cost item but not disproportionate |
| Social | Medium | Potential labour displacement in downstream industries; monitored at portfolio level |
| Governance | Medium | Founder-led; standard protective provisions in place; board to be constituted post-raise |
| Data Privacy & Security | High | Handles customer data and/or user inputs; GDPR compliance required; data processing agreements standard |
| AI Ethics & Bias | High | Core product involves AI outputs; model bias, transparency, and accountability are material; founder has articulated an approach to responsible AI |

**Customise**: If the product is used in hiring, healthcare, credit, or law enforcement, upgrade AI Ethics to **High** with a note on sector-specific regulatory risk (EU AI Act Annex III high-risk systems).

---

### B2B SaaS (non-AI)

| ESG Factor | Materiality | Notes |
|------------|-------------|-------|
| Environmental | Low | Cloud-native SaaS; minimal physical footprint |
| Social | Low–Medium | No direct social impact; standard employment practices |
| Governance | Medium | Early-stage governance; founders to add independent board members post-Series A |
| Data Privacy & Security | High | Handles customer data; SOC 2 / ISO 27001 roadmap standard at this stage |
| AI Ethics & Bias | Low | No AI in core product; flag if AI features are added post-investment |

---

### Healthcare / Biotech / MedTech

| ESG Factor | Materiality | Notes |
|------------|-------------|-------|
| Environmental | Low–Medium | Lab operations or device manufacturing if applicable; clinical trial waste managed under regulatory requirements |
| Social | High | Patient safety, access equity, and clinical data ethics are material; regulatory approvals (CE, FDA) provide external validation |
| Governance | High | Clinical governance required; IRB approval for trials; potential whistleblower and conflict-of-interest policies needed |
| Data Privacy & Security | High | Patient data under GDPR Article 9 (special category); additional safeguards required |
| AI Ethics & Bias | Medium–High | If AI used for clinical decision support; bias in training data (demographic underrepresentation) is a known risk |

---

### Climate Tech / Energy / Sustainability

| ESG Factor | Materiality | Notes |
|------------|-------------|-------|
| Environmental | High | Product is inherently environmental; assess actual impact vs. claims (greenwashing risk); carbon accounting and LCA preferred |
| Social | Medium | Supply chain sourcing (critical minerals, labour standards) if hardware; just transition considerations for energy companies |
| Governance | Medium | Regulatory exposure to energy/climate policy; assess jurisdiction risk |
| Data Privacy & Security | Low | Unless product involves consumer energy data |
| AI Ethics & Bias | Low | Unless AI drives automated decisions with social impact |

**Customise**: For carbon markets or ESG data companies, add a row on **Data Integrity / Greenwashing Risk**.

---

### Fintech / Marketplace / Payments

| ESG Factor | Materiality | Notes |
|------------|-------------|-------|
| Environmental | Low | Digital-only; minimal footprint |
| Social | High | Financial inclusion, consumer protection, predatory lending risk, and fraud are material; assess whether product widens or narrows access |
| Governance | High | Regulated entity or operating adjacent to regulation; AML/KYC obligations; licensing requirements by market |
| Data Privacy & Security | High | Financial data is high-sensitivity; PCI DSS, PSD2, or equivalent compliance required |
| AI Ethics & Bias | Medium | If credit scoring or fraud detection uses ML; bias in credit decisions is a regulatory risk (ECOA, EU AI Act) |

---

### Developer Tools / Infrastructure

| ESG Factor | Materiality | Notes |
|------------|-------------|-------|
| Environmental | Low | SaaS delivery; compute is a cost item managed at infrastructure level |
| Social | Low | B2B product; no direct consumer exposure |
| Governance | Low–Medium | Open source components require licence compliance; standard early-stage governance |
| Data Privacy & Security | Medium | Code or infrastructure may touch customer environments; supply chain security (SBOM) increasingly required |
| AI Ethics & Bias | Low | Flag if product incorporates AI-generated code suggestions (potential IP and bias risks) |

---

## ESG Risk Flags — When to Escalate

Raise an ESG concern in the memo narrative (not just the table) if any of the following are present:

1. **Dual-use technology** — product could be used for surveillance, weapons, or mass control
2. **High-risk AI systems** (EU AI Act Annex III) — biometric ID, employment screening, critical infrastructure
3. **Clinical AI without regulatory pathway** — FDA/CE for SaMD (Software as a Medical Device)
4. **Data sourcing ethics** — training data scraped without consent; GDPR-incompatible collection
5. **Supply chain exposure** — critical minerals, Xinjiang sourcing, or conflict-region operations
6. **Carbon-intensive operations** — hardware or manufacturing with significant Scope 1/2 emissions
7. **Governance red flags** — missing ESOP, no board, founder veto over all decisions with no checks

---

## ESG Closing Language (optional)

Use if the company has articulated a positive ESG angle or impact thesis:

> Beyond risk management, [Company]'s product may generate positive [environmental / social] externalities by [specific mechanism — e.g. "reducing manual review time in compliance workflows, freeing compliance teams to focus on higher-value risk assessment"]. 42CAP will monitor ESG performance at the portfolio company level via our annual ESG survey.

---

## Notes on 42CAP ESG Policy

- 42CAP does not invest in companies whose primary business involves weapons, tobacco, gambling, or fossil fuel extraction.
- AI investments are reviewed against the EU AI Act risk tiers. Annex III high-risk systems require enhanced due diligence and a clear regulatory roadmap.
- ESG assessments are updated at each major funding round and stored in Affinity under the company record.
