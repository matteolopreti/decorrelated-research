---
name: decorrelated-research
description: Ground a consequential question in 3+ independent model lineages, require 2+ to return, and consolidate them with your own answer — which is written and sealed BEFORE reading theirs. Agreement across lineages is corroboration, never proof; divergence is the signal. Use for judgment-class questions with no cheap oracle — design calls, architecture choices, literature questions, "is this approach right?" — before a decision someone will rely on. Produces the material to act on, not a verdict.
---

# decorrelated-research — a blind multi-lineage panel for questions with no oracle

## Purpose

Answer a question that has **no check cheaper than the work itself** — where you cannot verify your
own answer on any substrate (no test, no grep, no compiler), so a wrong answer would simply sound
right. The defence is not a better prompt; it is **uncorrelated error**. Ask several models from
different lineages, blind, and treat the places they disagree as the map of what is actually
uncertain.

This skill produces **material to ground a decision**, not a verdict. You still decide, and whatever
review discipline governs your sign-offs still applies.

Distinct from its sibling, and don't duplicate it:
[spatiotemporal-critique](https://github.com/matteolopreti/spatiotemporal-critique) **critiques an
artifact that exists**; a single-model research fan-out (subagents + web) explores **within one
lineage**; this skill **gathers an answer across lineages** for a question that has no artifact yet.

## When to use

Use when all three hold:

- The question is **consequential** — irreversible, external, or a judgment someone will rely on.
- It is **judgment-class** — no test, grep, hash, or compile could settle it. (If one could, run it.
  That is cheaper and it is a real oracle. This skill is what you use when no oracle exists.)
- You hold a view. *That is the danger*, not the qualification. A view you cannot check is the exact
  failure this exists to catch.

**Do not use it** for anything with a substrate (your debugging loop owns that), for a trivial or
reversible call (no ceremony for small stakes), or to manufacture support for a decision already
made. A run whose purpose is to be agreed with is theatre, and it burns quota you will want later.

## Required inputs

1. **The question**, in one sentence, with its falsifier — what evidence would change the answer.
2. **A context bundle** — ONE self-contained file that a model with zero knowledge of your project
   can read. One file, not a folder: a chat window cannot take nineteen attachments, and a single
   flattened bundle with each source's original path in its header works everywhere.
3. **A sensitivity ruling from the human owner.** The bundle leaves the machine, and egress is not
   reversible. Method artifacts (rules, code, structure) are usually fine; anything naming private
   content, clients, or unpublished work is the **owner's decision, not the assistant's**. Split the
   bundle and state what was withheld — never quietly drop files, never quietly send them.
4. **A quota check.** Subscription CLIs have hard usage walls that return exit 1 with empty stdout —
   which reads exactly like a bad prompt. Check before you promise a panel.

## Deterministic steps

1. **Frame.** Write the question and its falsifier. If a cheap oracle exists, stop and use it.
2. **Write your own answer FIRST, and seal it.** Commit-before-reveal, applied to yourself. An answer
   written after reading three others is not an independent fourth data point; it is a summary of
   them wearing a fourth hat. Save it to a file, timestamped, before dispatching.
3. **Build the blind prompt.** *The load-bearing step.* Strip every hypothesis of your own from it.
   A prompt that carries your priors correlates the seats you are paying to be uncorrelated, and the
   whole exercise returns your own opinion in three voices. State the question, the evidence, the
   rules (cite or say unverified; grade every empirical claim; refute before agreeing), and the
   deliverable — nothing else. Tell the seats explicitly that another model has answered the same
   question and they have deliberately **not** been shown its answer.
4. **Dispatch 3+ seats spanning 2+ lineages.** Mixed automated and manual is the normal case, not a
   degradation:
   - **Codex CLI:** put the prompt on **stdin** — a prompt in argv with stdin left open hangs forever:
     `codex exec --sandbox read-only --skip-git-repo-check --color never -o OUT.md - < prompt_plus_bundle.md`
   - **Gemini CLI:** `gemini -p "<system>"` with the payload on stdin. (If your install was migrated
     to Antigravity, the binary is `agy` and the model flag is `--model`, not `-m`.)
   - **A chat window is a full seat.** Attach the bundle, paste the prompt, save the answer beside
     the others. Web-only models (e.g. Perplexity) join the panel this way; a manual seat counts
     exactly as much as a CLI seat.
5. **Floor check.** Fewer than 2 returns, or all returns from one lineage → the run is **DEGRADED**.
   Say so and do not consolidate. Two seats of one lineage agreeing is one seat.
6. **Consolidate, divergence-first.** Label every finding:

   | Label | Means |
   |---|---|
   | `CORROBORATED` | all returning lineages agree |
   | `MAJORITY (n/m)` | most agree; the dissent is **recorded, never averaged away** |
   | `DIVERGENT` | they disagree — **this is the signal; lead with it** |
   | `SINGLE` | one seat only; a contested point to surface, not a finding |
   | `OWNER-FORK` | the disagreement is about values or framing → the human's call, always |

7. **Spot-check the citations.** External models fabricate references. Check what you can; mark the
   rest `unverified`. A fabricated citation that reaches the decision-maker is the most damaging
   thing this skill can produce.
8. **Unseal your answer and compare.** Where you were wrong, say so explicitly. That is a result,
   not an embarrassment.
9. *(Optional second pass.)* Now show the seats your answer and ask them to break it — citation
   check first. Only now: a critique written by a model that never formed its own view is worth
   little, and a seat that has answered blind cannot be anchored retroactively.

## Output schema

One consolidation file per run:

- **Question + falsifier** — one line each.
- **Panel** — seat · lineage · returned yes/no · quota state. A missing seat is named, not omitted.
- **DIVERGENT findings first**, each with both positions and each position's evidence.
- **MAJORITY**, with the dissent quoted.
- **CORROBORATED**, each naming the 2+ lineages behind it.
- **SINGLE / OWNER-FORK.**
- **Citation audit** — checked · unverified · fabricated.
- **Where you were wrong**, versus your sealed answer.
- **What to do**, and what would change it.

## Validation gate

Each can fail; a run that cannot fail is not a run.

1. `2+ seats returned, spanning 2+ distinct lineages` — else stamp **DEGRADED**.
2. The blind prompt contains **none** of your hypotheses. Check it: grep the dispatched prompt for
   your own verdict words. A hit means you correlated the panel and the run is void.
3. Your sealed answer file is **older** than the first seat's answer. Check the timestamps. If it is
   newer, commit-before-reveal did not happen and your answer is contaminated.
4. Every `CORROBORATED` names 2+ distinct lineages. Same-lineage agreement labelled as corroboration
   is a false positive and the most likely way this skill lies to you.
5. Every citation is `checked` or `unverified`. No unlabelled citation ships.
6. At least one `DIVERGENT`, or an explicit statement that the panel did not diverge. A panel that
   agrees on everything either was correlated or was asked a question with an obvious answer — both
   are findings about the run, not about the topic.

## Escalation triggers

- Fewer than 2 seats return, or all seats share a lineage → **the owner**: a degraded run may not
  ground a consequential decision.
- The seats agree, but none cites a source that checks out → **the owner**: unanimous unsupported
  agreement is a shared blind spot, not evidence.
- A `DIVERGENT` finding turns on values or framing → **the owner**, always. The panel never
  adjudicates a frame.
- A seat contradicts something the owner asserted → surface it with the evidence **before** your
  verdict, never after.
- Every CLI seat is quota-blocked → **the owner**: say so and offer the manual path. Do not silently
  run a one-lineage panel and present it as decorrelated.

## Worked example

*(2026-07-12, real — the run this skill was extracted from.)* Question: four design questions about
a project-template system, including whether its house rule "working prototype → parallel feature
upgrades → top-down integration" holds up.

The blind prompt deliberately carried none of the dispatcher's hypotheses; the evidence was one
flattened 35k-token bundle of method files; the owner's private task history was **withheld**, with
the cost stated in the prompt rather than hidden — the external seats could not run the
retrospective checks, and were required to say where that limited them.

Panel: Perplexity (manual — no CLI exists), Gemini (manual — the CLI was quota-walled), Codex (CLI).
On the per-project-type-scaffolding question the verdicts split `accepted-with-caveats` vs
`rejected` — **DIVERGENT, and the most useful line in the whole run**: a single seat would have
handed the owner a confident answer in whichever direction it happened to fall.

The same run demonstrated why the panel exists: the dispatcher had confidently claimed a
contradiction between two project documents — a claim built on a partial read (grep found `FAIL`,
reading stopped 60 lines before the addendum that retracted it). One external seat read the file to
the end and refuted the claim. Three models and the dispatcher agreeing would have proven little;
one model reading more carefully than the dispatcher was worth the whole run.

## Weak-model version

Do steps 1, 3, 4, 5, 6 only. Dispatch, apply the floor check, sort into the five labels, and stop.
Skip the citation audit and the sealed-answer comparison — a weak seat's own answer is not worth
sealing, and a weak seat cannot reliably audit a citation. **Hand the labelled panel to the owner
raw.** Do not synthesise a recommendation from it: a weak model consolidating three strong ones
produces confident mush and hides the divergence, which was the only thing worth having.

## Strong-model version

All nine steps, plus the adversarial second pass (step 9), plus a deliberate **lineage spread**:
pick seats that fail differently, not seats that are merely branded differently. Two frontier models
from vendors who trained on the same web dump are less decorrelated than one frontier model and one
grep. Prefer a substrate check over a fourth model whenever one exists — a test that can fail beats
a fourth opinion that cannot.

## Failure recovery

- **Empty answer file, exit 1.** Read stderr before blaming the prompt. Quota-wall messages
  (`usage limit`, `quota reached`) look exactly like silence. This is the single most common failure.
- **Codex hangs.** The prompt is in argv with stdin open. Put it on stdin (`-`) or close stdin
  (`< /dev/null`).
- **Gemini CLI fails on auth** (`IneligibleTierError` or similar): the client may have been
  deprecated in favour of Antigravity — try `agy` with `--model`.
- **A seat summarises instead of critiquing** (a "null seat"). Discard it and say the panel is short
  one; a null seat that is counted inflates the corroboration count, which is worse than not
  running it.
- **The panel agrees with you on everything.** Suspect the prompt, not the world. Re-read what you
  dispatched for leaked priors (gate 2).

## Elevation contract

This skill **gathers**; it never signs. Its output is input to a decision, and the decision still
passes whatever sign-off discipline governs your work. A panel is not an oracle: a unanimous
external panel may be confidently wrong together, and panel signals are **raise-only** — they may
add scrutiny to a decision, never remove it. "Three models agreed" is **not** verification and may
never be presented as one.

Values and framing are never elevated to the panel. They go to the human.
