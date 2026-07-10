# NKI Redesign Project

This directory holds the "Other Land x NKI" UX redesign engagement for **www.nki.no**
(NKI Nettstudier — Norwegian online education company, B2C-heavy). Not a git repo.

## Contents
- `md-files/` — UX research: `nki-heuristic-findings.md` (all 10 Nielsen heuristics fail),
  `nki-session-recordings-analysis.md` (11 PostHog recordings), `nki_user_flow.md` (7-step
  checkout funnel), `project-overview.md` (business model/KPIs)
- `01 Logos/` — brand assets for 4 sub-brands (NKI, Privatist, Kurs, Co-brand)
- `02 Colors/` — Lime/Spring palette references (CMYK + RGB .ase swatches)
- `design-system/` — reference docs generated from the Figma NKI Design System file
  (`owPdaUpO1PIBtFgizm3P7x`): `nki-design-tokens-and-text-styles.md` (color/spacing/
  radius/type tokens) and `nki-design-system-components.md` (component inventory +
  node IDs). **Strict source of truth** for every color, spacing, radius, type, and
  component used in the prototype — apply these, don't improvise. See "Current work"
  below.
- PDFs: stakeholder interview script, user interview, first-week UX findings
- `Project Timeline.png`

## Current work: HTML prototype

Building a clickable prototype of the redesigned site to validate layout/flow before
dev handoff. Plan: `/Users/nkttlk/.claude/plans/i-want-to-create-cuddly-sketch.md`

**Workflow — go page by page.** The user describes each page and shares reference
screenshots/sites BEFORE building. Do not build ahead or scaffold unprompted.

**Scope:** 4 pages — Homepage, Search/listing, Course detail, Checkout flow.

**Fidelity — history & current mandate:** the prototype was first built as a pure
**greyscale lo-fi wireframe** (no color, placeholder styling) to validate layout and
flow. Since then, the NKI Design System — color tokens, spacing/radius/type tokens, and
component specs — has been built in Figma and captured in two reference docs under
`design-system/`. **The current job is to bring the prototype up to full design-system
fidelity by strictly applying the tokens and components those two docs define.** No build
step: plain HTML + one shared `wireframe.css`, no framework.

**The two docs are the strict source of truth — apply them, don't improvise:**
- `design-system/nki-design-tokens-and-text-styles.md` — every color, spacing, radius,
  and type value. Use ONLY tokens defined here (Primitives / Semantic / Component /
  Layout collections + the type scale). Do not invent hex values, ad-hoc spacing, or
  off-scale font sizes.
- `design-system/nki-design-system-components.md` — the component inventory (26 kebab-case
  component sets: `button`, `header`, `accordion`, `checkbox`, `dropdown`,
  `payment-selection`, `checkout-steps`, `file-upload`, etc. — with variant axes and
  Figma node IDs). Build the prototype's UI out of these components and their documented
  variants; match their structure, states, and naming rather than one-off markup.

**How the docs map into the prototype:**
- Every color/spacing/radius/type in `wireframe.css` and `index.html` must reference the
  CSS custom properties under the "NKI DESIGN SYSTEM — TOKENS" block of
  `prototype/wireframe.css` (e.g. `--bg-subtle`, `--text-primary`,
  `--action-primary-default`, `--radius-12`, `--spacing-24`, `--font-size-20`) — these
  mirror the token doc 1:1. Do NOT use the legacy greyscale variables (`--gray-*`,
  `--r-sm`/`--r-md`/`--r-lg`, `--sh-*`) further up the file. Their presence in a rule is
  a signal that that section is **still on the old greyscale wireframe and needs to be
  migrated to the design system** — not the intended style.
- When a doc leaves a token/component ambiguous, or you need a node the docs don't cover,
  pull it live from Figma via the MCP tools (`get_design_context` / `get_metadata` /
  `get_screenshot`) against file key `owPdaUpO1PIBtFgizm3P7x`, then translate the returned
  Tailwind/React into the project's plain HTML/CSS + token conventions — never introduce
  Tailwind or a build step, and fold any newly-pulled token into the token block rather
  than hardcoding it.

**Logo source:**
`01 Logos/01 NKI/01 Digital/01 Large/01 Primary Logos/SVG/NKI_Logo_Black_L.svg`

**Research → fix mapping** (fold in only where it doesn't conflict with user's direction):
- Homepage — expose sub-nav/categories in header, shorten path-to-course
- Search — sticky filters + scannable grid for comparison
- Course detail — key info above the fold, real ratings, non-clickable-looking social proof
- Checkout — mid-flow login, complete stepper w/ docs step, early disclosure of
  postal/document requirements, live installment price breakdown, trust badge near CTA,
  unbundled consent checkboxes

---

# Project Overview — NKI × Other Land
**Kickoff Date:** June 08, 2025
**Session:** Discovery kickoff (3 parts)

---

## 1. About NKI

**NKI** (pronounced "NKI") is a 50-year-old Norwegian digital education company. Originally a classroom-based school, it has transitioned through mail-based, hybrid, and now fully digital course delivery.

- ~22 employees (recently restructured from 60 after a merger)
- Strong brand recognition in Norway, especially among older generations
- 200+ courses across a wide range of subjects (math, history, medical secretary, accounting, AI at work, driving-related, etc.)
- Courses are built by 5–6 in-house learning designers, sometimes co-developed with external subject-matter experts
- No product manager on the team; product design is being introduced through this engagement

### Key Value Proposition
NKI helps people who "don't fit the triangle" — those who dropped out, need to reskill, or want a formal diploma — complete their education through adaptive, multi-modal digital learning (video, audio, quizzes, games, text). The goal is to make learning outcomes consistent while the path to them is flexible.

---

## 2. Business Model

### B2C (current primary — ~95% of revenue)
- Users purchase individual courses or bundled packages
- Self-serve checkout with optional customer support intervention
- Course prices range from ~€500 to ~€5,000
- Conversion rate: ~1.4% overall; checkout funnel conversion below 1%
- Heavily seasonal: peaks in August, November, and January (tied to Norwegian exam/application cycles)

### B2B / B2G (growing focus — currently ~5% of revenue)
- Goal: reach 20% B2B within 3–5 years
- Sales via personal connections, cold outreach, and existing B2C-to-B2B upsells
- Model: companies purchase seats for specific courses (e.g., 20 seats for medical secretaries)
- Recurring contracts, invoiced quarterly
- Two active B2B pilots underway (including medical secretary course)
- B2B admin portal: company admin distributes licenses top-down

### North Star Metric
**Annual Recurring Revenue (ARR)** — moving from one-time purchases toward subscription-based B2B contracts.

---

## 3. Product Architecture

### Current Stack (being replaced)
- Old website: CMS built on top of Salesforce — described as technically outdated and a poor UX
- LMS: **Canvas** — a third-party platform, limited customizability, no AI integration
- Analytics: **PostHog** (checkout funnel tracking started May 28); also some marketing analytics via GA4
- Payment providers: 5 separate providers (Nets for card, Vipps for Norwegian bank payments, invoice, installments, third-party pay link)
- Authentication: Microsoft accounts with student numbers (being replaced)

### New Stack (in development)
- **React + Next.js** (server-side rendering)
- Simplified auth: private email + one-time code
- PostHog retained for analytics
- Goal: feature parity with old site at launch, then iterate
- Design-to-dev handoff preference: HTML/CSS files or Figma MCP integration

### Frozen / Constrained Areas
- LMS (Canvas) — no changes possible during this project
- All payment options must be preserved at launch (no reduction in methods)
- Data residency: Norway → Scandinavia → Europe (strictly no data outside EU)

---

## 4. User Segments

### B2C Personas (key examples)
| Persona | Age | Context |
|---|---|---|
| High school student | 17–19 | Preparing for or retaking school exams |
| Young adult re-taker | 20–21 | Retaking missed subjects to qualify for university |
| Career changer / reskiller | 30s–40s | e.g., woman pursuing medical secretary qualification |
| Corporate employee | Varies | e.g., office workers taking AI-at-work or legal courses |

- Parents are often involved in the purchase decision for younger students
- Users are generally not highly tech-savvy but not elderly either
- Norwegian language only (content is in Norwegian; no plans to translate)
- Mix of mobile browsing (high funnel) and desktop conversions (checkout)

### B2B Personas
- HR managers / company admins buying seats for employees
- No established persona yet; segment is early-stage

---

## 5. User Journey (B2C — Current State)

1. **Awareness** — Paid search, organic search, social media (TikTok, Instagram, Meta), email newsletter (~4–5k subscribers)
2. **Landing** — Course product page, homepage, or blog article (varies by channel)
3. **Consideration** — Multi-visit, research-heavy; often 3–4 touchpoints before purchase
4. **Checkout** — Multi-step self-serve flow:
   - Personal details (collected early — used for outbound sales recovery)
   - Optional checkbox to be contacted (many click thinking it's required)
   - Payment method selection (card, Vipps, invoice, installments, government pay, pay-for-someone-else link)
   - Document upload (required for one specific package only)
   - Confirmation / access provisioning (~20 seconds due to backend processes)
5. **Activation** — Email with Microsoft account credentials → Canvas LMS → course modules
6. **Learning** — Self-paced; some courses include teacher grading (assignments); others are quiz-only
7. **Exam** — External Norwegian government exam (NKI prepares students, does not administer exams)
8. **Retention / Churn** — No formal retention program; extensions sold at discount; ~5–10% of revenue from course extensions

---

## 6. Known UX Friction Points

- Checkout drop-off is high at every step (funnel data from PostHog, collected since May 28)
- Users don't understand course requirements upfront
- Payment options are unclear until late in the funnel
- Progress bar doesn't include all steps (e.g., document upload missing)
- Post-purchase provisioning takes ~20 seconds with no clear feedback
- Login process (Microsoft accounts) is confusing — being replaced
- Mobile browsing common, but checkout optimized for desktop
- Current website navigation requires ~5 steps to find the right course from the homepage

---

## 7. Competitive Landscape

**Key competitors (B2C):**
- Akademiet
- Glasspaper
- BI Online
- Sonans (well-funded, acquiring competitors)
- NKS (bankrupt), Folk Universitetet (struggling)

**Key competitors (B2B):**
- Glasspaper, BI, sector-specific LMS vendors

**NKI's differentiators:**
- 50-year brand with strong trust and recognition
- Norwegian-language content (advantage vs. Coursera/Udemy)
- Multi-modal learning (video, audio, quizzes, interactive games)
- Co-developed courses with domain experts
- Formal diploma/grade recognition through government exam system

---

## 8. Analytics & Research Status

| Tool | Status | Data Available |
|---|---|---|
| PostHog (checkout funnel) | Live since May 28 | Funnel drop-off per step |
| PostHog (session recordings) | Off (cost) | Can be enabled at sampled rate |
| PostHog (heatmaps) | Off | Can be enabled |
| Google Analytics 4 | Active | Traffic, device breakdown, seasonality |
| Marketing analytics (campaigns) | Active | GA4 / Meta |
| LMS analytics (Canvas) | Exists but unused | Course start/finish rates |
| Customer satisfaction survey | Exists but unmanaged | Unknown accessibility |
| Customer support chat | Active | Exportable; Jonerik/William has data |

**Requested from NKI team:**
- PostHog access for Nikita & Liliya
- Enable session recordings (~100 sample)
- GA4 access
- Staging credentials + test flow steps
- Top-revenue course list (screenshots/links)
- Competitor landscape (Norwegian)
- Seasonality graph
- Competitor checkout flows to analyze: NKS, Akademiet, Glasspaper, BI
- Device breakdown graph from PostHog
- Stakeholder list + Slack intro to William
- Demo course access (via Artem)
- Corporate Figma account check
- Claude/NQE account access for Liliya (via CFO)

---

## 9. Technical Constraints for Design

- **Payment provisioning delay:** ~20 seconds post-checkout (payment processing + auth + LMS registration)
- **LMS (Canvas):** Cannot be modified; learning experience is out of scope
- **All payment methods** must be preserved in v1 (card, Vipps, invoice, installments, government pay, third-party pay link)
- **GDPR:** Data must reside in Norway/Scandinavia/Europe; no data outside EU without review
- **Norwegian text:** Longer than English — allow for text expansion in UI elements; different capitalization rules for titles
- **Mobile:** High traffic volume on mobile, but checkout conversions skew desktop — mobile-first design recommended

---

## 10. Design KPIs

| KPI | Direction |
|---|---|
| Checkout funnel conversion rate | Increase |
| Revenue per conversion (focus on higher-value courses) | Increase |
| Customer support contact rate | Decrease |
| Checkout drop-off per step | Decrease |

---

## 11. Design & Handoff Approach

1. **Discovery phase (now):** UX audit via heuristics, competitor analysis, PostHog funnel analysis, customer support ticket review, user interview recruitment
2. **Prototyping:** Quick checkout flow prototypes in Figma, validated against UX laws and heuristics before user testing
3. **User research:** Minimum 5 user interviews; recruit via William (customer support/sales) and email campaigns; English interviews are fine (especially younger users)
4. **Design system:** Build foundations in Figma (variables, components); use Figma MCP for AI-assisted code generation
5. **Handoff:** HTML/CSS output preferred by dev team; Figma as source of truth for design system

---

## 12. Key Stakeholders

| Name | Role | Contact Method |
|---|---|---|
| Jonerik | CDO (marketing, brand, strategy) | Email / Teams |
| Mykyta Chernenko | Head of Engineering | Slack |
| William | B2C Sales + Customer Support | Email (intro via Mykyta) |
| Artem | Full Stack Dev (demo course access) | Via Mykyta |
| Sebastian | Integrations Dev | — |
| Oleg | Platform Engineer | — |
| CEO | Final approvals (brand, major decisions) | Via Jonerik/Mykyta |

---

## 13. Open Questions & Decisions Pending

- Whether to redesign the checkout flow significantly from v1, or keep close to current for A/B comparison
- Refund policy: if user starts the course, refund is no longer allowed (being implemented in new platform)
- Survey on old website: on hold due to shared platform freeze (other companies moving out)
- Account sharing concern: low priority for now, higher concern as private email auth rolls out
- B2B pricing model: still being defined (seats + course, invoiced quarterly seems most likely)
- AI integration in LMS: long-term goal, blocked by Canvas rigidity; custom LMS replacement planned further out
