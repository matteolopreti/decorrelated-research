# decorrelated-research

A Claude Code skill for questions that have **no check cheaper than the work itself** — design calls,
architecture choices, literature questions, "is this approach right?". When no test, grep, or compiler
can settle it, a wrong answer simply sounds right. The defence is not a better prompt; it is
**uncorrelated error**.

## What it does

1. You (the seat) write your own answer FIRST and seal it — commit-before-reveal, applied to yourself.
2. Build a **blind prompt**: the question and evidence, stripped of every hypothesis of your own. A
   prompt that carries your priors correlates the seats you are paying to be uncorrelated.
3. Dispatch to **3+ seats spanning 2+ model lineages** (CLI seats like codex/agy, plus manual seats —
   a chat window with a file attached is a full seat).
4. Consolidate **divergence-first**: `CORROBORATED` (all lineages agree) / `MAJORITY (n/m)` (dissent
   recorded, never averaged away) / `DIVERGENT` (the signal — lead with it) / `SINGLE` / `OWNER-FORK`
   (values or frame → always the human's).
5. Audit citations (external models fabricate references), then unseal your answer and say where you
   were wrong.

The output is material to ground a decision — the skill never signs one. A unanimous panel may be
confidently wrong together; agreement across lineages is corroboration, never proof.

## Install

```
/plugin marketplace add matteolopreti/decorrelated-research
/plugin install decorrelated-research@decorrelated-research
```

Or copy `skills/decorrelated-research/` into `~/.claude/skills/`.

Pairs with [spatiotemporal-critique](https://github.com/matteolopreti/spatiotemporal-critique), which
critiques an artifact that exists; this skill gathers an answer for a question that has no artifact yet.

MIT.
