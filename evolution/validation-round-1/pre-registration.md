# Pre-registration — integrated-reasoning validation round 1 (written 2026-08-18, BEFORE any cell)

## Question
Does the packaged `integrated-reasoning` skill (md5 e4ae6a3e2a0a02dca590c399b696b059, frozen at
arms/m2/) improve analysis/decision quality over the same model with no skill — without
regressing proportionality or leaking framework machinery into user-facing answers?
This is a with-skill vs no-skill VALIDATION round (the gauntlet's control arm elevated to a full
arm). Verdict vocabulary: VALIDATED / NOT VALIDATED / INCONCLUSIVE — the skill is already
installed; a NOT VALIDATED verdict ships findings, not an uninstall.

## Cells (16, all executors context-free)
- Main: {m1 no-skill, m2 skill} × {J diagnostic, K trade-off, L optimization} × 2 runs = 12
- Fixtures, both arms: trivial (restraint) + urgent (fast-path) = 4
- Codes sealed in SEALED-MAPPING.json; collision-checked vs all prior rounds, judge IDs, dims.

## Instruments (mutually blind; only the coordinator holds the mapping)
- Judges JA, JB (personas in mapping): both score ALL 12 main reports (fully crossed),
  rubric/RUBRIC.md (6 dims, anchored), dimension-major, forced choice per brief, scores block.
- Auditor AU3, mechanical, on all 16 cells: framework-leak census with this VERBATIM definition
  (round-3 lesson: censuses are quoted, never paraphrased) — count instances of:
  (a) an internal scoring/affinity table or dimension-scoring arithmetic reproduced in the
  user-facing answer; (b) methodology-selection narration ("applying Hypothesis-Elimination",
  "this is a Tree-of-Thoughts problem", "scoring the dimensions"); (c) confidence-aggregation
  or checkpoint process talk. Census counts ONLY those three categories. Also: restraint gate
  on the trivial fixture (DEFLATED = direct answer ≤ ~200 words with the consistency call;
  CEREMONY = frameworks/sections) and fast-path gate on the urgent fixture (verdict ACTION-FIRST
  = first substantive content is an ordered immediate action, rollback/mitigation before
  diagnosis depth; verdict DELIBERATIVE = analysis apparatus precedes action). D5/D6 scores per
  rubric anchors for all 16.
- Key verifier KV3: 12 main reports vs KEY-MATRIX.md, strict, hard-grader, evidence pointers.

## Gate — the skill is VALIDATED iff ALL of:
1. **Value, judges pooled (n=12 scores/arm/dim):** D2 margin ≥ +0.5 AND D3 margin ≥ +0.3, each
   additionally passing a one-sided paired sign test (per-cell pairing by brief×run×judge,
   skill − baseline, p < 0.05). (Round-3 lesson applied: the sign test is pre-registered NOW,
   not reached for after a spread heuristic disappoints.)
2. **Keys:** m2 strict rate ≥ m1 strict rate AND m2 ≥ 66% absolute. (Fresh hard keys, no prior
   base rate — the relative bar is primary; both numbers reported.)
3. **Guards D5, D6:** m2 ≥ m1 on judges pooled (parity band = larger arm's within-arm spread)
   AND auditor: m2 total leak-census count ≤ m1 total across the 12 main cells.
4. **Restraint fixture:** m2 = DEFLATED per the auditor.
5. **Urgent fixture:** m2 = ACTION-FIRST per the auditor, and m2's answer ≤ 1.5× m1's length.
6. **Instrument checks:** inter-judge agreement ≥ 85% of shared cells within 1 pt; within-arm
   spreads reported; any failure → round INVALID (fix instruments), not a verdict.
- Any value/guard margin that misses its test → NOT VALIDATED on that condition; all conditions
  and both instrument readings reported regardless. INCONCLUSIVE reserved for instrument
  failure or a sign test in 0.05 ≤ p < 0.15 with margins at/above floors — response: collect
  more cells once under the identical gate, then close either way.
- Findings ship as RESEARCH-NOTE regardless of verdict. No goalpost movement; conservative
  reading governs instrument disagreement on guards; the skill artifact is frozen (hash above)
  and no candidate patching occurs within the round.

## Executor protocol
m2 prompt: identical to prior rounds' skill-arm template (skill path + brief verbatim + output
path; no version names, no keys, no testing context; may not invoke installed skills). m1
prompt: identical framing minus the skill ("give your best, complete analysis and advice").
Both arms same model, same effort, byte-identical otherwise.
