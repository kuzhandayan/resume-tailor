# resume-tailor

A JD-driven resume tailoring workspace. Paste a job description, get a tailored
one-page LaTeX resume generated from a single frozen base template plus a
fact-checked personal inventory — nothing on the resume that isn't verified
true.

## How it works

1. `master.md` is the only source of truth — every claim on any resume traces
   back to it. It was built from a portfolio dump, the base resume, and a
   verification pass against actual git history across every real project
   repo, followed by an interview to fill in what code alone can't answer.
2. `base_stable_template.tex` is the frozen, ATS-safe LaTeX base — layout and
   structure are final; only text content is ever tailored.
3. `CLAUDE.md` defines the tailoring rules: what's frozen, what's editable,
   hard size/structure caps to keep the resume at one page, and truth
   requirements (no fabricated numbers, no invented tech, no inflated scope).
4. Paste a job description (or run `/tailor <slug>`) and a tailored resume is
   generated into `out/<slug>.tex`, with a report on what matched and what
   gaps remain.

## Structure

| Path | Role |
|---|---|
| `CLAUDE.md` | Tailoring rules the assistant follows |
| `base_stable_template.tex` | Frozen base resume (LaTeX, compiles in Overleaf) |
| `master.md` | Verified factual inventory — the only source of resume claims |
| `portfolio-dump.md` | Raw portfolio site copy, used to seed `master.md` |
| `jd/` | Job descriptions pasted in |
| `out/` | Generated tailored resumes |
| `log.md` | Running record of every resume generated |
| `.claude/commands/tailor.md` | The `/tailor` command |

This repo is private — `master.md` and `portfolio-dump.md` contain personal
career details not meant for public visibility.
