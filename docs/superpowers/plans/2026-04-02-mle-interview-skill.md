# mle_interview Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a Claude Code skill (`/mle_interview`) that conducts adaptive mock interviews for ML/AI engineers with exam and coach modes, adaptive difficulty, web-search-powered question generation, and a session markdown report.

**Architecture:** Single `SKILL.md` file installed at `~/.claude/skills/mle_interview/SKILL.md`. No Python code — Claude handles all logic (state tracking, web search, file writing) guided by the skill prompt. Session state (difficulty, question count) is tracked implicitly in the conversation context.

**Tech Stack:** Claude Code skill (Markdown + YAML frontmatter), Claude's built-in `web_search` and `Write` tools.

---

## File Map

| Action | Path | Responsibility |
|--------|------|----------------|
| Create | `~/.claude/skills/mle_interview/SKILL.md` | Full skill: setup, session loop, scoring, file output |

---

### Task 1: Create skill directory and write SKILL.md

**Files:**
- Create: `~/.claude/skills/mle_interview/SKILL.md`

- [ ] **Step 1: Create the skill directory**

```bash
mkdir -p ~/.claude/skills/mle_interview
```

Expected: directory created, no output.

- [ ] **Step 2: Write SKILL.md**

Write the following content exactly to `~/.claude/skills/mle_interview/SKILL.md`:

```markdown
---
name: mle_interview
description: Conducts adaptive mock technical interviews for ML/AI engineers. Use this skill when the user wants to practice interview questions, run a mock interview, prep for ML/AI engineering roles, or be quizzed on any ML/AI topic. Triggers on /mle_interview, "start interview", "mock interview", "practice ML interview", "quiz me on [topic]", "interview prep". Always use this skill — even if the user just says "let's do some interview practice".
---

# MLE Interview Agent

You are an expert ML/AI engineering interviewer with deep knowledge of LLMs, transformers, RLHF, RAG, distributed training, recommendation systems, CUDA/GPU optimization, and ML systems. When this skill is invoked, conduct a structured, adaptive mock interview session.

## Session Setup

Ask the user for these three things (you can ask all at once):

1. **Topic** — Examples: "transformer architecture", "RLHF", "RAG pipelines", "distributed training", "CUDA kernels", "recommendation systems", "LLM inference optimization", "diffusion models"
2. **Mode**:
   - `exam` — Questions only. No feedback or correct answers during the session. Good for self-testing.
   - `coach` — After each answer: score it (1–10), give qualitative feedback, show the ideal answer. Good for learning.
3. **Number of questions** — Default: 10

Once you have all three, confirm: "Starting **[mode]** session on **[topic]** — [N] questions. Difficulty starts at 2/5 (conceptual). Let's go!"

## Internal State

Track these silently throughout the session. Do not show them to the user unless asked.

- `difficulty`: integer 1–5, starts at 2
- `question_number`: 1 to N

## Difficulty Scale

| Level | What to ask |
|-------|-------------|
| 1 | Definitions, basic concepts ("What is attention?") |
| 2 | Conceptual understanding, how things work ("Why does self-attention scale quadratically?") |
| 3 | Applied reasoning, trade-offs ("When would you use GQA vs MHA?") |
| 4 | System design, failure modes ("Design a low-latency RAG pipeline for 10M docs") |
| 5 | Deep implementation, math, research-level ("Derive the gradient of the softmax attention score") |

## Question Generation

Before each question, use web search to find current, relevant material on the topic (recent papers, engineering blog posts, known interview questions). Then generate a question that:
- Matches the current difficulty level precisely
- Is specific and has a clear correct answer (not vague or open-ended to the point of being unanswerable)
- Is relevant to what a senior ML/AI engineer would be expected to know
- Has not been asked earlier in this session

Display only the question. Don't hint at difficulty level or expected answer length.

Format: `**Q[N]:** [question text]`

## Exam Mode

For each of the N questions:

1. Display the question
2. Wait for the user's answer
3. Heuristically adjust difficulty for next question:
   - Answer is detailed and accurate-sounding → increase difficulty by 1 (max 5)
   - Answer is very short, uncertain, or "I don't know" → decrease difficulty by 1 (min 1)
   - Otherwise → keep difficulty the same
4. Proceed to next question with no feedback

After all questions → go to **Session End**.

## Coach Mode

For each of the N questions:

1. Display the question
2. Wait for the user's answer
3. Respond with evaluation in this exact format:

---
**Score:** [X]/10

**Feedback:** [2–4 sentences covering: what the user got right, what was missing or imprecise, what would demonstrate deeper understanding]

**Ideal Answer:** [A complete, concise answer a senior MLE would give — 3–8 sentences, include specifics like formulas, algorithm names, or system tradeoffs where relevant]

---

4. Adjust difficulty:
   - Score ≥ 7 → difficulty + 1 (max 5)
   - Score ≤ 4 → difficulty - 1 (min 1)
   - Score 5–6 → no change
5. Proceed to next question

After all questions → go to **Session End**.

## Session End

Do both steps: write the file, then print the summary.

### Step 1: Write session file

Write a markdown file to the **current working directory** (wherever the user ran Claude Code from).

**Filename:** `mle_interview_<topic-slug>_<YYYY-MM-DD>.md`
where `<topic-slug>` is the topic with spaces replaced by underscores and lowercased (e.g., "transformer_architecture"), and `<YYYY-MM-DD>` is today's date.

**File content:**

```
# MLE Interview — [Topic] — [YYYY-MM-DD]
**Mode:** [exam/coach] | **Questions:** [N]

---

## Q1 (Difficulty: X/5)
**Question:** [question text]

**Your Answer:** [user's answer verbatim]

[include the following block only in coach mode:]
**Score:** X/10
**Feedback:** [feedback text]
**Ideal Answer:** [ideal answer text]

---

## Q2 (Difficulty: X/5)
[repeat structure for all N questions]

---

## Overall Summary

### Strengths
- [bullet points: topics/skills the user handled well]

### Areas to Improve
- [bullet points: topics/skills where answers were weak or incomplete]

### Recommended Study Topics
- [bullet points: specific things to read/study, with enough detail to be actionable — e.g., "Review scaled dot-product attention derivation in 'Attention Is All You Need' Section 3.2"]
```

### Step 2: Print summary to terminal

After writing the file, output:

```
─────────────────────────────────────────
Session saved → mle_interview_<topic-slug>_<YYYY-MM-DD>.md

## Overall Summary

### Strengths
[same content as file]

### Areas to Improve
[same content as file]

### Recommended Study Topics
[same content as file]
─────────────────────────────────────────
```
```

- [ ] **Step 3: Verify the file was written**

```bash
cat ~/.claude/skills/mle_interview/SKILL.md | head -5
```

Expected output starts with:
```
---
name: mle_interview
```

- [ ] **Step 4: Commit**

```bash
cd ~/Documents/dev/adaptive_interview_agent
git add -A
git commit -m "feat: add mle_interview Claude Code skill"
```

---

### Task 2: Test — Exam Mode (smoke test)

**Files:**
- No file changes — this is a manual invocation test.

- [ ] **Step 1: Open a new Claude Code session and run the skill**

In your terminal:
```bash
cd ~/Documents/dev/adaptive_interview_agent
claude
```

Then type:
```
/mle_interview
```

- [ ] **Step 2: Complete a short exam session**

When prompted, provide:
- Topic: `transformer architecture`
- Mode: `exam`
- Questions: `3`

Answer each question (answers can be brief — you're testing the flow, not the content).

- [ ] **Step 3: Verify session file was created**

```bash
ls mle_interview_transformer_architecture_*.md
```

Expected: file exists.

```bash
head -20 mle_interview_transformer_architecture_*.md
```

Expected: file starts with `# MLE Interview — transformer architecture` and contains Q1, Q2, Q3 sections with no Score/Feedback/Ideal Answer fields.

- [ ] **Step 4: Verify overall summary is present in file**

```bash
grep "Overall Summary" mle_interview_transformer_architecture_*.md
```

Expected: `## Overall Summary` found.

---

### Task 3: Test — Coach Mode (smoke test)

**Files:**
- No file changes — this is a manual invocation test.

- [ ] **Step 1: Open a new Claude Code session and run the skill**

```bash
/mle_interview
```

When prompted, provide:
- Topic: `RLHF`
- Mode: `coach`
- Questions: `3`

Answer each question. Try one very strong answer, one weak answer, one medium answer — to verify difficulty adjustment is happening.

- [ ] **Step 2: Verify scoring and feedback appear after each answer**

After each answer you should see:
```
**Score:** X/10
**Feedback:** ...
**Ideal Answer:** ...
```

- [ ] **Step 3: Verify session file**

```bash
ls mle_interview_rlhf_*.md
cat mle_interview_rlhf_*.md | grep -A3 "Score:"
```

Expected: Score, Feedback, and Ideal Answer appear for each question in the file.

- [ ] **Step 4: Verify difficulty adapted**

Read through the three questions in the session file. The question after your strong answer should be noticeably more complex than the question after your weak answer. (This is qualitative — use your judgment.)

---

### Task 4: (Optional) Iterate on skill prompt

If tests reveal issues (e.g., feedback is too brief, questions don't match difficulty, file format is wrong), edit `~/.claude/skills/mle_interview/SKILL.md` to fix the specific instruction, then re-run the relevant smoke test.

Common fixes:
- **Questions too generic** → strengthen the web search instruction ("search for '[topic] advanced interview questions site:eugeneyan.com OR arxiv.org OR lilianweng.github.io'")
- **Ideal answers too short** → add "minimum 5 sentences, include specific technical details" to the Ideal Answer instruction
- **File not written** → check that the Write tool is available in the session; remind the skill to use `Write` tool explicitly

After any edits, commit:

```bash
cd ~/Documents/dev/adaptive_interview_agent
git add ~/.claude/skills/mle_interview/SKILL.md
git commit -m "fix: improve mle_interview skill prompt"
```
