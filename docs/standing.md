---
updated: 2026-09-13
by: director, planning conversation
---

# Standing brief

## What we are building

A local tool for one job seeker. It reads a job post and a resume, checks every
requirement in the post against evidence in the resume, and says exactly which
line supports it or marks it not evidenced. It then drafts tailored resume and
cover letter text using only the evidenced material. Single user, runs on the
user's own machine. No accounts, no hosted data, English only.

**Why it exists:** it is a technical assignment for a DataRobot Professional
Services interview. The deliverable is a working application plus a fifteen
minute presentation with a live demo, and the code goes on GitHub at
`makariim/job-hunter`.

**Fixed by the assignment, not by us:**

- a complete application, **frontend and backend both**. A notebook or a bare
  command line tool does not satisfy this.
- **code-first, on an open-source agent framework** — LangChain, LangGraph,
  CrewAI, Pydantic AI or LlamaIndex.
- scored on problem-solving acumen, strategic vision, technical credibility,
  creativity and passion. **Four of those five are not code.** They said
  explicitly they do not expect production quality.

**What that scoring means in practice:** a demo that breaks live costs far more
than a feature that is missing. Build for the demo path, not for coverage.

**The demo will be run live on the DataRobot job post itself, gaps included.**
So the tool must work from pasted text and must not fetch the post over the
network.

**Time available is evenings only.** Size every brief for that.

## Where we are now

Nothing is built. No application code exists.

- The kit is installed and `formwork check` is green, 12 checks.
- `docs/briefs/0001-evidence-audit-core.md` is **open**. No report yet.
- **Brief 0001 does not satisfy the agent constraint above and must be revised
  before work starts.** One model call returning structured JSON is extraction,
  not an agent. See "what is open".
- Repository is on GitHub, public, three commits, authored correctly.

## What is decided

**`docs/decisions/` is empty. Nothing is recorded, so by this project's own
rule nothing is decided yet.** The four below are settled in conversation and
waiting to be written down. Until they are, they are things somebody remembers.

- **the audit governs the tailor** — tailoring may use only material the audit
  marked evidenced, and must refuse to claim anything marked not evidenced
- **the judging step is a language model call**, not deterministic matching —
  a decision record is to be drafted for this one
- **single user, local, no accounts, no hosted data, English only**
- **the audit must work end to end before any tailoring work starts** — the
  tailor is the thing dropped if time runs out

## What is open

- **The deadline date is NOT ESTABLISHED.** Not confirmed by them. Working
  assumption is the coming weekend. Everything is sized against that.
- **The agent shape.** The assignment needs several steps, real tool use, and at
  least one decision the system makes without being told. What those are is not
  settled, and it changes the scope of brief 0001.
- **Which framework.** LangGraph, Pydantic AI and the rest are all allowed and
  none is chosen.
- **Reading a job post from a URL.** Position is **no**, because live scraping
  fails in the worst possible minute. Not decided.
- **How accurate the audit has to be.** NOT ESTABLISHED. Nothing has been run
  once, so there is no bar and no basis for one.

## What is next

1. Settle the agent shape, then revise brief 0001 against it.
2. Build the audit core and run it once on the real DataRobot post.

## What we tried and stopped

Nothing yet.
