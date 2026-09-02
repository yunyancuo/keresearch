---
name: research-project-playbook
description: End-to-end playbook for systems measurement research, distilled from a completed 8xRTX4090 LLM serving study (70+ configs, findings F1-F18, dual-language MLSys-target paper). Covers gap defense, phase-zero microbenchmarks, experiment discipline, findings logging, number auditing, advisor communication, adversarial self-review, and venue strategy. Use when starting a research project, designing an experiment campaign, defending a topic choice, writing the story for a non-specialist, or deciding where to submit.
---

# Research Project Playbook

This skill is the distillation of one full research cycle that worked: topic selection through submission-ready paper. It captures the operating rules that survived contact with a real project, a real advisor, and real reviewers. It complements, not replaces, the two writing skills in this repo (`paper-writing` for sentence-level craft, `research-paper-writing` for section structure and reviewer-facing presentation). Use this playbook for everything *around* the writing: what to measure, how to log it, how to defend it, how to tell the story, where to send it.

## Phase 0: Topic Selection and Gap Defense

**Evidence-chain method.** Do not claim "nobody did X". Build a chain of adjacent papers where each link has a concrete blind spot, and position your work as the conjunction nobody measured. Example from the source project (six rings): fleet study shows 1.3% utilization → replication-beats-TP paper but on NVLink → dual-card study stopped at TP-2 net-negative → analytical planners omit communication → PP-recommender calibrated on offline traces → limit-study framework lacks the communication constant. Each ring is one sentence with a citation. The chain *is* the motivation section.

**Spectrum-end positioning.** If a design axis is becoming explicit in the literature (e.g., interconnect coupling tightness), check which end is populated. Occupied-tight + empty-loose = your paper completes the spectrum, and the related-work section writes itself ("they occupy the tight end; the loose end, where deployed hardware actually lives, is unmeasured").

**Kill test.** Write the one-sentence claim "first <verb> of <axes> on <platform> under <conditions>". Name the three closest papers and each one's blind spot in one sentence. If any of this is vague, there is no gap yet; go read more, do not start experiments.

**Decide the paper type early.** Measurement study vs mechanism/system paper. This decision determines venues, reviewer expectations, and what "enough" means. A measurement paper's bar: current practice demonstrably wrong at scale + counter-intuitive boundaries + large deltas. If those three don't hold, upgrade the plan (see Phase 4) before writing.

## Phase 1: Phase-Zero Microbenchmarks

Before any end-to-end experiment, profile the platform itself. On the source project: 1KB all-reduce latency per TP degree, peak bus bandwidth, P2P toggle, memory ceiling. Cost: minutes. Value:

1. Every later claim anchors to measured constants, not datasheets.
2. The constants become the predictor's inputs (see below).
3. Surprises here redirect the whole campaign (bus bandwidth collapsing from 14 to 9.3 GB/s at TP-4 reshaped everything).

The operator test: whatever the cluster, the three dominant quantities (collective latency, bandwidth, memory ceiling) can be measured in less time than it takes to read the planning literature. If your method needs more than that to produce a recommendation, it is not deployable.

## Phase 2: Experiment Discipline

**Single-variable or nothing.** Every configuration point is a fresh server start. No config inherits state from a previous point. Comparisons you can't explain in one sentence ("only the concurrency differs") don't go in the paper.

**The ladder.** Concurrency {1, 16, 32, 64, 128} plus probes {256, 512}. Always include the low end: single-request latency isolates the communication term and is where analytical models are checkable.

**Workload shapes.** Closed-loop default sweep, then at least: open-loop Poisson arrivals (SLO knees; closed-loop numbers *understate* serving gaps), prefix-reuse trace (cache interactions with replication), long-context (KV pressure, preemption cascades). A ranking that survives all shapes is a claim; a ranking from one shape is an anecdote.

**Repeats and margins.** Three full restarts on key configs. Report CV. Express headline margins as multiples of measured 3-sigma ("25-100x the run-to-run spread"). This one habit kills the "is it noise?" review before it is written.

**Artifact convention.** `results/DATE_CONFIG_CONC_RUN/{bench.json, dmon.log, vllm_startup.log, meta.yaml}`, plus a script that regenerates the aggregated CSV from raw. The paper's every number must be traceable to one directory. When a number dispute arises (a prose value vs a table value), the raw JSON arbitrates; this caught a real typo (3,296 vs 3,349) before submission.

**Findings log.** Maintain `notes/findings-log.md`: numbered findings F1, F2, ... Each is 2-5 sentences: what was measured, the number, the one-line interpretation, and the pointer to the raw directory. Date every entry. This log becomes the paper's spine; sections and tables are rearrangements of it. It also decouples "doing experiments" from "writing" so neither blocks the other.

**Engineering pitfalls are first-class findings.** Every environment failure (JIT version gates, driver-level P2P blocking, bash silently ignoring special-variable assignment, container startup races) gets documented with its fix and a reproduction path. In deployment-facing research these notes are often what practitioners cite you for.

## Phase 3: The Paper

**Single source of truth.** Markdown masters (English + Chinese if needed) → render to LaTeX (newtx / ctex) and docx by script. Never hand-edit the rendered artifacts.

**Abstract template.** What the task is → what traditional and recent methods each get wrong (one clause each) → we designed X → experiments show advantage (leading number + secondary metric + cost/quality caveat). No numbers dump; three numbers maximum.

**Structure.** Standard six sections. Introduction: four paragraphs (task and stakes / traditional + recent failures / closest attempts and their blind spots / this paper), contributions list is the *only* list in the intro. Details belong in Method, not Introduction.

**Style transfer.** Pick one closest-genre paper as the prose model and imitate its rhetorical structure, not its content. Combined with de-AI gates: zero em-dashes, zero translationese, zero colloquialisms, varied sentence length, concrete numbers over adjectives. See references/abstract-register-deai.md for the full AI-tell pattern catalog, the three-register discipline (paper prose / academic oral / casual story), the abstract density budget, and the wording substitution table.

**Symbol audit (run before every render).** Every symbol defined at first use. No letter pulls double duty (a real collision: `c` as config vector and as per-sequence KV cost; `d` as head dim and as drafter memory). Read only the equations and their immediate prose in one pass; this pass takes ten minutes and reviewers do notice.

**Number audit.** Script a cross-check of every numeric token in the draft against the raw JSONs and against the other language version. Numbers drift during revision; drift is silently fatal.

**Compile and look.** Compile locally (tectonic or equivalent), render pages to images, inspect every page. Also run a programmatic margin-overflow scan; then manually confirm flagged spans are only legitimate punctuation hanging.

**Adversarial pass.** Read once as a hostile reviewer: circularity (was the "prediction" calibrated on the data it predicts?), overclaims ("to our knowledge" must survive a search), baseline fairness (were baselines tuned as hard as your method?), and the limitation a reviewer would lead with. Fix or pre-empt each with data, not prose.

## Phase 4: The Story and the Audience

**The advisor card.** Maintain a small file with: one-sentence elevator version (plain language, zero jargon), two-sentence version (problem / method, aligned to the advisor's own preferred template if they have one), three one-sentence contributions, and a number lookup table for follow-up questions. Translate jargon for non-specialists ("tensor parallelism" → "wiring cards into one big computer").

**The user's sentence wins.** When the principal investigator condenses the work in their own words, that sentence goes in the card verbatim, translated but not rewritten. Their phrasing tells you what they understood and what they will defend in rooms you are not in.

**Honesty about contribution weight.** If the owner says "the innovation feels thin", test it: is current practice actually wrong at scale (fleet statistics, not anecdotes)? Is the boundary counter-intuitive (would a practitioner guess the flip point)? Is the delta large and free? If all three hold, the work is defensible as a measurement paper; if not, upgrade before polishing prose.

**Upgrade ladder when more novelty is needed** (cheapest first): package the predictor + decision rules into a runnable tool → closed-loop controller that reconfigures with load (measurement becomes a system) → heterogeneous placement/routing mechanism → kernel-level moonshot (future work only, unless time is unlimited).

## Phase 5: Venues

**Ladder, not lottery.** One stretch (measurement-friendly top venue), two realistic (CCF-B systems conferences), one floor (CCF-C or solid workshop), one journal fallback. Check current deadlines by search, never from memory. Measurement studies belong at measurement-friendly venues (MLSys, EuroSys, ICDCS/IPDPS line), not mechanism-mandated ones (OSDI/SOSP/NSDI) unless a system contribution was added in Phase 4.

**Dual-language warning.** Chinese-journal + English-conference versions of the same work count as dual submission in most Chinese venues. Keep the second language version for internal review, or rewrite substantially, or pick one.

**Preprint.** ArXiv before review is allowed at most systems venues; verify per venue, then post. It costs nothing and stakes the claim.

## Cross-Session Operations

- Keep a project `AGENTS.md` (or equivalent handoff file): one-paragraph project definition, directory map, current progress checklist, environment pitfalls with fixes, working conventions. Update it at every milestone; it is the cheapest context restoration that exists.
- Persist non-derivable facts (server credentials location, advisor preferences, user style rules) in memory files, not in chat history.
- Commit and push at every milestone. The repo is the lab notebook.
