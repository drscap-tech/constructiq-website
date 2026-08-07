# ConstructIQ — Work Log

## 2026-08-07

### Buy Box ("Grow My Pipeline") — Opportunity Size custom-range fix (index.html)
Fixed a data-loss defect in the qualification modal's Step 5 (Opportunity Size). Selecting **"Custom range"** revealed the Min/Max inputs and captured them into `qualData.sizeMin`/`sizeMax`, but the submitted Formspree message and payload emitted only `qualData.size` = "Custom range" — so the actual dollar figures were **dropped** (surfaced by George Samarjian's intake, which returned `Opportunity Size: Custom range` with no value).
- **Labels** — the Min/Max fields now have persistent, clearly-labeled captions ("Minimum/Maximum project size ($)", required `*`), not just placeholders. Added `inputmode="numeric"` and guiding examples ("e.g. 5M or 5,000,000").
- **Required before proceeding** — `nextStep()` now blocks advancing past Step 5 when "Custom range" is selected until both a valid Min and Max are entered (and Max ≥ Min), with a clear alert.
- **Normalized display** — new `parseMoney()`/`fmtMoney()`/`sizeDisplay()` helpers convert the raw inputs into a normalized label like **`$5M–$35M`** (handles `5M`, `5,000,000`, `$5m`, `500000` → `$500K`, etc.). Used in the Step-8 review summary, the post-submit summary, and the "Schedule a consultation" prefill.
- **Captured in the submission** — the Formspree POST now includes structured fields `opportunity_size` (normalized), `opportunity_size_min`, and `opportunity_size_max`; the email message shows `Opportunity Size: $5M–$35M (min: …, max: …)`.
- **Bonus fix (captured-but-dropped, non-conditional):** `linkedin` was collected at Step 1 but omitted from the sent message/payload — now included. (`country` is still collected but not emitted; left as-is — not in the Odoo field map.)
- **Scope:** only the Opportunity Size custom-range path (+ the LinkedIn line) changed. No redesign of the intake, no other steps touched. The Step 2 single-select and Step 6 single-select multi-select requests remain open product items (not part of this fix).
- Verified end-to-end via a headless DOM harness driving the real page functions: validation blocks empty ranges; payload emits `$5M–$35M` + raw min/max + linkedin. `index.html` inline scripts syntax-check clean.
- **Status:** committed on branch `fix/buybox-custom-range` — **not yet pushed/deployed to constructiqpb.com** (awaiting go-ahead).

## 2026-07-29

### Founding Member funnel — founding.html + /apply
Reworked the Founding 10 conversion flow. Underlying Stripe subscription unchanged (`buy.stripe.com/6oUfZibWVf7ebtt05caEE00`).
- **Un-gated checkout** — added a direct "Become a Founding Member" Stripe CTA in the hero, directly below the $598/mo price. The application form is now the secondary "still have questions / not ready" path; reaching Stripe no longer requires submitting the form.
- **/apply short link (new)** — `apply/index.html` (directory form, reliably served at extensionless `/apply`) redirects to `/founding.html`, preserving the `?to=Name` personalization. Includes `rel=canonical` + `noindex`.
- **Sticky mobile checkout bar**; scarcity line beside the CTA ("Limited to 10 Founding Members · closes Aug 31, 2026 or when all positions filled"); "Month-to-month. Cancel anytime." under the button.
- **Copy** — pricing standardized to "$598/month locked in for as long as your subscription remains active" (removed the stale "6 months" from the meta description). Deadline moved July 31 → **August 31, 2026, or until all 10 seats filled**.
- **Accessibility** — associated all form labels (`for`/`id`), added `tel`/`inputmode`/`autocomplete`, strengthened body-text weight.
- **Analytics** — GA event `founding_checkout_click` (tagged by CTA location) on every Stripe click; existing `founding_application_submitted` retained.
- **Follow-up fixes** — removed the duplicate lower CTA (kept the hero one); removed the post-submit `scrollTo(top)` so the "You're on the list" thank-you stays in view.
- Commits: `cd5e68c` (funnel + /apply), `e364277` (CTA + scroll fixes). Deployed to constructiqpb.com; verified live on desktop + mobile.
- **Known issue:** body copy is white on brand orange (#E05A1A ≈ 3.7:1) — headings and CTA buttons pass WCAG AA, small body text does not. Full fix needs a deeper orange or navy panels; design left unchanged per request.

## 2026-07-15

### Revenue Intelligence Brief™ (RIB) — template v2.0
Elevated the RIB from the v1 output into a Fortune 500 consulting-grade, self-contained HTML template. Refinement, not redesign — existing layout, branding, and content preserved.

- **New framable cover page** — full-bleed ConstructIQ orange (#F58220), white reversed serif title, ghosted "CIQ" monogram, Prepared-For block, navy confidentiality footer bar.
- **Prepared-For elevated** — three bordered cards with solid-orange label chips and navy serif values.
- **Stronger section hierarchy** — orange eyebrow + larger navy serif heading + orange underline rule on every section.
- **Confidence Framework (new)** — color-coded tags (Verified / High Confidence / Market Intelligence / Emerging) on project cards and signals, defined on the methodology page.
- **"What the Data Is Telling You"** — reframed to foreground interpretation over aggregation.
- **Stronger CTA** — replaced weak grey "Schedule a Consultation" with a navy block + orange "Schedule Your Complimentary Strategy Session" button.
- **New Research Methodology page** — Intelligence Sources checklist (11 sources), methodology flow, confidence legend.
- **Premium polish** — more white space, hairline rules, refined type, print-tuned US Letter pagination.
- **Logo embedded** — ConstructIQ badge added to cover and every interior letterhead, inlined as a data URI (self-contained).
- Placeholders retained for dynamic client data; suitable for automated per-client generation.
- Files: `ConstructIQ_RIB_Template.html` (master), `ConstructIQ_RIB_Greenwich_Preview_v2.pdf` (proof).
- **Open:** optional matching Word .docx version; optional white/knockout logo for the orange cover.

### Website — constructiqpb.com (repo: drscap-tech/constructiq-website)
- **Diagnosed the "Claude Artifact" error.** The deployed `index.html` is the Claude artifact *viewer wrapper* (11 KB shell referencing claudeusercontent.com), not the real site.
- Confirmed via git: all three commits ("Initial site", logo swap, "Trigger redeploy") contain only the wrapper — the real site source was never committed, and nothing is recoverable from history or reflog. `logo.png` is intact.
- Ruled out browser cache / "delete browsing data" — the wrong file is served server-side, so clearing cache does nothing.
- **Decision:** recover the exact original by reopening the original Claude Code chat and having it write the real `index.html` to the repo, commit, and push (do not paste browser source back in). Fallback: rebuild from logo + Dropbox brand materials.
- **Open:** replace `index.html`; then enable **Enforce HTTPS** in GitHub Pages settings (currently HTTP).

### Daily Command Center — live dashboard (Cowork artifact)
- Built a self-contained live dashboard pulling from connected services, refreshing on open.
- Panels: **Today & Upcoming** (Google Calendar, buffers filtered), **Calendly Bookings**, **Inbox** (Gmail), **Recent Files**.
- Removed **Google Drive** panel per request; files panel now Dropbox-only (scoped to "2026", last-modified sort — Dropbox API requires a query term).
- Artifact id: `daily-command-center`.

### Scheduled task — morning briefing
- Created a daily **7:00 AM** run that gathers calendar, Calendly, unread email, and recent files, then drafts a **Gmail draft to drscap@gmail.com** ("Daily Briefing — [date]") and notifies in Cowork.
- Note: task drafts (cannot auto-send) email; run while the app is open. Recommend clicking **Run now** once to pre-approve connector permissions.

### Research — "ConstructIQ" conversations
- No email (Gmail) or Google Drive threads reference ConstructIQ.
- Found the active **Construct IQ** workspace in Dropbox (`/DRS Capital/ENTITIES/Construct IQ/`): strategy brief, RIB (SoFla + this template), prospect one-pager, internal launch sheet, workflow doc, brand assets, domain order.
