---
status: open
date: 2026-09-13
---

# 0001 — Evidence audit core

> **Needs a yes before work starts.** This brief assumes the judging step is a
> language model call, not deterministic matching. That is a recommendation, not
> a decision, and there is no record in `docs/decisions/`. If the answer is
> "deterministic instead", section 2 changes and the rest stands.

## 1. Goal

Take a job post as text and a resume as text. Pull out the discrete requirements
the post is actually asking for. For each one, say whether the resume evidences
it, partly evidences it, or does not, and quote the exact resume line that
supports it when there is one.

This is the floor everything else stands on. Tailoring later is only allowed to
use material this step marked evidenced, so if this step is loose, every later
step inherits the looseness and nobody can see it any more.

**If we do not do this at all:** the tool becomes another rewriter that makes
the resume sound better without knowing whether any of it is true. That is the
thing this project exists not to be.

## 2. Scope

Build the audit as a library plus a thin command line entry point.

- **In:** two plain text files, paths given on the command line. One job post,
  one resume.
- **Extract:** split the post into discrete requirements. Each keeps an id and
  the requirement text **verbatim from the post**. Do not paraphrase a
  requirement into something tidier than it was written.
- **Judge:** for each requirement, return exactly one of `evidenced`,
  `partly_evidenced`, `not_evidenced`; the supporting resume line **verbatim**,
  or nothing; and one short sentence of reason.
- **Out:** one structured result (JSON), and a plain readable rendering of the
  same thing on stdout. Same data, two shapes.
- **Seam:** the judging step sits behind one interface, called from one place.
  Whatever model or method is behind it, the caller does not know.

A "line" is a line as it appears in the resume file. That is the unit, and it is
what makes the verbatim rule checkable.

**Out of scope — do not touch, do not add:**

- any frontend, web page, or server
- tailoring, rewriting, drafting, cover letters, suggestions for improvement
- PDF, DOCX, or HTML parsing; reading a job post from a URL
- scoring, percentages, an overall match number
- storing resumes, caching runs, any database
- multiple languages

## 3. Must not happen

Standing ones apply: no writing to version control, no deciding anything — stop
and report, no changing a check because it failed, and where something cannot be
established say so rather than estimating.

Specific to this work:

- **A supporting line that is not in the resume.** Every quoted line must be
  present in the resume text character for character. This is the whole point.
- **A requirement that is not in the post.** Same rule, other direction.
- **A fourth verdict.** No "likely", no "unclear", no empty. Three, always.
- **Filling a gap to look complete.** `not_evidenced` with no line is a correct
  and expected answer, not a failure.
- **Any network call other than the judging step.** No fetching, no telemetry.
- **Writing the resume or post anywhere on disk** other than where the user
  already has them.

## 4. Done when

- Running the entry point on two text file paths prints an audit and writes the
  JSON.
- **Every supporting line in the output is a verbatim substring of the resume
  file.** This is an automated check, not an eyeball. Write it.
- **Every extracted requirement is a verbatim substring of the post file.** Same.
- Every requirement carries exactly one of the three verdicts.
- A resume with nothing in common with the post returns all `not_evidenced` and
  quotes nothing. Build that pair as a test input.
- An empty resume, and an empty post, each fail with a clear message rather than
  returning an empty audit that looks like a real result.
- It has been run once on a real post and a real resume the human supplies, and
  the output is in the report in full.

**What would tell us it failed:** the human reads that one real run and
disagrees with the verdicts, or the extraction invents requirements, merges two
into one, or drops one that is plainly in the post. **How many disagreements is
too many is NOT ESTABLISHED** — no bar has been set, and guessing one here would
be inventing evidence. Record the real run in the report so a bar can be set
from it next turn.

## 5. Checked by

`formwork check`, as a whole, plus the two verbatim checks named in section 4.

The gate cannot tell whether a verdict is *right*. It can only tell whether the
quoted text is real. The real evidence for correctness is the one hand-read run,
and that is why section 6 asks for it in full.

## 6. The report must contain

The standing list in `formwork/templates/report.md`, plus:

- the full output of the one real run, not a summary of it
- the list of requirements the extraction produced from that post, so the
  splitting can be judged separately from the judging
- **where the judging was shaky** — the requirements where the call between
  `partly_evidenced` and one of its neighbours was close, named individually
- anything in section 2 that turned out to be the wrong shape once it was built
