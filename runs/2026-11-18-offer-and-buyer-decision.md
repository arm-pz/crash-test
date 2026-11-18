# Run record: offer and buyer decision (2026-11-18)

First real-world run of `crash-test` on a business decision rather than code.

## Task

A solo founder running a Dubai AI-automation agency and an open agent-skills library had a 4-agent AI
sales team ready to prospect. The plan sold a $750 custom-skill sprint to small software teams that
publicly use Claude Code, Cursor or Codex. The founder objected: "if they are using Claude, why us?"
The panel had to decide which offer and buyer to pursue first for profit within 3 months, and how to
answer "why us".

## Setup

| Item | Value |
| --- | --- |
| Mode | Full panel, no Operator (suggested in one line, not added) |
| Host | Claude Code (cloud session), lead on Opus |
| Members | Pragmatist: Sonnet; Skeptic: Opus; Specialist: Opus; Contrarian: Sonnet, rerun on Haiku |
| Independence | Separate contexts, mixed models |
| Calls | 4 drafts + 1 rerun + 4 cross-reviews + chair = 10 |
| Subagent tokens | about 681k reported in total: 369k drafts (rerun included), 312k reviews |
| Members' tools | None. Facts were given in the framed task. |

## What happened

1. **All four drafts converged.** Each dropped the developer buyer and recommended the agency's own
   offer (a fixed-fee review leading to a WhatsApp lead-triage pilot) to Gulf SMBs.
2. **The diversity rule fired.** Following "four drafts that all agree usually mean the roles were not
   followed", the chair reran the least differentiated member (the Contrarian) with its role card
   repeated at the top, on a different model. **The rerun converged again.** The chair then treated the
   convergence as real and recorded that the framing, which quoted the founder's objection, may have
   pushed every member the same way.
3. **The cross-review found evidenced flaws the drafts missed.** Each item below names the reviewers
   that caught it:
   - The Haiku rerun used a developer tool (a SKILL.md checker) as the "reason to reply" for clinic and
     brokerage owners. Three of four reviewers caught it, each giving the same concrete reason.
   - Two reviewers worked out that a kill threshold of "zero calls from 50 sends" is noise at a ~5%
     reply rate (2–3 replies expected).
   - The Skeptic's timing argument changed the structure of the answer: review-then-pilot pushes pilot
     cash to weeks 9–13 of a 3-month window. The other reviewers' objection to a pilot-only offer (a
     large commitment from an agency with no case studies) produced the merged "two doors" answer.
4. **Every member said it would change its own draft** after cross-review, and each named the specific
   idea it would take.
5. **The grounding pass was applied to web-sourced facts, not files.** The chair checked offer names,
   durations and the "no published prices or case studies" claim against pages fetched earlier in the
   session (confirmed). It marked platform-rule and price-band claims unverifiable.

## Decision

**Merged**, confidence **medium**. The full report was given to the user in the session.

## What this run says about the skill

| Observation | Evidence | Suggested change |
| --- | --- | --- |
| The rerun rule can't tell real convergence from framing bias | The rerun on a different model converged a second time | When the framed task includes the user's own objection or a preferred answer, say so in the report. Optionally frame it neutrally and run the Contrarian against the consensus in plan-debate mode. |
| Cross-review caught a cross-domain error from the weakest model | 3 of 4 reviewers flagged the same flaw with the same evidence | None. This is the mechanism working. Worth citing in the README. |
| The grounding pass is useful beyond code | Web-sourced claims about the business's own offers were checked against fetched pages | Widen step 4's wording from "files, code, config or data" to include facts the task supplied from sources the chair can re-open |
| The cost was higher than the stated ~9 calls | One rerun took it to 10 calls; ~681k subagent tokens | Mention in "Honest about cost" that a diversity rerun adds one call |
| The Operator was offered but not run | The answer was a plan with an operational afterlife (pilots to deliver) | None. The skill's "suggest it in one line, never add it silently" was followed. |
