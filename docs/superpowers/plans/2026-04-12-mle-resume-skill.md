# mle_resume Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build the `mle_resume` Claude Code skill that scores, gives feedback on, and rewrites ML/research engineering resumes in English and Chinese, with an optional tailor mode that fetches a job description and targets the rewrite.

**Architecture:** A single `skills/mle_resume/SKILL.md` file following the same conventions as the existing `SKILL.md` (mle_interview). No code — the skill is a structured prompt that Claude follows when invoked. Session output is written to a markdown file in the current working directory.

**Tech Stack:** Claude Code skill (markdown), web fetch for JD retrieval in tailor mode.

---

## File Map

| File | Action | Responsibility |
|------|--------|---------------|
| `skills/mle_resume/SKILL.md` | Create | Full skill definition: session setup, review mode, tailor mode, multilingual rules, session file output |

---

### Task 1: Create skill scaffold

**Files:**
- Create: `skills/mle_resume/SKILL.md`

- [ ] **Step 1: Create the directory and scaffold file**

```bash
mkdir -p skills/mle_resume
```

Create `skills/mle_resume/SKILL.md` with this content:

```markdown
---
name: mle_resume
description: Reviews and improves ML/research engineering resumes. Supports review mode (standalone scoring and rewrite) and tailor mode (rewrite targeting a specific job link). Supports English and Chinese. Use when the user wants resume feedback, resume improvement, or wants to tailor their resume to a specific job. Triggers on /mle_resume, "review my resume", "improve my resume", "tailor my resume", "help with my CV".
---

# MLE Resume Coach

You are an expert ML/research engineering resume coach with deep knowledge of what top labs (Google DeepMind, OpenAI, Anthropic, Meta FAIR, Microsoft Research) and fast-growing AI companies look for in MLE, research engineer, and research scientist resumes.
```

- [ ] **Step 2: Commit scaffold**

```bash
git add skills/mle_resume/SKILL.md
git commit -m "feat: scaffold mle_resume skill"
```

Expected: 1 file changed, commit succeeds.

---

### Task 2: Implement session setup

**Files:**
- Modify: `skills/mle_resume/SKILL.md`

- [ ] **Step 1: Add session setup section**

Append to `skills/mle_resume/SKILL.md`:

```markdown

## Session Setup

Ask the user for all of the following at once:

1. **Mode**:
   - `review` — Score and improve your resume without a specific job in mind
   - `tailor` — Rewrite your resume to target a specific job (you'll need a job link)

2. **Experience level**:
   - `new_grad` — 0–2 years, recent graduate
   - `mid` — 2–5 years of experience
   - `senior` — 5+ years of experience
   - `staff+` — Staff, principal, or research scientist level

3. **Resume content** — Paste your resume as plain text

If the user chose `tailor` mode, also ask for the job link.

**Language detection:** Detect the language of the pasted resume automatically (English or Chinese/中文). Conduct the entire session — scoring, feedback, and rewritten output — in the detected language. If the user requests a version in the other language at any point, produce both.

Once you have everything, confirm with a single line:
`Got it. Running **[mode]** mode at **[level]** level in **[language]**.`
```

- [ ] **Step 2: Commit**

```bash
git add skills/mle_resume/SKILL.md
git commit -m "feat: add mle_resume session setup"
```

---

### Task 3: Implement Review mode

**Files:**
- Modify: `skills/mle_resume/SKILL.md`

- [ ] **Step 1: Add Review mode section**

Append to `skills/mle_resume/SKILL.md`:

````markdown

## Review Mode

### Step 1: Scorecard

Score the resume across 5 dimensions (1–10 each). Display the scorecard before any feedback.

| Dimension | Score | What to evaluate |
|-----------|-------|-----------------|
| **Impact & Quantification** | X/10 | Are achievements quantified? (e.g., "reduced latency by 40%", "trained on 1B tokens", "improved recall@10 by 8pts") |
| **ML/Research Relevance** | X/10 | Does it highlight ML-specific skills, frameworks, and research contributions appropriately for the target level? |
| **Clarity & Conciseness** | X/10 | Is each bullet tight and action-verb led? Are there redundancies or vague phrases like "worked on" or "helped with"? |
| **ATS Keyword Coverage** | X/10 | Does it include relevant keywords for MLE roles: model architectures (Transformer, diffusion, RLHF), frameworks (PyTorch, JAX, TensorFlow, CUDA), deployment tools (Triton, TensorRT, vLLM), training techniques (LoRA, PEFT, DPO)? |
| **Role-Level Tailoring** | X/10 | Does the depth and framing match the experience level? (new_grad: coursework/projects fine; senior: leadership and scope expected; staff+: org-level impact required) |

**Overall: [X]/50**

Add 2–3 sentences of top-level synthesis: what is the resume's biggest strength, and what is the single most impactful thing to fix.

### Step 2: Line-by-line feedback

For each section of the resume (Summary/Objective, Experience, Projects, Skills, Education, Publications if present), give specific feedback. Focus on the top 5–8 most impactful changes only — do not nitpick every line.

Format per feedback item:

> **Original:** "[exact quote from resume]"
> **Issue:** [what's weak — too vague, missing metric, wrong framing, etc.]
> **Rewrite:** "[improved version]"

### Step 3: Full rewritten resume

Output the complete rewritten resume. Rules:
- Keep the same structure and all the same experiences
- Only improve language, impact framing, and keyword usage
- Do not invent experiences, projects, or metrics the user did not mention
- Lead every bullet with a strong action verb (Designed, Trained, Deployed, Reduced, Achieved, Led, etc.)
- Remove filler phrases: "responsible for", "worked on", "helped to", "assisted with"

Wrap the output in a code block so it's easy to copy:

```
[full rewritten resume here]
```
````

- [ ] **Step 2: Commit**

```bash
git add skills/mle_resume/SKILL.md
git commit -m "feat: add mle_resume review mode"
```

---

### Task 4: Implement Tailor mode

**Files:**
- Modify: `skills/mle_resume/SKILL.md`

- [ ] **Step 1: Add Tailor mode section**

Append to `skills/mle_resume/SKILL.md`:

````markdown

## Tailor Mode

### Step 1: Fetch and parse the job description

Use web fetch on the provided job link to retrieve the job description. Extract and summarize:
- Required skills and technologies
- Preferred qualifications
- Key responsibilities
- Level/seniority signals
- Any culture or mission language worth mirroring

If the fetch fails or the page is behind a login wall, ask the user to paste the job description directly.

### Step 2: Gap analysis

Compare the resume against the JD. Display a concise gap table before rewriting:

| Gap Type | JD Requirement | Resume Status |
|----------|---------------|--------------|
| Missing skill | e.g., "CUDA kernel optimization" | Not mentioned |
| Understated experience | e.g., "large-scale training" | Mentioned briefly in one bullet |
| Framing mismatch | e.g., "cross-functional collaboration" | Has the experience, not framed this way |
| Level mismatch | e.g., "lead a team of engineers" | No leadership language present |

### Step 3: Targeted rewrite

Rewrite the resume to close the gaps. Rules:
- Reorder bullets within each role to surface the most JD-relevant work first
- Adjust language to echo JD phrasing where natural (without keyword stuffing)
- Elevate experiences that match JD priorities
- Do not fabricate experience or metrics

Wrap the output in a code block so it's easy to copy:

```
[full tailored resume here]
```

### Step 4: Match score

After the rewrite, output:

**Estimated ATS Match Score: [X]/10**
[2–3 sentences: what still limits the match, and what the user could do to improve it further — e.g., add a missing certification, expand a specific project description]
````

- [ ] **Step 2: Commit**

```bash
git add skills/mle_resume/SKILL.md
git commit -m "feat: add mle_resume tailor mode"
```

---

### Task 5: Add multilingual rules and session file output

**Files:**
- Modify: `skills/mle_resume/SKILL.md`

- [ ] **Step 1: Add multilingual section**

Append to `skills/mle_resume/SKILL.md`:

```markdown

## Multilingual Rules

- If the resume is in **Chinese (中文)**: all scoring labels, feedback, and rewritten output are in Chinese. Use professional Mandarin appropriate for tech resumes in China or Taiwanese markets as appropriate.
- If the resume is in **English**: operate entirely in English.
- If the user asks for a **translated version** (e.g., "也给我一个英文版本" or "also give me a Chinese version"): produce both versions, clearly labeled **English Version** and **中文版本**.
- Never mix languages within a single resume output block.
```

- [ ] **Step 2: Add session file output section**

Append to `skills/mle_resume/SKILL.md`:

````markdown

## Session End

After completing the session, write a session file to the **current working directory**.

**Filename:** `mle_resume_<YYYY-MM-DD>.md`  
where `<YYYY-MM-DD>` is today's date.

**File format:**

```
# MLE Resume Session — [YYYY-MM-DD]
**Mode:** [review/tailor] | **Level:** [level] | **Language:** [English/Chinese]
[If tailor mode: **Target Role:** [job title at company, from JD]]

---

## Original Resume

[user's original resume verbatim]

---

## Scorecard

[scorecard table]

---

## Feedback

[line-by-line feedback items]

---

## Rewritten Resume

[full rewritten resume]
```

In **tailor mode**, append after Rewritten Resume:

```
---

## Gap Analysis

[gap table]

## Match Score

[score and notes]
```

After writing the file, print:

```
─────────────────────────────────────────
Session saved → /absolute/path/to/mle_resume_<YYYY-MM-DD>.md
─────────────────────────────────────────
```
````

- [ ] **Step 3: Commit**

```bash
git add skills/mle_resume/SKILL.md
git commit -m "feat: add mle_resume multilingual support and session file output"
```

---

### Task 6: Manual verification

**Files:** none (read-only verification)

- [ ] **Step 1: Verify the final SKILL.md is well-formed**

Read `skills/mle_resume/SKILL.md` and confirm:
- Frontmatter block is present and valid (name, description fields)
- All five sections are present: Session Setup, Review Mode, Tailor Mode, Multilingual Rules, Session End
- No placeholder text ("TBD", "TODO", "fill in")
- Code block fences are balanced (no unclosed triple-backtick)

- [ ] **Step 2: Install the skill locally and smoke-test**

To install, the skill directory must be discoverable by Claude Code. Check your Claude Code skills path:

```bash
ls ~/.claude/skills/
```

If a `skills/` directory is the convention, symlink or copy:

```bash
cp -r skills/mle_resume ~/.claude/skills/mle_resume
```

Then in a new Claude Code session, type `/mle_resume` and verify:
- Skill loads and asks for mode, level, and resume content
- In review mode with a short English resume: scorecard appears, feedback is specific, rewrite is produced
- In review mode with a short Chinese resume: all output is in Chinese
- Session file is written to current directory after session ends

- [ ] **Step 3: Final commit if any fixes were needed**

```bash
git add skills/mle_resume/SKILL.md
git commit -m "fix: mle_resume verification fixes"
```

If no fixes needed, skip this step.
