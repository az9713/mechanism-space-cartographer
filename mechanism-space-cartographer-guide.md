# The mechanism-space-cartographer: development journey and user guide

This document traces the full arc from a YouTube video about a $1M ARR SaaS to a reusable Claude Code skill. It explains what the host's Clarvo story actually teaches, how an analysis by GPT 5.5 turned one case study into a general framework, why the framework became a skill, and how to apply that skill to your own problems.

---

## The origin: the host's $1M ARR story

The ideas in this document stem from the video [I Built a $1M/y SaaS with Claude Code, Here's How](https://www.youtube.com/watch?v=K65vd9EYbDU&t=13s), in which the host walks through how they built Clarvo — an AI-powered outbound dialer for local service businesses like HVAC, plumbing, and roofing — and grew it to a million dollars in annual recurring revenue using Claude Code. The product solves a specific and expensive problem: sales teams make 100 outbound calls per hour but only 40% of those calls get picked up, which means 60 wasted dials for every hour of a salesperson's day. Clarvo attacks both sides of that equation — more calls per hour, and a higher fraction of those calls answered.

What makes the story instructive is not the product. It's the process the host used to figure out what to build.

### What the host actually did

Before writing a line of code, the host asked Claude to enumerate every possible way to improve a single metric: call pickup rate, defined as the percentage of dialed numbers that result in a live human answering within 10 seconds.

He didn't ask "what should I build?" He didn't ask "what features does a power dialer need?" He gave Claude a precise metric, a baseline, a ceiling, and a target — then instructed it to spawn 10 parallel sub-agents, each proposing 10 distinct mechanisms, forced to diverge across orthogonal categories: algorithmic, behavioral, infrastructural, regulatory, psychological, time-based, identity-based.

The result was 200–300 raw ideas. Most were junk. A temporal propensity model (calling leads at times when pickup is statistically higher) made the cut. Weather-based pickup regression did not. After filtering, roughly five or six ideas were worth building. Two became core features: predictive pacing (dialing multiple leads simultaneously with algorithm-tuned offsets to minimize dead ring time) and optimal call windows (scheduling calls based on historical pickup rates by time of day).

### The key lessons from the transcript

**Lesson 1: Mine mechanisms, not features.**
The host's prompt didn't ask for product features. It asked for causal levers — mechanisms that could move the metric. This is a different cognitive operation. A feature is something you build. A mechanism is a reason why building that thing would change reality. Most people jump straight to features and miss the 90% of the solution space they can't see.

**Lesson 2: Volume of ideas matters because most are trash.**
The host mined 200–300 ideas to find 5–6 worth pursuing. The majority are, by the host's own description, "absolute trash." The value is in the few. You can't find them without generating the many. Generating 10 ideas produces one mediocre one. Generating 300 produces five good ones.

**Lesson 3: Force orthogonal divergence.**
Giving Claude a single prompt like "how do I improve pickup rates?" produces a cluster of similar ideas. Explicitly assigning mechanism categories — algorithmic, behavioral, infrastructural, regulatory — forces the model to search different regions of the solution space. This is the move that surfaces non-obvious mechanisms.

**Lesson 4: Data is the moat, not the software.**
The host notes that the simulation phase — feeding 50,000 historical call records into a Bayesian optimization harness to find the right pacing offset — required data that most people don't have. The company already ran call-heavy operations. The software was replicable. The data was not.

**Lesson 5: Pick problems that pay.**
The pricing insight is blunt: solve a problem that's already costing someone millions, price at 10–15% of the value you create, and work with companies that have enough seats to generate meaningful MRR from a single deal. Low-touch, low-ticket SaaS is increasingly vulnerable to replication. High-touch SaaS with regulatory moats (A2P phone number registration, HIPAA compliance, human onboarding requirements) is structurally harder to copy.

**Lesson 6: Avoid framework churn.**
The host tried ~50 different agent frameworks and libraries. The verdict: each switch introduced regression, confused the model's assumptions about the codebase, and pulled time away from the actual product. The intelligence comes from the base model, not the wrapper. Vanilla Claude with a clean CLAUDE.md outperforms fancy frameworks in production use.

---

## From case study to framework: what the GPT 5.5 analysis formalized

An analysis by GPT 5.5 took the host's workflow and reverse-engineered it into a general framework. The central question it answered: how do you know what mechanism categories to generate? The host listed "algorithmic, behavioral, infrastructural, regulatory, psychological, time-based, identity-based" — but nothing in the phrase "pickup rate" tells you to generate those categories. Where do they come from?

### The answer: derive categories from the causal anatomy of the system

The categories aren't memorized. They're derived. For outbound calling, pickup rate is a function of several causal variables:

| Causal variable | Mechanism family it induces |
|----------------|----------------------------|
| Call timing | Time-based |
| Caller identity / number | Identity / trust |
| Recipient's mental state | Psychological / behavioral |
| Number spam score | Infrastructural |
| Dial cadence | Algorithmic / pacing |
| Regulatory rules | Regulatory |

The host's categories map directly to the causal structure of the system. They aren't arbitrary. You can derive the right mechanism families for any system by decomposing the target metric into its causal variables, then asking which of those variables are controllable or influenceable.

### The formal definition

A **mechanism** is a plausible causal intervention that changes at least one variable in the system such that the expected value of the target metric shifts. A **mechanism space** is the set of all such mechanisms.

Formally:

```
Metric Y = f(x₁, x₂, ..., xₙ)

A mechanism m is anything that changes one or more xᵢ
such that ∂E[Y]/∂m ≠ 0 under real-world constraints.

Mechanism space M = {m₁, m₂, ..., mₙ}
```

In plain language: all the ways reality could be bent to improve the metric.

### The 14 universal mechanism families

The analysis identified 14 families that recur across most business systems:

| Family | Core question |
|--------|--------------|
| Timing | Does the same action have higher value at a different time? |
| Selection | Are some opportunities much more valuable than others? |
| Routing / matching | Does outcome depend on which actor handles which case? |
| Pacing / queueing | Is value lost to idleness or overload? |
| Information | Would better info at decision time change the action? |
| Trust / identity | Does the target hesitate due to lack of trust? |
| Behavioral / psychological | Does human attention, habit, or fear drive the outcome? |
| Incentives | Are people behaving rationally under bad incentives? |
| Constraints / compliance | Is value blocked by rules, approvals, or credentials? |
| Infrastructure | Does the technical substrate affect deliverability? |
| Sequencing | Would a different step order reduce drop-off? |
| Automation | Can a high-volume human step be replaced? |
| Friction removal | Where do users abandon due to effort or ambiguity? |
| Learning loops | Does each transaction produce data to improve future decisions? |

Not all 14 apply to every system. The right set is derived from the system's causal structure, not copied from a generic list.

### The path from problem to product

Most builders follow a two-step path: problem → features. The framework inserts three steps in between:

```
Problem → Metric → Mechanism → Simulation → Feature → Product → Pricing
```

The mechanism step is where the exploration happens. The simulation step is where mechanisms get validated against historical data before being built. Most people skip both and build the wrong thing.

### The mechanism grammar

Every mechanism can be written as a sentence:

> Change **[control variable]** for **[actor/object]** at **[point in workflow]** using **[information/constraint]** so that **[causal variable]** moves **[target metric]**.

Example from Clarvo:
> Change *call timing* for *each lead* before *first outreach* using *historical pickup patterns* so that *answer probability* increases *pickup rate*.

If a proposed mechanism can't be written in this form, it's probably a feature or a story, not a mechanism.

### The scoring matrix

After generating mechanisms, you rank them. The framework provides a scoring formula:

```
Score = (V × C × D × T) / (R + F + K)

V = economic value if the mechanism works
C = causal plausibility (is the hypothesis believable?)
D = data availability to validate it
T = testability (can it be A/B tested or replayed?)
R = regulatory / compliance risk
F = implementation friction
K = complexity
```

This isn't meant to produce precise numbers. It's meant to force explicit reasoning about each mechanism across all dimensions simultaneously, so you don't unconsciously rank "build a dashboard" above "redesign the incentive structure" because the former is easier to build.

---

## From framework to skill

The mechanism space framework is powerful but cognitively demanding. Without structure, a user will:
- Ask "what should I build?" instead of "what metric am I moving?"
- Jump to features when they hit the first plausible-sounding idea
- Generate mechanisms only within a single family (e.g., all timing, no routing)
- Skip the economic equation and lose the ability to rank by value
- Treat all mechanisms as equal rather than scoring and ranking

A skill encodes the right process so it runs reliably without the user needing to hold the full framework in their head.

### Design decisions made during skill creation

Five decisions shaped the final skill:

**1. Interactive intake, not a single prompt.**
The skill asks six questions one at a time before doing anything. This is because mechanism exploration is only as good as the problem definition beneath it. A vague metric produces a vague mechanism space. The skill pushes back on answers like "improve engagement" before proceeding.

**2. Derive the taxonomy, don't copy a generic list.**
The skill derives domain-specific mechanism families from the system's causal structure before spawning exploration agents. The host's list (algorithmic, behavioral, etc.) worked for outbound calling. It would be wrong for medical claims processing. The skill asks: what kind of system is this, what variables drive the metric, which variables are controllable — then derives the right families.

**3. Parallel agents for orthogonal divergence.**
The skill spawns one agent per mechanism family. Each agent sees only its assigned families and is explicitly told not to self-censor for feasibility. This enforces the divergence the host described. If a single agent handles all families, it naturally gravitates toward the easiest ideas in each family and produces a homogeneous cluster.

**4. Scoring is mandatory, not optional.**
Every mechanism gets scored using V×C×D×T/(R+F+K). No mechanism ranks in the top 15 without justification. This prevents the common failure where the most feasible mechanisms rank highest regardless of value.

**5. Output is a file, not a conversation.**
The skill writes `mechanism-space.md` to the project root. This makes the output durable, reviewable, and usable as input for the next phase (spec writing, simulation design, stakeholder review). A conversation artifact disappears. A file persists.

**Two decisions deferred for future versions:**
- Skill chaining: after `mechanism-space.md` is written, the skill should suggest running `agent-skills:spec-driven-development` on the top mechanisms. Not implemented in v1 to keep the skill lean.
- Simulation guidance: the skill currently stops at the ranked list. A Phase 6 covering how to design simulation harnesses and run Bayesian optimization on mechanism parameters is planned but deferred — it requires proprietary historical data that most users won't have on first run.

---

## The skill: what, why, and how

### What it is

`mechanism-space-cartographer` is a five-phase Claude Code skill that takes a business problem and produces a `mechanism-space.md` file containing: the system model, economic equation, derived mechanism taxonomy, full mechanism list grouped by family, and a scored top-15 ranked list with data requirements for each.

### Why it exists

The core failure mode in software product development is building the wrong thing confidently. Teams ship features based on what's easy to build, what users ask for, or what competitors have — not based on what causally moves the metric that makes the business work. The skill exists to create a mandatory pause between "I have a problem" and "I'm writing a spec," and to fill that pause with a rigorous exploration of the full causal lever space.

As the host put it: the bottleneck used to be "can we build it?" It's now "what should we build?" Anybody can convert tokens into product. The skill addresses the second question.

### How it works (the five phases)

**Phase 1 — Interactive intake.**
The skill asks six questions, one at a time, and waits for each answer: business/project, target metric (pushes back on vague answers), economic equation, workflow states, actors, and known constraints. A vague metric at this stage invalidates everything downstream.

**Phase 2 — Derive mechanism taxonomy.**
Using the answers from Phase 1, the skill maps the system type, identifies the elastic variables in the economic equation, and derives 5–8 domain-specific mechanism families. It checks these against the 14 universal families and adds any that are relevant but missing.

**Phase 3 — Parallel agent exploration.**
One agent per mechanism family is spawned in parallel. Each receives full context (business, metric, equation, workflow, actors, constraints) plus its assigned families and explicit instructions to diverge wildly and not self-censor. Each agent produces 15 mechanisms in a structured format: name, causal hypothesis, equation variable affected, required data, experiment design, expected upside, key risks.

**Phase 4 — Synthesize and rank.**
All agent outputs are merged. Hard duplicates are removed. Every remaining mechanism is scored using V×C×D×T/(R+F+K). The top 15 are annotated with rationale.

**Phase 5 — Write mechanism-space.md.**
The output file contains: system model, economic equation, mechanism taxonomy, full mechanism list grouped by family, top-15 ranked with scores, and data requirements for each top mechanism.

---

## Applying the skill: five examples

### Example 1: Outbound pest control

**Scenario:** Owner of a Phoenix pest control company. Three salespeople make outbound calls to Google Ads leads. Lead cost is $45. Close rate is ~8 deals per week across the team. Owner wants to "build software" but doesn't know what.

**What you tell the skill:**
- Business: residential pest control, outbound sales to homeowners, Phoenix market
- Metric: deals closed per salesperson per week
- Economic equation: Revenue = Leads × P(contact) × P(qualified) × P(close) × Average Job Value
- Workflow: lead arrives → assigned to rep → rep calls → no answer / voicemail / live conversation → qualification → quote → close
- Actors: sales reps, homeowners, dispatch
- Constraints: no existing CRM data, all manual today

**What the skill surfaces:**
- *Timing*: call leads within 60 seconds of form submission (research shows contact probability drops 90% after 5 minutes)
- *Sequencing*: qualify inbound intent before assigning to expensive rep time
- *Pacing*: predictive dialing to eliminate dead ring time
- *Trust/identity*: local Phoenix number display vs. toll-free (increases pickup)
- *Selection*: rank leads by estimated job value before calling
- *Incentives*: rep comp on qualified conversations, not raw call count
- *Information*: show rep lead source, property size, and prior pest history before dialing

**Top-ranked mechanism:** Speed-to-lead. A timing mechanism: call within 60 seconds. Requires a CRM with auto-assignment and a click-to-call integration. High value, high causal plausibility, requires only basic data (timestamp of form submission vs. timestamp of first call), testable with a simple A/B split.

**What gets discarded:** Weather-based call timing regression (data exists, but causal signal likely swamped by time-of-day effects already captured in the timing mechanism). Multi-channel simultaneous outreach (compliance risk, TCPA exposure).

---

### Example 2: Dental clinic no-shows

**Scenario:** Two-person startup building SaaS for independent dental clinics. First customer loses ~$800/day to no-shows and last-minute cancellations. Dentist manually calls patients the day before as reminders.

**What you tell the skill:**
- Business: dental practice management SaaS, US market
- Metric: no-show rate (percentage of scheduled appointments where patient does not appear)
- Economic equation: Revenue = Scheduled Appointments × (1 − No-Show Rate) × Average Reimbursement − Overhead
- Workflow: appointment booked → reminder sent → patient confirms or doesn't → day of appointment → patient arrives or no-shows
- Actors: dentist, front desk staff, patients, insurance payers
- Constraints: HIPAA compliance required, integration with existing PMS (Dentrix, Eaglesoft), limited dev capacity

**What the skill surfaces:**
- *Prediction*: no-show probability scoring per patient (using history, appointment type, insurance status, day-of-week)
- *Timing*: send reminders at patient-specific best-response times, not a fixed "day before" schedule
- *Incentives*: require a card on file or small deposit for high-risk appointments
- *Friction removal*: one-click confirm/reschedule via SMS link (no phone call required)
- *Sequencing*: verify insurance eligibility before appointment, not day-of
- *Routing*: fill cancellations from a smart waitlist using predicted no-show windows
- *Trust*: send reminders from a branded local number, not a generic SMS short code

**Top-ranked mechanism:** Smart waitlist + no-show prediction. Combines a selection mechanism (predict which appointments are high-risk) with a routing mechanism (automatically offer those slots to waitlist patients). High value (each recovered slot is worth $200–$400), high causal plausibility, data available in the existing PMS.

---

### Example 3: Medical claims denial reduction

**Scenario:** Revenue cycle management company helping small physician practices. Average first-pass denial rate is 22%. Each denied claim costs ~$25 to rework and takes 45 days to resolve.

**What you tell the skill:**
- Business: RCM SaaS for physician practices, US health insurance market
- Metric: first-pass acceptance rate (% of claims accepted by payer on initial submission)
- Economic equation: Net Collections = Claims Submitted × P(accepted first-pass) × Average Claim Value + P(accepted after appeal) × Appeal Value − Rework Cost − Write-offs
- Workflow: patient visit → charge capture → coding → claim submission → payer adjudication → accepted / denied → rework / appeal
- Actors: coder, biller, payer (insurance), physician, patient
- Constraints: HIPAA, payer-specific rule sets that change frequently, ICD-10/CPT coding standards

**What the skill surfaces:**
- *Information*: pre-submission claim linting against payer-specific rules before submission
- *Documentation*: missing-field detector catches incomplete claims before they leave the practice
- *Coding accuracy*: ICD-10 / CPT mismatch checker flags code combinations that reliably trigger denials
- *Sequencing*: prior authorization requirement predictor flags procedures that need pre-auth before scheduling, not after the visit
- *Eligibility*: real-time insurance eligibility verification at the time of appointment booking, not day-of
- *Selection*: route high-complexity or high-value claims to senior coders before submission
- *Learning loops*: denial pattern learning per payer — each denial feeds back into the linting rules
- *Timing*: appeal deadline escalation system (most practices miss the appeal window entirely)

**Top-ranked mechanism:** Pre-submission payer-specific claim linting. An information mechanism that catches the most common denial triggers before the claim leaves the system. High value (each prevented denial saves $25 + 45 days), high causal plausibility (payers publish their rules), highly testable (A/B on claim batches).

**What gets discarded:** Weather-based claim timing (no causal basis). Generic chatbot for patient communication (doesn't touch the claim submission workflow).

---

### Example 4: B2B SaaS churn reduction

**Scenario:** Project management SaaS for architecture firms. 180 customers at $299/month, 6% monthly churn. Three internal hypotheses: bad onboarding, missing integrations, pricing misalignment.

**What you tell the skill:**
- Business: vertical SaaS for architecture firms
- Metric: monthly churn rate (% of active customers canceling in a given month)
- Economic equation: MRR = Active Customers × $299 − Churned MRR + Expansion MRR
- Workflow: customer signs up → onboarding → first project created → ongoing usage → renewal decision
- Actors: firm principal, project managers, junior architects, billing admin
- Constraints: small team, no ML infrastructure, limited customer success headcount

**What the skill surfaces:**
- *Prediction*: churn probability scoring using engagement signals (login frequency, features used, project count, last active date)
- *Timing*: intervene before churn probability crosses a threshold, not after the cancellation email
- *Information*: give customer success reps a pre-call brief showing exactly which features the churning account has never used
- *Sequencing*: define "successful onboarding" as completion of first real project, not completion of setup checklist
- *Incentives*: offer annual prepay discount during the first 90 days when intent is highest
- *Routing*: high-value or high-risk accounts get a named CSM; small accounts get automated success flows
- *Behavioral*: in-app progress indicators showing time-to-value milestones ("You've saved 4 hours this week")
- *Friction removal*: eliminate the steps between signup and first project created — reduce onboarding to one action

**Top-ranked mechanism:** Churn prediction + proactive intervention. A selection mechanism (identify at-risk accounts before they decide to churn) combined with a timing mechanism (intervene during the decision window, not after). Testable with existing data (login logs, feature usage events already in the product database).

**Key insight from the exploration:** All three internal hypotheses (onboarding, integrations, pricing) may be correct, but for different customer segments. Architecture firms with fewer than 5 seats churn differently than firms with 20+ seats. Routing and selection mechanisms should segment the churn intervention by firm size.

---

### Example 5: AI coding agent PR acceptance rate

**Scenario:** Team building an internal AI coding agent. The agent submits pull requests but only 40% get merged without significant rework. The team keeps adding features to the agent but acceptance rate doesn't improve.

**What you tell the skill:**
- Business: internal AI coding agent (not commercial SaaS)
- Metric: accepted PR rate (% of agent-submitted PRs merged without major revision)
- Economic equation: Developer Leverage = Agent PRs Accepted / Human Hours Invested − Rework Cost
- Workflow: issue assigned to agent → agent reads context → agent implements → agent submits PR → human reviews → accepted / revision requested / rejected
- Actors: agent, human reviewer, CI/CD system, codebase
- Constraints: codebase has ~200k lines, multiple owners, strong style guidelines

**What the skill surfaces:**
- *Context*: context pack builder — agent reads only the relevant files/specs, not the entire codebase
- *Decomposition*: task contract generator — agent writes a one-paragraph spec before coding, human approves before implementation begins
- *Verification*: test-first loop — agent writes failing tests before implementation, runs them before submitting PR
- *Scope control*: diff size budget — agent commits to touching at most N files before starting
- *Trust*: human-readable rationale attached to every PR explaining why each change was made
- *Memory*: rejected-PR miner — agent learns from the reasons past PRs were rejected
- *Coordination*: merge-conflict coordinator — limits parallel agent tasks on the same files
- *Review*: adversarial reviewer agent — a second agent reviews the first agent's PR before submission

**Top-ranked mechanism:** Task contract + human approval before implementation. A sequencing mechanism: the agent proposes a plan before writing code, and a human approves the plan. This catches misunderstandings before expensive implementation, not after. Zero data requirements. Testable immediately.

**What gets discarded:** Generating more test coverage (addresses symptoms, not causes — the agent is implementing the wrong thing, not implementing the right thing badly). Switching models (the problem is context and planning, not raw capability).

---

## What to expect when running the skill

### Phase 1: expect pushback

The skill will push back if your metric is vague. "Improve user engagement" will generate a follow-up: what exactly is engagement, how is it measured, and what's the unit? This isn't pedantry — a vague metric produces a mechanism space that's impossible to rank by value. Budget 5–10 minutes for the intake conversation.

### Phase 2: expect unfamiliar categories

The derived mechanism taxonomy will probably include at least one family that surprises you. That's a signal the exploration is working. If the taxonomy looks exactly like your mental model of the problem, the skill hasn't added value yet.

### Phase 3: expect noise

The parallel agent outputs will include junk. Some mechanisms will be obvious. Some will be unrealistic. Some will be duplicates phrased differently. This is expected and intentional — the generation phase is deliberately non-judgmental. The ranking phase filters.

### Phase 4: expect the top mechanisms to be uncomfortable

The mechanisms that rank highest under V×C×D×T/(R+F+K) are often not the ones you wanted to build. A pricing mechanism may outrank a feature. A human process change may outrank software. An incentive restructuring may outrank both. This is the point — if you already knew what to build, you wouldn't need the skill.

### The output: mechanism-space.md

The file written to your project root contains:
1. System model (actors, workflow, bottlenecks)
2. Economic equation
3. Mechanism taxonomy (the derived families)
4. Full mechanism list grouped by family
5. Top 15 ranked with scores and rationale
6. Data requirements for each top mechanism

This file is the input to your next phase — whether that's writing a spec, designing a simulation, or presenting options to stakeholders. It is not a roadmap. It is a map of the opportunity space. You still have to decide which mechanisms to pursue, in what order, with what resources.

### What the skill won't do

- It won't tell you which mechanism to build. It will tell you which mechanisms are worth building and why.
- It won't validate your existing roadmap. It will surface mechanisms your roadmap may have missed.
- It won't do the simulation. Validating a mechanism against historical data is a separate phase.
- It won't write the spec. The next skill for that is `agent-skills:spec-driven-development`.

---

## The deeper insight

The host's video makes one point above all others: the bottleneck in software product development has shifted. Building is no longer the constraint — anyone can convert tokens into a working product. The constraint is knowing what to build.

The mechanism space cartographer is a response to that shift. It doesn't help you build faster. It helps you decide what to build before you start building — by mapping the full causal structure of the problem you're trying to solve, scoring every lever by value and feasibility, and surfacing the mechanisms that are non-obvious, defensible, and worth pursuing.

Most product teams skip this step. They have a problem, they generate three ideas in a 30-minute meeting, they build the one that feels most achievable. The skill doesn't make that impossible. It makes the cost of skipping it visible.
