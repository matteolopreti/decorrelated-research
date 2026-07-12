# decorrelated-research

A Claude Code skill for questions that have **no check cheaper than the work itself** — design calls,
architecture choices, literature questions, "is this approach right?". When no test, grep, or
compiler can settle it, a wrong answer simply sounds right. The defence is not a better prompt; it
is **uncorrelated error**: several models from different lineages, asked blind, with the places they
disagree treated as the map of what is actually uncertain.

## Five-minute start

1. **Seal your own answer first.** Write what you currently believe to a file, timestamped, before
   any seat sees the question. An answer written after reading three others is a summary of them
   wearing a fourth hat.
2. **Build one blind prompt + one evidence bundle.** The prompt carries the question, the evidence,
   and the rules — and **none of your hypotheses**. The bundle is ONE flattened file (a chat window
   cannot take nineteen attachments). Anything private stays home, and the prompt says what was
   withheld.
3. **Dispatch 3+ seats across 2+ model lineages.** CLI seats (codex, gemini/agy) and manual seats
   (a chat window with the bundle attached) count equally.
4. **Consolidate divergence-first.** `CORROBORATED` / `MAJORITY (n/m)` — dissent recorded, never
   averaged — / `DIVERGENT` — the signal, lead with it — / `SINGLE` / `OWNER-FORK` (values and
   framing go to the human, always).
5. **Audit the citations, then unseal.** External models fabricate references; check what you can.
   Then compare the panel against your sealed answer and say where you were wrong — that is a
   result, not an embarrassment.

The output is material to ground a decision — the skill never signs one. Agreement across lineages
is corroboration, never proof: a unanimous panel may be confidently wrong together, and panel
signals are raise-only (they add scrutiny, never remove it).

## Why blind, why sealed

Both halves are the same rule — **commit-before-reveal** — pointed in two directions. A prompt that
carries your priors correlates the seats you are paying to be uncorrelated: the exercise returns
your own opinion in three voices. And an opinion you form after reading the panel is not an
independent data point. The skill's validation gate makes both checkable: grep the dispatched
prompt for your verdict words (a hit voids the run), and compare file timestamps (your sealed
answer must predate the first seat's return).

In the run this skill was extracted from, the dispatcher had confidently claimed a contradiction
between two project documents — a claim built on a partial read. One external seat read the file to
the end and refuted it. Three models *agreeing* with the dispatcher would have proven little; one
model reading more carefully was worth the whole run.

## Install

```
/plugin marketplace add matteolopreti/decorrelated-research
/plugin install decorrelated-research@decorrelated-research
```

Or copy `skills/decorrelated-research/` into `~/.claude/skills/` (pick one install path, not both —
two copies drift).

## Pairs with

[spatiotemporal-critique](https://github.com/matteolopreti/spatiotemporal-critique) — the sibling
skill. It **critiques an artifact that exists** (preserve-first review, multi-model critic panel);
this one **gathers an answer where no artifact exists yet**. Same epistemics — different-lineage
seats, capability over availability, agreement ≠ proof — applied before the work instead of after.

MIT.
