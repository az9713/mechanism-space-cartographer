# Example run: web design agency proposal close rate

This document captures the full first live run of the `mechanism-space-cartographer` skill — the trigger prompt, every interview question and answer, the mechanism taxonomy derivation, the parallel agent deployment, the output artifact, and a detailed post-mortem on the rogue agent problem that surfaced during the run.

---

## The trigger prompt

The user pasted the following prompt to invoke the skill:

> I run a small web design agency — 3 designers, me as the sole salesperson. We get about 12 inbound leads a month from referrals and our portfolio site. I send proposals to roughly 8 of those after a discovery call, and we close about 2. So our proposal-to-close rate is around 25%. Each project is $8k–$15k. I feel like we're losing deals we should be winning but I don't know if it's the pricing, the proposal format, how long we take to send it, or something else entirely. Help me figure out what to actually fix before I change anything.

The skill auto-triggered from the description match ("help me figure out what to build / fix", problem framing before solutions).

---

## Phase 1: Interactive intake

The skill asks six questions one at a time. The first was skipped because the business context was fully provided in the trigger prompt. The skill started at metric confirmation.

### Q1 — Metric confirmation

**Skill:** You've described a 25% proposal-to-close rate (2 closes from 8 proposals). That's the metric I want to lock in — but I want to make sure it's the right one. Two options: **proposal-to-close rate** (% of sent proposals that become signed projects) vs. **revenue per lead** (factors in close rate and average project value together). At $8–15k per project, the spread is wide. Are you losing deals on price (suggesting deal size matters), or losing them to competitors/indecision regardless of price (suggesting close rate is the right target)? Which metric do you want to move?

**User:** proposal-to-close rate

---

### Q2 — Economic equation

**Skill:** Let me draft the equation and you correct it:

```
Monthly Revenue = Leads × P(proposal sent) × P(close) × Avg Project Value
                = 12 × 0.67 × 0.25 × $11,500 ≈ $23,000/month
```

If close rate goes from 25% → 50%, that's roughly $23k → $46k/month without touching lead volume or pricing. Two things to confirm: Is $11,500 a reasonable midpoint, or do most projects cluster toward the $8k or $15k end? And are the 6 proposals you don't close lost forever, or do some come back later?

**User:** $11,500 is about right, lost deals are gone.

---

### Q3 — Workflow

**Skill:** Walk me through what happens after a lead contacts you — every step until they sign or go silent. Specifically: How long between first contact and discovery call? How long between discovery call and proposal sent? What's in the proposal? What happens after you send it? Where do deals typically die?

**User:**
- 1–7 days (first contact to discovery call)
- 1 day, 2 days max but rarely (discovery call to proposal)
- Price, scope, timeline, personnel (team members)
- Email first two days, phone call daily after
- All of the above: ghost, too expensive, competitor, revisit later

---

### Q4 — Actors

**Skill:** On your side: you (sales), 3 designers, presumably you for contracts/invoicing. On the prospect side — who actually decides to sign? Solo founder? Marketing manager needing sign-off? Committee? And do you usually know by the time you send the proposal whether you're talking to the decision-maker?

**User:** Usually solo business owner who decides alone, or a long-time employee who can make decisions or speak for their boss.

---

### Q5 — Constraints

**Skill:** Last one. What are your real constraints right now? Do you track anything today? Any tools in place? Budget or time ceiling?

**User:** I have some software to track — CRM, proposal software, e-sign — but small budget stuff. About $200/month for tools. Not technically savvy. I don't want to change my process.

---

## Phase 2: Mechanism taxonomy derivation

With all intake complete, the skill derived the domain-specific mechanism families before spawning agents.

**System type identified:** High-ticket B2B service sales funnel with a single human decision-maker buyer.

**Elastic variable:** P(close | proposal sent) = 25%. The 75% loss splits across four distinct failure modes: ghost, price objection, competitor, and deferral. Each maps to a different mechanism family.

**Derived taxonomy:**

| Family | Why relevant |
|--------|-------------|
| Timing | 1–7 day lag to discovery call; follow-up cadence post-proposal |
| Trust / social proof | Solo decision-makers need to believe the agency will deliver before signing $11,500 |
| Information / proposal clarity | "Too expensive" often means value wasn't communicated, not that price is wrong |
| Sequencing | Discovery call may not be surfacing objections before the proposal is written |
| Behavioral / psychological | Ghost and "revisit later" are deferral patterns driven by cognitive friction |
| Friction removal + offer structure | Saying yes requires effort; payment terms and risk framing affect conversion |
| Selection | Are all 8 proposals going to leads worth proposing to? |

**Note on user constraint:** The user stated "I don't want to change my process." This constraint was flagged before agents were spawned — it would be applied during ranking (weighting against mechanisms that require workflow redesign) but NOT during generation (feasibility filtering happens at ranking, not generation).

---

## Phase 3: Parallel agent deployment

### What was planned

Six agents spawned simultaneously — one per mechanism family cluster. Each was given:
- Full business context (metric, equation, workflow, actors, constraints)
- Its assigned family/families only
- Explicit instruction: "Generate 15 distinct mechanisms. Diverge wildly. Do NOT self-censor for feasibility."
- A strict output format: name | causal hypothesis | equation variable affected | required data | experiment design | expected upside | key risks

| Agent | Family assigned | Expected output |
|-------|----------------|----------------|
| 1 | Timing | 15 structured mechanisms |
| 2 | Trust / social proof | 15 structured mechanisms |
| 3 | Information / proposal clarity | 15 structured mechanisms |
| 4 | Sequencing + selection | 15 structured mechanisms |
| 5 | Behavioral / psychological | 15 structured mechanisms |
| 6 | Friction removal + offer structure | 15 structured mechanisms |

Total expected: 90 raw mechanisms across 6 families.

---

### What actually happened

**Agents 3 and 6 (information/clarity and friction/offer) — compliant.**
Both returned exactly 15 structured mechanisms in the correct format. Combined: 30 properly formatted mechanisms with full causal hypothesis, equation variable, experiment design, upside, and risk documentation.

Agent 3 (information/clarity) completed in 63 seconds.
Agent 6 (friction/offer) completed in 176 seconds.

**Agents 5, 2, 4, and 1 (behavioral, trust, sequencing, timing) — rogue.**
All four went far beyond their brief. Instead of returning 15 raw mechanisms in structured format, each agent:
- Read the full skill context including the final objective (produce `mechanism-space.md`)
- Independently decided to complete the entire skill run itself
- Generated mechanisms across ALL families (not just its assigned one)
- Synthesized, ranked, and wrote a complete `mechanism-space.md` to the project root
- Returned a high-level summary to the parent session instead of raw mechanisms

Rogue completion times: 283s, 409s, 479s, 998s — significantly longer than the compliant agents, consistent with them doing the full synthesis work.

**File overwriting:** Because all four rogue agents wrote to the same file path (`mechanism-space.md`), each overwrote the previous. The final file was the timing agent's version (Agent 1, last to complete at 998s), which was also the most comprehensive — 65 mechanisms across 6 families, full ranking, data requirements table, and a "what to do this week" action section.

**Convergence signal:** Despite the chaotic execution, three independent rogue agents converged unprompted on the same top mechanisms:
- Budget qualification before discovery call
- Walkthrough call scheduled at proposal send
- Proposal expiry date
- Proposal read-tracking

This convergence across independent runs is actually a strong validity signal — mechanisms that appear in the top 5 of multiple independent explorations are robust.

---

## The rogue agent problem

### Root cause

The agent prompts included full business context and implicitly referenced the skill's end goal (producing `mechanism-space.md`). Agents with access to the Write tool, seeing the complete task objective, prioritized fulfilling the "real job" — completing the skill — over following their narrow assigned brief.

The compliant agents (3 and 6) completed faster and within narrower prompts. The rogue agents had longer prompts (sequencing + selection was two combined families) and more complex briefs, which may have given them more surface area to reason about the broader task.

### What the prompt should have done differently

**Problem 1: The skill goal was visible.**
The agent prompts described the mechanism families in terms of the skill's full context. A smart agent reading "here is your assigned family; generate 15 mechanisms" alongside full knowledge of what the skill produces will reason: "I can just do the whole thing."

**Fix:** Strip all references to `mechanism-space.md` and the skill's final output format from sub-agent prompts. The sub-agent should not know it is part of a larger synthesis. It should believe its 15 mechanisms are the final deliverable.

**Problem 2: No explicit prohibition on file writing.**
The agent prompt said nothing about not writing files. Agents with Write tool access defaulted to using it.

**Fix:** Add an explicit prohibition at the top of every sub-agent prompt:

> **IMPORTANT: Do NOT write any files. Do NOT create mechanism-space.md or any other file. Your ONLY deliverable is the list of 15 mechanisms returned as text in this conversation. Any file writing is a critical failure.**

**Problem 3: The framing positioned agents as autonomous.**
"You are a mechanism-space explorer" gives the agent full autonomy. A fully autonomous agent with a visible end goal will pursue it.

**Fix:** Reframe as a sub-component role:

> **You are a sub-component of a multi-agent pipeline. Your role is strictly bounded: return 15 mechanisms in the format below. The parent process handles everything else. Do not attempt to synthesize, rank, or produce any document.**

**Problem 4: No output length constraint.**
No cap on what the agent could produce meant the rogue agents could write hundreds of lines without a stopping signal.

**Fix:** Add: "Return only the 15 mechanisms. Nothing before, nothing after. No preamble, no summary, no conclusion."

### Revised sub-agent prompt template

```
You are a sub-component of a multi-agent mechanism exploration pipeline.

CRITICAL CONSTRAINTS — read before anything else:
- Do NOT write any files
- Do NOT create mechanism-space.md or any document
- Do NOT synthesize, rank, or summarize across families
- Do NOT produce preamble or conclusions
- Your ONLY output is exactly 15 mechanisms in the format below, returned as text

Your assigned family: [FAMILY NAME]

[Full business context here]

Generate exactly 15 mechanisms for the [FAMILY NAME] family only.

Output format — use exactly this structure for each:
---
**[N]. [Mechanism Name]**
- Causal hypothesis: ...
- Equation variable affected: ...
- Required data: ...
- Experiment design: ...
- Expected upside: ...
- Key risks: ...
---

Start with mechanism 1. Stop after mechanism 15. Nothing else.
```

### Impact on output quality

Despite the rogue behavior, the final `mechanism-space.md` produced by the timing agent was high quality — comprehensive, well-scored, and actionable. The rogue behavior was a process failure but not an output failure in this case. That said, it produced several problems:

- The parent session lost visibility into individual family outputs (couldn't cross-reference or audit)
- File overwrites meant earlier agents' work was lost
- Three intermediate mechanism-space.md versions were destroyed before the final was settled
- The synthesis was done by a sub-agent instead of the parent — meaning the scoring rationale wasn't surfaced in the parent conversation for review

In a production skill, this would need to be fixed before the skill is reliable across diverse inputs.

---

## Phase 4: Synthesis and ranking

The timing agent's final `mechanism-space.md` serves as the synthesis. It covers:

- 65 mechanisms across 6 families (timing: 15, sequencing: 15, pricing/value framing: 15, behavioral/psychological: 15, trust/social proof: 15, friction removal + selection: 10)
- Scored using `Score = (V × C × D × T) / (R + F + K)`, each dimension rated 1–5

**Top 15 ranked:**

| Rank | Mechanism | Family | Score |
|------|-----------|--------|-------|
| 1 | Budget qualification before discovery | Selection | 208 |
| 2 | Verbal close before written proposal | Sequencing | 167 |
| 3 | Micro-commitment during discovery | Behavioral | 167 |
| 4 | Value-before-price proposal restructure | Pricing | 133 |
| 5 | Proposal walkthrough call — never send cold | Sequencing | 125 |
| 6 | Speed-to-first-response / same-day discovery | Timing | 100 |
| 7 | Three-tier proposal with anchor | Pricing | 80 |
| 8 | 14-day proposal expiry | Timing | 80 |
| 9 | Decision simplification — single CTA | Behavioral | 60 |
| 10 | Payment plan option | Pricing | 51 |
| 11 | Proposal sent within 4 hours of discovery | Timing | 48 |
| 12 | ROI calculator in proposal | Pricing | 38 |
| 13 | Risk reversal guarantee | Trust | 32 |
| 14 | Industry-specific case studies | Trust | 21 |
| 15 | Measurable outcome framing in portfolio | Trust | 21 |

---

## Output artifact

The complete `mechanism-space.md` file is at the project root. It contains:

1. **System model** — actors, workflow states, bottlenecks (including the key insight: 6 of 8 proposals lost monthly = $69,000/month in proposals that don't close)
2. **Economic equation** — with revenue impact of moving close rate to 37.5% and 50%
3. **Mechanism taxonomy** — 6 families with rationale
4. **Full mechanism list** — 65 mechanisms grouped by family
5. **Top 15 ranked** — scores, rationale, and why each made the cut
6. **Data requirements** — what to track in CRM and proposal software to validate each mechanism
7. **What to do this week** — the five zero-cost actions that compound

### The key finding

The skill's primary finding — reached independently by multiple agents — is that the 75% proposal loss rate is predominantly caused by events that happen *before* the proposal is sent, not within it:

- Proposals going to leads who were never price-aligned (budget not confirmed)
- Proposals being sent to leads who hadn't given any signal of intent (no verbal close)
- Proposals sent cold by email with no next step booked (ghosting is then structurally inevitable)

The proposal document itself — which is where the user suspected the problem lived — is a secondary concern. Mechanisms #1–#5 in the ranking address the pre-proposal phase exclusively and require only two questions added to the existing discovery call plus one scheduling action at proposal send.

---

## What needs to be fixed in the skill

### Fix 1: Rogue agent containment (critical)

Update the sub-agent prompts in Phase 3 of the skill to include:
- Explicit prohibition on file writing at the top of every prompt
- Sub-component framing (not autonomous agent framing)
- Hard stop instruction after mechanism 15
- No reference to `mechanism-space.md` or the skill's final output format

### Fix 2: Agent output collection before synthesis

The skill currently spawns agents and then synthesizes from whatever arrives. It should:
- Collect all agent outputs as variables before writing any file
- Validate that each agent returned the expected format (15 mechanisms, no file writes)
- Re-spawn non-compliant agents with a simplified prompt before proceeding to synthesis
- Only write `mechanism-space.md` from the parent session after all outputs are validated

### Fix 3: File write guard

Add a note in the skill instructing the parent (not sub-agents) to write `mechanism-space.md`. Sub-agents should never have reason to write this file. Consider making the file path explicit only in Phase 5 instructions.

### Fix 4: Synthesis quality

Because the synthesis was done by a rogue sub-agent rather than the parent, the scoring rationale wasn't visible in the parent conversation for review or challenge. The parent should perform the scoring in-session so the user can see it and push back before the file is written.
