# ConstructIQ — Work Log

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
