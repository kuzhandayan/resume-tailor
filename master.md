# Master Inventory — Kuzhandayan K V
Everything true about my work. The resume draws ONLY from here.
Rule: if it isn't in this file, it does not go on a resume.

> Verification pass (2026-08-26): all Property Automate claims below were checked
> against the actual git history of the 8 PropGoto repos on disk
> (`/Users/sabbari/propgoto/*`), filtered to git identity
> `kv.kuzhandayan@propertyautomate.com` (+ noreply/local variants). Claims are
> marked **VERIFIED** (code/commit evidence found), **CORRECTED** (prior claim
> was wrong, fixed here), or `[VERIFY]` (still needs your input — usually a
> number or a "why" that isn't in git history).

---

## Property Automate — work areas

### Core multi-tenant API (`backend-server`)
- **What it is:** REST API covering leasing, CRM, finance, HR, maintenance, inventory across many tenant databases (database-per-tenant on PostgreSQL). Convention-based routing — folder name auto-becomes URL segment, no central route registry. Controllers are plain async functions on a single destructured object, never touching `req`/`res` directly.
- **My scope — VERIFIED:** 1,008 of 23,019 total commits (~4.4%), active continuously since Jun 2024. One of several engineers, not sole owner of the whole API. Heaviest personal footprint (by commit-touch count): **quotation** (178), **report-engine/queries** (134), **portfolio_management** (52), **lead_qualified** (51), **account** (50), **contract** (45), **unit** (35), **wallet reports** (33), **project_report** (32), **agreement** (26), **offer/AP contract** (~40 combined).
- **Scale (repo-wide, not just mine) — VERIFIED:** 538 model files, 287 route-module folders, 241 controller domains, 1,588 hand-rolled tenant-DB migrations.
- **CORRECTED — AI/ChromaDB semantic search:** exhaustively grepped this repo (chroma, openai, embedding, semantic, vector) — **no trace of it exists in `backend-server`**. Portfolio claim is either about a different, un-investigated service, or inaccurate. **Do not put this on a resume until located and confirmed** — currently a `[VERIFY — likely drop]`.
- **Resume-ready bullet (draft):** "Build and maintain core REST API modules — quotation, offer/contract lifecycle, reporting engine, lead qualification, and portfolio management — on a database-per-tenant PostgreSQL architecture with convention-based routing across 287 route modules."

### Pricing & escalation scheduler refactor — VERIFIED in full, the real story
- **What was wrong:** `Scheduler.js` was a single 3,747-line file mixing three unrelated concerns in one function-heavy blob: dispatch logic (which payment-period type applies), the actual period-specific math (monthly/yearly/quarterly/half-yearly/every-4-months/bi-monthly/weekly-biweekly), and milestone/non-recurring/variable-payment handling — all inline.
- **What I did:** Commit `c66bb265a` ("schedular revamp for yearly", 10,180 lines changed — the single largest diff in my history on this repo). Split it into a dispatcher (`schedulerFunctions/scheduleperiodcalc.js`) that switches on `lease_payment_period` and delegates to one focused calculator per period type (`periods/calc_monthly.js`, `calc_yearly.js`, `calc_quarterly.js`, `calc_halfyearly.js`, `calc_every4months.js`, `calc_bimonthly.js`, `calc_weekly_biweekly.js`), plus separate `scheduleMilestone.js`, `scheduleNonRecurring.js`, `scheduleVariable.js`.
- **How billing correctness was protected:** the entire original 3,747-line file was preserved verbatim as `Scheduler.archive.js` — frozen, never imported, kept purely as a rollback/reference copy. (This is the "original archived for rollback" claim — genuinely true, code-confirmed.)
- **Result — VERIFIED, corrects the exact number:** file went from 3,747 → 704 lines immediately after the split; sits at 815 lines today after later fix commits. (Portfolio's "3,700 → 822" is accurate to within normal post-refactor drift — close enough to state as-is, or use the more precise 3,747 → ~800.)
- **Known follow-on debt (worth knowing for interviews, not for the resume):** `offer/Scheduler.js` (651 lines) duplicates this logic for AP-side offers and has drifted out of sync with the main scheduler over time — a real, self-identified technical-debt point if asked "what would you do differently."
- **Resume-ready bullet:** "Refactored a 3,747-line monolithic billing scheduler into a ~800-line dispatcher delegating to seven period-specific calculators (monthly/yearly/quarterly/half-yearly/etc.) plus dedicated milestone, non-recurring, and variable-payment handlers — largest single diff in my history on the platform (10,180 lines) — with the original archived intact for rollback safety."

### RabbitMQ worker service (`pa-queue`)
- **What it is:** Python/FastAPI service, no public API — consumes RabbitMQ messages to run bulk work off the request cycle: chunked CSV/Excel imports validated into per-tenant Postgres, Excel/PDF report generation.
- **My scope — CORRECTED:** 246 of 914 commits (~27%) — a real, substantial contributor, but **not the sole architect and not a rewrite of a prior synchronous JS pipeline**. The service was RabbitMQ/Python from its very first commit (predates my involvement by ~1 month); top contributor is a teammate at ~34%. I joined during initial buildout as one of several core contributors.
- **CORRECTED — worker thread count:** actual code (`main.py`, one `threading.Thread` per queue, `prefetch_count=1`) shows **17 threads today**, not 15 — likely grew as queues were added; if you want a number, say "17" or "15+", not a fixed stale "15".
- **CORRECTED — the headline metric doesn't exist:** exhaustively searched all code, docs, tests, and commit messages for any benchmark, load test, or logged timing. **"5,000+ records in under 15 seconds" / "~400 records/sec" is not substantiated anywhere in this repo.** There is no test suite at all (repo docs explicitly say so). This metric **must come off the resume** unless you can point to where it was actually measured — otherwise it violates Rule 4 (never fabricate numbers). Safe replacement: describe the vectorized design without a number, or go measure it once and cite that.
- **VERIFIED — DataFrame-first design:** repo's own coding conventions doc mandates this explicitly; only the final write uses `bulk_insert_mappings`/`bulk_update_mappings` in 100-row chunks. This part of the claim is solid.
- **VERIFIED — hard problems, mine specifically:** commit `5788c9f` added `durable=True` to queue declarations so messages survive broker/consumer crashes (a real crash-resilience fix). Commit `855de62` added chunking to lead-import processing to handle large files. No dead-letter-queue exists (failures just re-raise and implicitly requeue) — don't claim DLQ/retry infrastructure.
- **Resume-ready bullet (corrected):** "Core contributor (~27% of commits) to a Python/FastAPI RabbitMQ worker service running 17 concurrent per-queue consumer threads for asynchronous bulk Excel/CSV import and report generation, using a DataFrame-first pipeline where all validation/cleaning/dedup is vectorized and only the final write is per-row/chunked; added queue durability (`durable=True`) to survive broker crashes and chunked processing to handle large lead-import files."

### Authentication & identity (`pa-auth`)
- **What it is:** Standalone Fastify microservice — signup, login, OTP, TOTP 2FA, per-tenant dynamic DB connections (one Knex connection per tenant, built lazily per-request, no pooling/warm-up at startup).
- **My scope — CORRECTED:** 284 of 2,872 commits (~10%), one of ~10 heavy contributors. My real, evidenced footprint is **RBAC/route/menu seed-data maintenance** (222 of my file-touches are in one seeder file, `db/seeders/repositories.js`) and **cross-product bug fixes** across coworkGoto/LeaseGoto/realtyGoto config.
- **TOTP 2FA — pair-implemented, not solo-authored:** commit is under a teammate (srilayaa-propgoto), but per user: they paired on the implementation — drove/contributed code during the build, teammate held the actual commit. Defensible resume phrasing: "collaborated on implementing TOTP-based 2FA" — never "built" or "designed" solo, and be ready to walk through the actual TOTP flow (library: `speakeasy`, `window:1`) in an interview since the git trail alone won't back up more than that.
- **Maintenance-lock-during-restore (vigneshcrayond's commit) — no pairing claimed by user for this one.** Do not put this on the resume; it's a separate feature from TOTP and nothing ties the user to it.
- **Fastify vs Express:** documented in the repo's own CLAUDE.md as a deliberate divergence from sibling services, but no rationale is recorded anywhere and it's not attributable to a decision the user made. `[VERIFY]` if the user personally made or influenced this call — otherwise leave the "why" out of any interview answer about it.
- **Resume-ready bullet (corrected, honest scope):** "Maintained the multi-tenant RBAC and route/module reference data (roles, menus, permissions) for a standalone Fastify identity microservice serving all tenants, collaborated on implementing TOTP-based two-factor authentication, and resolved cross-product configuration and login-flow bugs across the platform's product lines."

### Manager web app & job engine (`manager-app`)
- **What it is:** React 18 (now Vite) SPA property managers live in — leasing, portfolio, finance dashboards, HR, maintenance. i18next localization, 5 locales confirmed in code (en-US, en-UK, ar, fr, es).
- **My scope — VERIFIED + CORRECTED:** 1,032 of 34,687 commits (~3%), heaviest in `src/screens` (job execution type / pre-post checklists, AMC/insurance/depreciation UI, common-area-unit creation). Real, representative shipped features: unit-readiness pre/post checklists with attachments, job-type execution collision handling, AMC/insurance/depreciation screens, common-area-unit upsert flow.
- **CRA→Vite migration — pair-implemented, not solo-authored:** the migration commit is under a teammate (a.raslan), but per user: they paired on it, teammate held the commit; user's own tracked commit is a follow-up documentation note (Vite config known-issue writeup). Defensible resume phrasing: "collaborated on migrating the build tooling from CRA to Vite" — never "led" or "migrated" solo, and be ready to speak to the actual Vite config specifics (custom esbuild JSX-in-.js plugin, `@mui/styles` alias shimming, `nodePolyfills`) since that's the one piece directly evidenced as the user's own work.
- **CORRECTED — the job scheduler is not in this repo.** It lives entirely in `pa-cron` (confirmed by this repo's own docs). Don't describe the scheduler as part of manager-app work.
- **CORRECTED — "frontend became a pure display layer over one endpoint" is not evidenced.** The escalation/pricing feature branch has 30+ contributing branches; I'm not a top contributor to it in this repo — my only touch is one modal date-handling fix. The actual pricing-logic move (frontend→backend) is real and verified (see Core API section above), but I did not do the corresponding frontend simplification work myself — don't claim that half of the story.
- **VERIFIED — real bug fixes:** several ticketed fixes (table pagination resets, Arabic status-chip color bug, contacts search empty-state, property-wallet dropdown filtering) — legitimate scoped fixes, not large refactors.
- **Resume-ready bullet (corrected):** "Built manager-facing screens on the React 18 SPA (unit-readiness pre/post checklists with attachments, job-type execution collision handling, AMC/insurance/depreciation workflows, common-area-unit management) across a platform localized in 5 languages."

### Job scheduler (`pa-cron`) — separate repo, thin personal footprint
- **What it is:** 50 distinct cron jobs (billing/invoicing, lease lifecycle, maintenance SLA tracking, HR, CRM lead handling, third-party sync with Zoho/NetSuite, backups), scheduled per-(tenant, company) using each company's IANA timezone — genuinely code-confirmed, not just described.
- **My scope — CORRECTED, thin:** only 12 commits total across all my git identities in this repo, 9 of which are merge commits (merging other engineers' PRs, not my own code). **One real code fix**: a 1-line typo fix (`taxable`→`taxtable`) in unit-transfer tax computation. The 50 job files and the timezone-scheduling engine itself predate my involvement and were built by other team members. **Do not claim authorship of the scheduler or its timezone logic.**
- **VERIFIED — what I did build here:** the repo's `CLAUDE.md` architecture documentation (180 lines) — bootstrap flow, connection manager, job contract, notification fan-out — confirmed via `git blame` as entirely mine, plus matching `docs/` feature notes. This is real evidence for the "LLM memory-layer / agent-contract" claim (see below) — I wrote the actual onboarding doc for this service.
- **No retry logic exists** in this service (errors are logged, not retried/requeued) — don't claim retry/backoff design for pa-cron.

### LLM coding-agent memory-layer architecture — VERIFIED, stronger than the portfolio states
- **What it is:** Self-initiated architecture — per-repo `CLAUDE.md` binding agent-contract files plus per-feature `docs/<feature>/NOTES.md` folders, hard 200-line cap per memory file, task scoping rules (scope to git diff, don't re-read files already in context, stop-and-ask on repeated errors).
- **VERIFIED — direct evidence across repos:**
  - `pa-cron/CLAUDE.md` (180 lines) — authored entirely by me, `git blame`-confirmed.
  - `alertshub-backend` — authored the full `CLAUDE.md`, `docs/README.md`, `docs/common/encryption.md`, `docs/common/notification-channels.md` in a focused week (Aug 8–14, 2026), documenting a service I only had one feature commit in — i.e. I wrote the onboarding architecture doc for a system mostly built by others.
  - `backend-server` — commit `9acfbb869`, "claude memory layer for the customize quote" (2,183 lines) — direct evidence of building this system inside the core API repo too, plus `docs/quotation_pricing_refactor.md`, `docs/scheduler_payment_periods.md`, `docs/scheduler_period_calc.md`.
  - `manager-app` — authored `docs/asset-insurance-amc-depreciation/NOTES.md`, `docs/job-execution-type/NOTES.md` following the same convention.
- **Metrics:** still qualitative — "reduces redundant context re-derivation, more consistent output" is the repo docs' own framing, not a measured number. `[VERIFY]`: any before/after you can measure (agent session token count, PR review turnaround) would meaningfully strengthen this for AI-tooling JDs — otherwise keep it qualitative.
- **Resume-ready bullet (strengthened, still honest):** "Self-initiated and authored a memory-layer and token-governance architecture for LLM coding agents — binding per-repo agent-contract files, per-feature notes folders with a 200-line cap, and diff-scoped task rules — and personally wrote the onboarding documentation adopted across 4+ repositories on the platform (core API, cron scheduler, notification service, manager SPA)."

### AlertsHub — real work not on the portfolio at all
- **What it is:** Multi-channel notification microservice (email/SMS/push/WhatsApp) for the platform, dispatch driven by per-client alert rules, per-tenant BYO provider credentials (AES-256-CTR encrypted), full delivery audit log. Live in production across multiple regions (branches: `live`, `live_dubai`, `live_india`, `live_saudi`).
- **My scope — VERIFIED:** added a new push-notification endpoint (`/sendvisitorapproval`, ~239 lines) for visitor approval flows, and authored the service's full architecture documentation (`CLAUDE.md`, `docs/common/encryption.md`, `docs/common/notification-channels.md`, `docs/visitor-approval-push/NOTES.md`) in one focused week (Aug 8–14, 2026).
- **Honest framing:** this is a small, recent, scoped contribution (5 of 310 commits) — real and shippable, but not a large ongoing ownership area. Good as a minor bullet or interview talking point about ramping up on an unfamiliar service quickly, not as a headline achievement.

### PASM (Plan & Subscription Management) — minor, not resume material
- Internal admin system for plans/subscriptions/billing/RBAC on PropertyAutomate's own product line. Live production service (real Jenkins CI to prod), but my contribution is thin: one trivial PR-merge commit on the backend (Feb 2025), and 3 commits building a "Report Master" CRUD feature on the UI in a single day (Jun 19, 2026). Not enough evidenced ownership to put on a resume as a work area.

### Collections & reconciliation
- Portfolio describes this module (wallet → property → agreement → invoice → receipt tracing, 4 wallet types, multi-currency) but it was **not one of the repos investigated in this pass** — no direct code verification done yet.
- `[VERIFY]`: which repo does this live in (likely `backend-server`, under a wallet/collections controller)? Worth a targeted follow-up if this module matters for a specific JD.

### DevOps / CI-CD / AWS — CORRECTED, currently overstated
- **VERIFIED, real but modest:** one Dockerfile bump (Node 16→20 + Postgres client) in `backend-server`; one small Dockerfile tweak in `pa-queue`; one single-line Jenkins credential addition in `pa-queue`.
- **CORRECTED — "AWS (EC2, S3)" is not accurate as stated.** The one object-storage integration I built (`pa-queue`, `src/services/aws.py`, uses `boto3`) points at **Oracle Cloud Infrastructure's S3-compatible API** (`OCI_NAMESPACE`/`OCI_ACCESS_KEY` env vars, `oraclecloud.com` endpoints) — not real AWS. No EC2 usage found anywhere.
- **Recommendation:** either drop "AWS (EC2, S3)" from the skills row, or reword to something honest like "Object storage (S3-compatible APIs)" / "Cloud object storage (boto3)" — do not claim AWS specifically unless you've used real AWS somewhere not yet investigated.

---

## Next Wave ERP internship (Jun–Aug 2023)
- Role: Data Analytics Intern, Thiruvallur, India.
- Data analysis, visualization, and reporting using Python, Excel, and Tableau. Built dashboards surfacing key trends; prepared recurring reports for non-technical stakeholders.
- `[VERIFY]`: dataset size, number of dashboards, any specific business decision influenced — no way to verify this from code (no repo access), needs your recall.

---

## Personal products — DineFlow, envault, Finio
(Unchanged from portfolio — these are your own repos, not investigated in this pass since ownership isn't in question. Still flagged for real-usage metrics.)

### DineFlow — Multi-Tenant Restaurant SaaS
- Cloud-based multi-tenant restaurant POS/management platform, one codebase, unlimited isolated tenants. Full order lifecycle, automatic GST (CGST+SGST), live inventory, 5 staff roles, platform-wide admin panel.
- Tech: Next.js 16, React 19, TypeScript, Prisma 7, PostgreSQL, NextAuth, Tailwind CSS v4, Docker.
- Link: dine-flow-app.vercel.app.
- **Usage — per user:** currently demo-stage only. Built with multi-tenant/shared-database architecture with the intent to eventually run it for the user's own restaurant business, but no real restaurant/transaction volume yet. Honest resume phrasing: describe as a built/live demo product, not as having real production usage — do not imply live paying customers or transaction volume.

### envault — Zero-Knowledge Secrets CLI
- CLI syncing `.env` files across machines via a private GitHub repo as storage, zero-knowledge (GitHub token + master password, password never stored).
- AES-256-GCM, 15 commands, 8 dependencies, published live on npm.
- Links: npm (envault-sync), docs (kuzhandayan.github.io/envault-docs).
- **Usage — per user, npm stats screenshot confirms:** npm shows 173 downloads in a single week (2026-07-01 to 2026-07-07, a spike), with smaller weekly numbers since; user's own running total estimate is **~250 downloads as of now**. Caveat for resume wording: npm download counts include CI/bot traffic, not just distinct human users — safe resume phrasing is "~250 npm downloads" or "250+ installs," not "250 users," unless real distinct-user data exists (it doesn't).

### Finio — Personal Finance Mobile App
- Fully on-device Flutter app, no financial data leaves the phone. Auto-reads Indian bank/UPI SMS, categorizes transactions, budgets, Excel/PDF export. "Cristy AI v0.1" — own model layer for spending analysis.
- Tech: Flutter, Dart, sqflite, Provider, Google Drive API, Google Sign-In, WorkManager.
- **Cristy AI — VERIFIED by user, no longer [VERIFY]:** self-hosted, local LLM via Ollama running a Qwen model (~Qwen 2.5/3.5 class), with some custom adaptation/prompting layered on top of the base model for the finance-analysis use case — not a from-scratch trained model, not a hosted third-party API. Honest resume phrasing: "self-hosted local LLM (Ollama, Qwen) with custom prompting/adaptation for on-device financial analysis" — do not claim "custom-trained model" or "proprietary AI model," since the base model is Qwen and the customization is adaptation/prompting, not full fine-tuning from scratch (confirm with user if actual fine-tuning was done vs prompt/context engineering only — currently understood as the latter).
- **Usage — per user:** private, ~10–15 users (family and friends only). Honest resume phrasing: describe as built/functional and in personal use, not as having broad/public adoption.

---

## Hackathons & side builds
- **ShareGoto** — blockchain real estate tokenization (NFTs, fractional ownership). Office hackathon, Winner, Aug 2025. Next.js, Solidity, PostgreSQL, Web3, MetaMask, ERC-721.
- **HackGoto** — CONFIRMED by user as a separate, distinct hackathon win from ShareGoto (different event, different timing/month). Base resume's existing achievement text ("led a four-member team to build an enterprise solution during the company hackathon, secured first place") stays attributed to HackGoto specifically, not ShareGoto. Both are real, keep both as separate achievements.
- **Agent Goto** — AI agent using MCP + vector DB, Cityscape Global 2025 Finalist, Riyadh. Matches current resume's achievement bullet as-is.
- **Momentum** — Flask + Instagram/LinkedIn automation, Gemini API content generation. Not on base resume — swappable project candidate for Python/AI-flavored JDs.
- **Bus Tracking System** — React + Django real-time tracking, Leaflet.js. Not on base resume — swappable candidate; only place Django experience shows up.

---

## Skills — tiered
*(honesty pass pending — see open questions below; this tiering reflects the verification pass so far)*

### Shipped to production — verified by code
JavaScript, TypeScript, Python, SQL, React, Node.js, Express, PostgreSQL, FastAPI, REST APIs, RabbitMQ, JWT-adjacent auth work (RBAC/route data), TOTP-based 2FA (pair-implemented — can speak to `speakeasy`/TOTP flow, not solo-authored), Vite build tooling (pair-implemented + own config-issue documentation), Docker (modest — version bump + small tweaks), Git, GitHub, i18next, Material UI (MUI), Objection.js, self-hosted LLMs via Ollama (Qwen), boto3-style object storage against S3-compatible APIs.

### Used in side projects only
Dart, Flutter, Next.js, Prisma, Tailwind CSS, shadcn/ui, NextAuth, Solidity, Web3/MetaMask, ERC-721, Flask, Gemini API, Django, Leaflet.js, sqflite, Google Drive API.

### Corrected — not evidenced as your work at all, still don't claim
The pa-cron scheduler/timezone engine itself (teammate's work, no pairing claimed), the maintenance-lock-during-restore feature (teammate's work, no pairing claimed), real AWS EC2 (no evidence found anywhere — only OCI's S3-compatible API is evidenced), ChromaDB/OpenAI semantic search (not found in any investigated repo — locate or drop).

### Learning / read about — NEVER on resume, Gaps only
`[VERIFY in interview]`

---

## Achievements
- HackGoto 2025 Winner — led a four-member team to build an enterprise solution during the company hackathon, secured first place. Confirmed as a distinct event from ShareGoto.
- ShareGoto — Aug 2025 office hackathon win, blockchain real estate tokenization project. Currently only on the portfolio/projects list, not the resume's Achievements section — could be added as a third achievement or kept as a project depending on space (Rule 3 caps Achievements at exactly 2 items).
- Cityscape Global 2025 Finalist, Riyadh — Agent Goto, MCP + vector DB. Verified against portfolio.

## Certifications (not on resume — backup/Gaps only)
1. AI Masterclass — Freedom With AI — Nov 2023 — FWA-AI-2023-KKV.
2. iCUBE — SVCE — Dec 2022 — SVCE-iCUBE-2022-KKV.
3. Python (Basic) — HackerRank — May 2022 — 818287fd6578.
4. AI in Python — Great Learning — Dec 2021 — ZEBVIARB.
5. Intro to Cyber Security — Simplilearn — Aug 2021 — 2801553.
6. Responsive Web Design — freeCodeCamp — Feb 2024 — fcc301a0bfd-b6c7-45a5-bd54-e7ea15b0d4ca.

## Education
1. M.Tech, AI and Data Science — SRM Institute of Science and Technology, Kattankulathur, Chennai — May 2026 – Present. Working-professional postgraduate program alongside full-time work.
2. B.Tech, AI and Data Science — Sri Venkateswara College of Engineering, Chennai — Aug 2021 – Jun 2024.
3. Diploma in Mechanical Engineering — Thiagarajar Polytechnic College, Tamil Nadu — May 2018 – Apr 2021. Pre-degree credential, likely not resume-relevant.

---

## Gaps flagged during interview / verification pass

**Corrections needed on the CURRENT base resume (`base_stable_template.tex`) — flag before next tailor:**
1. The RabbitMQ bullet's "5,000+ records in under 15 seconds (~400 records/sec)" has no evidence anywhere in the repo — no benchmark, no test, no logged timing. This is a Rule 4 violation as it stands (unsourced number). Needs to be removed or replaced with a real measurement.
2. "15 worker threads" should be "17" if kept at all, or softened.
3. The RabbitMQ bullet's framing ("re-architected... from a synchronous JavaScript pipeline") is not supported — the service was Python/RabbitMQ from its first commit; you were an early core contributor (~27%), not the sole architect of a migration.
4. "AWS (EC2, S3)" in the tools row is not accurate — the real integration is Oracle Cloud's S3-compatible API via boto3. No EC2 evidence anywhere.
5. The portfolio's AI/ChromaDB semantic-search claim for the core API has no code evidence in `backend-server` — needs locating or dropping.

**Resolved via interview:**
- Cristy AI (Finio) = self-hosted Ollama running a Qwen model, with custom adaptation/prompting for financial analysis — not a from-scratch trained model, not a hosted API.
- TOTP/2FA and CRA→Vite migration: user paired on implementation, teammate held the commit — kept on resume as collaborative work, worded as "collaborated on" not "built"/"led" solo.

**Open questions still needing your input (not derivable from code):**
- Next Wave internship: dataset size, dashboard count, any decision influenced.
- DineFlow/envault/Finio: any real usage numbers beyond the demo/npm listing.
- HackGoto vs ShareGoto naming — same thing, different label?
- Skills honesty pass — final tiering confirmation, especially anything in "learning/read about."
- Collections & reconciliation module — which repo, not yet verified.
- Team size context: you said "2–3 engineers, manager mostly manages" — this describes your day-to-day pod, but commit history shows much larger repo-wide teams (10–20+ contributors per repo). Worth clarifying for interview answers: your immediate team vs the platform's total engineering headcount are different numbers.
