# Sprint 1 — Public Funnel Polish

**Branch:** `sprint-1-public-funnel`
**Status:** In Progress
**Spec Reference:** ConstructIQ v1.0 Functional Specification, Sections 5, 6, 14, 19

---

## Objective

Close all remaining gaps in the live public site before any backend infrastructure is built.
No server-side code, no database, no auth. Every change in this sprint is static HTML/CSS/JS.

---

## Scope

| # | Task | File(s) | Status |
|---|---|---|---|
| 1 | Fix contact form — Formspree integration | `index.html` | ✅ Done |
| 2 | Remove phone field from contact form | `index.html` | ✅ Done |
| 3 | Wire Google Analytics 4 | `index.html` | ✅ Done |
| 4 | Create Terms of Service stub | `terms.html` | ✅ Done |
| 5 | Create Privacy Policy stub | `privacy.html` | ✅ Done |
| 6 | Add Terms / Privacy links to footer | `index.html` | ✅ Done |
| 7 | Create Sample RIB™ page | `sample-rib.html` | ✅ Done |

---

## Implementation Notes

### 1 & 2 — Contact Form (Formspree)

**Problem:** Contact form had `onsubmit="return false"` — submissions were silently discarded.
Phone field was still present despite being removed from the qualification modal.

**Solution:** Formspree AJAX integration.
- Form posts to `https://formspree.io/f/FORMSPREE_FORM_ID` via fetch()
- On success: form is hidden, confirmation message shown
- On error: fallback alert with direct email address
- Phone field removed
- All inputs have `name` attributes for Formspree field labeling

**Formspree setup required (manual):**
1. Create account at https://formspree.io
2. Create a new form — name it "ConstructIQ Contact"
3. Set notification email to hello@constructiqpb.com
4. Copy the form ID (format: `xxxxxxxx`)
5. In `index.html`, replace `FORMSPREE_FORM_ID` with the actual ID

### 3 — Google Analytics 4

**Problem:** `trackEvent()` wrapper called `gtag()` which was undefined — all event calls silently failed.

**Solution:** GA4 global site tag added to `<head>`.
- Measurement ID placeholder: `G-XXXXXXXXXX`
- Must be replaced with actual ID before analytics data will appear

**GA4 setup required (manual):**
1. Go to https://analytics.google.com
2. Create a new property for constructiqpb.com
3. Copy the Measurement ID (format: `G-XXXXXXXXXX`)
4. In `index.html`, replace both instances of `G-XXXXXXXXXX` with the actual ID

**Events already tracked (will fire once GA4 ID is active):**
- `qual_started` — qualification modal opened
- `qual_step_N` — each step advance in the modal
- `qualification_completed` — qual form submitted
- `qual_purchase_rib` — RIB purchase button clicked
- `hero-primary-cta`, `hero-secondary-cta` — hero button clicks
- `why-cta`, `rib-cta`, `purchase-rib`, `purchase-professional` — section CTAs

### 4 & 5 — Terms and Privacy Stubs

Placeholder pages with attorney-review notice. These satisfy the spec's requirement
for linked legal pages (Section 5.1) and prevent broken links in the footer.

Per spec Section 19: "Terms, Privacy Policy, and report disclaimer require attorney
review before broad commercialization." These stubs must be replaced with reviewed
content before the platform is marketed at scale.

### 6 — Footer Links

Terms and Privacy links added to the footer disclaimer area.

### 7 — Sample RIB™ Page

Spec Section 5.1: "Provide a controlled sample without exposing confidential client data."

- Standalone page at `/sample-rib.html`
- Clearly labeled SAMPLE throughout with fictional illustrative data
- Fictional client: Meridian Building Group (no real company)
- Covers required RIB sections: Executive Summary, Key Metrics, Sample Opportunities
  with CIS™ scores, Decision-Maker Intelligence, Outreach Strategy, Methodology,
  Sources, Disclaimer
- CTA links to qualification modal on main site
- Matches ConstructIQ brand: Exo 2, color tokens, design system

---

## Pre-Sprint 2 Manual Actions Required

Before Sprint 2 begins, the following must be completed by the founder:

| Action | Where | Status |
|---|---|---|
| Create Formspree account + form | formspree.io | ⬜ Pending |
| Replace `FORMSPREE_FORM_ID` in index.html | index.html line ~571 | ⬜ Pending |
| Create GA4 property for constructiqpb.com | analytics.google.com | ⬜ Pending |
| Replace `G-XXXXXXXXXX` in index.html | index.html lines 5–6 | ⬜ Pending |
| Configure reports@ and support@ aliases in Google Workspace | Google Workspace Admin | ⬜ Pending |
| Create Vercel account | vercel.com | ⬜ Pending |
| Create Supabase project (US East region) | supabase.com | ⬜ Pending |
| Create Resend account + verify constructiqpb.com domain | resend.com | ⬜ Pending |
| Create OpenAI API billing account + spending limit | platform.openai.com | ⬜ Pending |
| Create Anthropic API billing account + spending limit | console.anthropic.com | ⬜ Pending |
| Choose workflow orchestration: Trigger.dev vs Inngest | — | ⬜ Pending |
| Confirm Square subscription setup for Growth + Professional | Square Dashboard | ⬜ Pending |

---

## Commit Log

| Commit | Description |
|---|---|
| `sprint-1/docs` | Add Sprint 1 implementation plan |
| `sprint-1/contact-form` | Fix contact form: Formspree integration, remove phone field |
| `sprint-1/analytics` | Wire Google Analytics 4 (placeholder ID) |
| `sprint-1/legal-pages` | Add Terms and Privacy stub pages |
| `sprint-1/footer-links` | Add Terms/Privacy links to footer |
| `sprint-1/sample-rib` | Create Sample RIB™ page |
