# AI Agent Builder

> A lean, practical method for building AI agents you can actually trust.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

Most people still use AI as a fancy autocomplete: copy data in, paste the answer out, repeat. This repo is a system for the opposite: give the agent access, define what *good* means, and never let a single session grade its own work.

## What's inside

| Component | Path | Purpose |
|-----------|------|---------|
| **Claude skill** | [`skill/ai-agent-builder/SKILL.md`](skill/ai-agent-builder/SKILL.md) | Runs the whole method for you, step by step |
| **Prompt library** | [`prompts/prompts.md`](prompts/prompts.md) | Copy-paste prompts P1–P8 for each stage |
| **Agent Brief template** | [`templates/agent-brief.md`](templates/agent-brief.md) | Blank brief to define an agent's goal and done criteria |
| **Worked example** | [`templates/example-brief.md`](templates/example-brief.md) | A filled-in brief to copy from |

## Contents

- [Quick start](#quick-start)
- [The method](#the-method)
  - [The three rules](#the-three-rules)
  - [The review loop](#the-review-loop)
  - [One orchestrator, no fixed roles](#one-orchestrator-no-fixed-roles)
  - [Defining "good"](#defining-good)
  - [Build procedure](#build-procedure)
  - [Matching review effort to risk](#matching-review-effort-to-risk)
  - [Turning your feedback into rules](#turning-your-feedback-into-rules)
- [Reference](#reference)
- [Credits](#credits)
- [License](#license)

---

## Quick start

There is nothing to build and no package to install. Pick the option that matches how you use Claude.

### Option A: Install the skill from the terminal (Claude Code)

**macOS / Linux**

```bash
git clone https://github.com/py-www/ai-agent-builder.git
mkdir -p ~/.claude/skills
cp -r ai-agent-builder/skill/ai-agent-builder ~/.claude/skills/
```

**Windows (PowerShell)**

```powershell
git clone https://github.com/py-www/ai-agent-builder.git
New-Item -ItemType Directory -Force "$HOME\.claude\skills"
Copy-Item -Recurse ai-agent-builder\skill\ai-agent-builder "$HOME\.claude\skills\"
```

Restart Claude Code to load the skill.

> [!TIP]
> To install it for a single project only, copy the folder into that project's `.claude/skills/` instead.
> To update later, run `git pull` in the cloned repo and copy the folder again.

### Option B: Upload the skill in Claude

Upload [`SKILL.md`](skill/ai-agent-builder/SKILL.md) as a skill in Claude.

### Option C: Use it by hand

1. Copy [`templates/agent-brief.md`](templates/agent-brief.md) into a new Claude Project's instructions and fill it in.
2. Run the task in that Project.
3. Paste the output into new chats with the prompts in [`prompts/prompts.md`](prompts/prompts.md): **P3** (attacker) → **P4** (judge) → **P5** (revision).

### First run

With the skill installed (Option A or B), ask something like:

> *"Help me build an agent that prepares my weekly sales report."*

The skill walks you through the brief, the first run and the review loop.

---

## The method

### The three rules

| # | Rule | Why |
|---|------|-----|
| 1 | **Don't be the conveyor belt.** | If you hand-carry data to the AI, *you* are the bottleneck. Give it tools and access. |
| 2 | **Define "good", not a persona.** | "You are an expert X" adds vocabulary, not skill. Ask for the steps and tools a professional would use. |
| 3 | **No self-grading.** | One session judging its own work is unreliable. Use separate attacker and judge sessions. |

### The review loop

![Maker, Attackers, Judge review loop](docs/images/review-loop.svg)

| Step | Role | Job |
|------|------|-----|
| 1 | **Maker** | Does the task using the brief and the tools it has been given. |
| 2 | **Attackers** | 3–5 fresh, parallel sessions with one job: prove the output is wrong. A smaller, cheaper model is fine, because finding flaws is easier than creating. |
| 3 | **Judge** | Reviews every criticism and keeps only what is valid and actionable. |
| 4 | **Maker** | Gets only the validated feedback, decides what to act on, and revises. |

> [!NOTE]
> **Stop signal:** when the attackers can only find points that are "reasonable but miss the point", the output is probably good.
>
> **Prefer hard evidence:** another session is a useful outside signal, but passing tests, primary sources and real user behaviour are better.

### One orchestrator, no fixed roles

![Fixed roles vs dynamic split](docs/images/orchestrator-vs-roles.svg)

You don't build a cast of "Research Agent / Writer Agent / Editor Agent". Instead:

- **One Project per work item.** The Project *is* the agent. Its instructions are a short Agent Brief.
- **The main session is the orchestrator.** For every task it decides how to split the work and opens temporary sub-sessions, then closes them.
- **Nothing to redesign** when models improve. The split is decided fresh every time.

What you write:

```text
1 skill (the method, written once)
   ├─ Agent Brief: weekly sales report
   ├─ Agent Brief: invoice processing
   └─ Agent Brief: market research
```

### Defining "good"

**The weather test.** Ask an agent for the current temperature:

| Result | Verdict |
|--------|---------|
| Guessed a number, and it happened to be right | ❌ **Fail.** The process was unreliable. |
| Off by 0.5°, but used the right location and a reputable source | ✅ **Pass.** The process can be trusted. |

**Good = correct result + trustworthy process.** Every brief includes pass and fail criteria, and a guessed detail always counts as a failure.

For taste-driven work (writing, design) where quality is hard to measure:

- Ask for **several variations** instead of one "make it good" output.
- Ask for the **strongest counter-argument** to your draft.
- Judge by whether a real user can use it quickly, without explanation.

### Build procedure

![Build procedure](docs/images/build-procedure.svg)

| Step | Action |
|------|--------|
| 1. **Brief** | Write the goal and the definition of done ([template](templates/agent-brief.md)). |
| 2. **Run once** | No big plans or setup before the first real run. |
| 3. **Review** | Attackers, then the judge, then the maker revises. |
| 4. **Side by side** | For 3–5 rounds, review the same item yourself *and* let the agent review it. Compare, then fix the brief where you differ. |
| 5. **Automate** | Add connectors and schedules only once results are reliable. |

After each model update, retest without old skills or long prompts, keep only what clearly helps, and raise the goal one level.

### Matching review effort to risk

| Stakes | Example | Review | Prompts |
|--------|---------|--------|---------|
| Low | A quick draft | Maker only, plus a self-check | P8 |
| Medium | Facts and numbers | One fact-check session with primary sources | P6 |
| High | Reports, money, legal, production code | Full loop: Attackers → Judge → Revision | P3 → P4 → P5 |

Start with **one** attacker and add more only if mistakes still slip through.

> [!IMPORTANT]
> **Always ask a human first** before anything irreversible: sending, submitting, paying or deleting.

### Turning your feedback into rules

The last bottleneck is usually the human reviewing everything. Shrink it in three steps:

1. **Collect** every piece of feedback you have given on past outputs.
2. **Condense** it into a list of principles.
3. **Surface hidden beliefs:** ask *"Which principles do I follow but have never written down?"*

Add the list to the Agent Brief and to the attacker prompts. Reviews start to sound like you.

---

## Reference

### Principles at a glance

| # | Principle | Action |
|---|-----------|--------|
| 1 | Don't be the conveyor belt | Give access, not copy-paste |
| 2 | Work items, not roles | One Project per work item; the orchestrator splits work each time |
| 3 | Method, not costume | Ask for the professional's process |
| 4 | Define good | Right answer **and** trustworthy method |
| 5 | Tools over data dumps | Browser first, connectors later |
| 6 | Skills expire | A/B test with vs. without after model updates |
| 7 | No self-grading | Maker → Attackers → Judge |
| 8 | Codify taste | Collect → condense → surface hidden rules |
| 9 | Earn trust | Run side by side before handing over |
| 10 | Attention is scarce | Prototype wide, show only the best few |

### Repo structure

```text
ai-agent-builder/
├── README.md
├── LICENSE
├── .gitignore
├── skill/
│   └── ai-agent-builder/
│       └── SKILL.md          # the method as a Claude skill
├── prompts/
│   └── prompts.md            # P1–P8 copy-paste prompts
├── templates/
│   ├── agent-brief.md        # blank Agent Brief
│   └── example-brief.md      # filled-in example
└── docs/
    └── images/               # diagrams used in this README
```

---

## Credits

The ideas behind this method were inspired by a talk with **Felix Rieseberg** about giving AI bigger goals and checking its work ([video clip](https://www.youtube.com/watch?v=D-SVFneJ_Nk)).

> [!NOTE]
> This repo is an independent write-up. It is not affiliated with or endorsed by Felix Rieseberg or Anthropic.

## License

Released under the [MIT License](LICENSE).
