---
name: decorrelated-research
description: Ground a consequential question in ≥3 independent model lineages, require ≥2 to return, and consolidate them with the seat's own answer — which is written BEFORE reading theirs. Agreement across lineages is corroboration, never proof; divergence is the signal. Produces the material to act on, not a verdict.
triggers: a consequential question with no cheap oracle (design, architecture, literature, "is this approach right?") · before a judgment-class sign-off · when the seat and the top tier are the same model (governor §5) · operator asks for a second opinion
model_tier: seat = any tier (it dispatches and consolidates, it does not adjudicate); external seats must span ≥2 lineages
allowed_tools: Bash (external CLIs: codex, agy) · Read/Write (bundle + answers) · manual paste for seats with no CLI (Perplexity)
risk_level: medium — external seats mean data egress; the bundle leaves the machine and cannot be recalled
---

## Purpose

Answer a question that has **no check cheaper than the work itself** — where the seat cannot verify
its own answer on a substrate, so a wrong answer would simply sound right. The defence is not a
better prompt; it is **uncorrelated error**. Ask several models from different lineages, blind, and
treat the places they disagree as the map of what is actually uncertain.

This skill produces **material to ground an action**, not a verdict. The seat still decides;
`floor-detector` still governs whether the decision may be signed.

Distinct from its neighbours, and don't duplicate them: `spatiotemporal-critique` **critiques an
artifact that exists**; `deep-research` fans out **within one lineage** (Claude subagents + web);
this skill **gathers an answer across lineages** for a question that has no artifact yet.

## When to use

Use when all three hold:

- The question is **consequential** — irreversible, external, on a protected path, or a verdict the
  operator will rely on.
- It is **judgment-class** — no test, grep, hash, or compile could settle it. (If one could, run it.
  That is cheaper and it is a real oracle. This skill is what you use when no oracle exists.)
- The seat holds a view. *That is the danger*, not the qualification. A view you cannot check is the
  exact failure this exists to catch.

**Do not use it** for anything with a substrate (`systematic-debugging` owns that), for a trivial or
reversible call (the floor rule: no ceremony), or to manufacture support for a decision already made.
A run whose purpose is to be agreed with is theatre and burns quota you will want later.

## Required inputs

1. **The question**, in one sentence, with its falsifier — what evidence would change the answer.
2. **A context bundle** — one file, self-contained, that a model with zero knowledge of the repo can
   read. One file, not a folder: a chat window cannot take nineteen.
3. **A sensitivity ruling from the operator.** The bundle leaves the machine. Method artifacts
   (rules, code, structure) are usually fine; anything naming private content, clients, unpublished
   work, or personal notes is an **operator decision, not the seat's**. Split the bundle and say
   what you withheld, rather than quietly dropping files or quietly sending them.
4. **A quota check.** `codex` and `agy` both have hard walls that return exit 1 with empty stdout —
   which reads exactly like a bad prompt. Check before you promise a panel.

## Deterministic steps

1. **Frame.** Write the question and its falsifier. Run `floor-detector`: if a cheap oracle exists,
   stop and use it.
2. **Write the seat's own answer FIRST, and seal it.** Commit-before-reveal, applied to yourself. An
   answer written after reading three others is not an independent fourth data point; it is a
   summary of them wearing a fourth hat. Save it to a file before dispatching.
3. **Build the blind prompt.** *The load-bearing step.* Strip every hypothesis of your own from it.
   A prompt that carries your priors correlates the seats you are paying to be uncorrelated, and the
   whole exercise returns your own opinion in three voices. State the question, the evidence, the
   rules (cite or say unverified; grade the evidence; refute before agreeing), and the deliverable —
   nothing else. Tell the seats explicitly that another model has answered and they have deliberately
   **not** been shown it.
4. **Dispatch ≥3 seats spanning ≥2 lineages.** Mixed automated and manual is the normal case, not a
   degradation:
   - `codex exec --sandbox read-only --skip-git-repo-check -C <dir> --color never -o OUT.md - < prompt+bundle`
     (prompt on **stdin**; a prompt in argv with stdin open hangs forever)
   - `agy -p "<system>"` with the payload on stdin. **`gemini` is a dead binary** — it fails auth
     unconditionally. The seat is `agy`.
   - **Perplexity has no CLI.** It is a manual seat: attach the bundle, paste the prompt, save the
     answer beside the others. A manual seat is a full seat.
5. **Floor check.** Fewer than 2 returns, or all returns from one lineage → the run is **DEGRADED**.
   Say so and do not consolidate. Two seats of one lineage agreeing is one seat.
6. **Consolidate, divergence-first.** Label every finding with the existing house vocabulary — do not
   invent a new one:

   | Label | Means |
   |---|---|
   | `CORROBORATED` | all returning lineages agree |
   | `MAJORITY (n/m)` | most agree; the dissent is **recorded, never averaged away** |
   | `DIVERGENT` | they disagree — **this is the signal; lead with it** |
   | `SINGLE` | one seat only; a contested point to surface, not a finding |
   | `OWNER-FORK` | the disagreement is about values or frame → the operator's, always |

7. **Spot-check the citations.** External models fabricate references. Check what you can; mark the
   rest `unverified`. A fabricated citation that reaches the operator is the most damaging thing this
   skill can produce — it has happened before and an external panel caught it.
8. **Unseal your answer and compare.** Where you were wrong, say so explicitly. That is a result, not
   an embarrassment.
9. *(Optional second pass.)* Now show the seats your answer and ask them to break it. Only now — a
   critique written by a model that never formed its own view is worth little.

## Output schema

One file: `<topic>_consolidation_<date>.md`

- **Question + falsifier** — one line each.
- **Panel** — seat · lineage · returned yes/no · quota state. A missing seat is named, not omitted.
- **DIVERGENT findings first**, each with both positions and each position's evidence.
- **MAJORITY**, with the dissent quoted.
- **CORROBORATED**, each naming the ≥2 lineages behind it.
- **SINGLE / OWNER-FORK.**
- **Citation audit** — checked · unverified · fabricated.
- **Where the seat was wrong**, versus its sealed answer.
- **What to do**, and what would change it.

## Validation gate

Each can fail; a run that cannot fail is not a run.

1. `≥2 seats returned, spanning ≥2 distinct lineages` — else stamp **DEGRADED**.
2. The blind prompt contains **none** of the seat's hypotheses. Check it: grep the dispatched prompt
   for your own verdict words. A hit means you correlated the panel and the run is void.
3. The seat's own answer file exists and is **older** than the first seat's answer. Check the
   timestamps. If it is newer, commit-before-reveal did not happen and the seat's answer is
   contaminated.
4. Every `CORROBORATED` names ≥2 distinct lineages. Same-lineage agreement labelled as corroboration
   is a false positive and the most likely way this skill lies to you.
5. Every citation is `checked` or `unverified`. No unlabelled citation ships.
6. At least one `DIVERGENT` or an explicit statement that the panel did not diverge. A panel that
   agrees on everything either was correlated or was asked a question with an obvious answer — both
   are findings about the run, not about the topic.

## Escalation triggers

- Fewer than 2 seats return, or all seats share a lineage → **operator**: the run is degraded and a
  degraded run may not ground a consequential action.
- The seats agree, but none of them cites a source that checks out → **operator**: unanimous
  unsupported agreement is a shared blind spot, not evidence.
- A `DIVERGENT` finding turns on values or the frame → **operator**, always. The seat never
  adjudicates a frame.
- A seat contradicts something the operator asserted → surface it with the evidence **before** the
  verdict (the owner-claim check), never after.
- Both external CLIs are quota-blocked → **operator**: say so and offer the manual path. Do not
  silently run a one-lineage panel and present it as decorrelated.

## Worked example

*(2026-07-12, real.)* Question: four open questions about the project template — does the house rule
"prototype → parallel upgrade → top-down integration" hold up?

The seat's blind prompt (`briefs/external/PROMPT_A_blind.md`) deliberately carried none of the seat's
hypotheses; the bundle was one file, 35k tokens (`CONTEXT_BUNDLE.md`), and the operator's task log,
vault maps, and unpublished work were **withheld** — with the cost stated, not hidden: the external
seats could not run the retrospective checks, and the prompt told them to say where that limited them.

Panel: Perplexity (manual — no CLI), Gemini via `agy` (manual, quota-walled at the CLI), codex.
Result on Q2: Perplexity `accepted-with-caveats`, Gemini `rejected`. **DIVERGENT — and that is the
most useful line in the whole run.** A single seat would have handed the operator a confident answer
in whichever direction it happened to fall.

## Weak-model version

Do steps 1, 3, 4, 5, 6 only. Dispatch, apply the floor check, sort into the five labels, and stop.
Skip the citation audit and the sealed-answer comparison — a weak seat's own answer is not worth
sealing, and a weak seat cannot reliably audit a citation. **Hand the labelled panel to the operator
raw.** Do not synthesise a recommendation from it: a weak model consolidating three strong ones
produces confident mush and hides the divergence, which was the only thing worth having.

## Strong-model version

All nine steps, plus the adversarial second pass (step 9), plus a deliberate **lineage spread**: pick
seats that fail differently, not seats that are merely branded differently. Two frontier models from
vendors who trained on the same web dump are less decorrelated than one frontier model and one grep.
Prefer a substrate check over a fourth model whenever one exists — a test that can fail beats a
fourth opinion that cannot.

## Failure recovery

- **Empty answer file, exit 1.** Read stderr before blaming the prompt. `You've hit your usage limit`
  (codex) and `Individual quota reached` (agy) both look like silence. Quota walls are the single
  most common failure of this skill.
- **Codex hangs.** The prompt is in argv with stdin open. Put it on stdin (`-`) or close stdin.
- **`gemini` fails on auth.** Expected — that binary is dead. Use `agy`.
- **A seat summarises instead of critiquing** (a "null seat"). Discard it and say the panel is short
  one; a null seat that is counted inflates the corroboration count, which is worse than not running it.
- **The panel agrees with the seat on everything.** Suspect the prompt, not the world. Re-read what
  you dispatched for leaked priors (gate 2).

## Elevation contract

This skill **gathers**; it never signs. Its output is input to a decision, and the decision still
passes `floor-detector` before any consequential sign-off. A panel is not an oracle: a unanimous
external panel may be confidently wrong together, and `raise-only` applies — a panel may add scrutiny,
never remove it. "Three models agreed" is **not** verification and may never be presented as one.

Values and frames are never elevated to the panel. They go to the operator.
