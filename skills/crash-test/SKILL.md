---
name: crash-test
description: >-
  Crash-tests a decision before you build it. Four sub-agents (Pragmatist, Skeptic,
  Specialist, Contrarian) draft the same task in separate contexts, attack each other's
  drafts anonymously, and a chair merges what survived, logs unresolved dissent, and
  checks the winner's claims about existing files against the source before adopting it.
  Use when the user says "crash test this", "second opinion", "council", "review panel",
  "pressure-test this", "I don't trust this answer", or faces a costly decision
  (architecture, plan, positioning, debugging hypothesis). Lighter plan-debate mode
  (Skeptic vs Contrarian, ~5 calls) for a verdict before committing. Optional Operator
  seat reviews cost, rollout and reversibility (+1 call). Custom role files and
  per-member models supported. About 9 calls, no API keys, no server. Not for factual
  lookups, small edits, debugging an actual crash, or reviewing code for defects: it
  examines a decision, not a diff.
---

# Crash test

A single draft inherits the blind spots of whoever wrote it. This gets four deliberately different drafts, makes them attack each other, and merges what survives. Ideas win on evidence, and the chair is told to say so when no draft is good enough.

## When to use it

Use it when the cost of a wrong answer is higher than the cost of ~9 calls: design and architecture choices, plans, strategy and positioning, ambiguous debugging, approach selection before a big code change, anything the user has already said "feels off" about.

Do not use it for factual lookups, formatting, small edits, tasks with one obviously correct answer, or reviews that scan code for defects (a panel of four re-reading the same diff adds cost, not coverage). If the task is that simple, answer it directly and say a run wasn't needed.

## The four members

Each member gets the same task, word for word, plus exactly one role card. Fixed roles keep results predictable and make the dissent log readable.

| Member | Role card (give this verbatim) |
|---|---|
| **Pragmatist** | Find the simplest answer that fully solves the task. Cut anything not needed. Prefer boring, proven approaches over clever ones. |
| **Skeptic** | Assume the obvious approach fails. Find the hidden assumption, edge case or cost that breaks it, then produce the answer that survives it. |
| **Specialist** | Answer as the deepest domain expert would: correct terminology, real constraints, known pitfalls. Prefer accuracy over elegance. |
| **Contrarian** | Question the framing. Start from the opposite assumption, or reframe the problem, and produce the best answer that follows. |

## The Operator (optional fifth seat)

Add the Operator when the answer has to be run, paid for, supported and changed by someone six months from now — rollouts, migrations, pricing changes, staffing plans, anything with an operational afterlife. Suggest it in one line when the task is a plan rather than a question; never add it silently, because it costs calls.

The Operator does not draft and is not cross-reviewed, so it is the one voice nobody attacks. It runs **after** the chair produces a candidate answer and **before** the chair finalises it: chair candidate → Operator report → chair final answer. Give it the candidate and the four drafts. It returns:

- **Cost** — what this buys in money, time and attention, ongoing.
- **Rollout** — who has to do what for this to exist, and in which order.
- **Reversibility** — how to undo it, and how much that costs.
- **Maintenance** — what quietly degrades if nobody tends it.

Its role card: *"You are the person who has to run this in six months. Assume it ships and works. Report the cost, the rollout steps, how to reverse it, and what you will be maintaining. Flag anything the panel treated as free."*

The same evidence rule binds it: an Operator objection with no concrete failure behind it goes to the dissent log, not the answer. A run with the Operator is about 10 calls. Keep the default at four so the cost stays predictable.

**Why the Operator does not draft.** Cross-review scales as n×(n−1). A fifth member that drafts and is reviewed by four others is 5 drafts + 20 reviews + 1 chair ≈ 26 calls — nearly triple, for one more angle. The review-only seat buys the same coverage for 1 call, and five drafting members converge on the same answer anyway.

### Custom role files

If the user names a role file, or the project has a `crash-test-roles/` folder, use each file's contents verbatim as a role card. A custom card can replace a default member or fill the Operator seat. Never run more than one drafting seat beyond the four defaults: past five the drafts converge and cost climbs without adding new angles.

### Different models per member (optional)

If the host can assign a model per sub-agent, give the members different models for real diversity, since four copies of one model share its blind spots. Say so in the report (`Independence: separate contexts, mixed models`). One model for everyone is the default and is fine.

## Workflow

### 1. Frame the task
Restate the task in two or three sentences, including constraints and what a good answer looks like. Include any files, context or numbers the members need, because they will not see the conversation. If the task is genuinely ambiguous, ask one clarifying question first; otherwise proceed with stated assumptions.

### 2. Independent drafts
Spawn four sub-agents in parallel, one per role. Each receives: the framed task, its role card, and these rules:
- Work alone. You have not seen and will not see the other drafts.
- Keep the draft under ~400 words unless the task needs more.
- Mark anything you are unsure of as an assumption. Never invent facts, figures or sources.

Independence is the whole value of this step. Do not let drafts see each other yet.

### 3. Cross-review
Give each member the other three drafts, relabelled **Draft A, B, C** with no role names (role labels cause deference). For each draft, the reviewer returns:
- **Strongest idea** worth keeping
- **Most serious flaw**, or "none found"
- **Evidence** for the flaw: a concrete failure, not a taste difference

Then one line: *"After reading these, would you change your own draft? How?"*

### 4. Chair synthesis
You are the chair. You are not a fifth member and you do not vote. Read all four drafts and all twelve reviews, then choose exactly one:
- **Adopt** one draft that survived review, with small fixes.
- **Merge** the strongest surviving ideas into one answer. Resolve conflicts by weighing claims against the task and constraints, not by counting which member agreed.
- **None good enough.** Say so, explain why, and suggest a sharper reframing or escalation (see below).

That choice is a **candidate** answer. If the Operator seat is running, send it the candidate plus the four drafts, then finalise with its report in hand; otherwise finalise now. Either way the candidate is not shown to the user as the answer.

A flaw only counts if the reviewer gave concrete evidence. A flaw that was raised but not evidenced goes in the dissent log rather than silently shaping the answer.

**Grounding pass.** When the task refers to existing files, code, config or data, check the claims the winning answer treats as given before you adopt it: a path, a function name, a schema, a count, a default. Open the source and mark each claim **confirmed**, **wrong** or **unverifiable**, with a file and line where you can. A wrong claim must be fixed before the answer ships; an unverifiable one is stated as an assumption inside the answer. Drafts written from memory carry assertions about the current state that nobody re-derives, so this is the cheapest way to catch them. Skip it when the task is purely conceptual.

### 5. Report
Use this exact structure so results are comparable across runs:

```
# Crash-test result
**Task:** one line
**Decision:** Adopted Draft X / Merged / None good enough
**Independence:** separate contexts (sub-agents) / separate contexts, mixed models / shared context (sequential)

## Answer
The final answer, ready to use.

## Operating it
Only when the Operator ran. Cost, rollout order, reversibility and maintenance the final answer has to carry. Omit otherwise.

## What the panel changed
How the final answer differs from the first drafts, and which review caught it.

## Rejected and why
Ideas dropped, each with the evidenced flaw that killed it.

## Grounding
Only when the task refers to existing files, code or data. Each checked claim marked confirmed / wrong / unverifiable, with file:line where known. Omit otherwise.

## Dissent log
Disagreements the chair could not resolve. Name the role that held each side.

## Confidence
High / medium / low, plus the one thing that would raise it.
```

Keep it short. A reader should get the answer in the first screen and the reasoning only if they want it.

## Plan debate mode (lighter, ~5 calls)

Use this instead of the full workflow when the user brings a plan they are about to act on ("debate this plan", "crash test my plan", "should we do X?") and wants a verdict, not four full drafts. Only two members take part: the Skeptic and the Contrarian, with the same role cards as above. Add the Operator for a rollout-shaped plan (~6 calls).

1. **Frame the plan.** State the goal, constraints, and what has already been decided, in a few sentences.
2. **Openings, in parallel.** The Skeptic lists the top three ways the plan fails, each with a concrete trigger and what it would cost. The Contrarian makes the strongest case for a *different* plan, built on the opposite assumption or a reframing, not just a negation of the original.
3. **Rebuttals.** Each side reads the other's opening, relabelled **Position A / B**, and answers: which points land, which do not, and the evidence for each.
4. **Chair verdict.** Pick one: **Proceed**, **Proceed with changes** (list them), **Switch to the alternative**, or **Need information** (name the single question that would settle it). Neither side wins on rhetoric; a point counts only if it has concrete evidence.

Report with the same structure as step 5, where **Answer** is the verdict and the **Dissent log** holds what the rebuttals left unresolved. If the verdict is "Need information" or confidence is low, offer to run the full panel.

## When sub-agents are not available

Some agents cannot spawn sub-agents. Then run the roles sequentially in one context:
- Write all four drafts before reading any review, each in its own labelled section, as if the others did not exist.
- Do the cross-review with Draft A, B, C labels, not role names.
- Set **Independence: shared context** in the report. Independence is weaker because one model holds all four drafts, so tell the user and lower confidence by one level.

## Escalate when the panel is stuck

If the decision is "None good enough", or confidence is low with a large unresolved dissent log, say so and suggest a bigger run (a larger tournament-style skill such as do-better if installed), or ask the user the one question whose answer would settle the dissent. Do not fake a clean answer.

## Principles

- **Diversity before debate.** Four drafts that all agree usually mean the roles were not followed. If so, rerun the weakest-differentiated member with its role card repeated at the top.
- **Evidence over eloquence.** Judge claims on whether they hold against the task, not on how confidently they are written.
- **No invented facts.** Members and chair mark uncertainty instead of filling gaps. Never fabricate metrics, quotes or sources.
- **Honest about context.** Four members are one model in four contexts: independent in context, not in training — say so when confidence is claimed, and use mixed models where the host allows it.
- **Honest about cost.** About nine calls for a full run, ten with the Operator, five for a plan-debate verdict, six with the Operator; the grounding pass adds a few file reads. Mention this if the user seems surprised by the wait.
