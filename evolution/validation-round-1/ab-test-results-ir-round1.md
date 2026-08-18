# IR validation round 1: integrated-reasoning (skill) vs no-skill baseline

**Pre-registered spec:** `pre-registration.md` (gate sealed before any cell). Skill frozen at md5 `e4ae6a3e2a0a02dca590c399b696b059`.
**Data:** 16 blind executor cells (2 arms × 3 briefs × 2 runs + 2 fixtures × 2 arms), 2 blind judges (fully crossed, all 12 main reports each), mechanical auditor (verbatim pre-registered census), key verifier (12 sealed keys). Same model both arms.

## Verdict: NOT VALIDATED — on this model and these briefs, the skill added no measurable decision-quality lift and regressed both guards by narrating its own machinery.

## Scorecard (judges pooled, n=12 scores/arm/dim; rubric max 30)

| Dim | Baseline | Skill | Margin | Sign test (12 pairs) | Gate | Result |
|---|---|---|---|---|---|---|
| D1 problem grip | 5.00 | 4.92 | −0.08 | 0W/1L/11T | — | |
| D2 method discipline | 4.67 | 4.67 | 0.00 | 2W/2L/8T, p=0.69 | ≥ +0.5, p<0.05 | **FAIL** |
| D3 decision quality | 5.00 | 5.00 | 0.00 | 0W/0L/12T, p=1.0 | ≥ +0.3, p<0.05 | **FAIL** |
| D4 actionability | 5.00 | 4.83 | −0.17 | 0W/2L/10T | — | |
| D5 proportionality (GUARD) | 4.75 | 4.25 | **−0.50** | 2W/5L/5T | ≥ baseline | **FAIL** |
| D6 machinery leakage (GUARD) | 4.75 | 2.92 | **−1.83** | 1W/11L/0T | ≥ baseline | **FAIL** |
| **Total (30)** | **29.17** | **26.58** | **−2.58** | | | |

Auditor (mechanical, verbatim census): baseline 5 leaks total / D5 4.67 / D6 4.50 · skill **22 leaks** / D5 3.50 / D6 2.83. Both instruments agree: guard regression.
Keys: baseline 23.5/24 (97.9%) · skill 23.5/24 (97.9%) — condition 2 PASS (relative ≥, absolute ≥66%), but zero lift.
Fixtures: skill trivial = **DEFLATED, CORRECT** (194w) · skill urgent = **ACTION-FIRST** (693w, rollback at minute 0–2) — conditions 4 and 5 **PASS**. Baseline also deflated (355w) and action-first (782w).
Instrument checks: inter-judge agreement 68/72 (94%) ≥ 85%; the 4 disagreement cells are all D5/D6 on two reports, same direction both judges — condition 6 **PASS**.
Forced choices: **5/6 baseline, 1/6 skill** (JB preferred skill run W9 on the bug brief for its forensics — "equally sound but arrives wrapped in machinery" was JA's read of the same report).

## The three findings

1. **No headroom on a frontier model.** Both arms hit 97.9% on sealed keys and 5.00 on decision quality: the base model already found the retry+append bug, exonerated the intern, synthesized the golden-path decision, and let the stated weights pick SQS. On these briefs the router chose patterns the model would have executed anyway. Scope note, honestly held: the skill's own text targets small local models (its turn-discipline note documents a 7.5B failure mode); this round tested only the strong-model case, which is the case that matters for this installation.
2. **The defect is machinery leakage, and it is structural.** The skill instructs dimension scoring and affinity arithmetic but never says *keep it backstage* — so 5 of 6 skill runs shipped scoring tables, "HE = 4.90, ToT = 4.45" arithmetic, or "How I approached this" narration to the user. 22 census hits vs the baseline's 5; D6 lost 11 of 12 paired cells; every judge complaint about the skill arm was this. The premortem skill learned this exact lesson across three evolution rounds (backstage generation, silent screens, mechanical leak scans); integrated-reasoning has none of it.
3. **The restraint machinery genuinely works.** Direct Analysis deflated the trivial fixture more crisply than the baseline (194w vs 355w) and the RTR fast-path produced the sharpest possible incident opening ("Minute 0–2: Roll back search-service. Immediately."). The skill's proportionality *thresholds* are right; its ceremony discipline on mid-size problems is what fails.

## Adjudication notes
- Baseline run Z6 also drew 5 census hits (method-talk is partly model-endemic) — the skill quadruples it, and Z6 still won both judges' forced choice on its brief on the strength of content.
- Round-3 lessons applied and held: census quoted verbatim in the pre-registration (no instrument split this time); paired sign test pre-registered from the start (no noise-band ambiguity); codes disjoint from judge IDs and dims.
- No goalposts moved; the skill artifact was never modified mid-round (hash verified).
