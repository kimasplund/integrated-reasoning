---
name: parallel-execution
description: Parallel execution patterns for cognitive reasoning tasks. Covers built-in Claude Code parallelism (tool batching, background agents, teams, worktrees) and advanced cognitive patterns (DPTS, BSM, MoA, GoT, RASC) for accelerated reasoning with fan-out/fan-in, MCTS-style search, and ensemble aggregation.
license: MIT
---

# Parallel Execution Patterns

## Built-in Parallel Patterns (no skill needed)

Claude Code v2.1.76+ provides native parallelism. Use these first before reaching for advanced patterns.

| Pattern | How | Best For |
|---------|-----|----------|
| **Parallel tool calls** | Multiple tools in single response auto-batch | Reading files, searching, independent lookups |
| **Parallel subagents** | Multiple Task calls with `run_in_background: true` | Independent research, analysis, exploration |
| **Agent teams** | `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` | Coordinated multi-file work with shared tasks |
| **Worktree isolation** | `isolation: worktree` in agent frontmatter | Parallel agents editing code on separate branches |
| **Background execution** | Ctrl+B or `run_in_background: true` | Long-running tasks you don't need results from immediately |

### Parallel Tool Calls

Just make multiple tool calls in one response. Claude Code batches them automatically:

```
# These run in parallel:
- Read file A
- Read file B
- Grep for pattern X
- Glob for *.ts files
```

### Parallel Subagents

Spawn multiple independent agents that run concurrently:

```
# Fan-out: 3 agents in parallel
Task 1 (run_in_background: true): "Analyze database performance"
Task 2 (run_in_background: true): "Review API response times"
Task 3 (run_in_background: true): "Check frontend bundle size"

# Fan-in: collect results
TaskOutput from Task 1, 2, 3
Synthesize findings
```

### Agent Teams

For coordinated parallel work where agents need to communicate:

```
Enable: CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1

Shared task list:
1. Refactor models (no deps)
2. Update routes (depends: 1)
3. Update tests (depends: 1)
4. Update docs (depends: 1, 2, 3)

Agents pick available tasks and coordinate automatically.
```

### Worktree Isolation

For parallel code editing without conflicts:

```yaml
# Agent 1
---
isolation: worktree
model: sonnet
---
"Implement feature A in src/features/a/"

# Agent 2
---
isolation: worktree
model: sonnet
---
"Implement feature B in src/features/b/"
```

Each agent gets its own git worktree. Merge when done.

## When to Parallelize

**Parallelize when:**
- Problem decomposes into independent sub-problems
- Multiple solution approaches need exploration (BoT, ToT branching)
- High confidence required (ensemble methods)
- Multiple hypotheses need simultaneous testing (HE)
- Cross-domain analogies need parallel investigation (AT)

**Do not parallelize when:**
- Steps have sequential dependencies (use SRC instead)
- Each step depends on previous results
- Problem is inherently linear (debugging traces)
- Merge strategy is undefined

### Decision Tree

```
Is the task decomposable?
├─ NO -> Use sequential execution
└─ YES -> Are sub-tasks independent?
    ├─ NO -> Use sequential with checkpoints
    └─ YES -> Is merge strategy clear?
        ├─ NO -> Define merge strategy first
        └─ YES -> How many sub-tasks?
            ├─ 2-3 -> Parallel tool calls or subagents
            ├─ 4-8 -> Subagents with run_in_background
            └─ 8+  -> Agent teams or DPTS pattern
```

---

## Advanced Cognitive Patterns

These patterns complement the built-in execution with structured reasoning strategies.

### Pattern 1: Dynamic Parallel Tree Search (DPTS)

**Efficiency**: 2-4x improvement, 70% faster convergence

Adaptive parallel exploration that dynamically allocates resources to promising branches while pruning unpromising ones.

**Key Mechanisms:**
- Dynamic worker allocation: more workers on promising branches
- Adaptive pruning thresholds: adjust based on best-found confidence
- Early termination: stop when confidence exceeds threshold
- Resource rebalancing: move workers from exhausted/pruned branches

**Integration with ToT:**

```markdown
## DPTS + Tree of Thoughts

### Phase 1: Initial Parallel Expansion
- Spawn N workers for Level 0 branches (N = 5-10)
- Each worker explores one branch independently
- Workers report confidence scores as they complete

### Phase 2: Dynamic Reallocation
- Rank branches by score
- Top 2 branches get 3 workers each for Level 1
- Prune branches below dynamic threshold

### Phase 3: Convergence
- Continue until winning branch > 85% confidence
- OR all branches at Level 4+

### Pruning Threshold Formula
dynamic_threshold = max(0.40, best_confidence - 0.30)
```

**Example:**

```
Level 0: 5 branches, 5 workers (parallel subagents)
├─ Branch A: 75% (3 workers for L1)
├─ Branch B: 68% (2 workers for L1)
├─ Branch C: 52% (1 worker)
├─ Branch D: 48% (1 worker)
└─ Branch E: 35% (PRUNED)

Best found: A.2 at 82% -> New threshold: 52%
-> Branch D PRUNED (48% < 52%)
```

---

### Pattern 2: Branch-Solve-Merge (BSM)

Decompose problems, solve sub-problems in parallel, merge results. Use when problems partition cleanly.

```markdown
## Branch Phase
1. Analyze problem structure
2. Identify independent sub-problems
3. Define interfaces between sub-problems

## Solve Phase (parallel subagents)
1. Spawn worker per sub-problem (run_in_background: true)
2. Each worker solves independently
3. Workers report partial solutions

## Merge Phase
1. Collect all partial solutions
2. Apply merge strategy
3. Resolve conflicts
4. Synthesize final solution
```

**Merge Strategies:**

| Strategy | When to Use | Method |
|----------|-------------|--------|
| **Consensus** | Multiple workers on same problem | Majority agreement |
| **Voting** | Competing approaches | Weighted score aggregation |
| **Aggregation** | Complementary results | Union with deduplication |
| **Synthesis** | Conflicting valid results | Dialectical resolution |

---

### Pattern 3: Mixture of Agents (MoA)

Layered proposer/aggregator architecture for ensemble confidence.

```
┌────────────────────────────────────────┐
│            Aggregator Layer             │
│  (Synthesizes proposals, resolves      │
│   conflicts)                           │
└──────────────────┬─────────────────────┘
                   │ Proposals flow up
┌────────────────────────────────────────┐
│            Proposer Layer               │
│  ┌────────┐ ┌────────┐ ┌────────┐      │
│  │Prop 1  │ │Prop 2  │ │Prop 3  │      │
│  │(ToT)   │ │(BoT)   │ │(AT)    │      │
│  └────────┘ └────────┘ └────────┘      │
└────────────────────────────────────────┘
```

**Conflict Resolution:**
- If proposers agree: Boost confidence +5%
- If 2/3 agree: Use majority, document minority
- If no agreement: Escalate to DR (Dialectical Reasoning)

---

### Pattern 4: Graph of Thoughts (GoT)

Arbitrary graph-structured reasoning with merge, split, and cycle operations.

| Operation | Description | Use Case |
|-----------|-------------|----------|
| **Branch** | Split one thought into multiple | Generate alternatives |
| **Merge** | Combine multiple thoughts into one | Synthesize findings |
| **Refine** | Iterate on a single thought | Improve solution |
| **Backtrack** | Return to previous thought | Error correction |
| **Cycle** | Re-evaluate with new information | Iterative improvement |

```
     [Problem]
        │
    ┌───┴───┐
    ▼       ▼
  [A]     [B]
    │       │
    └───┬───┘
        ▼
    [Merged]
        │
        ▼
    [Refined] <── Cycle
        │
        ▼
    [Solution]
```

---

### Pattern 5: Self-Consistency with RASC

**Efficiency**: 70% compute reduction vs naive self-consistency

Generate multiple reasoning paths, cluster by rationale similarity, use representative answers.

```markdown
## Phase 1: Generate K reasoning paths (parallel subagents)
## Phase 2: Cluster paths by rationale similarity
## Phase 3: Select representative from each cluster
## Phase 4: Weighted vote among representatives

Cluster A (size 3): "team autonomy" -> Microservices
Cluster B (size 3): "scalability" -> Microservices
Cluster C (size 2): "simplicity" -> Monolith

Final: Microservices (70% weighted agreement)
```

---

## Integration with Cognitive Skills

### BoT: Parallel Branch Exploration

```markdown
Spawn 8-10 parallel workers (subagents with run_in_background)
Each explores one approach independently
Pruning after ALL Level 0 complete (static 40% threshold)
```

### ToT: MCTS-Style Search

```markdown
UCB1(branch) = avg_score + C * sqrt(ln(N) / n_branch)
Run parallel rollouts from each branch (subagents)
Use UCB1 to decide which branches get more rollouts
```

### HE: Parallel Hypothesis Testing

```markdown
Phase 1: Generate hypotheses
Phase 2: Identify independent evidence (parallel subagents)
Phase 3: Parallel evidence gathering
Phase 4: Synchronized hypothesis update + elimination
```

### AT: Multi-Perspective via MoA

```markdown
Proposer 1: Technology domain analogies (subagent)
Proposer 2: Nature/biology analogies (subagent)
Proposer 3: Business/economics analogies (subagent)
Aggregator: Cross-analogy synthesis (main session)
```

---

## Pruning Strategies

| Pattern | Threshold | Rationale |
|---------|-----------|-----------|
| **BoT** | Static 40% | Conservative breadth |
| **ToT** | Static top-1/top-2 | Aggressive, find single best |
| **DPTS** | Dynamic (best - 30%) | Adaptive to landscape |
| **HE** | Evidence-based | Prune when evidence eliminates |
| **MoA** | Agreement-based | Prune minority after consensus |

## Anti-Patterns

1. **Parallelizing sequential dependencies** - If step 2 needs step 1's output, don't parallelize
2. **Too many branches** - 5-10 at Level 0 is reasonable; 20+ causes diminishing returns
3. **No merge strategy defined** - Always know how to combine results before fan-out
4. **Ignoring shared state** - Workers must write to isolated paths; merge in fan-in
5. **Parallel everything** - Some problems are inherently sequential; accept it

## Quick Reference

| Situation | Pattern | Why |
|-----------|---------|-----|
| Independent file reads | Parallel tool calls | Built-in, zero overhead |
| Independent research | Parallel subagents | Background execution |
| Coordinated editing | Agent teams / worktrees | Avoid conflicts |
| Explore unknown space | DPTS + BoT | Dynamic pruning, parallel breadth |
| Find optimal option | MCTS + ToT | Simulation-guided depth |
| Decomposable problem | BSM | Clean partition and merge |
| Need ensemble confidence | MoA or RASC | Multiple perspectives |
| Iterative refinement | GoT | Merge and cycle support |
| Test hypotheses | Parallel HE | Independent evidence gathering |

## Configuration

> **Note**: The configuration below is illustrative — these are conceptual thresholds for reasoning about parallelization, not actual Claude Code configuration settings.

```json
{
  "parallel_execution": {
    "max_workers": 10,
    "worker_timeout_minutes": 30,
    "pruning": {
      "bot_threshold": 0.40,
      "tot_keep_top": 2,
      "dpts_margin": 0.30
    },
    "merge_strategies": {
      "default": "aggregation",
      "conflict_resolution": "weighted_voting",
      "confidence_agreement_boost": 0.05
    }
  }
}
```
