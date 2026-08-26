# Resume Tailoring — Project Rules

## Purpose
Given a job description, produce a tailored **one-page** LaTeX resume by editing
**only the text content** of `base_stable_template.tex`. The design is finished.
Nothing about the layout is under discussion.

## Files
| Path | Role |
|---|---|
| `base_stable_template.tex` | THE BASE. **Read-only. Never edit, never delete.** |
| `master.md` | Full career inventory. The **only** source of factual claims. |
| `jd/<slug>.md` | Job description I paste in. |
| `out/<slug>.tex` | Your generated output for a normal, well-matched JD. |
| `out/stretch/<slug>.tex` | Your generated output for a stretch JD (see Rule 7). **Frozen the moment it's written.** |
| `log.md` | Running record of every resume generated. |

## Trigger
When I paste a job description (in chat or as a file), you:
1. Derive a slug yourself: `<company-kebab>-<role-kebab>`
   e.g. `wysa-associate-fullstack-engineer`, `leucine-full-stack-engineer`
2. Save the JD to `jd/<slug>.md`
3. Decide if this JD is a **normal match** or a **stretch JD** (Rule 7) — a
   stretch JD is one asking for skills/domain outside my current work or
   skill set as recorded in `master.md`.
4. Normal match: copy `base_stable_template.tex` → `out/<slug>.tex`.
   Stretch JD: copy `base_stable_template.tex` → `out/stretch/<slug>.tex`.
5. Tailor per the rules below (Rule 7 modifies keyword rules for stretch JDs).
6. Report (see **Output**)

If I paste several JDs at once, process each **independently, from the base**.
Never chain one tailored resume off another.

---

## RULE 1 — Frozen zones (absolute)

**Everything from `\documentclass` to `\begin{document}` is frozen.** Do not
touch: documentclass, `\pdfminorversion`, any `\usepackage`, geometry margins,
`titlesec` / `\titlespacing`, all `\definecolor`, `\hypersetup`, `\linespread`,
`fancyhdr`, every `\newcommand`, and especially the **PARSER SAFETY BLOCK**
(`\input{glyphtounicode}`, `\pdfgentounicode=1`, `\DisableLigatures`). That block
is what makes the PDF ATS-parseable. Removing it silently breaks everything.

Also frozen inside the document body:
- The `\section` list, its order, and the section names
- Masthead contact line: phone, email, location, portfolio / GitHub / LinkedIn URLs
- Both `\resumeSubheading` **headers** — company names, job titles, dates, locations
- Project names and their `\href` links
- The entire Education section, verbatim
- `\entrygap`, `\vspace`, `itemsep`, font sizes — never adjust these to make text fit

## RULE 2 — Editable zones (the only things you may change)

- The tagline under my name (currently `Full Stack Software Engineer`)
- The Summary paragraph
- The **second argument** of each `\skillRow` (the skill list, not the category label)
- The text inside any `\resumeItem{...}`
- The text inside any `\stack{...}`
- The descriptor after the em dash in `\projectHeading` (e.g. `--- Multi-Tenant Restaurant SaaS`)
- The two Achievement item texts
- Placement of `\hl{}` bolding

That is the complete list. If a change you want isn't on it, don't make it.

## RULE 3 — One page, enforced structurally

I compile in Overleaf, so **you cannot check the page count.** The base already
fits exactly one page. Therefore you hold the page by never exceeding the base's
size. These are hard caps, not targets:

| Element | Cap |
|---|---|
| `out/<slug>.tex` total bytes | **≤ `base_stable_template.tex`** |
| Summary | ≤ 4 sentences, ≤ 520 characters |
| `\skillRow` lines | exactly 5 |
| Each `\skillRow` value | ≤ 78 characters |
| Property Automate `\resumeItem` | exactly 5 |
| Next Wave `\resumeItem` | exactly 2 |
| Projects | exactly 3, each exactly 2 `\resumeItem` + 1 `\stack` |
| Any single `\resumeItem` | ≤ 300 characters |
| Achievements | exactly 2 items |
| `\hl{}` occurrences | ≤ 12 total |

Verify with `wc -c base_stable_template.tex out/<slug>.tex` before reporting.
**To add anything, cut something of similar length first.** If you cannot fit a
keyword without breaking a cap, it goes in Gaps — not in the resume.

## RULE 4 — Truth

1. Every factual claim must trace to `master.md` or the base template. Never
   invent a technology, metric, employer, date, team size, or scope.
2. If the JD wants something I don't have, it goes in **Gaps**. Never onto the page.
   Not as "familiar with", not as "exposure to", not buried in a skills row.
3. Never change "2.6 years" or imply more experience than that.
4. Never fabricate numbers. If `master.md` has no metric, write the bullet without one.

## RULE 5 — Keyword optimization (the actual work)

1. **Mirror the JD's exact wording** where it truthfully matches mine — write
   "REST APIs" if they say that, "RESTful services" if they say that. Pick one;
   never list both synonyms to game the parser.
2. **Reorder, don't inflate.** Inside each `\skillRow`, move JD-relevant items to
   the front. Inside each role, move JD-relevant bullets to the front. Keep the
   five row categories as they are.
3. **Reorder projects** so the most JD-relevant is first. You may swap in a
   different project from `master.md` if it's a materially better match — same
   shape: 2 bullets + 1 `\stack`.
4. **`\hl{}` the JD's top true keywords.** Bold what a recruiter scans for, not
   generic words like "developed" or "team".
5. Spell tech exactly as the industry does (Node.js, PostgreSQL, FastAPI), even
   if the JD misspells it.
6. **No stuffing.** If a keyword can't sit in a natural, readable sentence, leave
   it out. A human reads this after the ATS does.

## RULE 6 — Positioning

- Always **Full Stack Software Engineer**. Never backend-only. Never "Data Engineer"
  or "Data Scientist", even if the JD is AI/data flavoured.
- The tagline may shift to match the JD title **only** if it stays a truthful
  full-stack/software-engineering variant (e.g. "Full Stack Engineer",
  "Software Engineer, Full Stack"). Never "Senior" anything.
- ATS safety: no new packages, no icons, no glyphs, no tables, no columns.

---

## RULE 7 — Stretch JDs (unrelated to my current work or skills)

Sometimes I'll paste a JD for a role I'm not currently qualified for on paper —
different domain, different stack, skills I'm still learning. I intend to
close that gap myself before any interview (self-study, prep). These get
different handling:

1. **Detect it.** A stretch JD is one where the core requirements don't match
   `master.md`'s "shipped to production" or "used in side projects" tiers —
   it leans on the "learning / read about" tier, or something not in
   `master.md` at all.
2. **Output goes to `out/stretch/<slug>.tex`, never `out/<slug>.tex`.**
3. **Rules 1, 2, and 3 still apply in full — no exceptions here.** Frozen
   zones stay frozen, editable zones stay the only things touched, and every
   Rule 3 structural cap (one page, exact bullet counts, byte-size ceiling,
   etc.) is enforced exactly as for a normal resume. A stretch JD changes
   WHAT keywords are honest to use — never the layout, structure, or length.
   No new UI, no new sections, no font/spacing tricks to fit more in.
   `base_stable_template.tex` itself is NEVER touched — for a stretch JD you
   still only ever copy from it into `out/stretch/<slug>.tex`, exactly like
   the normal flow. There is no scenario, stretch or otherwise, where the
   base template gets edited.
4. **Relaxed keyword rule, this tier only:** you may list skills from the
   "learning / read about" tier in the skills row / bullets as if in active
   use, since I plan to actually learn them before an interview. This is the
   ONE exception to Rule 5.6 (no stuffing) and to treating "learning" as
   Gaps-only.
5. **Everything else in Rule 4 (Truth) still holds, no exceptions:**
   - Never invent an employer, project, date, team size, or metric.
   - Never change "2.6 years" or imply more experience than I have.
   - Never claim ownership/authorship of something `master.md` says was a
     teammate's work, or something with no evidence at all.
   - The "stretch" allowance is about **skills I intend to learn**, not about
     fabricating history.
6. **Frozen on write — this is absolute, stronger than the base-template
   freeze:**
   - The moment `out/stretch/<slug>.tex` is written, it is frozen forever.
   - **No agent may ever edit, regenerate, or overwrite it again** — not on a
     follow-up request, not to "fix" something, not even if I ask casually.
     If I want changes, I edit it myself.
   - **Never `git add`, `git commit`, or `git push` anything under
     `out/stretch/`.** These files stay untracked/local-only unless I
     explicitly do it myself.
   - If I ask you to touch an existing file under `out/stretch/`, refuse and
     remind me this folder is locked — don't ask "are you sure," just decline
     and tell me why.

---

## Output

Write the file, then report in chat — **do not print the .tex**:

1. **Size**: base bytes vs out bytes
2. **Structure check**: each cap from Rule 3, actual vs limit
3. **Keywords matched**: JD terms now truthfully present
4. **Gaps**: JD requirements I cannot claim — so I can prep for the interview
5. **Changes**: max 6 lines, what you altered and why

Then append one row to `log.md`:
`| date | company | role | slug | top gaps |`

## Never
- Never edit `base_stable_template.tex`
- Never start from a previous `out/*.tex`
- Never shrink font, margins, or spacing to fit
- Never write a cover letter unless I ask
- Never edit, regenerate, or overwrite anything under `out/stretch/` once written
- Never `git add`/`commit`/`push` anything under `out/stretch/`
