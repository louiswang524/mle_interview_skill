# MLE Career Skills Repo — Design Spec
**Date:** 2026-04-12
**Status:** Approved

---

## Overview

A collection of Claude Code skills covering the full MLE/research job-hunting lifecycle — from cold outreach through offer selection. Skills are independently invokable and can also be chained by a top-level orchestrator skill (`mle_career`).

**Target users:** All experience levels — new grads, career-switchers, and senior MLEs targeting top labs. Skills adapt to experience level.

**Architecture:** Option A — flat skill library + thin orchestrator. Each skill is a single `SKILL.md` file, self-contained, independently testable.

---

## Skill Inventory

### Pre-Interview

#### `mle_resume`
- **Trigger:** `/mle_resume`
- **Modes:**
  - **Review mode** — User pastes resume. Skill scores it across 5 dimensions: impact/quantification, ML/research relevance, clarity, ATS keyword coverage, and role-level tailoring. Gives line-by-line suggestions and outputs a full revised version.
  - **Tailor mode** — User provides a job link. Skill fetches and parses the job description via web fetch, then rewrites the resume to target that specific role (keywords, framing, prioritization of experience).
- **Language support:** English and Chinese. Skill auto-detects resume language and responds in the same language. All scoring, feedback, and rewritten output are in the detected language. If the user requests a translation (e.g., Chinese resume for a US company), skill produces both versions.
- **Data strategy:** Web fetch for job descriptions in tailor mode; static knowledge for scoring rubrics.

#### `mle_cold_outreach`
- **Trigger:** `/mle_cold_outreach`
- **What it does:** Drafts cold emails or LinkedIn DMs targeting recruiters, researchers, or hiring managers at specific companies. Adapts tone for academic labs vs. industry teams. Asks for: target person/role, target company, user's background summary, and desired outcome (referral, coffee chat, direct apply).
- **Data strategy:** Web search for company culture signals and recent research output when relevant.

#### `mle_intro`
- **Trigger:** `/mle_intro`
- **What it does:** Coaches the user on their "tell me about yourself" elevator pitch for MLE/research roles. Interactive loop: user gives a draft → skill scores it (clarity, relevance, narrative arc, length) and gives feedback + rewrite → drills until pitch is sharp. Adapts target length and framing to context (phone screen vs. onsite vs. networking event).
- **Data strategy:** Static knowledge only.

---

### During Interview

#### `mle_company_research`
- **Trigger:** `/mle_company_research`
- **What it does:** User names a company and role. Skill web-searches for recent interview reports and synthesizes:
  - Interview format and number of rounds
  - Common question themes (technical, behavioral, system design)
  - Culture signals and team structure
  - Known red flags or green flags
  - Compensation range and leveling (from levels.fyi and recent data points)
- **Data strategy:** Web search at runtime (Glassdoor, Blind, levels.fyi, company research blogs, team pages).

#### `mle_behavioral`
- **Trigger:** `/mle_behavioral`
- **Modes:**
  - **Practice mode** — Drills behavioral questions using STAR method. Adaptive difficulty (1–5). Exam or coach sub-mode (matching `mle_interview` conventions). Covers: leadership, conflict resolution, failure/learning, cross-functional collaboration, ambiguity, research impact.
  - **Story builder mode** — Skill interviews the user with guided questions to extract a raw story ("tell me about a time you disagreed with a teammate"). Through dialogue, it draws out the Situation, Task, Action, and Result. Then writes a polished STAR-format answer the user can memorize and reuse. Saves answers to a session file.
- **Data strategy:** Static knowledge for question bank; session file written to current directory at end.

#### `mle_interview` *(existing)*
- **Trigger:** `/mle_interview`
- **What it does:** Adaptive mock technical interview. Topics: transformers, RLHF, RAG, distributed training, CUDA, recommendation systems, diffusion models, LLM inference. Exam or coach mode. Saves session file on completion.
- **No changes needed.**

---

### Post-Interview

#### `mle_negotiate`
- **Trigger:** `/mle_negotiate`
- **What it does:** User inputs an offer (base, equity grant, vesting schedule, signing bonus, level, location, company). Skill:
  1. Web-searches for comp benchmarks (levels.fyi, recent reports)
  2. Assesses offer vs. market for that level/company/location
  3. Proposes a negotiation strategy (what to push on, what to accept)
  4. Drafts a counter-offer email
  5. Optionally role-plays the negotiation conversation
- **Data strategy:** Web search for comp benchmarks; static knowledge for negotiation tactics.

#### `mle_offer_compare`
- **Trigger:** `/mle_offer_compare`
- **What it does:** User inputs 2–5 offers. Skill:
  1. Computes total comp across vesting schedules (4-year, with/without cliff, refresh cadence)
  2. Scores offers on non-comp dimensions: mission alignment, team/manager quality signals, growth trajectory, WLB, location/remote policy, company stage risk
  3. Produces a ranked comparison table with a clear recommendation and reasoning
- **Data strategy:** User-provided offer data; web search for company-stage and culture signals when needed.

---

### Orchestrator

#### `mle_career`
- **Trigger:** `/mle_career`
- **What it does:** Entry point for end-to-end career coaching sessions. On start:
  1. Asks where the user is in their journey (pre/during/post-interview, or "I need help figuring out where to start")
  2. Collects a lightweight profile: experience level, target role type (MLE/research engineer/research scientist), target companies
  3. Routes to the appropriate skill(s)
  4. Can chain skills for multi-step sessions (e.g., resume → cold outreach → company research → interview prep)
  5. Passes session profile context to downstream skills so they don't re-ask for it
- **Data strategy:** Delegates to individual skills.

---

## Shared Conventions

All skills follow these conventions for consistency:

1. **Experience level awareness** — Each skill asks for (or inherits from orchestrator) the user's experience level: `new_grad`, `mid` (2–5 yrs), `senior` (5+ yrs), `staff+`. Adjusts depth of feedback, question difficulty, and comp expectations accordingly.

2. **Session file output** — Skills that produce artifacts (behavioral answers, interview Q&A, resume rewrites, offer comparisons) write a markdown file to the current working directory at session end. Filename convention: `mle_<skill>_<slug>_<YYYY-MM-DD>.md`.

3. **Web search discipline** — Skills that use web search do so once during setup (not mid-session), following the pattern established in `mle_interview`.

4. **Skill naming** — All skills prefixed with `mle_` for namespace clarity.

---

## Repo Structure

```
mle_interview_skill/
├── SKILL.md                          # mle_interview (existing)
├── skills/
│   ├── mle_resume/SKILL.md
│   ├── mle_cold_outreach/SKILL.md
│   ├── mle_intro/SKILL.md
│   ├── mle_company_research/SKILL.md
│   ├── mle_behavioral/SKILL.md
│   ├── mle_negotiate/SKILL.md
│   ├── mle_offer_compare/SKILL.md
│   └── mle_career/SKILL.md           # orchestrator
└── docs/
    └── superpowers/specs/
        └── 2026-04-12-mle-career-skills-design.md
```

---

## Out of Scope

- Job board scraping or application tracking
- Coding interview practice (LeetCode-style) — not specific to MLE roles
- Interview scheduling or calendar integration
