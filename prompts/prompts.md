# Prompt Library

Copy-paste prompts for every step of the method. Replace anything in `[brackets]`.

| ID | Prompt | Where to run it |
|----|--------|-----------------|
| P1 | Agent Brief | Project instructions |
| P2 | Raise the goal | Before you start |
| P3 | Attacker | 1–5 fresh chats |
| P4 | Judge | 1 fresh chat |
| P5 | Maker revision | Back in the Project |
| P6 | Fact-check | 1 fresh chat |
| P7 | Challenge an assumption | Any chat with data access |
| P8 | Low-stakes self-check | Same chat |

---

## P1 · Agent Brief

See [`../templates/agent-brief.md`](../templates/agent-brief.md).

## P2 · Raise the goal

```text
I need a deeper version of this task, not a bigger one. Before executing, answer:
1. What outcome do I actually want from this?
2. What would the best practitioner do that I am not asking for?
3. What tools or raw data should you have access to?
4. What outside evidence would prove the result is good?
Then rewrite my goal and wait for my approval.
Task: [task]
```

## P3 · Attacker

Run in 1–5 fresh sessions. A smaller model is fine.

```text
Below is an output made by another session. Your only task is to prove it is wrong:
wrong facts, wrong reasoning, wrong approach, misunderstood the question, missed points,
hidden assumptions. Rank each issue Critical / Major / Minor with concrete evidence or
a counter-example.
Principles to check against: [principles list, if any]

[output]
```

## P4 · Judge

```text
Below is an output and criticisms from several reviewers. For each criticism decide:
valid and actionable / valid but not actionable / false positive, with one line of
reasoning. Return only the valid, actionable points.

Output: [paste]
Criticisms: [paste]
```

## P5 · Maker revision

```text
Here is validated feedback on your output. Decide which points to act on. You may reject
a point if it is wrong or misses the purpose; say why. Then revise.

[judge result]
```

## P6 · Fact-check

```text
Find a primary source for every number and factual claim in this document. Check each
source is reputable. Flag anything unsupported.
Output a table: claim | source | verified? | note.

[document]
```

## P7 · Challenge an assumption

```text
We currently believe: "[belief]". Do not accept this as fact. Using the data you can access:
1) try to disprove or reproduce it,
2) find the actual cause with evidence,
3) list blind spots or early signals,
4) output "Our belief" vs "What the data shows".
```

## P8 · Low-stakes self-check

```text
Before the final answer: restate the goal and constraints; solve step by step; list 2
assumptions that would make this wrong; list outside evidence that would confirm or
disprove it.
```

> For high-stakes work (money, legal, production code), use P3 → P4 → P5 instead of P8.

## Bonus · Feedback to principles

```text
Here is all the feedback I have given on past outputs:
[feedback]

1. Condense it into a short list of principles I care about.
2. List principles I seem to follow but have never written down.
3. Output one combined list I can paste into an Agent Brief.
```
