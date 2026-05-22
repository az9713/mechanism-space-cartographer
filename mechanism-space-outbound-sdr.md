# Mechanism Space: Outbound B2B SDR Agency
**Target Metric:** Qualified Meetings Booked per 100 Contacts Worked (QMB/100)
**Date:** 2026-05-22

---

## 1. System Model

**Actors:**
- SDR rep (executes outreach, handles objections, books meetings)
- Contact / prospect (decision-maker or influencer at target company)
- Client (SaaS company paying the agency; defines ICP and offers)
- CRM / sequence tool (Apollo, Outreach, Salesloft, HubSpot)
- Data enrichment vendor (ZoomInfo, Clay, LinkedIn, intent platforms)

**Workflow States:**
```
Contact List Built
  → Contact Enriched
  → Sequence Started
    → Email Opened (or not)
    → Reply Received (or not)
  → Call Attempted
    → Connected
    → Pitched
    → Objection Handled
  → Meeting Booked
  → Meeting Held
  → Meeting Qualified
```

**Key Bottlenecks / Value Leakage Points:**
| Arrow | Leak Type | Estimated Loss |
|---|---|---|
| List Built → Enriched | Wrong contact, bad data | 20–40% of contacts unreachable |
| Sequence Started → Reply | Message irrelevance, bad timing | ~97% of sequences get no reply |
| Connected → Pitched | Rep skill variance, wrong talk track | Top rep 3–5x median rep |
| Meeting Booked → Meeting Held | No-show rate | 20–30% of meetings lost |
| Meeting Held → Qualified | ICP mismatch, bad discovery | 30–50% disqualified after the call |

---

## 2. Economic Equation

```
Agency Revenue =
  Clients
  × (Contacts Worked / Client / Month)
  × (QMB/100 / 100)
  × Per-Meeting Bonus
  + Monthly Retainer

System-level insight:
  A 2× improvement in QMB/100 doubles the per-meeting bonus line
  without adding headcount or clients.

Example at scale:
  10 clients × 500 contacts/month × 2% QMB/100 × $500/meeting
  = $50,000/month in meeting bonuses

  Same inputs at 4% QMB/100 = $100,000/month (+$50K/month)
```

---

## 3. Mechanism Taxonomy

Six domain-specific mechanism families, derived from the workflow and economic equation:

| Family | Rationale |
|---|---|
| **Signal-Based Targeting** | List quality is the root multiplier — garbage contacts make every downstream mechanism irrelevant |
| **Timing Optimization** | Connection and reply rates are highly time-sensitive; currently managed by habit, not data |
| **Message Personalization at Scale** | Most sequence messages are generic; specificity is the primary driver of reply rate |
| **Rep Skill Amplification** | Rep variance is the largest unmanaged lever; top rep output is 3–5x bottom rep |
| **Channel Sequencing** | Channel mix and order affects response rate; most agencies follow a single fixed template |
| **Conversion Leakage Recovery** | Booked-but-no-show and replied-but-didn't-book are high-yield, near-zero-cost recovery pools |

---

## 4. Full Mechanism List

### Family 1: Signal-Based Targeting

| # | Name | Causal Hypothesis | Equation Variable Affected |
|---|---|---|---|
| 1 | Job-change recency trigger | Contacts who changed jobs <90 days ago have higher buying authority anxiety; 3× more likely to take a meeting to evaluate new vendors | List hit rate |
| 2 | Intent signal stacking | Contacts showing 3+ intent signals (G2 visits, LinkedIn engagement with competitor content, relevant job posting) convert at 4–8× baseline | QMB/100 |
| 3 | Hiring velocity as proxy | Companies hiring SDRs are in growth mode and more likely to buy sales-adjacent tools | List quality |
| 4 | Funding recency filter | Companies that raised in last 60–120 days have budget authority and urgency; reply rate elevated | List hit rate |
| 5 | Technology displacement signal | Contacts at companies whose current tech stack is being sunset (announced EOL, acquisition) are actively evaluating replacements | Pitch-to-meeting rate |

### Family 2: Timing Optimization

| # | Name | Causal Hypothesis | Equation Variable Affected |
|---|---|---|---|
| 6 | Email send-time personalization | Sending at recipient's historical open-time lifts open-to-reply rate 20–40% | Reply rate |
| 7 | Call-attempt timing by seniority | VPs answer calls 7:30–8:15am and 5:15–6pm; IC-level contacts peak 10–11am | Connect rate |
| 8 | Monday-morning sequence pause | First-touch email on Monday is 35% less likely to be read vs. Tuesday–Thursday; rescheduling sequences lifts effective reply rate | Open-to-reply conversion |
| 9 | Fiscal quarter-end urgency windows | Contacts in buying roles are more receptive in the last 3 weeks of their fiscal quarter | Reply-to-meeting rate |
| 10 | Time-zone aware call sequencing | Reps calling across time zones during local business hours (not rep's hours) increases connect rate | Connect rate |

### Family 3: Message Personalization at Scale

| # | Name | Causal Hypothesis | Equation Variable Affected |
|---|---|---|---|
| 11 | Pain-signal mirroring | Emails referencing a specific, verifiable pain the company is experiencing convert 2–5× vs. generic ICP pitch | Reply-to-meeting rate |
| 12 | Competitor displacement framing | If enrichment reveals current tool usage, leading with the displacement angle increases reply rate for that segment | Pitch-to-meeting rate |
| 13 | Social proof tier-matching | Referencing a customer the prospect has likely heard of (same vertical, similar size) in the opener beats generic logos | Open-to-reply |
| 14 | Role-specific outcome framing | VP of Sales hears revenue language; VP of Ops hears efficiency language; same product, different frame | Reply rate |
| 15 | Trigger-event personalization | Using a specific news event (funding, product launch, hiring surge) as the opener increases relevance perception | Open-to-reply |

### Family 4: Rep Skill Amplification

| # | Name | Causal Hypothesis | Equation Variable Affected |
|---|---|---|---|
| 16 | Real-time objection routing | Reps who receive a branching script overlay on objection triggers convert objections to continued calls at 2× rate | Connected-to-pitched rate |
| 17 | Performance distribution compression | Top rep QMB/100 is 3–5× bottom rep; deploying top-rep's exact sequence+talk track to bottom quartile raises system-level QMB/100 without new headcount | Overall QMB/100 |
| 18 | Post-call micro-feedback loops | Reps who receive a 30-second AI-generated call debrief immediately after each call improve connect-to-pitch ratio 15–20% over 30 days | Rep skill slope |
| 19 | Discovery question standardization | Top reps ask 3–5 specific discovery questions that expose urgency and budget; deploying these questions system-wide lifts qualification rate | Meeting-to-qualified rate |
| 20 | Rep-specific weak-link diagnosis | Each rep has one bottleneck step (some fail at connect, others at pitch); targeted coaching on the single worst step produces faster improvement than general training | QMB/100 per rep |

### Family 5: Channel Sequencing

| # | Name | Causal Hypothesis | Equation Variable Affected |
|---|---|---|---|
| 21 | LinkedIn view before email | Viewing a prospect's LinkedIn profile 24–48 hours before first email triggers a profile-view notification, increases email open rate 15–25% for aware prospects | Open rate |
| 22 | Voicemail-email 90-second gap | Leaving a voicemail then sending a follow-up email mentioning it within 90 minutes produces 2–3× reply rate vs. either channel alone | Reply rate |
| 23 | Channel rotation by non-response pattern | Contacts stalled after 3 emails may respond to a direct LinkedIn InMail; rotating channel reactivates 8–12% of stalled contacts | List exhaustion recovery |
| 24 | Direct mail for high-value ICP | For enterprise contacts (>$50K ACV potential), a physical touchpoint (personalized book, handwritten note) breaks through email noise | Reply rate for enterprise segment |
| 25 | Video prospecting for technical buyers | A 60-second personalized Loom showing a specific product capability relevant to their stack converts at 3–4× vs. text for technical buyers | Open-to-reply (technical ICP) |

### Family 6: Conversion Leakage Recovery

| # | Name | Causal Hypothesis | Equation Variable Affected |
|---|---|---|---|
| 26 | "Almost booked" re-engagement | Contacts who replied positively but didn't book (calendar link clicked, no meeting created) have 40–60% book rate if re-contacted within 2 hours | Meeting-booked/reply rate |
| 27 | No-show rescue sequence | 20–30% of booked meetings no-show; a 3-touch rescue sequence within 15 minutes of no-show recovers 25–35% | Meeting-held/booked rate |
| 28 | "Replied negative, revisit in 90 days" pipeline | Contacts who said "not now" are 3–4× more likely to book after 90 days than a fresh cold contact — they already know the sender | List reactivation rate |
| 29 | Meeting-held but disqualified re-route | Contacts who held a meeting but didn't qualify may be a fit for a different client in the agency's portfolio | Cross-client yield |
| 30 | Unsubscribe salvage via alternate channel | Contacts who unsubscribed from email are still reachable via call or LinkedIn; unsubscribe ≠ not interested | List preservation rate |

---

## 5. Top 10 Ranked

```
Score = (V × C × D × T) / (R + F + K)

V = economic value if mechanism works (1–5)
C = causal plausibility (1–5)
D = data availability to test it (1–5)
T = testability / A/B feasibility (1–5)
R = regulatory/compliance risk (1–5, lower = less risky)
F = implementation friction (1–5, lower = easier)
K = complexity (1–5, lower = simpler)
```

| Rank | # | Mechanism | V | C | D | T | R | F | K | Score | Key Rationale |
|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | 27 | **No-show rescue sequence** | 4 | 5 | 5 | 5 | 1 | 1 | 1 | 33.3 | Recovers already-warmed contacts at near-zero cost; implementable in hours; data already exists (no-show event in CRM) |
| 2 | 28 | **Replied-negative, revisit 90 days** | 4 | 4 | 5 | 5 | 1 | 1 | 1 | 26.7 | Exploits an asset sitting unused in every agency's CRM; no new data or infra required |
| 3 | 26 | **"Almost booked" re-engagement** | 4 | 5 | 4 | 5 | 1 | 2 | 2 | 18.0 | Very high causal confidence; calendar-click event detectable in sequence tools today |
| 4 | 17 | **Performance distribution compression** | 5 | 4 | 4 | 4 | 1 | 2 | 2 | 16.0 | Largest system-level upside; data is already in CRM; no new technology required |
| 5 | 7 | **Call-attempt timing by seniority** | 4 | 4 | 4 | 4 | 1 | 2 | 2 | 13.3 | CRM already logs call outcomes and timestamps; experiment requires only tagging |
| 6 | 18 | **Post-call micro-feedback loops** | 3 | 4 | 4 | 4 | 1 | 2 | 3 | 8.0 | Builds compounding rep improvement over time; call recordings already exist in most stacks |
| 7 | 2 | **Intent signal stacking** | 5 | 4 | 3 | 4 | 1 | 3 | 3 | 9.0 | High upside; depends on vendor access (Clay, Bombora) but increasingly affordable |
| 8 | 11 | **Pain-signal mirroring** | 5 | 4 | 3 | 4 | 1 | 3 | 3 | 9.0 | Highest reply-rate lever; requires enrichment pipeline but Clay/GPT combo makes this tractable |
| 9 | 6 | **Email send-time personalization** | 3 | 3 | 4 | 4 | 1 | 2 | 2 | 8.0 | Moderate upside, low friction; available in tools like Lavender and Outreach natively |
| 10 | 22 | **Voicemail-email 90-second gap** | 3 | 3 | 4 | 4 | 1 | 2 | 2 | 8.0 | Simple to implement and test; moderate but consistent upside |

---

## 6. Data Requirements Per Top Mechanism

| Mechanism | Data Needed | Where It Lives | Gap |
|---|---|---|---|
| No-show rescue (#27) | Meeting no-show event timestamp | CRM / calendar integration | None — already trackable |
| Replied-negative revisit (#28) | Reply sentiment tag + reply date | CRM sequence tool | Minor — requires negative-reply tagging |
| Almost-booked re-engagement (#26) | Calendar link click event | Sequence tool (Calendly/Chili Piper webhook) | Minor — requires webhook setup |
| Performance distribution compression (#17) | Per-rep QMB/100 by sequence variant | CRM reporting | None — already in CRM |
| Call timing by seniority (#7) | Call outcome + timestamp + contact seniority | CRM call log + enrichment | Minor — enrichment for seniority tier |
| Post-call micro-feedback (#18) | Call recording + outcome label | Call recording tool (Gong, Chorus, Otter) | Requires recording tool |
| Intent signal stacking (#2) | Intent data feed | Bombora, G2, Clay | Requires paid vendor |
| Pain-signal mirroring (#11) | Company news, job postings, tech stack | Clay, LinkedIn, Clearbit | Requires enrichment pipeline |

---

## 7. What This Points To: Product Opportunity

Three SaaS products derivable from this mechanism space:

### Product A — No-Show + Leakage Recovery Automation
An agency-focused tool that monitors CRM/calendar events in real-time and fires recovery sequences automatically:
- No-show detected → rescue sequence fires within 15 minutes
- Calendar link clicked but no booking → re-engagement fires within 2 hours
- Negative reply tagged → contact surfaces in 90-day re-activation queue

**Addressable value:** At $500/meeting, recovering 5 meetings/month/client across 10 clients = +$25K/month recovered. Tool justifies $500–1,500/month SaaS pricing easily.

### Product B — Rep Performance Gap Closer
A tool that identifies each rep's single-worst bottleneck step (connect rate, pitch rate, or close rate), then automatically routes that rep to the top-performer's sequence variant for that step.
No coaching session needed — the fix is algorithmic.

**Addressable value:** Closing half the gap between median and top rep = 30–50% QMB/100 improvement system-wide.

### Product C — Enrichment-to-Personalization Pipeline
A Clay-style enrichment layer that ingests a contact list and outputs a sequence-ready message per contact, using job-change, funding, hiring, and tech-stack signals to write the opener.
Sells to agencies that want pain-signal mirroring (#11) at scale without a copywriter.

**Addressable value:** 2–5× reply rate improvement; agencies pay per enriched contact or flat monthly.
