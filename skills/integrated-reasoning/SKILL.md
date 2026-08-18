---
name: integrated-reasoning
description: Meta-orchestration guide for choosing and combining optimal reasoning patterns. Scores a problem's characteristics and routes it to the right methodology — debugging, optimization, trade-off synthesis, novel problems, incidents, multi-stakeholder decisions — or to direct analysis when no framework is warranted. Use when facing a complex problem and unsure how to approach it, or when a decision is high-stakes enough that picking the wrong analysis style is itself a risk.
model: opus
user-invocable: true
---

# Integrated Reasoning — Meta-Orchestration

**Purpose**: Select and orchestrate optimal reasoning pattern(s) for your problem. V2 addresses limitations of v1: adds new patterns, replaces order-dependent decision tree with weighted scoring, includes feedback loops, and fixes confidence aggregation.

## Available Reasoning Patterns (9)

| Pattern | Purpose | Best For |
|---------|---------|----------|
| **Tree of Thoughts (ToT)** | Find optimal solution through deep exploration | Optimization, clear criteria, find THE best |
| **Breadth of Thought (BoT)** | Map solution space comprehensively | Unknown space, need multiple options |
| **Self-Reflecting Chain (SRC)** | Sequential reasoning with validation | Dependent steps, proofs, linear traces |
| **Hypothesis-Elimination (HE)** | Systematic elimination through evidence | Diagnosis, debugging, root cause |
| **Adversarial Reasoning (AR)** | Stress-test through attack simulation | Validation, security, pre-mortems |
| **Dialectical Reasoning (DR)** | Synthesize opposing valid perspectives | Trade-offs, conceptual conflicts |
| **Analogical Transfer (AT)** | Solve via cross-domain parallels | Novel problems, no direct precedent |
| **Rapid Triage Reasoning (RTR)** | Fast decisions under time pressure | Incidents, emergencies, time-boxed choices |
| **Negotiated Decision Framework (NDF)** | Multi-stakeholder coordination | Politics, competing interests, buy-in needed |

---

## Pattern Selection: Weighted Multi-Dimensional Scoring

**Turn discipline (all models, critical for small ones):** announcing this
skill or a selected pattern is NOT a response. After any announcement,
continue immediately in the same turn with the pattern's actual steps and
the tool calls that do the work. (Measured failure mode: a 7.5B local model
announced the skill and ended its turn — three times in five runs.)

**The scoring pass is silent.** Dimension scores, affinity arithmetic, pattern names, and selection narration never appear in the user-facing answer — the reader sees the analysis, not the apparatus that chose it. A selected pattern's structure may appear only as content built from the problem's own facts (a hypothesis table, a criteria matrix), never as methodology narration. Announce nothing; just analyze.

### Step 1: Assess Problem Characteristics

Score each dimension (1-5):

```markdown
| Dimension | Score | Description |
|-----------|-------|-------------|
| **Sequential Dependencies** | _/5 | Do steps depend on previous steps? |
| **Criteria Clarity** | _/5 | Can you clearly evaluate solutions? |
| **Solution Space Known** | _/5 | Do you know the options? |
| **Single Answer Needed** | _/5 | Need ONE answer vs multiple options? |
| **Evidence Available** | _/5 | Can you gather discriminating evidence? |
| **Opposing Valid Views** | _/5 | Are there legitimate conflicting perspectives? |
| **Problem Novelty** | _/5 | Is this unprecedented in your domain? |
| **Robustness Required** | _/5 | Need to stress-test before committing? |
| **Solution Exists** | _/5 | Do you have a candidate solution to evaluate? |
| **Time Pressure** | _/5 | How constrained is decision time? (5=minutes) |
| **Stakeholder Complexity** | _/5 | Multiple parties with competing interests? |
```

### Step 1.5: Time Pressure Fast-Path

**CRITICAL**: If Time Pressure = 5 (emergency/incident):
- Skip full scoring
- Use **Rapid Triage Reasoning (RTR)** directly
- RTR is optimized for decisions under extreme time constraints

If Time Pressure ≥ 4:
- Consider RTR unless problem is clearly sequential (use SRC) or diagnostic (use HE)
- Apply abbreviated scoring (skip orchestration considerations)

### Step 2: Calculate Pattern Affinity Scores

```
ToT  = (Criteria × 0.35) + (SingleAnswer × 0.30) + (SpaceKnown × 0.20) + ((6-Novelty) × 0.15)

BoT  = ((6-SpaceKnown) × 0.35) + ((6-SingleAnswer) × 0.30) + ((6-Criteria) × 0.20) + (Novelty × 0.15)

SRC  = (Sequential × 0.45) + (Criteria × 0.25) + (SingleAnswer × 0.20) + ((6-OpposingViews) × 0.10)

HE   = (Evidence × 0.40) + (SingleAnswer × 0.30) + ((6-Novelty) × 0.20) + ((6-OpposingViews) × 0.10)

AR   = (Robustness × 0.40) + (SolutionExists × 0.30) + ((6-Novelty) × 0.15) + (Evidence × 0.15)
       # NOTE: AR requires SolutionExists ≥ 3, otherwise score = 0

DR   = (OpposingViews × 0.50) + (Criteria × 0.20) + ((6-Evidence) × 0.15) + (MIN(SingleAnswer, OpposingViews) × 0.15)
       # NOTE: V2.1 fix - SingleAnswer no longer penalized when OpposingViews is high

AT   = (Novelty × 0.45) + ((6-SpaceKnown) × 0.30) + ((6-Evidence) × 0.15) + ((6-Sequential) × 0.10)

RTR  = (TimePressure × 0.50) + (SingleAnswer × 0.25) + (Evidence × 0.15) + ((6-Novelty) × 0.10)
       # NOTE: RTR auto-selected when TimePressure = 5

NDF  = (StakeholderComplexity × 0.45) + (OpposingViews × 0.25) + ((6-Criteria) × 0.15) + ((6-TimePressure) × 0.15)
       # NOTE: NDF requires StakeholderComplexity ≥ 3 to be considered
```

**Formula Validation Rules** (V2.1):
- AR returns 0 if SolutionExists < 3 (nothing to attack)
- RTR auto-triggers when TimePressure = 5 (emergency mode)
- NDF returns 0 if StakeholderComplexity < 3 (single decision-maker)
- If multiple patterns score within 0.3 of each other, use uncertainty propagation (Step 2.5)

### Step 2.5: Uncertainty Propagation (V2.1)

When dimension scores are uncertain, propagate uncertainty to pattern selection:

```markdown
## Uncertainty Assessment

For each dimension where you're unsure (±1 point uncertainty):

1. Calculate pattern scores at LOW end (dimension - 1)
2. Calculate pattern scores at HIGH end (dimension + 1)
3. If different pattern wins at each end → **Flag as uncertain selection**

### Handling Uncertain Selections

**If same pattern wins both ends**: Proceed with confidence
**If different patterns win**:
  - Run BOTH patterns in parallel (if time permits)
  - OR use the pattern that's more robust to being wrong
  - OR gather more information to reduce dimension uncertainty

### Uncertainty Discount
Apply -5% to final confidence for each uncertain dimension that affects the winning pattern.
```

### Step 3: Interpret Scores

| Scenario | Action |
|----------|--------|
| One pattern scores >4.0 | Use that pattern directly |
| Top 2 within 0.5 of each other | Consider multi-pattern orchestration |
| Top 3 within 0.3 of each other | Apply uncertainty propagation first |
| All patterns <3.0 | Problem may need decomposition first |
| Top pattern <2.5 | None fit well; use Direct Analysis |

### Direct Analysis (10th Pattern)

When all pattern scores are below 2.5, no specialized methodology is warranted:

```markdown
## Direct Analysis

**When to use**: All patterns score <2.5 (problem doesn't match any pattern's strengths)

**Approach**:
1. No special framework needed
2. Just think through the problem directly, step by step
3. Use common sense and straightforward reasoning
4. Appropriate for simple problems that don't need cognitive overhead

**Examples**:
- Simple factual questions
- Straightforward calculations
- Basic lookups or translations
- Problems with obvious solutions

**Confidence**: Use intuitive confidence based on problem clarity
- Clear problem + clear answer = high confidence
- Any ambiguity = document it explicitly
```

### Direct Analysis Threshold

Use Direct Analysis (no specialized pattern) when:
- MAX(all pattern scores) < 4.0 AND
- No dimension scored >= 4 AND
- Problem is not time-critical (TimePressure < 4)

Direct Analysis = simple step-by-step reasoning without framework overhead.

---

## Effort Level Mapping

After selecting the optimal pattern, set effort level:

| Pattern | Default Effort | Max Effort Trigger |
|---------|---------------|-------------------|
| ToT | high | Critical optimization decisions |
| BoT | high | Unknown solution spaces |
| HE | high | Production incidents |
| SRC | medium | Standard step-by-step |
| AR | high | Security/safety validation |
| DR | high | Architecture trade-offs |
| AT | max | Completely novel domains |
| RTR | low | Time-critical (< 5 minutes) |
| NDF | medium | Standard stakeholder alignment |

For max effort, include "ultrathink" in the analysis prompt to enable extended thinking budget.

---

## Execution Modes

Patterns with `context: fork` in their frontmatter will automatically run in isolated subagents when invoked via skills. This means:
- They get their own context window (won't bloat main conversation)
- They can't see conversation history (provide full context in the prompt)
- Results are returned as a summary

For inline execution (when you need conversation context), invoke the pattern's methodology directly without using the skill.

**Patterns that fork**: ToT, BoT, HE, AR, SRC, DR, AT
**Patterns that run inline**: RTR (speed), NDF (needs conversation context for stakeholder interaction)
**IR-v2 itself**: Always inline (orchestrates within main context)

---

## Model-Aware Selection

When running on different models:
- **Opus**: All patterns available at full depth
- **Sonnet**: Reduce branch counts (ToT: 3 branches, BoT: 5 approaches). SRC, HE, RTR work well. DR and AT may lose synthesis quality.
- **Haiku**: Only RTR recommended; other patterns lose quality

---

## Sequential vs Parallel Execution Decision

### Use SEQUENTIAL when:
- Pattern B depends on Pattern A's output (e.g., BoT finds options → ToT optimizes)
- Confidence from A affects what to do in B
- Evidence from A eliminates need for B
- One pattern discovers the problem is different than expected

### Use PARALLEL when:
- Patterns explore independent dimensions
- No dependency between pattern outputs
- Need ensemble confidence (run same problem through 2 patterns)
- Time allows and you want cross-validation
- Patterns are complementary (e.g., different perspectives on same problem)

### Common Chains (Sequential):
```
BoT → ToT → AR     (explore → optimize → validate)
HE → SRC → AR      (diagnose → trace → validate fix)
AT → DR → ToT      (analogize → synthesize → optimize)
RTR → HE → ToT     (triage → root cause → proper fix)
NDF → ToT → AR     (align stakeholders → optimize → validate)
DR → NDF → ToT     (resolve concepts → negotiate politics → optimize)
AT → BoT → ToT     (find analogies → explore adaptations → select best)
```

### Common Parallel Combinations:
```
BoT || AT          (parallel exploration from different angles)
ToT || DR          (optimize while synthesizing trade-offs)
HE branches        (test multiple hypotheses simultaneously)
AR attack vectors  (stress-test from multiple directions at once)
```

---

## Full Reasoning Chain: Input → Output

Complete end-to-end flow for IR-v2 orchestration:

```
1. **Problem Input**
   └─→ Receive problem statement, constraints, and context

2. **Score 11 Dimensions**
   └─→ Sequential Dependencies, Criteria Clarity, Solution Space Known,
       Single Answer Needed, Evidence Available, Opposing Valid Views,
       Problem Novelty, Robustness Required, Solution Exists,
       Time Pressure, Stakeholder Complexity

3. **Fast-Path Check**
   └─→ TimePressure = 5? → RTR immediately (skip remaining steps)

4. **Pattern Selection**
   └─→ IR-v2 calculates affinity scores for all 9 patterns
   └─→ Apply validation rules (AR needs solution, NDF needs stakeholders)

5. **Direct Analysis Check**
   └─→ All patterns < 2.5? → Use Direct Analysis (no framework)

6. **Orchestration Decision**
   └─→ Single pattern > 4.0? → Use that pattern directly
   └─→ Top 2 within 0.5? → Consider multi-pattern orchestration
   └─→ Top 3 within 0.3? → Apply uncertainty propagation first
   └─→ Decide: Sequential or Parallel execution?

7. **Pattern Execution**
   └─→ Apply selected pattern(s) using their methodologies
   └─→ Track confidence and key findings

8. **Checkpoint (15 min)**
   └─→ Progress check, pattern fit check, new information check
   └─→ Re-evaluate and potentially switch patterns if needed

9. **Handover (if multi-pattern)**
   └─→ Transfer context via .reasoning/ directory
   └─→ Preserve insights from completed pattern
   └─→ Set up next pattern with full context

10. **Synthesis (if multi-pattern)**
    └─→ Combine findings from all patterns
    └─→ Resolve conflicts, identify agreements

11. **Confidence Aggregation**
    └─→ Single pattern: use internal confidence
    └─→ Multi-pattern: apply agreement analysis
    └─→ Apply uncertainty discounts if applicable

12. **Output**
    └─→ Deliver answer with reasoning trail
    └─→ Document confidence level and key uncertainties
    └─→ Provide actionable recommendations
```

---

## Multi-Pattern Orchestration

### When to Orchestrate

- Top 2 patterns within 0.5 points AND
- Problem is high-stakes (consequences matter) AND
- Time budget allows (>45 minutes available)

### Orchestration Patterns

**Sequential Orchestration** (most common):
1. Use exploration pattern first (BoT, AT)
2. Use optimization pattern second (ToT, HE)
3. Use validation pattern last (AR, SRC)

**Parallel Orchestration** (when patterns are complementary):
- Run 2 patterns independently
- Compare conclusions
- Use agreement/disagreement to calibrate confidence

**Nested Orchestration** (when patterns address different aspects):
- Apply different patterns to different sub-problems
- Synthesize at the end

### Orchestration Decision Table

| Pattern A High | Pattern B High | Orchestration |
|----------------|----------------|---------------|
| BoT | ToT | Sequential: BoT (explore) → ToT (optimize top options) |
| BoT | HE | Sequential: BoT (generate hypotheses) → HE (eliminate) |
| ToT | AR | Sequential: ToT (select) → AR (validate before commit) |
| ToT | SRC | Sequential: ToT (decide) → SRC (plan implementation) |
| DR | ToT | Sequential: DR (resolve tension) → ToT (optimize within synthesis) |
| AT | ToT | Sequential: AT (find analogies) → ToT (evaluate derived solutions) |
| AT | BoT | Parallel: Both explore, merge findings |
| HE | SRC | Sequential: HE (find cause) → SRC (trace mechanism) |
| RTR | HE | Sequential: RTR (immediate triage) → HE (post-incident RCA) |
| RTR | AR | Sequential: RTR (quick decision) → AR (post-decision validation) |

| NDF | ToT | Sequential: NDF (get buy-in) → ToT (optimize within agreed bounds) |
| NDF | DR | Sequential: DR (resolve conceptual tension) → NDF (negotiate stakeholders) |
| BoT | NDF | Sequential: BoT (explore options) → NDF (negotiate which to pursue) |

**RTR Orchestration Rules**:
- RTR is typically a STARTING pattern, not an ending one
- After RTR stabilizes situation, follow up with deeper analysis
- RTR → HE for incident root cause analysis
- RTR → ToT for revisiting decision with more time

**NDF Orchestration Rules**:
- NDF typically FOLLOWS technical analysis (know options before negotiating)
- BoT → NDF: Explore space, then negotiate which options to pursue
- NDF → ToT: After stakeholder agreement, optimize implementation
- DR → NDF: Resolve conceptual tensions first, then stakeholder tensions

---

## Parallel Pattern Execution

When top 2 patterns score within 0.3:
- Use built-in parallel subagent execution (multiple Task tool calls)
- Each pattern runs in its own forked context
- Merge results using agreement analysis

### When to Parallelize

Parallel execution is appropriate when independent reasoning paths can run concurrently without blocking each other:

| Condition | Parallelization Strategy |
|-----------|-------------------------|
| **Top 2 patterns within 0.3 of each other** | Run both patterns in parallel, compare results |
| **BoT natural parallelism** | 8-10 branches can explore simultaneously |
| **Hypothesis testing (HE)** | Parallel evidence gathering for multiple hypotheses |
| **Multi-perspective needs (MoA pattern)** | Different "expert personas" analyze in parallel |

### Parallel Orchestration Patterns

| Pattern Combination | Parallel Strategy | Merge Approach |
|--------------------|-------------------|----------------|
| **BoT \|\| AT** | Parallel exploration from different angles | Merge findings, deduplicate insights |
| **ToT branches** | Parallel subtree exploration at each level | Take best-scoring subtree |
| **HE hypotheses** | Parallel evidence collection for each hypothesis | Aggregate evidence, eliminate losers |
| **AR attacks** | Parallel threat simulation (different attack vectors) | Union of discovered vulnerabilities |

### Parallel Configuration

```yaml
parallel_config:
  max_concurrent_patterns: 3      # Max patterns running simultaneously
  max_concurrent_branches: 8      # Max branches within a single pattern
  merge_strategy: "consensus"     # "consensus" | "voting" | "aggregation" | "best-of-n"
  timeout_per_branch_ms: 60000    # 60 second timeout per branch
  early_termination_threshold: 0.95  # Stop early if confidence exceeds this
```

**Configuration Guidelines**:
- Use `max_concurrent_patterns: 2` for typical orchestration
- Use `max_concurrent_branches: 8` for BoT exploration
- Increase `timeout_per_branch_ms` for complex sub-problems
- Lower `early_termination_threshold` (e.g., 0.85) when speed matters more than certainty

### Merge Strategies

| Strategy | When to Use | Behavior |
|----------|-------------|----------|
| **Consensus** | High-stakes, need confidence | All must agree → boost confidence by +10%; any disagreement → flag for review |
| **Voting** | Multiple viable options | Majority wins; ties broken by highest individual confidence |
| **Aggregation** | Complementary findings | Synthesize all findings into unified result; no filtering |
| **Best-of-N** | Competitive exploration | Take highest confidence result; discard others |

**Merge Strategy Selection**:
```
If robustness critical → "consensus"
If options are mutually exclusive → "voting"
If findings are additive → "aggregation"
If racing for speed → "best-of-n"
```

### Integration with .reasoning/ Protocol

For the full `.reasoning/` handover protocol (directory structure, branch templates, protocol rules), see the `parallel-execution` skill.

---

## Feedback Loop: 15-Minute Checkpoint

**After 15 minutes of applying selected pattern:**

```markdown
## Checkpoint Evaluation

### Progress Check
- [ ] Have I made meaningful progress toward goal?
- [ ] Is my confidence increasing?

### Pattern Fit Check
- [ ] Am I fighting the methodology?
- [ ] Have I discovered new problem characteristics?

### New Information
- [ ] Has the problem changed?
- [ ] Do my characteristic scores need updating?

### Decision
If 2+ checks FAIL:
  → PAUSE: Re-score characteristics
  → If different pattern scores highest: SWITCH
  → If same pattern: Continue with awareness

If all checks PASS:
  → Continue current pattern
  → Set next checkpoint at 30 min mark
```

---

## Confidence Aggregation (Fixed)

**V1 Problem**: Additive confidence boosting was statistically invalid.

**V2 Approach**: Agreement-based bounded adjustment.

### Single Pattern Confidence
Use the pattern's internal confidence score (per its methodology).

### Multi-Pattern Confidence

```markdown
## Multi-Pattern Synthesis

### Raw Scores
- Pattern A conclusion: [Answer A] at [X]% confidence
- Pattern B conclusion: [Answer B] at [Y]% confidence
- Pattern C conclusion: [Answer C] at [Z]% (if used)

### Agreement Analysis

**FULL AGREEMENT** (same conclusion):
- Final Confidence = MIN(MAX(X, Y, Z) + 5%, 95%)
- Rationale: Independent paths converging increases trust

**Confidence Bounds**:
- Floor: 10% (never report lower confidence)
- Ceiling: 95% (never report higher confidence)

**PARTIAL AGREEMENT** (2/3 agree):
- Final Confidence = (AVG of agreeing × 0.7) + (disagreeing × 0.15)
- Must document the disagreement
- Consider: Why does one pattern disagree?

**NO AGREEMENT** (different conclusions):
- Final Confidence = MIN(X, Y, Z) - 10%
- This is a FEATURE not a bug - disagreement reveals complexity
- Action: Either (a) gather more information, or (b) present trade-offs to stakeholder

### Shared Assumption Discount
If patterns share significant assumptions, apply -5% adjustment.
(Same LLM, same problem framing, same information = shared blind spots)
```

---

## Pattern Limitations Reference

**Tree of Thoughts (ToT)**:
- Requires clear evaluation criteria
- Deep recursion may overfit to evaluation function
- Fixed branching can force artificial distinctions

**Breadth of Thought (BoT)**:
- Cannot truly be "exhaustive"
- 8-10 branches may not cover solution space
- Returns multiple options requiring further decision

**Self-Reflecting Chain (SRC)**:
- Limited by weakest step in chain
- Backtracking is costly
- Assumes linear dependency structure

**Hypothesis-Elimination (HE)**:
- Requires discriminating evidence
- Can only find causes in the hypothesis set
- Time-sensitive (may not suit exploration)

**Adversarial Reasoning (AR)**:
- Requires existing solution to attack
- Can be demoralizing if overused
- May miss non-adversarial failure modes

**Dialectical Reasoning (DR)**:
- Requires genuinely opposing valid views
- Synthesis isn't always possible
- Can be slower than just deciding

**Analogical Transfer (AT)**:
- Analogy quality varies widely
- Source domain may mislead
- Requires creativity in finding parallels

**Rapid Triage Reasoning (RTR)**:
- Sacrifices depth for speed
- May miss optimal solution (accepts "good enough")
- Requires follow-up analysis for important decisions
- Not suitable when time is actually available

**Negotiated Decision Framework (NDF)**:
- Requires multiple genuine stakeholders
- Time-intensive (relationship building takes time)
- May produce suboptimal technical solutions for political acceptance
- Doesn't help when one party has absolute authority

---

## Quick Selection Guide

```
"I need to find the BEST option among known choices"
  → Tree of Thoughts

"I need to explore ALL possible approaches"
  → Breadth of Thought

"I need to trace through a logical chain step by step"
  → Self-Reflecting Chain

"I need to find THE CAUSE of something"
  → Hypothesis-Elimination

"I need to VALIDATE a solution before committing"
  → Adversarial Reasoning

"I'm stuck between two valid but opposing approaches"
  → Dialectical Reasoning

"This problem is novel - no one has solved it in my domain"
  → Analogical Transfer

"I need to decide RIGHT NOW (minutes, not hours)"
  → Rapid Triage Reasoning

"Multiple stakeholders with competing interests must agree"
  → Negotiated Decision Framework

"I'm not sure which to use"
  → Score the dimensions (Step 1)

--- Parallelism Quick-Reference ---

"Top 2 patterns scored within 0.3"
  → Run both in parallel, merge with "consensus" or "voting"

"Need to explore many options fast"
  → Use BoT with max_concurrent_branches: 8

"Testing multiple hypotheses"
  → HE with parallel evidence gathering

"Need diverse perspectives on same problem"
  → MoA pattern: parallel expert personas

"Running parallel but need to merge"
  → consensus (high-stakes) | voting (exclusive) | aggregation (additive) | best-of-n (speed)
```

---

## Before delivering — mechanical leak scan

Run a literal text search over the user-facing answer for: every pattern name and abbreviation in the table above (ToT, BoT, SRC, HE, AR, DR, AT, RTR, NDF and their full names), "affinity", "dimension score", "scoring the", "confidence aggregation", "checkpoint", "pattern". Every hit is rewritten as content or deleted; deliver only at zero unrewritten hits. A hypothesis list, a criteria matrix built from the question's own stated criteria, or a confidence statement about the answer itself ("~85% this is the cause") are content — they stay.

---

## Example Application

**Problem**: "Design our company's approach to AI governance"

### Characteristic Scoring
| Dimension | Score | Reasoning |
|-----------|-------|-----------|
| Sequential Dependencies | 2 | Not really step-by-step |
| Criteria Clarity | 3 | Some criteria, but subjective |
| Solution Space Known | 2 | Emerging field, options unclear |
| Single Answer Needed | 4 | Need one policy |
| Evidence Available | 2 | Few precedents to learn from |
| Opposing Valid Views | 5 | Big tension: innovation vs caution |
| Problem Novelty | 5 | Very new challenge |
| Robustness Required | 4 | High stakes, need validation |
| Solution Exists | 1 | No candidate solution yet |
| Time Pressure | 2 | Strategic decision, not urgent |

### Pattern Affinity Scores
- ToT: (3×.35)+(4×.30)+(2×.20)+(1×.15) = 2.80
- BoT: (4×.35)+(2×.30)+(3×.20)+(5×.15) = 3.35
- SRC: (2×.45)+(3×.25)+(4×.20)+(1×.10) = 2.55
- HE: (2×.40)+(4×.30)+(1×.20)+(1×.10) = 2.30
- AR: **0** (SolutionExists=1 < 3, nothing to attack yet)
- DR: (5×.50)+(3×.20)+(4×.15)+(4×.15) = 4.30
- **AT: (5×.45)+(4×.30)+(4×.15)+(4×.10) = 4.45** ← Highest
- RTR: (2×.50)+(4×.25)+(2×.15)+(1×.10) = 2.40

### Recommendation
**Primary**: Analogical Transfer (4.45) - Look at how other governance challenges were solved
**Secondary**: Dialectical Reasoning (3.80) - Innovation vs caution tension needs synthesis

**Orchestration**: AT → DR → AR
1. Use AT to find analogous governance frameworks (environmental, financial, medical)
2. Use DR to synthesize the innovation/caution tension
3. Use AR to stress-test the proposed governance approach

