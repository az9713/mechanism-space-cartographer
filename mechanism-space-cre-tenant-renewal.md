# Mechanism Space: Commercial Real Estate — Tenant Renewal Rate
**Target Metric:** Tenant renewal rate (% of leases expiring in a quarter where the tenant signs a renewal vs. vacating)
**Date:** 2026-05-22

---

## 1. System Model

**Actors:**
- Tenant (decision-maker: CFO, CEO, or COO) — weighs cost of staying vs. cost of moving
- Tenant's real estate broker — financially incentivized to relocate (higher commission on new lease than on renewal); the primary adversarial actor
- Landlord / property owner — bears the full cost of turnover; often passive until too late
- Property manager — day-to-day relationship with tenant; first to hear dissatisfaction but rarely empowered to act on it
- Landlord's leasing broker — engaged reactively when a tenant is already leaving
- Competing landlords — actively pitching tenants starting 18–24 months before expiration

**Workflow States:**
```
Lease Signed
  → Active Tenancy (1–10 year terms, largely silent)
  → Lease Expiration Horizon (18–24 months out)
    → Landlord monitors tenant health (or doesn't)
    → Renewal Discussion Initiated (12–18 months out, ideally)
      → Tenant evaluates: stay vs. relocate vs. downsize
        → Competitor landlords pitch the tenant
        → Tenant broker engaged (often without landlord's knowledge)
      → Renewal Terms Negotiated
        → Deal struck → Lease signed
        → Deal fails → Tenant vacates
  → Space goes vacant or new tenant found
```

**Key Bottlenecks / Value Leakage:**

| Transition | Leak Type |
|---|---|
| Active Tenancy → Horizon | Landlord has no early warning; tenant dissatisfaction accumulates silently |
| Horizon → Renewal Discussion | Landlord initiates too late (6 months out); tenant broker already engaged and has toured alternatives |
| Renewal Discussion → Negotiation | Market has moved; landlord's ask is out of range; tenant has anchored on a competing offer |
| Negotiation → Signed | Deal friction (TI delays, legal back-and-forth, slow execution) causes tenant to accept a competitor's already-signed LOI |

---

## 2. Economic Equation

```
Landlord Revenue = Total Leasable SF × Occupancy Rate × Avg Rent/SF/Year

Cost of Tenant Turnover (per vacated space):
  Lost rent during vacancy:      $45/SF × 6–12 months avg vacancy
  Tenant improvement allowance:  $50–150/SF for new tenant buildout
  Leasing commission:            4–6% of total new lease value
  Legal / admin:                 $5–15/SF

  Total turnover cost per 10,000 SF space: $600K–$1.6M

Portfolio math:
  500,000 SF portfolio × 20% annual lease expiration = 100,000 SF at risk

  At 80% renewal: 20,000 SF turns over = $1.2M–$3.2M in turnover cost/year
  At 90% renewal: 10,000 SF turns over = $600K–$1.6M in turnover cost/year

  Each 1% renewal improvement on a 500K SF portfolio = $120K–$320K/year saved

  A single prevented vacancy on a 10,000 SF space pays for 5–10 years of
  any SaaS tool priced at $500–1,500/month.
```

---

## 3. Mechanism Taxonomy

Six domain-specific mechanism families:

| Family | Rationale |
|---|---|
| **Tenant health monitoring** | Most churn is detectable 12–18 months out from signals already available (public data, building systems, AMS); no one is watching |
| **Renewal timing optimization** | Landlords who initiate at 18+ months close at 2× the rate of those who start at 6 months — by then the tenant broker is already engaged |
| **Tenant broker management** | The tenant broker is the primary adversary; managing this relationship proactively changes the financial incentive structure |
| **Building experience differentiation** | Tenants who feel the landlord actively manages their experience renew at dramatically higher rates; most landlords are passive until there's a complaint |
| **Market intelligence and positioning** | Tenants who believe they can get cheaper space elsewhere don't renew; landlords who proactively reframe the economics retain more |
| **Deal friction reduction** | Renewal deals that take >60 days to close from LOI to signed lose to competitors who present a ready-to-sign alternative; speed is a retention mechanism |

---

## 4. Full Mechanism List

### Family 1: Tenant Health Monitoring

| # | Name | Causal Hypothesis | Equation Variable |
|---|---|---|---|
| 1 | Headcount trajectory tracking | Tenants whose headcount is shrinking are 4× more likely to downsize or vacate at renewal; LinkedIn data shows changes 12–18 months before the lease decision | At-risk tenant detection rate |
| 2 | Parking utilization monitoring | Tenants using <60% of allocated parking for 3+ consecutive months are underutilizing space — leading indicator of downsizing intent | At-risk detection |
| 3 | Maintenance request pattern analysis | Tenants who stop submitting maintenance requests aren't satisfied — they've given up; zero requests in last 6 months of term signals elevated churn risk | Tenant satisfaction signal |
| 4 | After-hours HVAC request trend | Declining after-hours HVAC usage signals the tenant is working the space less — operational contraction before a lease decision | Space utilization signal |
| 5 | Sublease listing detection | Tenants who list their space on CoStar/LoopNet are actively planning to exit; detecting this 18 months before expiration is the single highest-confidence churn signal | At-risk detection |
| 6 | Financial distress signals | Public credit events (D&B, court filings, news) showing tenant financial deterioration predict lease default or non-renewal 9–15 months in advance | Lapse/default risk |

### Family 2: Renewal Timing Optimization

| # | Name | Causal Hypothesis | Equation Variable |
|---|---|---|---|
| 7 | 18-month proactive outreach | Landlords who initiate renewal discussions 18+ months before expiration close at 2× the rate of those who start at 6 months; the earlier start eliminates the tenant broker's first-mover advantage | Renewal rate |
| 8 | Tenant broker pre-engagement | Contacting the tenant's broker before the tenant does, offering cooperation fee on renewal, flips the broker's incentive from "relocate" to "renew" | Broker-influenced renewal rate |
| 9 | Decision-maker lifecycle trigger | When the tenant's CFO or COO changes, the new executive re-evaluates all real estate commitments; detecting leadership changes and initiating immediate outreach captures a receptive window | Renewal rate for tenants with new decision-makers |
| 10 | Lease anniversary touchpoint | An annual personal contact from the landlord's principal on the lease anniversary — outside of any renewal or complaint context — builds relationship that makes renewal the default | Relationship depth → renewal rate |
| 11 | Competitor lease expiration monitoring | When a competing building's major tenant vacates, that landlord will aggressively pitch your tenants; pre-emptive renewal lockup before the competing building goes on the offensive | Renewal rate during competitive market windows |

### Family 3: Tenant Broker Management

| # | Name | Causal Hypothesis | Equation Variable |
|---|---|---|---|
| 12 | Renewal cooperation fee standardization | Offering a fixed, publicized cooperation fee to tenant brokers on renewals removes the financial incentive to relocate; brokers who know they'll be paid on renewal stop steering clients to alternatives | Broker-influenced renewal rate |
| 13 | Broker relationship program | Hosting quarterly broker events at the building and sharing market data creates goodwill that tilts borderline broker decisions toward renewal | Broker-referred renewal rate |
| 14 | Broker deal speed guarantee | Committing to a 48-hour LOI response and 30-day lease execution creates a competitive advantage vs. buildings where deal timelines are unpredictable | Deal close rate |
| 15 | Dual-agency offer to tenant broker | Offering the tenant's broker the opportunity to represent both sides of the renewal (landlord pays full commission) is sometimes a legal option that directly flips the broker's incentive | Renewal rate |

### Family 4: Building Experience Differentiation

| # | Name | Causal Hypothesis | Equation Variable |
|---|---|---|---|
| 16 | Annual tenant satisfaction survey | Tenants who are asked their opinion renew at higher rates independent of what the landlord does with the results; the act of asking is itself a retention signal | Renewal rate |
| 17 | Proactive amenity upgrade signaling | Announcing planned building improvements 12 months before a tenant's renewal decision — before they start touring alternatives — shifts the comparison set to include future building quality | Renewal rate |
| 18 | Tenant community programming | Buildings that host regular events (networking, wellness programs) create switching costs — tenants lose access to a community, not just a space | Renewal rate for community participants |
| 19 | Named tenant success story program | Publicly featuring tenants' business growth stories in building marketing creates pride-of-place and social identity tied to the building — psychological switching cost | Tenant satisfaction → renewal |
| 20 | Service SLA transparency | Publishing and tracking maintenance response times, HVAC uptime, and elevator performance creates accountability and demonstrates landlord attentiveness | Building satisfaction signal |

### Family 5: Market Intelligence and Positioning

| # | Name | Causal Hypothesis | Equation Variable |
|---|---|---|---|
| 21 | Effective rent comparison report | Presenting tenants with a market comparison showing their current effective rent (including TI and free rent amortized) vs. alternatives — before they tour those alternatives — reframes the renewal as cheaper than moving | Tenant's perceived switching cost |
| 22 | Total cost of move quantification | Providing tenants with a relocation cost analysis (moving costs, buildout disruption, productivity loss, new TI vs. renewal TI) before they request competitor proposals anchors the decision in total economics, not rent/SF | Perceived renewal value |
| 23 | Comparable deal transparency | Sharing recent comparable lease deals in the submarket demonstrates that renewal terms are at-market — removes the tenant's assumption that they could do better elsewhere | Negotiation close rate |
| 24 | New supply threat monitoring | When new Class A space enters the submarket, existing tenants get pitched aggressively; identifying which tenants are most likely to be targeted allows pre-emptive renewal conversations | Renewal rate during supply absorption |
| 25 | Tenant business growth facilitation | Landlords who help growing tenants expand within the portfolio (adjacent suites, sister buildings) retain those tenants through growth stages that would otherwise force a move | Renewal rate for expanding tenants |

### Family 6: Deal Friction Reduction

| # | Name | Causal Hypothesis | Equation Variable |
|---|---|---|---|
| 26 | Pre-negotiated renewal term sheet | Having a standard renewal term sheet ready to present at the 18-month meeting — rather than starting from scratch — reduces time-to-LOI from weeks to days; speed advantage over competing buildings requiring full RFP processes | Time-to-close → renewal rate |
| 27 | Tenant improvement pre-commitment | Offering a TI commitment upfront (before the tenant requests it) removes the #1 negotiation sticking point and prevents the deal from stalling while the tenant shops for a landlord who will commit to TI | LOI-to-signed close rate |
| 28 | Legal template standardization | Using a pre-approved renewal amendment template requiring minimal redlining reduces legal timeline from 30–60 days to 10–15 days; tenants who receive a competing LOI while in legal are 40% less likely to close | Deal close rate |
| 29 | Digital lease execution | Enabling DocuSign for renewal execution removes the scheduling friction of wet signatures; each day of delay in the signing window is an opportunity for a competing LOI to arrive | Time-to-signed |
| 30 | Renewal incentive front-loading | Structuring renewal incentives (TI, free rent, rate concession) to be front-loaded rather than spread across the term makes economic value tangible and immediate — tenants respond to near-term incentives more than long-term discounts | Renewal acceptance rate |

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
| 1 | 5 | **Sublease listing detection** | 5 | 5 | 5 | 4 | 1 | 1 | 1 | 33.3 | CoStar/LoopNet data is public; detection is automatable; a tenant listing their space IS planning to leave — highest-confidence signal in the entire space |
| 2 | 7 | **18-month proactive outreach** | 5 | 5 | 4 | 4 | 1 | 1 | 1 | 26.7 | Strong causal evidence; only constraint is discipline to act early; eliminates broker first-mover advantage entirely |
| 3 | 1 | **Headcount trajectory tracking** | 5 | 4 | 4 | 4 | 1 | 2 | 2 | 13.3 | LinkedIn data accessible via API; downsizing headcount leads lease decision by 12–18 months; high-value early warning |
| 4 | 22 | **Total cost of move quantification** | 4 | 4 | 4 | 4 | 1 | 2 | 2 | 10.7 | Reframes decision from rent/SF to total economic cost; data constructable from public sources; strong anchoring effect |
| 5 | 26 | **Pre-negotiated renewal term sheet** | 4 | 4 | 4 | 3 | 1 | 2 | 2 | 10.7 | Speed advantage is decisive when competing LOIs are in play; requires only internal process change, no new technology |
| 6 | 9 | **Decision-maker lifecycle trigger** | 4 | 4 | 3 | 4 | 1 | 2 | 2 | 10.7 | New executive = re-evaluation window; LinkedIn change detection is automatable; highly actionable signal with tight timing |
| 7 | 12 | **Renewal cooperation fee standardization** | 5 | 4 | 3 | 3 | 2 | 2 | 2 | 8.3 | Directly neutralizes the primary adversary; requires policy change only, no technology; high leverage per unit of effort |
| 8 | 21 | **Effective rent comparison report** | 4 | 4 | 4 | 3 | 1 | 2 | 3 | 8.0 | Reframes comparison before tenant tours alternatives; CoStar data makes this constructable; psychological anchoring effect |
| 9 | 27 | **Tenant improvement pre-commitment** | 4 | 4 | 4 | 3 | 1 | 2 | 3 | 8.0 | TI is the #1 negotiation sticking point; pre-committing removes the delay that allows competing LOIs to land during legal |
| 10 | 25 | **Tenant business growth facilitation** | 5 | 4 | 3 | 3 | 1 | 3 | 3 | 6.7 | Retention through expansion is the best possible outcome; requires portfolio inventory awareness and proactive account management |

---

## 6. Data Requirements Per Top Mechanism

| Mechanism | Data Needed | Where It Lives | Gap |
|---|---|---|---|
| Sublease listing detection (#5) | Active sublease listings by address | CoStar, LoopNet (public/API) | None — automatable scrape or API today |
| 18-month proactive outreach (#7) | Lease expiration date per tenant | Lease management system (Yardi, MRI, AppFolio) | None — data exists; process discipline is the gap |
| Headcount trajectory (#1) | Employee count over time by company | LinkedIn Company Pages, Revelio Labs, People Data Labs | Minor — requires paid API or scraping |
| Total cost of move (#22) | Moving costs, TI allowances, local vacancy rates | Internal + CoStar comparable data | Minor — requires analyst time or data partnership |
| Pre-negotiated term sheet (#26) | Standard renewal terms by property type | Internal legal / deal history | None — requires internal process creation only |
| Decision-maker lifecycle (#9) | CFO/COO/CEO changes at tenant companies | LinkedIn, ZoomInfo, Apollo | Minor — requires enrichment API |
| Cooperation fee standardization (#12) | Broker commission schedule | Internal policy | None — requires policy decision only |
| Effective rent comparison (#21) | Market comps by submarket, size, class | CoStar | Minor — CoStar subscription required |
| TI pre-commitment (#27) | TI budget approval process | Internal capital planning | None — requires internal authorization framework |
| Tenant growth facilitation (#25) | Portfolio inventory + tenant headcount trend | AMS + LinkedIn | Moderate — requires integration of two sources |

---

## 7. What This Points To: Three Product Opportunities

### Product A — Tenant Churn Intelligence Platform
A SaaS tool for commercial landlords and property managers that monitors their tenant roster continuously for churn signals: sublease listings on CoStar/LoopNet, headcount changes on LinkedIn, financial distress via D&B, and decision-maker changes. Ranks tenants by risk score × annual rent value and surfaces the top at-risk tenants weekly with a recommended action per tenant.

- **Why it wins:** All data is public or API-accessible; a single prevented vacancy on a 10,000 SF space saves $600K–$1.6M — pays for 5–10 years of SaaS fees at $500–1,500/month
- **Wedge:** Free sublease detection alert — highest-confidence signal, zero ongoing work for the landlord, immediate demonstrable value
- **Pricing:** $500–2,000/month per property management company (based on portfolio SF)

### Product B — Renewal Playbook Automation
A deal-management tool that guides property managers through the 18-month renewal process: triggers outreach at the right time, generates a customized "total cost of move" analysis for each tenant, tracks broker engagement and cooperation fee status, and manages the LOI-to-signed timeline with automated reminders. Integrates with Yardi, MRI, or AppFolio.

- **Why it wins:** Renewal process discipline is the primary gap; most property managers handle renewals ad-hoc from memory and calendar reminders; the tool makes the right process automatic
- **Pricing:** $300–800/month per property management company
- **Moat:** AMS integration creates switching cost once embedded in workflow

### Product C — Tenant Broker Relationship Manager
A purpose-built CRM for landlord-broker relationships: tracks which brokers represent which tenants in the portfolio, logs all broker interactions, automates cooperation fee calculations and communications, and surfaces which brokers are due for outreach before their clients' leases expire.

- **Why it wins:** Broker management is the most under-invested lever in landlord retention strategy; no purpose-built tool exists — landlords use generic CRMs or spreadsheets; the broker is the primary adversary and no one is managing that relationship systematically
- **Pricing:** $200–500/month per landlord/PM company
- **Differentiation:** The only tool explicitly designed to flip broker incentives from relocation to renewal
