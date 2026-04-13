# MLE Career Skills

A collection of [Claude Code](https://claude.ai/code) skills covering the full ML/AI engineering job-hunting lifecycle — from resume and cold outreach through interview preparation, offer negotiation, and offer selection.

Skills work independently or chained together via the `mle_career` orchestrator.

---

## Available Skills

| Skill | Trigger | What it does |
|-------|---------|-------------|
| `mle_interview` | `/mle_interview` | Adaptive mock technical interview (transformers, RLHF, RAG, CUDA, RecSys, and more). Exam or coach mode. Saves session file. |
| `mle_resume` | `/mle_resume` | Scores, gives line-by-line feedback on, and rewrites your resume. Review mode (standalone) or tailor mode (target a specific job link). Supports English and Chinese. |
| `mle_cold_outreach` | `/mle_cold_outreach` | Drafts recruiter emails, hiring-manager outreach, networking follow-ups, and LinkedIn DMs for ML/AI roles. |
| `mle_intro` | `/mle_intro` | Coaches your "tell me about yourself" pitch for MLE, research, and applied scientist interviews. |
| `mle_company_research` | `/mle_company_research` | Researches a target company, team, role, interview loop, compensation signals, and talking points. |
| `mle_behavioral` | `/mle_behavioral` | Runs STAR-based behavioral interview practice and story-building for ML/AI job searches. |
| `mle_negotiate` | `/mle_negotiate` | Builds negotiation strategy, counter-offer messaging, and role-play responses for offers. |
| `mle_offer_compare` | `/mle_offer_compare` | Compares 2–5 offers across cash, equity, scope, growth, immigration, and lifestyle dimensions. |
| `mle_career` | `/mle_career` | End-to-end ML/AI career coach that diagnoses your stage and orchestrates the right next deliverables. |

---

## Installation

### Install a single skill

```bash
# Clone the repo
git clone https://github.com/louiswang524/mle_interview_skill.git

# Copy the skill you want to your Claude Code skills directory
cp -r mle_interview_skill/skills/mle_resume ~/.claude/skills/mle_resume
cp -r mle_interview_skill/skills/mle_interview ~/.claude/skills/mle_interview
```

### Install all skills

```bash
git clone https://github.com/louiswang524/mle_interview_skill.git
cd mle_interview_skill

# All skills in skills/
cp -r skills/* ~/.claude/skills/
```

After installation, restart Claude Code and type `/mle_interview`, `/mle_resume`, or any of the other skill names above to get started.

---

## Usage

### `/mle_interview`

Adaptive mock technical interview. You choose:
- **Topic** — transformer architecture, RLHF, RAG, CUDA kernels, distributed training, recommendation systems, etc.
- **Mode** — `exam` (no feedback during session) or `coach` (scored feedback + ideal answer after each question)
- **Number of questions** — default 10

Difficulty adapts 1–5 based on your answers. A session file is saved at the end.

### `/mle_resume`

Resume review and rewrite. You choose:
- **Mode** — `review` (standalone scoring and rewrite) or `tailor` (provide a job link, get a targeted rewrite)
- **Experience level** — `new_grad` / `mid` / `senior` / `staff+`
- **Language** — auto-detected from your resume (English or Chinese/中文)

In tailor mode, the skill fetches the job description, runs a gap analysis, and rewrites your resume to match the role.

---

## Target Roles

These skills are designed for:
- Machine Learning Engineer (MLE)
- Research Engineer
- Research Scientist
- Applied Scientist
- ML Tech Lead / Staff / Principal / Director

All experience levels: new grad through staff+.

---

---

# MLE 求职技能库（中文）

一套 [Claude Code](https://claude.ai/code) 技能集合，覆盖 ML/AI 工程师求职的完整流程——从简历优化、冷启动拓展，到面试准备、薪资谈判与 Offer 选择。

技能可单独使用，也可通过 `mle_career` 编排器串联。

---

## 已上线技能

| 技能 | 触发命令 | 功能说明 |
|------|---------|---------|
| `mle_interview` | `/mle_interview` | 自适应难度模拟技术面试（Transformer、RLHF、RAG、CUDA、推荐系统等）。支持纯练习模式和带反馈的教练模式。面试结束后自动保存记录文件。 |
| `mle_resume` | `/mle_resume` | 对简历进行评分、逐条反馈并重写。支持通用优化模式（独立评审）和定向模式（针对指定职位链接进行改写）。支持中英文简历。 |
| `mle_cold_outreach` | `/mle_cold_outreach` | 撰写发给招聘人员、研究员或 Hiring Manager 的冷启动邮件、跟进邮件和 LinkedIn 私信 |
| `mle_intro` | `/mle_intro` | 辅导“自我介绍”电梯演讲，适配 MLE、研究工程师和应用科学家岗位 |
| `mle_company_research` | `/mle_company_research` | 调研目标公司的面试流程、团队背景、文化和薪资信号，并整理成面试准备要点 |
| `mle_behavioral` | `/mle_behavioral` | 行为面试练习（STAR 方法）+ 故事挖掘模式（引导提问，帮你写出完整 STAR 回答） |
| `mle_negotiate` | `/mle_negotiate` | 薪资谈判策略、反 Offer 邮件起草与角色扮演练习 |
| `mle_offer_compare` | `/mle_offer_compare` | 对比 2–5 个 Offer 的总薪酬和非薪酬维度，给出推荐 |
| `mle_career` | `/mle_career` | 端到端求职教练编排器，诊断当前阶段并串联上述技能 |

---

## 安装方法

### 安装单个技能

```bash
# 克隆仓库
git clone https://github.com/louiswang524/mle_interview_skill.git

# 将所需技能复制到 Claude Code 技能目录
cp -r mle_interview_skill/skills/mle_resume ~/.claude/skills/mle_resume
cp -r mle_interview_skill/skills/mle_interview ~/.claude/skills/mle_interview
```

### 安装全部技能

```bash
git clone https://github.com/louiswang524/mle_interview_skill.git
cd mle_interview_skill

# skills/ 目录下的所有技能
cp -r skills/* ~/.claude/skills/
```

安装完成后重启 Claude Code，输入 `/mle_interview`、`/mle_resume` 或上表中的任意技能命令即可开始使用。

---

## 使用说明

### `/mle_interview`

自适应模拟技术面试，你可以选择：
- **主题** — Transformer 架构、RLHF、RAG、CUDA Kernel、分布式训练、推荐系统等
- **模式** — `exam`（纯练习，不给反馈）或 `coach`（每题评分 + 理想答案）
- **题目数量** — 默认 10 题

难度根据你的回答在 1–5 级之间自适应调整，面试结束后自动保存记录文件。

### `/mle_resume`

简历评审与重写，你可以选择：
- **模式** — `review`（独立评审与重写）或 `tailor`（提供职位链接，针对性改写）
- **经验层级** — `new_grad` / `mid` / `senior` / `staff+`
- **语言** — 根据简历内容自动识别（中文或英文）

定向模式下，技能会自动抓取职位描述，进行差距分析，并将简历改写为与职位高度匹配的版本。

---

## 适用职位

本技能库专为以下职位设计：
- 机器学习工程师（MLE）
- 研究工程师（Research Engineer）
- 研究科学家（Research Scientist）
- 应用科学家（Applied Scientist）
- ML 技术负责人 / Staff / Principal / Director

适用于所有经验层级：应届毕业生至 Staff+ 及以上。
