# Master Inventory — Kuzhandayan K V
Everything true about my work. The resume draws ONLY from here.
Rule: if it isn't in this file, it does not go on a resume.

---

## Property Automate — work areas

### Core multi-tenant API
- **What it is:** The heart of the platform — REST API covering leasing, CRM, finance, HR, maintenance and inventory across many tenant databases (database-per-tenant architecture).
- **My scope:** [VERIFY] — portfolio says "I work across" this area; not stated whether I own it solo, co-own it, or contribute to it among a larger team.
- **Tech:** Node.js, Express, PostgreSQL, RabbitMQ, OpenAI, ChromaDB.
- **Metrics:** [VERIFY] — no tenant count, request volume, or DB size documented anywhere.
- **Notes:** Convention-based routing with no central registry. PDF/report generation offloaded to RabbitMQ. AI subsystem for semantic property/inventory search (ChromaDB + OpenAI).
- **Resume-ready bullet (draft):** "Built and maintain core REST API modules (leasing, CRM, finance, HR, maintenance, inventory) on a database-per-tenant PostgreSQL architecture, including an AI-powered semantic search subsystem over ChromaDB."

### RabbitMQ worker service
- **What it is:** Python/FastAPI service with no public API — consumes RabbitMQ messages to run long work off the request cycle.
- **My scope:** [VERIFY] — base resume bullet claims "re-architected... into a RabbitMQ-driven Python worker service" and "5,000+ records in under 15 seconds (~400 records/sec)" — portfolio corroborates the shape (chunked bulk CSV/Excel imports, Excel/PDF report generation) but not the metric's source or measurement date.
- **Tech:** Python, FastAPI, pandas, SQLAlchemy, RabbitMQ.
- **Metrics:** 15 worker threads (portfolio). 5,000+ records / <15s, ~400 records/sec (base resume) — [VERIFY: when/how measured].
- **Notes:** Chunked bulk CSV/Excel imports validated into per-tenant Postgres. Excel/PDF report generators for revenue recognition, lease summaries, quotations. DataFrame-first design — mapping/cleaning/validation/dedup vectorised, only the final bulk write is per-row.
- **Resume-ready bullet:** Already on base resume (RabbitMQ/FastAPI worker bullet).

### Authentication & identity
- **What it is:** Standalone Fastify microservice handling signup, login, OTP, and TOTP-based 2FA across every tenant.
- **My scope:** [VERIFY] — sole owner, co-builder, or contributor?
- **Tech:** Fastify, JWT, TOTP 2FA, multi-tenant per-tenant dynamic DB connections.
- **Metrics:** All tenants covered. [VERIFY: tenant count].
- **Notes:** Maintenance lock during tenant DB restores. Why Fastify here vs Express in core API — [VERIFY: decision reasoning].
- **Resume-ready bullet (draft):** "Built a standalone Fastify identity microservice handling multi-tenant signup, login, and TOTP-based two-factor authentication, with per-tenant dynamic DB connections and restore-safe maintenance locking."

### Manager web app & job engine
- **What it is:** Operational SPA property managers live in — leasing, portfolio, finance dashboards, HR, maintenance on a REST data layer. Migrated from CRA to Vite. Plus a multi-tenant scheduler running recurring jobs per tenant in each company's own timezone.
- **My scope:** [VERIFY].
- **Tech:** React 18, Vite, Context API, Material UI, i18next.
- **Metrics:** 5 locales (i18next). [VERIFY: user count / manager count].
- **Notes:** CRA → Vite migration — [VERIFY: why, and any measured build/dev-server speed improvement].
- **Resume-ready bullet (draft):** "Built and maintain the manager-facing SPA (leasing, portfolio, finance, HR, maintenance dashboards) in React 18 + Vite with i18next localization across 5 locales, migrated from Create React App to Vite, plus a multi-tenant job scheduler running per-tenant recurring jobs in each company's own timezone."

### Collections & reconciliation
- **What it is:** Collections module tracing payment across the whole chain — wallet → property → agreement → invoice → receipt.
- **My scope:** [VERIFY].
- **Tech:** Objection.js, multi-currency, reporting.
- **Metrics:** De-duplicates across 4 wallet types. [VERIFY: volume of transactions/receipts processed].
- **Notes:** Tracks unsettled balance, separates proxy from real receipts, filterable manager dashboard with live collected/unsettled totals.
- **Resume-ready bullet (draft):** "Built a collections & reconciliation module tracing multi-currency payments across wallet → property → agreement → invoice → receipt, de-duplicating across 4 wallet types with a live collected/unsettled dashboard."

### Pricing & escalation engines
- **What it is:** Led the move of billing-schedule and rent-escalation maths from frontend to backend.
- **My scope:** "Led" per portfolio copy — [VERIFY: led = designed and drove solo, or led a small group?].
- **Tech:** Node.js, business logic refactor, React (frontend became pure display layer).
- **Metrics:** 3,700-line scheduler → 822-line orchestrator (portfolio; also matches Rule 5 interview trigger #5 — full story to be captured).
- **Notes:** Base-price vs compounding modes, per-rule overrides, auto-renewal escalation, leap-year-aware day counting. Original archived for rollback. Contract-level pricing for resale/bulk contracts where one edit cascades across sibling units, plus a sequential approval workflow gating resale initiation.
- **Resume-ready bullet (draft):** "Led the migration of rent-escalation and billing-schedule logic from frontend to backend, refactoring a 3,700-line scheduler into an 822-line orchestrator of focused calculators (original archived for safe rollback), covering base-price/compounding modes, per-rule overrides, and leap-year-aware day counting."
- **[VERIFY — full story needed]:** What was wrong with the 3,700-line version, how the split was designed, how billing correctness was protected during the migration (tests? shadow runs? staged rollout?).

### LLM coding-agent memory-layer architecture
- **What it is:** Self-initiated. Designed the memory-layer architecture and token-governance rules LLM coding agents now run on across the team's repos (core API, manager SPA, cron scheduler, queue worker).
- **My scope:** Self-initiated, individually designed (per portfolio wording).
- **Tech:** LLM agents, context engineering, token governance, developer productivity tooling.
- **Metrics:** Adopted across 4 repos. 200-line cap per file enforced. [VERIFY: measured token/time savings, or is "far fewer tokens... faster... more consistent" qualitative only?]
- **Notes:** Each repo carries a binding agent-contract file for its architecture plus a per-feature notes folder. Explicit rules scope every task to a git diff instead of re-surveying whole domains, ban re-reading files already in context, and force a stop-and-ask on repeated errors or risky files.
- **Resume-ready bullet (draft):** "Self-initiated and architected a memory-layer and token-governance system for LLM coding agents, adopted across 4 team repositories — per-repo agent-contract files, 200-line file caps, and diff-scoped task rules — reducing redundant context re-derivation and improving agent output consistency."
- **[VERIFY]:** Any concrete before/after metric (tokens per session, PR review time, agent error rate) would strengthen this significantly for AI-tooling-flavored JDs.

---

## Next Wave ERP internship

- **Role:** Data Analytics Intern, Thiruvallur, India, Jun 2023 – Aug 2023.
- **Work:** Data analysis, visualization and reporting using Python, Excel and Tableau. Generated business insights for decision-making. Built Tableau dashboards surfacing key trends for stakeholders; prepared recurring reports for non-technical stakeholders.
- **[VERIFY]:** Dataset size, number of dashboards, specific business decisions influenced.

---

## Personal products — DineFlow, envault, Finio

### DineFlow — Multi-Tenant Restaurant SaaS
- Cloud-based, multi-tenant restaurant POS and management platform — one codebase, unlimited restaurants, each fully isolated.
- Handles full order lifecycle, automatic GST (CGST + SGST) calculation, live inventory tracking, role-scoped access (5 staff roles), platform-wide admin panel (tenants, subscriptions, announcements).
- Stats: unlimited tenants, 5 staff roles, 2 order types, live status.
- Tech: Next.js 16, React 19, TypeScript, Prisma 7, PostgreSQL, NextAuth, Tailwind CSS v4, Docker.
- Link: dine-flow-app.vercel.app
- [VERIFY]: any real usage (test restaurants, transactions processed) beyond the demo?

### envault — Zero-Knowledge Secrets CLI
- CLI that syncs .env files across machines via a private GitHub repo as storage — no shared backend, no central database.
- Two independent locks: GitHub token (access) + master password (decryption, never stored).
- Stats: AES-256-GCM encryption, 15 commands, 8 dependencies, published live on npm.
- Features: zero-knowledge encryption, GitHub-backed storage, interactive merge, in-terminal editing, security PIN gate, multi-project/multi-env support, auto-detect & bulk import, CI/CD ready.
- Tech: Node.js, Commander, Inquirer, Octokit, simple-git, AES-256-GCM, scrypt, npm.
- Links: npm (envault-sync), docs (kuzhandayan.github.io/envault-docs).
- [VERIFY]: install/download count, any external users.

### Finio — Personal Finance Mobile App
- Fully on-device Flutter app — no financial data leaves the phone.
- Auto-reads Indian bank/UPI SMS, categorizes transactions, tracks budgets, exports Excel/PDF.
- Cristy AI v0.1 — own model layer for spending-pattern analysis and financial guidance.
- Stats: 90+ banks & UPI formats read, 0 data leaves device, Google Drive auto-backup.
- Tech: Flutter, Dart, sqflite, Provider, Cristy AI v0.1, Google Drive API, Google Sign-In, WorkManager.
- Link: none public yet.
- [VERIFY]: is Cristy AI a fine-tuned model, a prompted LLM wrapper, or rule-based? Matters for how honestly to describe it as "AI-powered."

---

## Hackathons & side builds

### ShareGoto — Blockchain real estate platform
- Office hackathon, Winner, Aug 2025.
- Blockchain-based real estate tokenisation enabling fractional ownership via NFTs.
- Tech: Next.js, Solidity, PostgreSQL, Web3, MetaMask, ERC-721.
- Link: sharegoto.propgoto.app.
- Note: base resume's "HackGoto 2025 Winner" achievement — [VERIFY: is HackGoto the hackathon name and ShareGoto the project name? Confirm exact relationship before combining language.]

### Agent Goto — AI-powered intelligent agent
- Cityscape Global 2025 Finalist, Riyadh, Saudi Arabia.
- AI agent using Model Context Protocol (MCP) and vector databases for fast context retrieval, with built-in data-leak prevention.
- Tech: MCP, vector DB, AI/ML, Next.js, security.
- Link: agentgoto-web.propgoto.app.
- Matches base resume's "Cityscape Global 2025 Finalist" achievement bullet.

### Momentum — Social media automation platform
- Personal project. Flask automation platform with Instagram and LinkedIn integration.
- AI-powered content generation via Gemini API, scheduled posting.
- Tech: Flask, Python, Gemini AI, Docker, Instagram API, LinkedIn API.
- Link: github.com/kuzhandayan/momentum_agent_publisher.
- Not currently on base resume — swappable project candidate for Python/AI-flavored JDs.

### Bus Tracking System — Real-time transportation solution
- Personal project. College bus tracking with real-time location monitoring and schedule management.
- Tech: React.js, Django, Real-time API, Leaflet.js.
- Link: github.com/kuzhandayan/SVCE-Bus-Tracking.fork.
- Not currently on base resume — swappable project candidate; Django experience not otherwise represented elsewhere.

---

## Skills — tiered (draft, to be confirmed in interview Step 3)

### Shipped to production
JavaScript, TypeScript, Python, SQL, React, Node.js, Express, PostgreSQL, FastAPI, REST APIs, Microservices, JWT Authentication, RabbitMQ, Docker, Git, GitHub, AWS (EC2, S3) [VERIFY: AWS is on base resume tools row — confirm actual production use vs setup-only], Linux, CI/CD, Fastify, Material UI (MUI), i18next, Objection.js.

### Used in side projects only
Dart, Flutter, Next.js, Prisma, Tailwind CSS, shadcn/ui, NextAuth, Solidity, Web3/MetaMask, ERC-721, Flask, Gemini API, Django, Leaflet.js, ChromaDB [VERIFY: production or side-project tier — portfolio lists it under Property Automate core API, base resume lists it under Databases row — needs confirmation of actual production status], sqflite, Google Drive API.

### Learning / read about — NEVER on resume, Gaps only
[VERIFY in interview — nothing explicit documented yet in either source.]

---

## Achievements
- HackGoto 2025 Winner — led a four-member team to build an enterprise solution during the company hackathon, secured first place. [Base resume language; portfolio's "ShareGoto... Office hackathon Winner Aug 2025" appears to be the same event/project — VERIFY exact naming.]
- Cityscape Global 2025 Finalist, Riyadh — built an AI-powered assistant (Agent Goto) using MCP and vector databases for secure enterprise knowledge retrieval.

## Certifications (not currently on resume — available for Gaps/skills backup only)
1. AI Masterclass — Freedom With AI — Nov 2023 — ID FWA-AI-2023-KKV.
2. iCUBE — National Intercollegiate Technical Event — SVCE — Dec 2022 — ID SVCE-iCUBE-2022-KKV.
3. Python (Basic) — HackerRank — May 2022 — ID 818287fd6578.
4. Artificial Intelligence in Python — Great Learning — Dec 2021 — ID ZEBVIARB.
5. Introduction to Cyber Security — Simplilearn — Aug 2021 — ID 2801553.
6. Responsive Web Design — freeCodeCamp — Feb 2024 — ID fcc301a0bfd-b6c7-45a5-bd54-e7ea15b0d4ca.

## Education
1. M.Tech, Artificial Intelligence and Data Science — SRM Institute of Science and Technology, Kattankulathur, Chennai — May 2026 – Present. Working-professional postgraduate program, pursued alongside full-time work.
2. B.Tech, Artificial Intelligence and Data Science — Sri Venkateswara College of Engineering, Chennai — Aug 2021 – Jun 2024. Real-world projects applying AI/ML/data science to industry-relevant problems.
3. Diploma in Mechanical Engineering — Thiagarajar Polytechnic College, Tamil Nadu — May 2018 – Apr 2021. [Not on base resume — pre-degree credential; likely not resume-relevant but kept here for completeness.]

---

## Gaps flagged during interview
(To be filled during Step 2–3 of the interview. Nothing here yet — this is the seed draft.)
