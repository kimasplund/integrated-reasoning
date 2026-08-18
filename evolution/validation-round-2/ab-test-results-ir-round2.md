# IR suite round 2: backstage candidate vs champion suite (+ no-skill baseline reference)

**Pre-registered spec:** `pre-registration.md`, sealed before execution. Champion fingerprint `0daa6728…`, candidate `f34c3bb9…`. Executors: session model (all 24 cells, incl. 1 same-model re-run after a server error; file verified absent before re-run — no duplicate). Instruments: Sonnet ×4 (budget decision; within-round fairness intact — every instrument scores all arms).

## Verdict: NO ADOPTION on the pre-registered gate — condition 4's key leg. Five of six conditions pass, including total leak elimination.

| # | Condition | Result |
|---|---|---|
| 1 | Leak recovery: cand < champ AND ≤3 | **PASS — total elimination**: 0 leaks vs champion's 9 across main cells |
| 2 | D6 ≥ +1.0, sign p<0.05 | **PASS** — +1.50, 8W/0L/4T, p=0.0039; candidate D6 5.00 on both instruments |
| 3 | D5 no regression, both instruments | **PASS** — judges +0.58 (7W/2L); auditor 3.67 vs 3.33 |
| 4 | No value regression (D1–D4 sign tests; keys ≥ champion) | D1–D4: no significant regression (worst: D4 0W/3L, p=0.125). **Keys: FAIL** — candidate 22.5/24 (93.8%) vs champion 24/24 (100%) |
| 5 | Fixtures | **PASS** — all arms deflated the trivial and went action-first on the urgent; candidate census 0 |
| 6 | Instrument checks | **PASS** — 105/108 (97%) within 1 pt |

**Informational (round-1's open question): the fix fully recovers the suite to baseline.** Candidate vs no-skill baseline: D6 5.00 vs 4.92, D5 4.17 vs 4.50, total 28.25 vs 28.50 — statistical parity across the board, leaks 0 vs 0. The round-1 gap (−1.83 D6, 22 leaks) is gone.

## The failing leg, examined honestly
The 1.5 lost key-points are three hard-grader edge calls, all "torn → lowered" for *thinness relative to siblings*, two of them on a single report (M5: political-mechanics step present but thinner; revisit rule a bare gesture) and one on N6 (thinnest sensitivity section; the baseline's P7 took the identical dock). No key was missed outright; no wrong mechanism anywhere. Plausible causal story, worth taking seriously rather than dismissing as noise: the leak scan's "rewrite every hit as content **or delete it**" creates deletion pressure that can trim borderline-but-real content (sensitivity sections, follow-through steps). D2/D4's small negative margins (−0.25, n.s.) rhyme with this. **v-next wording fix, one clause: "rewrite as content — never delete analysis; if a hit guards real reasoning, the reasoning stays and only the narration goes."**

## Adjudication notes
- Champion's 24/24 keys this round (vs 97.9% in round 1) shows the champion-suite's *content* was never the problem — its leaks were. The candidate kept the content at 93.8% while silencing 100% of the leaks.
- Pre-registration discipline held: the key condition text ("candidate strict rate ≥ champion strict rate") decides; no noise provision was registered for keys, so none is applied. Both readings reported. Symmetric-loss clause applies: the v2.2 branch is discarded as a candidate; its deltas and this failure data go to the research note.
- Model split (executors on session model, instruments on Sonnet) recorded; all comparisons within-instrument and within-arm-model — no fairness impact.

---
## Post-round addendum (2026-08-18, after verdict delivery)
Owner decision, outside the harness: the backstage candidate adopted and shipped as suite
v2.2 with full disclosure. Rationale on record: the certain, measured win (100% leak
elimination, both guards recovered to no-skill baseline parity, D6 +1.50 at p=0.004) against
a 1.5-point key drop composed of three torn→lowered thinness edge calls with no outright
miss. The harness verdict above stands unchanged as the round's record; the v2.2b wording
improvement ("rewrite as content — never delete analysis") remains specified for any future
round. The gate was not bent; it was overridden, and this addendum is the disclosure.
