# MLE Interview Agent — Design Spec
**Date:** 2026-04-02  
**Status:** Approved

---

## Overview

A Claude Code skill (`/mle_interview`) that conducts adaptive mock interviews for ML/AI engineers. The user picks a topic, mode, and number of questions. Claude generates fresh questions via web search, adapts difficulty based on answer quality, and saves a full session report to markdown.

---

## Invocation

- Skill name: `mle_interview`
- Installed at: `~/.claude/skills/mle_interview/SKILL.md`
- Triggered via: `/mle_interview`

---

## Setup Phase

On invocation, Claude prompts the user for:

1. **Topic** — e.g., "transformer architecture", "RLHF", "RAG pipelines", "distributed training", "CUDA kernels"
2. **Mode** — `exam` or `coach`
3. **Number of questions** — integer, default 10

---

## Session Loop

Claude internally tracks two variables throughout the session:
- `difficulty` — integer 1–5 (starts at 2, "easy-medium")
- `question_number` — 1 to N

### Question Generation
For each question, Claude uses web search to find current, relevant material on the topic, then generates a question calibrated to the current difficulty level. Difficulty mapping:
- 1: Definitions and fundamentals
- 2: Conceptual understanding
- 3: Applied reasoning and trade-offs
- 4: System design and edge cases
- 5: Deep implementation, math, research-level

### Exam Mode
1. Display question
2. Wait for user's answer
3. Move to next question (no feedback given)
4. Difficulty adjusts based on answer length/confidence signals Claude infers heuristically

### Coach Mode
1. Display question
2. Wait for user's answer
3. Score answer: **1–10 numeric** + **qualitative feedback** (what was good, what was missing)
4. Show **correct/ideal answer**
5. Adjust difficulty:
   - Score ≥ 7 → increase difficulty (up to 5)
   - Score ≤ 4 → decrease difficulty (down to 1)
   - Score 5–6 → keep difficulty the same
6. Move to next question

---

## Session Output

### Markdown File
After all questions, Claude writes a session file to the **current working directory**:

**Filename:** `mle_interview_<topic>_<YYYY-MM-DD>.md`

**Format:**
```markdown
# MLE Interview — [Topic] — [Date]
**Mode:** coach | **Questions:** 10

---

## Q1 (Difficulty: 2/5)
**Question:** ...

**Your Answer:** ...

**Score:** 7/10

**Feedback:** [qualitative feedback]

**Correct Answer:** [ideal answer]

---

## Q2 (Difficulty: 3/5)
...

---

## Overall Summary

### Strengths
- ...

### Areas to Improve
- ...

### Recommended Study Topics
- ...
```

*In exam mode, the Score / Feedback / Correct Answer fields are omitted per question. The Overall Summary section is still written.*

### Terminal Summary
After writing the file, Claude prints the Overall Summary section directly to the terminal so the user sees it immediately.

---

## Difficulty Adaptation Details

- Difficulty is a single integer, not exposed to the user mid-session (they can infer from question complexity)
- In coach mode: adjusted after every question based on numeric score threshold
- In exam mode: adjusted heuristically (e.g., very short/uncertain answers → easier, detailed answers → harder) — less precise but keeps the session dynamic
- Difficulty never goes below 1 or above 5

---

## Scope / Out of Scope

**In scope (v1):**
- LLM/AI engineering topics
- Exam and coach modes
- Adaptive difficulty
- Web search for question generation
- Session markdown report
- Terminal summary at end

**Out of scope (v1):**
- Cross-session history or weak-topic tracking
- Leaderboards, scoring trends over time
- Non-LLM/AI topics
- Voice or GUI interface
