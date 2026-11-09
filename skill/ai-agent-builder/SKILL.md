---
name: ai-agent-builder
description: Use when the user wants to build, design, improve or review an AI agent or automated workflow, or verify an AI output with reviewer and judge sessions.
---

# AI Agent Builder

A lean method for building AI agents: give access instead of copy-paste, define what good looks like, and never let a session grade its own work.

Work with the user one step at a time. Before asking questions, reuse what the user has already said.

## 0. Core belief

**Don't be the conveyor belt.** If a human hand-carries data into the AI and says "please fix this", the human is the bottleneck.

1. Give the agent **access** (tools + data), not hand-picked slices.
2. Tell it **what good looks like**, not who to pretend to be.
3. **Never let one session grade its own work.**

## 1. What gets built

- **This skill holds the method.** Written once.
- **One Project per work item.** That Project is the "agent". Its instructions hold only the Agent Brief (P1, about 10–20 lines).
- **Zero fixed role agents.** No "Research Agent / Writer Agent / Editor Agent".
- **The main session is the orchestrator.** For each task it decides how to split the work, opens temporary sub-sessions (Maker, Attackers, Judge), then closes them. The split is decided fresh every time, so nothing needs redesigning when models improve.
- Where sub-agents are available, run the review loop automatically. Otherwise, give the user the prompts to paste into new chats.

## 2. Pick the right task

| Bucket | Meaning | Example |
|---|---|---|
| **Automation** | Removes boring, expensive, repeated work | Weekly report, invoice processing |
| **Augmentation** | Does something the user couldn't do before | 3D render, data analysis, a website |

- **Aim for the real outcome**, not the steps. If the goal is small, raise it with P2.
- If the user cannot describe a good result, do Section 4 first.

## 3. Method, not costume

- ❌ "You are an excellent senior accountant. Review my numbers." This adds vocabulary, not skill.
- ✅ "Review these numbers using the steps, tools and checks a professional accountant would use. List the steps first, then do them. Use real tools, not mental math."
- One strong generalist model + good tools beats a cast of narrow specialist agents.

## 4. Define "good"

- **Weather test:** a guessed temperature that happens to be right = **fail**. Off by 0.5° but right location and a reputable source = **pass**.
- **Good = correct result + trustworthy process.** A guessed detail counts as a failure.
- For taste work: ask for several variations, ask for the strongest counter-argument, and judge by whether a real user can use it quickly without explanation.

## 5. Access through tools

- Give well-documented tools, one line per tool describing the data behind it. Let the model decide what it needs. No data dumps.
- **Browser first, connectors later.** Add a connector only when the agent repeats the same clicks every day. Add schedules only after the workflow works.
- **Approve the irreversible.** Sending, submitting, paying and deleting wait for the user's approval.
- **Skills expire.** After a model update, A/B test the same task with and without old skills or long prompts. Keep only what clearly helps.

## 6. No self-judging: Maker → Attackers → Judge

```
MAKER ──output──▶ ATTACKERS ×1–5 ("prove it wrong")
  ▲                        │ criticisms
  │                        ▼
  └──valid feedback only── JUDGE ("is each criticism valid?")
```

- **Maker:** does the task with the professional process and the definition of done.
- **Attackers (P3):** fresh, parallel sessions whose only job is to prove it wrong. A smaller model is fine.
- **Judge (P4):** keeps only valid, actionable criticism.
- **Maker revision (P5):** decides what to act on; may reject points with reasons.
- **Stop signal:** attacker feedback becomes "reasonable but misses the point".
- **Prefer hard evidence:** passing tests, primary sources, real metrics, real users.

Match effort to risk:

- Low stakes → Maker + P8 self-check.
- User knows what good looks like → their judgment may be enough.
- Facts and numbers → one P6 fact-check.
- Important output (reports, money, legal, production code) → P3 → P4 → P5. Start with 1 attacker; add more if mistakes slip through.

## 7. Turn feedback into rules

1. **Collect** every piece of feedback the user has given.
2. **Condense** it into a principles list.
3. **Surface hidden beliefs:** "Which principles do I follow but never wrote down?"

Add the list to the Agent Brief and to attacker and judge prompts.

## 8. Side by side before handover

1. For 3–5 rounds, the user reviews as usual **and** the agent reviews the same item.
2. The user writes their feedback before reading the agent's.
3. Compare; fix the brief and principles where they differ.
4. Hand over when differences are small. Humans are inconsistent too.

## 9. Prototype wide, choose narrow

Building is cheap; human attention is scarce. Build many variants, run the review loop, and show only the best 1–3.

## Procedure

1. Pick one work item and its real outcome.
2. Write the Agent Brief with P1. Ask only for what is missing.
3. Run it once now. No big plans or setup first.
4. Review: P3 → P4 → P5. For facts and numbers, P6 alone is enough.
5. Side-by-side rounds (Section 8) and add principles (Section 7).
6. Connect and schedule once results are reliable.
7. Maintain: retest after model updates; raise the goal one level when it works.

## Prompts

### P1 Agent Brief
```text
Goal: [the real outcome, not the steps]
Type: Automation / Augmentation
Method: First list the tools and steps a professional would use for this. Then use the same tools and steps. Do not role-play.
Priorities: [ordered list]
Tools and data:
- [tool]: [what data it holds]
Good result: [criteria]. A guessed detail counts as a failure. Every factual claim links to a primary source.
Fail: [always-wrong cases]
Done when: [stop condition]
Review loop: [none / P6 / P3 ×N → P4 → P5], max 2 rounds
Needs my approval: [irreversible actions]
Never: [hard limits]
Principles: [added after side-by-side rounds]
Hand-over status: side-by-side round [n] / trusted
```

### P2 Raise the goal
```text
I need a deeper version of this task, not a bigger one. Before executing, answer:
1. What outcome do I actually want from this?
2. What would the best practitioner do that I am not asking for?
3. What tools or raw data should you have access to?
4. What outside evidence would prove the result is good?
Then rewrite my goal and wait for my approval.
Task: [task]
```

### P3 Attacker
```text
Below is an output made by another session. Your only task is to prove it is wrong: wrong facts, wrong reasoning, wrong approach, misunderstood the question, missed points, hidden assumptions. Rank each issue Critical / Major / Minor with concrete evidence or a counter-example.
Principles to check against: [principles list]
[output]
```

### P4 Judge
```text
Below is an output and criticisms from several reviewers. For each criticism decide: valid and actionable / valid but not actionable / false positive, with one line of reasoning. Return only the valid, actionable points.
Output: [paste]
Criticisms: [paste]
```

### P5 Maker revision
```text
Here is validated feedback on your output. Decide which points to act on. You may reject a point if it is wrong or misses the purpose; say why. Then revise.
[judge result]
```

### P6 Fact-check
```text
Find a primary source for every number and factual claim in this document. Check each source is reputable. Flag anything unsupported. Output: claim | source | verified? | note.
[document]
```

### P7 Challenge an assumption
```text
We currently believe: "[belief]". Do not accept this as fact. Using the data you can access: 1) try to disprove or reproduce it, 2) find the actual cause with evidence, 3) list blind spots or early signals, 4) output "Our belief" vs "What the data shows".
```

### P8 Low-stakes self-check
```text
Before the final answer: restate the goal and constraints; solve step by step; list 2 assumptions that would make this wrong; list outside evidence that would confirm or disprove it.
```
For high-stakes work (money, legal, production code) use P3 → P4 → P5 instead.
