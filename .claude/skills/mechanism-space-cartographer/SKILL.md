---
name: mechanism-space-cartographer
description: Use when exploring what to build for a business or project before generating product features. Use when the user wants to enumerate solutions, understand causal levers, map opportunity space, or says "what should I build", "enumerate mechanisms", "how do I improve X", "mechanism space", or "what moves the metric".
---

# Mechanism Space Cartographer

## Overview

Maps all plausible causal levers that move a target metric — before any feature or product decision.

Path: **Problem → Metric → Mechanism Taxonomy → Parallel Exploration → Ranked List → `mechanism-space.md`**

The key move: enumerate mechanisms (causal levers), not features. Features implement mechanisms. Most mechanisms will be trash. That's expected — mine for the few that aren't.

## When NOT to Use

- Features are already decided — go to `agent-skills:spec-driven-development`
- You're debugging or implementing — this is a discovery skill only

## Process

### Phase 1: Interactive Intake

Ask these one at a time, waiting for each answer. Push back on vague answers before moving on.

1. **Business/project** — what is it, what industry, what context?
2. **Target metric** — one metric only. Must be: observable, measurable, causal, frequent. Push back if the user says "engagement" or "productivity" — those are slogans. Good examples: pickup rate, accepted PRs/week, claims accepted first-pass, chair utilization.
3. **Economic equation** — how does this metric connect to revenue, cost, or risk? Help the user write it. Example: `Revenue = Calls × P(pickup) × P(qualified) × P(close) × Deal Value`
4. **Workflow** — walk the states from input to outcome. Where do things wait, fail, or leak value?
5. **Actors** — who acts? Customer, employee, system, vendor, regulator?
6. **Known constraints** — compliance, data availability, integrations, budget, human ops requirements?

### Phase 2: Derive Mechanism Taxonomy

Before spawning agents, derive the domain-specific mechanism families. Ask:

- What system type is this? (queueing system, sales funnel, marketplace, compliance workflow, knowledge-work process, human-behavior system, logistics network…)
- Which variables in the economic equation are elastic and currently unmanaged?
- Map the workflow: at each arrow between states, ask what could go wrong and what mechanism family lives there.

Check the derived families against the universal axes in `mechanism-families-reference.md`. Add any missing ones. Aim for 5–8 domain-specific families.

### Phase 3: Parallel Agent Exploration

Spawn one agent per mechanism family (or cluster 2 small families). Each agent gets:
- Full context: business, metric, economic equation, workflow, actors, constraints
- Its assigned family/families
- This instruction: "Generate 15 distinct mechanisms. Diverge wildly — algorithmic, regulatory, psychological, infrastructural, behavioral. Do NOT self-censor for feasibility."
- Output format per mechanism: name | causal hypothesis | equation variable affected | required data | experiment design | expected upside | key risks

Collect all outputs. Expect duplicates and trash — that's normal.

### Phase 4: Synthesize and Rank

Merge outputs. Remove hard duplicates. Score each remaining mechanism:

```
Score = (V × C × D × T) / (R + F + K)

V = economic value if it works
C = causal plausibility (does the hypothesis hold?)
D = data availability to validate it
T = testability (can it be A/B tested or replay-simulated?)
R = regulatory/compliance risk
F = implementation friction
K = complexity
```

Rank top 15. Annotate why each made the cut.

### Phase 5: Write mechanism-space.md

Write to the project root as `mechanism-space.md`. Structure:

1. **System model** — actors, workflow states, key bottlenecks
2. **Economic equation**
3. **Mechanism taxonomy** — the derived families with one-line rationale for each
4. **Full mechanism list** — all non-duplicate mechanisms grouped by family
5. **Top 15 ranked** — scores, rationale, required data per mechanism
6. **Data requirements** — what historical data is needed to validate each top mechanism

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Vague metric ("improve engagement") | Push until it's measurable. No exceptions. |
| Jumping to features | Name the causal mechanism first; features implement mechanisms |
| Single mechanism family | Force orthogonal divergence across all relevant families |
| Skipping the economic equation | Without it, mechanisms have no value grounding |
| Treating all mechanisms as equal | Score everything; most will be discarded |
| Accepting "this is feasible" as a filter during generation | Feasibility filtering happens at ranking, not generation |

## Verification: Is the Mechanism Space Good?

A good mechanism space has these properties:
1. Contains non-obvious levers (not just "make a dashboard" or "send an email")
2. Every mechanism ties to a specific variable in the economic equation
3. Each mechanism implies a simulation or experiment — if you can't imagine testing it, it's a story not a mechanism
4. The exploration reveals *why the problem is hard* (data gaps, incentive misalignment, regulatory limits)
5. The top mechanisms point clearly to what to build next
