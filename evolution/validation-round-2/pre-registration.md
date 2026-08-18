# Pre-registration — IR suite round 2: backstage candidate (written 2026-08-18, BEFORE any cell)

## Artifacts (frozen)
- **Champion** = current suite (repo HEAD), copied to arms/m1. Suite fingerprint (md5 of per-file md5s) `0daa67285d041bf1e01136155021099c`.
- **Candidate** = champion + backstage deltas ONLY, arms/m2, fingerprint `f34c3bb99b07dd9efe6f53ab5969812f`. Diff-verified: uniform 4-line "Backstage rule" block appended to each of the 9 pattern skills; 2 insertions in the router (silent-scoring clause per RESEARCH-NOTE-ir.md delta 1; mechanical leak scan per delta 2); parallel-execution and reasoning-handover-protocol untouched (agent-internal output — the measured defect cannot apply).
- **Rubric** = round-1 RUBRIC.md verbatim (6 dims; D5/D6 guards).
- Baseline arm m0 (no skill) runs alongside as an INFORMATIONAL reference (recovery target), not a gated comparison, except where condition 5 names it.

## Cells (24)
{m1, m2, m0} × {M diagnostic, N trade-off, P optimization} × 2 runs = 18 main; {trivial, urgent} fixtures × 3 arms = 6. Briefs are fresh (G–L burned). Codes sealed; collision-checked.

## Instruments (mutually blind)
- Judges JC, JD (personas in mapping): each scores all 18 main reports, panels interleave all three arms, dimension-major, forced choice per brief, scores block.
- Auditor AU4, all 24 cells: the round-1 census VERBATIM — count ONLY (a) internal scoring/affinity tables or dimension-scoring arithmetic in the user-facing answer; (b) methodology-selection narration; (c) confidence-aggregation or checkpoint process talk. Content exclusions as round 1 (hypothesis lists, criteria matrices from the question's own criteria, answer-confidence statements are content). D5/D6 per rubric for all 24; restraint gate on the trivial fixtures; fast-path gate on the urgent fixtures.
- Key verifier KV4: 18 main reports vs KEY-MATRIX.md, strict, hard-grader.

## Gate — candidate suite ADOPTED iff ALL of:
1. **Leak recovery (auditor, binding):** candidate main-cell census total < champion total AND candidate total ≤ 3 across its 6 main cells.
2. **D6 (judges):** margin (cand − champ) ≥ +1.0 AND one-sided paired sign test (pairs = brief×run×judge, n=12) p < 0.05.
3. **D5 (judges + auditor):** candidate ≥ champion on judges (no paired sign test against the candidate at p < 0.05) AND auditor D5 mean cand ≥ champ.
4. **No value regression:** D1–D4 — no dim where the champion wins the paired sign test at p < 0.05; keys — candidate strict rate ≥ champion strict rate.
5. **Fixtures:** candidate trivial = DEFLATED and correct; candidate urgent = ACTION-FIRST; candidate fixture census ≤ champion fixture census.
6. **Instrument checks:** inter-judge agreement ≥ 85% within 1 pt; spreads reported. Failure → INVALID, fix instruments.
- **Informational (reported, not gated):** candidate vs baseline m0 on D5/D6 (recovery distance) and keys/D3 (parity check) — round 1's open question.
- Symmetric loss: on FAIL the candidate branch is discarded, deltas return to the research note with failure data. No goalpost movement; conservative reading governs guard disagreements. On ADOPT: deltas merge to the repo as v2.2 before any push; README validation section updated with this round's record.

## Executor protocol
m1/m2: "You have a skill library at <arm dir>. Read integrated-reasoning/SKILL.md first and follow it; when it directs you to another methodology in the library, read that skill's SKILL.md and follow it too. Do not invoke the Skill tool." — byte-identical between arms except the path. m0: helpful-assistant framing, no skills. Same model everywhere.
