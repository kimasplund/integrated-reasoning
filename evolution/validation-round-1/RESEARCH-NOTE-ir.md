# Research note — integrated-reasoning after validation round 1

**Verdict NOT VALIDATED** (record: `ab-test-results-ir-round1.md`). The skill stays installed —
the round measured no harm to correctness (keys/D3 parity) — but its value on a frontier model
is currently negative-to-neutral: guards regressed on machinery narration; value dims flat.

## The v-next candidate, fully specified (a fresh gauntlet decision, not auto-run)

Exactly two deltas, mirroring the premortem lineage's proven fixes:
1. **Backstage clause** (the headline fix): "The scoring pass is silent. Dimension scores,
   affinity arithmetic, pattern names, and selection narration never appear in the user-facing
   answer — the reader sees the analysis, not the apparatus that chose it. A selected pattern's
   structure may appear only as content built from the problem's own facts (a hypothesis table,
   a criteria matrix), never as methodology narration. Announce nothing; just analyze."
2. **Pre-delivery leak scan**: literal text search over the answer for pattern names
   (Tree of Thoughts/ToT, Hypothesis-Elimination/HE, …), "affinity", "dimension score",
   "confidence aggregation", "checkpoint"; every hit rewritten as content or deleted.

Retest under the identical pre-registered gate (fresh briefs — J/K/L are burned). Predicted
effect from this round's data: D6 recovers toward baseline (the 22-vs-5 census gap is entirely
narration, not analysis), D5 partially recovers; D2/D3 likely remain parity on frontier models —
if they do, the honest README claim is "hygiene for strong models, routing for weak ones", and a
small-model round becomes the interesting experiment.

## Open questions worth a future round
- **Small-executor value**: run the same harness with a small local model as executor — the
  skill's own design notes claim that's where routing pays. This round neither confirms nor
  refutes it.
- **Harder briefs**: all three briefs hit key ceilings (97.9% both arms). A brief slate where
  the baseline demonstrably fails (multi-constraint, adversarial, or genuinely novel) would
  give D2/D3 headroom to show a difference — if one exists.

---
## Round-2 addendum (backstage candidate — tested, not adopted)

The suite-wide backstage clause + leak scan **works as designed**: leaks 22→(9 champion)→**0**,
D6 +1.50 (p=0.004), full recovery to no-skill baseline parity on both guards, fixtures clean.
It failed adoption on the pre-registered key leg: 22.5/24 vs the champion's 24/24 — three
"torn → lowered" thinness edge calls (two on one report), no outright miss. Suspected mechanism:
the scan's "or delete it" invites deletion of borderline real content. **v2.2b recipe = identical
deltas with the scan clause reworded: "rewrite as content — never delete analysis."** Retest
under the identical gate on a fresh slate (M/N/P now burned). Round record:
`../gauntlet-ir-round2/ab-test-results-ir-round2.md`.
