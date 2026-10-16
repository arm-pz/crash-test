# crash-test

Crash-test a decision before you build it. Four fixed roles — Pragmatist, Skeptic, Specialist,
Contrarian — get the same task in separate contexts, review each other's drafts anonymously as Draft
A/B/C, and then the main agent chairs: it **merges** what survived, keeps a **dissent log** of what it could not resolve, and runs a **grounding pass** over the winner's claims before adopting them. An optional fifth seat, the **Operator**,
reviews cost, rollout, reversibility and maintenance for anything you'll have to run six months from
now. A lighter **plan-debate** mode runs Skeptic against Contrarian on a plan for about 5 calls.

No server, no API keys, no other agent CLIs, nothing to install with pip. One `SKILL.md` that runs
inside the agent you already use.

Install:

```
npx skills add arm-pz/crash-test --skill crash-test
```

Ask it things like:

- "crash test this: should we split the app into two services?"
- "get a second opinion on this migration plan"
- "debate this plan before I commit to it"
- "I don't trust this bug hypothesis, review it from multiple angles"
- "run the panel with the operator — this is a pricing change"

## How it works

| Step | What happens | Calls |
|---|---|---|
| Draft | Four roles answer the identical task alone, each under ~400 words | 4 |
| Cross-review | Each reviews the other three as Draft A/B/C, naming the strongest idea and the most serious flaw **with evidence** | 4 |
| Chair | Adopt, merge, or say none is good enough. Unevidenced objections go to the dissent log, not the answer | 1 |
| Operator (opt-in) | Runs on the chair's candidate answer, before it is finalised: cost, rollout order, reversibility, maintenance | +1 |

Before the chair adopts an answer about code you already have, it runs a **grounding pass**: every
claim the winner treats as given (a path, a function, a count, a default) gets checked against the
source and marked confirmed, wrong or unverifiable. A wrong claim must be fixed before the answer
ships. In its first live run, that pass found a wrong claim made by one of the four members.

Every run reports the same sections: Answer, Operating it (if the Operator ran), What the panel changed,
Rejected and why, Grounding, Dissent log, Confidence — so runs are comparable and the interesting part is the diff, not the prose.

Two knobs if you want them: drop role cards in a `crash-test-roles/` folder to replace a member or
fill the fifth seat, and where your host allows it, give each member a different model.

## What it is not

The four members are one model in four contexts, so they are independent in context, **not** in
training. This is a cheap pressure-test, not a substitute for a panel built from different vendors.
Where your host lets you pick a model per sub-agent, you can raise the independence yourself.

Unrelated to karpathy/llm-council (one prompt to several *models*, its own web UI), ktaletsk/council
(ensemble *code* review across model CLIs), holochain/wind-tunnel (load testing) and the various
"council", "review panel" and "four voices" skills already listed in the ecosystem. Those ideas are
good; this one needs no infrastructure at all, and adds an evidence rule, a dissent log and a grounding pass over the winner.

## Siblings

[`do-better`](https://github.com/arm-pz/do-better) is the heavy version of the same instinct: up to
100 sub-agents, randomised strategy cards, a single-elimination bracket and a scored champion. Use
`crash-test` as the everyday check, `do-better` when one answer has already failed you.

## Install options

```bash
git clone https://github.com/arm-pz/crash-test.git
cp -r crash-test/skills/crash-test ~/.qoder/skills/     # Qoder
cp -r crash-test/skills/crash-test ~/.claude/skills/    # Claude Code
```

Needs an agent that can spawn sub-agents. Without one, the roles run sequentially in a single
context and every report is marked "Independence: shared context".

## License

MIT. See `LICENSE`.

The idea of verifying a synthesis against the source before adopting it was seen in
[STRML/cc-debate](https://github.com/STRML/cc-debate). No code or text from that project is copied
here; the grounding pass in `skills/crash-test/SKILL.md` was written for this skill.
