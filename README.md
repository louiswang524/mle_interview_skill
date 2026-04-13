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

# Copy one skill into your Claude Code skills directory
cp -r mle_interview_skill/skills/<skill_name> ~/.claude/skills/<skill_name>

# Example
cp -r mle_interview_skill/skills/mle_resume ~/.claude/skills/mle_resume
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

### `/mle_cold_outreach`

Cold outreach drafting for ML/AI job searches. You choose:
- **Target** — recruiter, hiring manager, researcher, alumni, founder, or referral contact
- **Channel** — email, LinkedIn DM, or follow-up reply
- **Goal** — referral request, intro call, application follow-up, research collaboration, or direct role interest

The skill produces a primary draft, two variants, and send-timing notes.

### `/mle_intro`

Interview self-introduction coaching. You choose:
- **Target role and company**
- **Interview stage** — recruiter screen, hiring manager, technical loop, executive, networking, etc.
- **Desired length** — 30 seconds, 60 seconds, 2 minutes, or custom

The skill writes a polished “tell me about yourself” answer plus concise and audience-specific variants.

### `/mle_company_research`

Company and role research for interview prep or offer diligence. You choose:
- **Company**
- **Role or job link**
- **Goal** — interview prep, application strategy, networking prep, or offer diligence

The skill summarizes likely role expectations, prep priorities, user-specific talking points, and sources.

### `/mle_behavioral`

Behavioral interview practice and STAR story building. You choose:
- **Mode** — `practice` or `story_builder`
- **Target role / level**
- **Themes** — leadership, conflict, failure, ambiguity, execution, mentoring, etc.

The skill either runs scored behavioral practice or turns rough experiences into reusable STAR answers.

### `/mle_negotiate`

Offer negotiation coaching. You provide:
- **Offer details** — base, bonus, equity, sign-on, level, location, deadlines
- **Other active processes**
- **Negotiation channel** — recruiter call, email, or live conversation

The skill assesses leverage, recommends a strategy, and drafts negotiation language or call scripts.

### `/mle_offer_compare`

Multi-offer comparison for ML/AI roles. You provide:
- **2–5 offers** with comp, level, location, work model, and scope details
- **Your priorities** — cash, upside, scope, research fit, lifestyle, immigration, etc.

The skill compares offers across comp and non-comp dimensions, recommends a top choice, and highlights negotiation opportunities.

### `/mle_career`

End-to-end career coaching and search planning. You provide:
- **Primary goal** — land interviews, convert interviews, negotiate, compare offers, or general strategy
- **Current stage** — resume, applications, networking, interviewing, or offer stage
- **Biggest bottleneck** and any existing materials

The skill diagnoses the search stage, creates a 2-week action plan, and delivers the highest-leverage artifact for that stage.

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

# 将单个技能复制到 Claude Code 技能目录
cp -r mle_interview_skill/skills/<skill_name> ~/.claude/skills/<skill_name>

# 示例
cp -r mle_interview_skill/skills/mle_resume ~/.claude/skills/mle_resume
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

### `/mle_cold_outreach`

面向 ML/AI 求职场景的冷启动沟通文案。你可以提供：
- **联系对象** — recruiter、hiring manager、researcher、校友、founder 或内推联系人
- **渠道** — 邮件、LinkedIn 私信或跟进回复
- **目标** — 求内推、约沟通、申请跟进、研究合作或表达岗位兴趣

该技能会输出主版本文案、两个变体，以及发送和跟进建议。

### `/mle_intro`

“自我介绍 / tell me about yourself” 辅导。你可以提供：
- **目标岗位和公司**
- **面试阶段** — recruiter screen、hiring manager、technical loop、executive、networking 等
- **时长要求** — 30 秒、60 秒、2 分钟或自定义

该技能会产出主版本自我介绍，以及更精简或更偏技术/领导力的变体。

### `/mle_company_research`

用于面试准备或 offer 尽调的公司/岗位调研。你可以提供：
- **公司**
- **岗位或职位链接**
- **目标** — 面试准备、申请策略、关系拓展准备或 offer 尽调

该技能会整理岗位关注点、准备重点、与你背景匹配的 talking points，以及信息来源。

### `/mle_behavioral`

行为面试练习与 STAR 故事打磨。你可以选择：
- **模式** — `practice` 或 `story_builder`
- **目标岗位 / 级别**
- **主题** — 领导力、冲突、失败、模糊问题、执行力、带人等

该技能可以进行带评分的行为面练习，或把零散经历整理成可复用的 STAR 回答。

### `/mle_negotiate`

Offer 谈判辅导。你可以提供：
- **Offer 细节** — base、bonus、equity、sign-on、级别、地点、截止时间
- **其他进行中的流程**
- **沟通渠道** — recruiter 电话、邮件或现场沟通

该技能会评估谈判筹码、给出策略，并起草邮件或通话脚本。

### `/mle_offer_compare`

多 offer 对比决策。你可以提供：
- **2–5 个 offer**，包含薪酬、级别、地点、办公模式和职责范围
- **你的优先级** — 现金、上升空间、scope、研究契合度、生活方式、身份等

该技能会从薪酬和非薪酬维度对比多个 offer，给出推荐，并指出每个 offer 的谈判机会。

### `/mle_career`

端到端求职规划与职业教练。你可以提供：
- **主要目标** — 拿面试、提升面试转化、谈 offer、比较 offer 或整体求职策略
- **当前阶段** — 简历、投递、拓展关系、面试中或 offer 阶段
- **最大瓶颈** 以及现有材料

该技能会判断当前求职阶段，给出两周行动计划，并产出当前阶段最有价值的交付物。

---

## 适用职位

本技能库专为以下职位设计：
- 机器学习工程师（MLE）
- 研究工程师（Research Engineer）
- 研究科学家（Research Scientist）
- 应用科学家（Applied Scientist）
- ML 技术负责人 / Staff / Principal / Director

适用于所有经验层级：应届毕业生至 Staff+ 及以上。
