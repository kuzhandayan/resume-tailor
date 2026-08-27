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


## RULE 4 — Keyword optimization (the actual work)

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

## RULE 5 — Positioning

- Always **Full Stack Software Engineer**. Never backend-only. Never "Data Engineer"
  or "Data Scientist", even if the JD is AI/data flavoured.
- The tagline may shift to match the JD title **only** if it stays a truthful
  full-stack/software-engineering variant (e.g. "Full Stack Engineer",
  "Software Engineer, Full Stack"). Never "Senior" anything.
- ATS safety: no new packages, no icons, no glyphs, no tables, no columns.
