# Writing Register Discipline and De-AI Patterns

Distilled from a full abstract-optimization arc (six user-feedback rounds, two external-model
cross-reviews) on a systems-measurement paper. The failure mode this file exists for: prose that
is grammatically clean, informationally dense, and still reads as machine-written — because the
problem is **structure, not word choice**. A second model reviewing the same text caught
structural issues that two self-passes missed.

## 1. Three registers for the same claim (mismatch = failure)

The same claim needs three phrasings. Mixing them is the most common register failure.

| Register | Where | Example (gap statement) |
|---|---|---|
| Paper prose | Abstract, body | "Configuration habits inherited from the NVLink era carry an implicit premise — negligible inter-GPU communication — that fails here; we measure, rule by rule, the cost of ignoring it." |
| Academic oral | Advisor briefings, defense | "These rules all carry an implicit premise — negligible inter-GPU communication. Whether that premise holds on PCIe-only clusters was never systematically tested — we tested each rule." |
| Casual story | Ice-breaking, informal | "Every rule assumes fast inter-GPU links. Nobody had tested whether that holds on our slow one. So I measured it." |

The casual version ("so I measured it") is an asset in the story opening and a register collapse
in a formal statement. The formal version's structure — implicit premise + missing validation +
per-rule empirical test — is what academic statements need; do not replace it with process
colloquialisms.

## 2. AI-tell pattern catalog (structure-level)

Each pattern below survived contact with real review. Fix by rebuilding the sentence shell;
the frozen content is the facts, numbers, and author judgments.

- **Symmetric skeletons.** "Quantization cuts compute while communication stays unchanged" —
  perfectly balanced clauses, especially when several appear in one passage. Break the symmetry:
  "quantization only makes compute faster; the inter-GPU cost does not move."
- **The story-arc template.** Problem → old wisdom fails → we found a core rule → built a tool →
  sealed validation → big win, with every step dramatized. The logic is fine; the tell is that
  every step is equally polished. Keep the logic, flatten the drama, vary sentence length.
- **"Every sentence too polished" is itself the tell.** Normal good abstracts are plain:
  problem → observation → method → result. Do not write badly to remove AI flavor, and do not
  compress the abstract into hardtack for information density.
- **Conclusion labels and inference stairs.** "The core mechanism is a computable rule:" /
  "this means → which shows → therefore" chains where each conclusion adds no new information
  over the previous one.
- **Component noun-stacks.** "A closed-form latency predictor assembled from microbenchmarks
  plus a single anchor" — a parts list, not a method. Rewrite as action narrative: "spend five
  minutes measuring inter-GPU communication speed on the new cluster, plug the numbers into the
  formula, and read out the latency of every configuration."
- **Four-character parallel pairs** (Chinese: 先行存证、后上机实测). Natural sequence is fine;
  the tell is when every clause is a four-character pair.
- **Metanarrative openers** ("说起来挺偶然" / "Interestingly, ..."). Enter on the fact.
- **Triple enumerations** where the three items are near-synonyms or scale up per item.
  Protected: genuine result lists with distinct metrics.

## 3. Abstract density budget

- **Two to four headline numbers maximum.** The rest goes to the body.
- Mechanism numbers in word form: "from a third to nearly a half", not "33% → 48%".
- Experiment scale (number of configurations, load points) belongs in the body, unless the
  campaign scale is itself a reviewer-fought claim — then one parenthetical, both languages.
- Mechanism-detail parentheses (where the valley sits, which cluster) are cut; the claim does
  not depend on them and the body owns the specifics.
- **Before slimming any abstract: count every gate/claim string in the whole file.** If a
  string occurs only once (abstract), cutting it breaks the claim lock — keep it verbatim or
  retarget the lock deliberately and record why.

## 4. Wording substitution table

| Reads as AI / imprecise | Professional plain |
|---|---|
| the commodity form is X | we focus on the typical X |
| will mislead; by how much | may not remain optimal / does not hold |
| carry them over, cost measured rule by rule | we systematically evaluate their performance impact |
| the core mechanism is a computable rule | we find that / experiments show |
| shifts left | decreases from TP-8 to TP-4 |
| five-minute measurement | requires only lightweight interconnect profiling |
| directly compute | estimate / predict |
| sealed protocol | held-out cluster evaluation; predictions written down before measurements |
| all rankings hit | correctly identifies the relative ranking of all evaluated configurations |
| quantize-then-replicate (first use) | quantization with increased replication |
| strongest conventional scheme | best-performing baseline |
| quality nearly unchanged | name the metric, or negligible quality degradation |
| data released upon acceptance | We will release ... (if venue policy allows) |

## 5. Rigor wording

Any "all correct / exact hit" claim must name a standard: full relative ranking of all
evaluated configurations, top-1 identification, pairwise agreement, or a median error bound.
A reviewer's first question on a prediction paper is which of these you mean. State it in the
abstract if the prediction is a headline contribution.

## 6. Contribution framing

If the headline observation is intuitive in hindsight (compression reduces compute, communication
does not shrink), the abstract's weight must sit on what is **not** obvious: that the mechanism is
modeled into a transferable predictor, that the prediction avoids exhaustive configuration search,
and that the transfer was validated out-of-sample. Otherwise reviewers read "tuning experience
plus profiling".

## 7. Process

- **Cross-model review before final.** Feed the same text to a second model as a hostile
  reviewer; in the source arc it produced a structural diagnosis (the story-arc template,
  "every sentence too polished") that two self-passes missed.
- **Batch edits atomically.** Script bulk replacements with per-replacement uniqueness
  assertions; write the file only after every anchor matched. Backslash-containing LaTeX
  fragments (\times, \, \%) must be built without escape-prone literals.
- **Rhythm audit.** After rewriting, read once only for sentence-length variety and clause
  shape; uniform rhythm is the residual tell after individual patterns are fixed.
