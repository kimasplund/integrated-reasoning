# integrated-reasoning

A reasoning suite for coding agents: nine reasoning methodologies as installable skills, two infrastructure skills that let them run in parallel and hand context to each other, and a router that picks which one a problem actually calls for — or tells you no framework is warranted at all.

Most reasoning failures aren't bad reasoning inside a method; they're the wrong method — exploring when you should eliminate, optimizing when you should synthesize, running a framework on a question that needed two sentences. The router makes that selection explicit, scored, and auditable; the patterns are the methods it routes to.

## The router — [skills/integrated-reasoning](skills/integrated-reasoning/)

Scores eleven problem characteristics (sequential dependencies, criteria clarity, evidence availability, novelty, time pressure, stakeholder complexity, …), computes weighted affinity scores for all nine patterns with validation rules (nothing to attack → adversarial scores zero; one decision-maker → negotiation scores zero), propagates uncertainty when a ±1 wobble would flip the winner, and orchestrates multi-pattern chains — sequential (explore → optimize → validate), parallel with explicit merge strategies, a 15-minute checkpoint loop, and bounded agreement-based confidence aggregation. A time-pressure fast-path skips straight to triage in emergencies, and a Direct Analysis threshold answers simple questions plainly instead of running the ceremony.

## The patterns

| Skill | The move | Reach for it when |
|---|---|---|
| [tree-of-thoughts](skills/tree-of-thoughts/) | Deep branching exploration with pruning | An optimization with clear criteria — find THE best |
| [breadth-of-thought](skills/breadth-of-thought/) | Wide shallow sweep, conservative pruning | Unknown solution space — you can't afford to miss options |
| [self-reflecting-chain](skills/self-reflecting-chain/) | Stepwise reasoning with per-step validation | Proofs, traces, plans with dependent steps |
| [hypothesis-elimination](skills/hypothesis-elimination/) | Discriminating-evidence elimination | Diagnosis and debugging — find THE cause |
| [adversarial-reasoning](skills/adversarial-reasoning/) | Systematic attack on a candidate solution | Validation before committing — security, robustness |
| [dialectical-reasoning](skills/dialectical-reasoning/) | Thesis–antithesis–synthesis | Genuine trade-offs where both sides are right |
| [analogical-transfer](skills/analogical-transfer/) | Structured cross-domain borrowing | Novel problems with no in-domain precedent |
| [rapid-triage-reasoning](skills/rapid-triage-reasoning/) | Time-boxed good-enough decisions | Incidents and emergencies — minutes, not hours |
| [negotiated-decision-framework](skills/negotiated-decision-framework/) | Multi-stakeholder alignment mechanics | Decisions that need actual buy-in, not just an answer |

## Infrastructure

| Skill | Role |
|---|---|
| [parallel-execution](skills/parallel-execution/) | Fan-out/fan-in for running patterns or branches concurrently, with merge strategies (consensus / voting / aggregation / best-of-N) |
| [reasoning-handover-protocol](skills/reasoning-handover-protocol/) | The `.reasoning/` directory protocol for chaining patterns — state, checkpoints, and context transfer |

## Install

```bash
git clone https://github.com/kimasplund/integrated-reasoning.git /tmp/integrated-reasoning
cp -r /tmp/integrated-reasoning/skills/* ~/.claude/skills/   # Claude Code; ~/.agents/skills/ for Kimi Code
```

Every skill directory is self-contained — copy only what you want. The router functions standalone (its selection methodology and per-pattern guidance are inlined) but routes best with the library installed.

## Validation status — read before relying on it

The router went through a blind, pre-registered with-skill vs no-skill round under [skill-gauntlet](https://github.com/kimasplund/skill-gauntlet) (2026-08-18, frontier-model executors, 16 cells, sealed keys — full record in [evolution/validation-round-1/](evolution/validation-round-1/)). Honest results:

- **NOT VALIDATED for decision-quality lift on a frontier model.** Sealed-key detection and judged decision quality landed at exact parity — the base model already reasoned at ceiling on the test briefs.
- **The restraint machinery passed both gates**: the Direct Analysis threshold deflated a trivial question more crisply than the baseline, and the time-pressure fast-path opened an incident response with the rollback, not analysis.
- **A machinery-leakage defect was found — and fixed in v2.2**: the round-1 router narrated its scoring apparatus (affinity arithmetic, dimension tables) into user-facing answers. A suite-wide backstage clause + mechanical leak scan was then gauntleted as a candidate ([evolution/validation-round-2/](evolution/validation-round-2/)): it eliminated **100% of the leaks** (0 vs 9 in the champion suite's main cells), lifted the leakage guard +1.50 (p=0.004), and recovered both guards to statistical parity with the no-skill baseline. The pre-registered gate still returned NO ADOPTION on one leg — the candidate's planted-key detection came in at 93.8% vs the champion's 100%, a 1.5-point gap made of three torn-edge-call thinness docks with no outright miss — and **v2.2 ships by disclosed owner decision over that gap**, with the residual wording improvement ("rewrite as content — never delete analysis") specified for a future round.

On strong models, treat the router today as reasoning *hygiene and restraint* rather than a quality lift. The routing-for-small-models claim (the original design target — the skill carries a measured 7.5B-model failure-mode guard) is untested; that round is the interesting next experiment.

**The eleven library skills have not been individually gauntleted.** The router round is the only blind validation any part of this suite has; the patterns' historical tests (in the old monorepo's `.reasoning/` artifacts) predate the gauntlet protocol and are not blind or pre-registered. Until per-skill rounds run, treat each pattern as a documented methodology, not a validated one — and expect the router round's machinery-leakage finding to apply to patterns that instruct visible scoring apparatus.

## Provenance

- **v1** router ([evolution/](evolution/)) used an order-dependent decision tree over four patterns.
- **v2** replaced it with weighted scoring over nine patterns plus Direct Analysis, uncertainty propagation, fast-paths, checkpoints, and statistically sane confidence aggregation. **v2.1** fixed the DR formula and added validation rules. Renamed `integrated-reasoning-v2` → `integrated-reasoning` at packaging (versions belong in changelogs, not names).
- **v2.2** (2026-08-18) added the backstage rule to the router and all nine patterns — the method never appears in the answer — plus a mechanical pre-delivery leak scan; adopted from validation round 2 by disclosed owner decision (see Validation status).
- Earlier copies of the whole suite lived in the [claude_cognitive_reasoning](https://github.com/kimasplund/claude_cognitive_reasoning) monorepo; this repo supersedes them with the current, evolved versions and is the maintained home.

## License

[MIT](LICENSE)
