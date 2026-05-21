# Mechanism Families Reference

## Universal Mechanism Families

| Family | Core question | Clue in workflow |
|--------|--------------|-----------------|
| **Timing** | Does the same action have higher value at a different time? | Delays, decay curves, urgency windows |
| **Selection** | Are some opportunities much more valuable or winnable? | Heterogeneous outcomes by customer/case |
| **Routing/Matching** | Does outcome depend on which actor handles which case? | Outcome variance by agent, provider, rep |
| **Pacing/Queueing** | Is value lost to idleness or overload? | Resources idle or overwhelmed |
| **Information** | Would better info at decision time change the action? | Actors deciding with incomplete data |
| **Trust/Identity** | Does the target hesitate due to lack of trust? | Screening, skepticism, unknown sender |
| **Behavioral/Psychological** | Does human attention, habit, or fear drive the outcome? | Dropout, abandonment, procrastination |
| **Incentives** | Are people behaving rationally under bad incentives? | Comp aligned to wrong metric |
| **Constraints/Compliance** | Is value blocked by rules, approvals, or credentials? | Regulatory gates, bureaucratic steps |
| **Infrastructure** | Does the technical substrate affect deliverability? | Latency, reliability, throughput, cost |
| **Sequencing** | Would a different step order reduce drop-off? | Pre-conditions done too late |
| **Automation** | Can a high-volume repetitive human step be replaced? | Same judgment made 100× per day |
| **Friction Removal** | Where do users abandon due to effort or ambiguity? | Form fields, approval hops, unclear CTAs |
| **Learning Loops** | Does each transaction produce data to improve future decisions? | No feedback from outcomes to decisions |

## Domain-Specific Libraries

**Sales/GTM:** targeting, lead scoring, outreach timing, message framing, channel selection, trust/identity, objection handling, follow-up cadence, sales routing, pricing/packaging

**Healthcare:** triage, eligibility verification, documentation QA, coding accuracy, prior authorization, adherence, scheduling optimization, clinical escalation, auditability, privacy/compliance

**Finance/Lending:** underwriting, risk scoring, fraud detection, pricing, collections prioritization, liquidity timing, compliance checks, portfolio monitoring

**Logistics:** routing, dispatch, capacity planning, inventory positioning, ETA prediction, load balancing, exception handling, maintenance scheduling

**Software Engineering:** context selection, task decomposition, test generation, code review, regression prevention, deployment gating, observability, memory, agent coordination

**Education:** misconception diagnosis, sequencing/scaffolding, retrieval practice, feedback loops, transfer problems, motivation, personalization, mastery gating

## Mechanism Grammar

Every mechanism is a sentence:

> Change **[CONTROL VARIABLE]** for **[ACTOR/OBJECT]** at **[POINT IN WORKFLOW]** using **[INFORMATION/CONSTRAINT]** so that **[CAUSAL VARIABLE]** moves **[TARGET METRIC]**.

Example:
> Change *call timing* for *each lead* before *first outreach* using *historical pickup patterns* so that *answer probability* increases *pickup rate*.

Use this grammar when an agent's proposed mechanism is vague — ask it to fill in the slots.

## System Type → Natural Mechanism Families

| System type | Natural mechanism families |
|-------------|--------------------------|
| Queueing system | pacing, batching, prioritization, capacity, routing |
| Sales funnel | targeting, timing, messaging, qualification, objection handling |
| Marketplace | matching, liquidity, pricing, trust, reputation |
| Compliance process | documentation, rules, auditability, approvals, exception handling |
| Human behavior process | incentives, trust, friction, salience, habit |
| Logistics network | routing, scheduling, capacity, inventory, downtime |
| Knowledge work | retrieval, summarization, decomposition, verification, memory |
| Financial process | risk scoring, pricing, fraud detection, cash timing, underwriting |
| Medical workflow | triage, eligibility, documentation, scheduling, adherence, coding |
| Software engineering | context, planning, tests, review, deployment, regression |
