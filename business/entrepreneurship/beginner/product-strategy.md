# Product Strategy

## Description

How software founders decide what to build — finding product-market fit, prioritizing features, managing technical debt, and evolving a product from zero to market leader.

## Prerequisites

- [Go-to-Market](go-to-market.md) — understanding who you're building for
- [Unit Economics](unit-economics.md) — the business case for product decisions

## Table of Contents

- [Product-Market Fit](#product-market-fit)
- [Discovery & Validation](#discovery--validation)
- [Prioritization](#prioritization)
- [Technical Debt & Architecture](#technical-debt--architecture)
- [Measuring Product Success](#measuring-product-success)

## Content / Material

### Product-Market Fit

Product-market fit (PMF) is the state where your product satisfies a strong market demand. Before PMF, nothing works. After PMF, everything works.

**Signs of PMF:**

- Users are disappointed when your product is down
- Usage grows organically without significant marketing
- Customers are telling others about you
- You're struggling to build features fast enough
- Retention curves flatten (users keep coming back)
- Net Promoter Score > 40

**The PMF funnel:**

```
Total addressable market → Served market → Active users → 
Retained users → Users who would be "very disappointed" without your product
```

The critical number: **40% of users should say they'd be "very disappointed" without your product** (Sean Ellis test). Below 40%, you haven't found PMF.

**Before PMF:**
- Ship fast, iterate on feedback
- Talk to every user personally
- Do things that don't scale
- Focus on a single user segment
- Ignore competitors

**After PMF:**
- Invest in scalability
- Build for broader market segments
- Hire specialists (sales, marketing, support)
- Compete deliberately

### Discovery & Validation

Most product ideas are wrong. Discovery is how you find the ones that aren't.

**Customer discovery framework:**

1. **Define your hypothesis** — who has what problem?
2. **Interview 20–50 potential users** — don't pitch, listen
3. **Identify patterns** — same problem mentioned independently by multiple people
4. **Build the smallest test** — landing page, manual service, prototype
5. **Measure signal** — did anyone try, pay, or return?
6. **Decide** — double down, pivot, or kill

**Validating demand:**

| Signal | Strength | Method |
|--------|----------|--------|
| "I'd pay for that" | Weak | What people say ≠ what they do |
| Someone enters credit card | Strong | Real willingness to pay |
| Regular usage over 30 days | Stronger | Habit formation |
| Customer tells others | Strongest | Viral growth potential |

**The MVP.** The Minimum Viable Product is the smallest thing that delivers real value to early adopters. It's not a prototype — it's a real product with minimal surface area.

### Prioritization

You have infinite ideas and finite engineering time. Prioritization is choosing what not to build.

**Frameworks:**

**RICE (Intercom):**

```
RICE Score = Reach × Impact × Confidence / Effort
```

- **Reach** — how many users in a time period
- **Impact** — how much each user benefits
- **Confidence** — how sure you are of the estimates
- **Effort** — engineering time (person-months)

**Value vs Effort matrix:**

| | High Value | Low Value |
|---|-----------|-----------|
| **Low Effort** | Do first | Quick wins if easy |
| **High Effort** | Big bets — schedule carefully | Avoid |

**Opportunity scoring (Kano model):**

- **Basic needs** — users expect these; their absence causes dissatisfaction
- **Performance needs** — more is better; satisfaction scales with investment
- **Delighters** — unexpected features that create excitement but don't drive retention
- **Indifferent** — don't build these

### Technical Debt & Architecture

Technical debt is inevitable. Good product leaders manage it intentionally.

**When debt is okay:**

- Before PMF — speed matters more than elegance
- For experiments — throwaway code to test hypotheses
- For a critical launch — ship now, refactor later

**When debt hurts:**

- Slowing down new features
- Frequent production incidents
- High onboarding time for new engineers
- Making simple changes requires touching 20 files

**Managing debt:**
- Allocate 15–30% of engineering time to debt reduction
- Refactor areas you touch frequently
- Pay down debt before scaling
- Document decisions and revisit them

### Measuring Product Success

**North Star metric** — the single metric that best captures customer value:

| Company | North Star |
|---------|-----------|
| Airbnb | Nights booked |
| Spotify | Time spent listening |
| Slack | Messages sent |
| Uber | Rides completed |

**Leading vs lagging indicators:**

| Type | Definition | Example |
|------|-----------|---------|
| **Leading** | Predicts future outcomes | Activation rate, session frequency |
| **Lagging** | Reflects past outcomes | Revenue, churn, LTV |

**Product KPIs:**

- **Activation rate** — % of signups reaching core value
- **DAU/MAU** — daily active users / monthly active users
- **Session frequency** — how often users return
- **Time to value** — days from signup to first core action
- **Feature adoption** — % of users using each feature
- **NPS** — net promoter score
- **Churn** — % of customers lost per period

## Glossary

| Term | Definition |
|------|------------|
| Product-market fit | When a product satisfies a strong market demand |
| MVP | Minimum Viable Product — smallest thing that delivers real value |
| RICE | Reach, Impact, Confidence, Effort — prioritization framework |
| North Star metric | The single metric that captures customer value |
| Activation | The moment a user experiences core product value |

## Next Steps

- [Team & Culture](team-and-culture.md) — hiring the team to execute your product vision
- [Go-to-Market](go-to-market.md) — bringing your product to customers
