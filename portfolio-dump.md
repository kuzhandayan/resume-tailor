# Portfolio Content Audit — Full Site Dump

> Every piece of visible copy on kuzhandayan-portfolio.site, section by section, top to bottom. Use this file to review/edit content in one place, then push the edits back into the matching component. This file is NOT rendered anywhere — it's a working doc.

---

## 0. Global Nav

**`NavBar.tsx`**
- Logo text: `Portfolio`
- Right side: Theme toggle (light/dark)

**`StickyPillNav.tsx`** (sticky pill, appears on scroll)
- Work → `#work`
- Stack → `#stack`
- Path → `#path`
- Contact → `#contact`

---

## 1. Hero (`Hero.tsx`)

- Eyebrow: `Full-stack · product · infra`
- Headline: **Builds the whole thing.**
- Subtext: *"Software engineer at Property Automate, on an enterprise multi-tenant property platform. Outside work I ship my own products: a restaurant SaaS, an on-device finance app, and a security CLI on npm."*
- CTAs: `See the work` (#work) · `Resume · View/Download` (Google Drive PDF link, download icon) · `Get in touch` (#contact)
- Chips: React, Node.js, PostgreSQL, Next.js, Flutter, Python
- Right side photo card:
  - Name: Kuzhandayan K V
  - Location: Chennai, India
- "Last shipped" live badge: **envault — published on npm**, `$ npm install -g envault-sync`
- "Currently" card: **Software Engineer — Property Automate**, tenure auto-calculated from Feb 2024 · Chennai

---

## 2. Summary (`SummaryCard.tsx`)

- Eyebrow: `Summary`
- Lead paragraph: *"At work I build across an enterprise, multi-tenant property platform — the core API, the identity service, the manager application and the Python workers behind them. On my own time I ship complete products: **DineFlow**, a restaurant SaaS; **Finio**, an on-device finance app; and **envault**, a zero-knowledge CLI on npm."*
- Second paragraph: *"Core stack is React, Node.js, Express and PostgreSQL, backed by RESTful APIs, JWT auth and microservice architecture — but I care about developer experience and security as much as scalability. Currently deepening cloud infrastructure and DevOps practice."*
- Stats row:
  - Tenure (auto-calculated) — "of experience and evolving"
  - **2** — Hackathon awards
  - **6** — Verified certifications
  - **1** — Package published on npm

---

## 3. Project Highlights (`ProjectHighlights.tsx`) — id `#work`

- Eyebrow: `Highlights · built on my own time`
- Headline: **The ones worth walking through.**
- Intro: *"All three are personal products — designed, built and shipped outside work. A selection, not the full list. My day job at Property Automate is below."*

### DineFlow
- Kicker: Personal · Live SaaS product
- Tagline: *Every restaurant. One system.*
- Description: Cloud-based, multi-tenant restaurant POS and management platform — one codebase running unlimited restaurants, each fully isolated. Handles the entire order lifecycle, calculates GST (CGST + SGST) automatically on every bill, tracks inventory live, and scopes access per staff role. A platform-wide admin panel manages every tenant, subscription and announcement from one dashboard.
- Link: View live → dine-flow-app.vercel.app
- Stats: Unlimited tenants · 5 staff roles · 2 order types · Live status
- Features (8): Multi-tenant SaaS, Automatic GST billing, Full order lifecycle, Live inventory, Role-based access, Kitchen display, Revenue & GST reports, Platform admin panel
- Tech: Next.js 16, React 19, TypeScript, Prisma 7, PostgreSQL, NextAuth, Tailwind CSS v4, Docker

### envault
- Kicker: Personal · Open source CLI
- Tagline: *Your secrets. Your GitHub. Zero-knowledge.*
- Description: A CLI that securely syncs .env files across laptops using your own private GitHub repo as storage — no shared backend, no central database. Every file is encrypted end-to-end before it leaves your machine. Two independent locks protect every secret: a GitHub token for access and a master password for decryption — neither alone is enough, and the password is never stored anywhere.
- Links: npm (envault-sync) · Docs (kuzhandayan.github.io/envault-docs)
- Stats: AES-256 encryption · 15 commands · 8 dependencies · Live status
- Features (8): Zero-knowledge encryption, GitHub-backed storage, Interactive merge, In-terminal editing, Security PIN gate, Multi-project/multi-env, Auto-detect & bulk import, CI/CD ready
- Tech: Node.js, Commander, Inquirer, Octokit, simple-git, AES-256-GCM, scrypt, npm

### Finio
- Kicker: Personal · Flutter app
- Tagline: *Track less. Know more.*
- Description: Fully on-device and secured layer by layer — no financial data ever leaves the phone. Auto-reads every Indian bank and UPI SMS, categorises transactions, tracks budgets, and exports Excel and PDF reports. Cristy AI v0.1, my own model layer, analyses spending patterns and delivers sharp, privacy-safe financial guidance on demand.
- Links: none (no public link yet)
- Stats: 90+ banks & UPI read · 0 data leaves device · v0.1 Cristy AI · Drive auto backup
- Features (8): SMS auto-reader, Smart dashboard, Cristy AI v0.1, Full customisation, On-device & secure, Auto Drive backup, Excel & PDF reports, AI chat
- Tech: Flutter, Dart, sqflite, Provider, Cristy AI v0.1, Google Drive API, Google Sign-In, WorkManager

---

## 4. At Work / Property Automate (`AtWork.tsx`)

- Eyebrow: `At work`
- Headline: **Property Automate.**
- Intro: *"Full-stack engineer on PropGoto — a multi-tenant, enterprise property management platform with a database per tenant. I work across the core REST API, the identity service, the manager SPA, the job scheduler and the async Python processing service. I'm also trusted as one of the team's **designated Git approvers**, reviewing every pull request against our engineering standards before it merges — safeguarding **code quality** and a **clean, production-ready deployment pipeline** across the platform."*

Tabs (interactive, click to switch):
1. **Core multi-tenant API** — *database per tenant* — Node.js, Express, PostgreSQL, RabbitMQ, OpenAI, ChromaDB. The heart of the platform — REST API covering leasing, CRM, finance, HR, maintenance and inventory across many tenant databases. Convention-based routing with no central registry, PDF/report generation offloaded to RabbitMQ, AI subsystem for semantic property/inventory search.
2. **RabbitMQ worker service** — *15 worker threads* — Python, FastAPI, pandas, SQLAlchemy. Python service with no public API — consumes RabbitMQ messages to run long work off the request cycle: chunked bulk CSV/Excel imports validated into per-tenant Postgres, Excel/PDF report generators for revenue recognition, lease summaries, quotations. DataFrame-first: mapping/cleaning/validation/dedup vectorised, only final bulk write is per-row.
3. **Authentication & identity** — *all tenants* — Fastify, JWT, TOTP 2FA, Multi-tenant. Standalone Fastify microservice handling signup, login, OTP and TOTP-based 2FA across every tenant — per-tenant dynamic DB connections, maintenance lock during tenant DB restores.
4. **Manager web app & job engine** — *5 locales* — React 18, Vite, Context API, Material UI, i18next. Operational SPA property managers live in — leasing, portfolio, finance dashboards, HR, maintenance on a REST data layer, migrated from CRA to Vite. Plus a multi-tenant scheduler running recurring jobs per tenant in each company's own timezone.
5. **Collections & reconciliation** — *finance* — Objection.js, Multi-currency, Reporting. Collections module tracing payment across the whole chain — wallet → property → agreement → invoice → receipt. De-duplicates across 4 wallet types, tracks unsettled balance, separates proxy from real receipts, filterable manager dashboard with live collected/unsettled totals.
6. **Pricing & escalation engines** — *leasing · architecture* — Node.js, Business logic, Refactor, React. Led the move of billing-schedule and rent-escalation maths from frontend to backend — base-price vs compounding modes, per-rule overrides, auto-renewal escalation, leap-year-aware day counting. 3,700-line scheduler → 822-line orchestrator delegating to focused calculators (original archived for rollback); frontend became a pure display layer over one endpoint. Contract-level pricing for resale/bulk contracts where one edit cascades across sibling units, plus a sequential approval workflow gating resale initiation.
7. **LLM coding-agent memory-layer architecture** — *AI tooling · token governance* — badge: *self-initiated* — LLM agents, Context engineering, Token governance, Developer productivity. Built after all the work above — designed the memory-layer architecture and token-governance rules LLM coding agents now run on across our repos (core API, manager SPA, cron scheduler, queue worker). Each repo carries a binding agent-contract file for its architecture plus a per-feature notes folder, with a hard 200-line cap enforced per file so context stays lean instead of ballooning. Explicit rules scope every task to a git diff instead of re-surveying whole domains, ban re-reading files already in context, and force a stop-and-ask on repeated errors or risky files. Result: agent sessions spend far fewer tokens re-deriving architecture, respond faster, and produce more consistent, reliable code across the team.

---

## 5. Hackathons & Side Work (`Projects.tsx`) — id `#projects`

- Eyebrow: `Hackathons & side work`
- Headline: **Office hackathons and personal builds.**

1. **ShareGoto** — Blockchain real estate platform — badge: *Office hackathon · Winner, Aug 2025*
   Blockchain-based real estate tokenisation enabling fractional ownership through NFTs. Next.js and Solidity on PostgreSQL, MetaMask integration for real-time transactions.
   Tech: Next.js, Solidity, PostgreSQL, Web3, MetaMask, ERC-721
   Links: live project (sharegoto.propgoto.app), LinkedIn post

2. **Agent Goto** — AI-powered intelligent agent — badge: *Cityscape Global 2025 Finalist · Riyadh, Saudi Arabia*
   AI agent using Model Context Protocol and vector databases for fast context retrieval, with built-in data-leak prevention.
   Tech: MCP, Vector DB, AI/ML, Next.js, Security
   Links: live project (agentgoto-web.propgoto.app)

3. **Momentum** — Social media automation platform — badge: *Personal project*
   Flask automation platform with Instagram and LinkedIn integration. AI-powered content generation via the Gemini API, scheduled posting.
   Tech: Flask, Python, Gemini AI, Docker, Instagram API, LinkedIn API
   Links: repository (github.com/kuzhandayan/momentum_agent_publisher)

4. **Bus Tracking System** — Real-time transportation solution — badge: *Personal project*
   College bus tracking with real-time location monitoring and schedule management, built for student transport and route efficiency.
   Tech: React.js, Django, Real-time API, Leaflet.js
   Links: repository (github.com/kuzhandayan/SVCE-Bus-Tracking.fork)

---

## 6. Stack (`Skills.tsx`) — id `#stack`

- Eyebrow: `Stack`
- Headline: **What I reach for.**

- **Languages:** JavaScript, TypeScript, Python, SQL, Solidity, Dart
- **Frameworks & libraries:** React, Node.js, Express, Next.js, Django, Flutter
- **Database & cloud:** PostgreSQL, Prisma, Supabase, Neon, ChromaDB
- **DevOps & infrastructure:** Linux, Docker, Vercel, Cloudflare, Netlify
- **Tools & technologies:** Git, GitHub, Zustand, Shadcn/ui, Bash, npm

---

## 7. Experience & Education (`Experience.tsx`) — id `#path`

- Eyebrow: `Path`
- Headline: **Experience & education.**

1. **Work** — Software Engineer @ Property Automate — Chennai, India — *Feb 2024 — present*
   - Full-stack work across a multi-tenant property management platform — core REST API, identity service, manager SPA, scheduler and async Python workers
   - Database-per-tenant architecture on PostgreSQL, with request-scoped tenant resolution across multiple tenant databases
   - Built RabbitMQ-driven background processing: chunked bulk Excel/CSV imports and Excel/PDF report generation off the request cycle
   - Implemented JWT-based authentication and TOTP two-factor auth in a standalone Fastify identity service

2. **Education** — M.Tech Artificial Intelligence and Data Science @ SRM Institute of Science and Technology — Kattankulathur, Chennai — *May 2026 — present*
   - Working professional postgraduate program in advanced AI, machine learning and data science
   - Pursued alongside full-time professional work

3. **Education** — B.Tech Artificial Intelligence and Data Science @ Sri Venkateswara College of Engineering — Chennai, India — *Sep 2021 — Jun 2024*
   - Fundamentals in AI, machine learning, deep learning and data science
   - Real-world projects applying core concepts to industry-relevant problems
   - Graduated with a strong foundation in Python, data analysis and applied AI

4. **Work** — Data Analytics Intern @ Next Wave ERP Solutions — Thiruvallur, India — *Jun 2023 — Aug 2023*
   - Data analysis, visualisation and reporting using Python, Excel and Tableau
   - Generated business insights for decision-making

5. **Education** — Diploma in Mechanical Engineering @ Thiagarajar Polytechnic College — Tamil Nadu, India — *May 2018 — Apr 2021*
   - Foundations in mechanical systems, engineering drawing and manufacturing
   - Problem-solving and technical skills through hands-on workshop training

---

## 8. Verified Certifications (`Certifications.tsx`)

- Eyebrow: `Credentials`
- Headline: **Verified certifications.**

1. **AI Masterclass** — Freedom With AI — Nov 2023 — ID `FWA-AI-2023-KKV` — Artificial Intelligence, AI Tools, Machine Learning
2. **iCUBE — National Intercollegiate Technical Event** — Sri Venkateswara College of Engineering — Dec 2022 — ID `SVCE-iCUBE-2022-KKV` — Hackathon, Innovation, Teamwork
3. **Python (Basic)** — HackerRank — May 2022 — ID `818287fd6578` — Python, Data Structures, Algorithms
4. **Artificial Intelligence in Python** — Great Learning — Dec 2021 — ID `ZEBVIARB` — Machine Learning, Deep Learning, Python
5. **Introduction to Cyber Security** — Simplilearn — Aug 2021 — ID `2801553` — Cybersecurity, Network Security, Risk Assessment
6. **Responsive Web Design** — freeCodeCamp — Feb 2024 — ID `fcc301a0bfd-b6c7-45a5-bd54-e7ea15b0d4ca` — HTML5, CSS3, Responsive Design, Accessibility

Each card links out to the credential verification page.

---

## 9. Settu Squad — side venture (`SettuSquad.tsx`)

- Eyebrow: `Beyond the code`
- Headline: **Settu Squad**
- Subhead: *Salem Thattu Vadai · Chennai*
- Copy: *"Bringing Salem's most loved street snack to Chennai — authentic thattu vadai sets with garlic chutneys, fresh vegetables and crispy rice flour vadai. Built with friends, and a love for good food."*
- Badge: **Opening August 2026** · Evening hours
- CTA: Visit Settu Squad → settusquad.in
- Right panel tagline: *"Not everything I build runs on a server — Some of it is built to **be tasted.**"*

---

## 10. Contact (`Contact.tsx`) — id `#contact`

- Eyebrow: `Contact`
- Headline: **Let's build something.**
- Copy: *"Open to engineering roles, collaborations and interesting problems. I typically respond within 24 hours."*
- Links:
  - Email → sabbari.kv013@gmail.com
  - GitHub → github.com/kuzhandayan
  - LinkedIn → linkedin.com/in/kuzhandayan-k-v-16ab32312
  - Resume → View/Download (Google Drive)
- Footer: © 2026 Kuzhandayan K V · Chennai, Tamil Nadu, India
- Badge: `Last updated: {LAST_UPDATED}` (constant at top of file — **must be bumped on every content/UI change**, see CLAUDE.md §4.5)
- Tagline: *Learn. Build. Repeat.*

---

## Quick reference — where to edit what

| Section | File |
|---|---|
| Nav / logo | `NavBar.tsx` |
| In-page pill nav | `StickyPillNav.tsx` |
| Hero copy, chips, resume link | `Hero.tsx` |
| Summary stats/copy | `SummaryCard.tsx` |
| DineFlow / envault / Finio | `ProjectHighlights.tsx` |
| Property Automate work tabs | `AtWork.tsx` |
| Hackathon & personal projects | `Projects.tsx` |
| Tech stack grid | `Skills.tsx` |
| Work + education timeline | `Experience.tsx` |
| Certifications | `Certifications.tsx` |
| Settu Squad | `SettuSquad.tsx` |
| Contact links, footer, last-updated | `Contact.tsx` |

**Reminder:** any content/UI edit → update `LAST_UPDATED` in `Contact.tsx` (CLAUDE.md §4.5).
