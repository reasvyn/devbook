# Unit Economics

## Description

Unit economics measure the profit or loss of a single customer transaction — the fundamental math that determines whether a business is viable, scalable, and attractive to investors.

## Prerequisites

- [Time Value of Money](../../../financial/intro/time-value-of-money.md) — discounting future cash flows

## Table of Contents

- [What Are Unit Economics?](#what-are-unit-economics)
- [Customer Acquisition Cost](#customer-acquisition-cost)
- [Lifetime Value](#lifetime-value)
- [Payback Period](#payback-period)
- [Gross Margin](#gross-margin)
- [Beyond the Basics](#beyond-the-basics)

## Content / Material

### What Are Unit Economics?

Unit economics answer one question: **do you make more money from a customer than it costs to acquire and serve them?** If yes, the business can scale. If no, every new customer makes the problem worse.

The two key numbers:
- **CAC** — Customer Acquisition Cost (what you spend to get one customer)
- **LTV** — Lifetime Value (total revenue one customer generates)

Healthy ratio: **LTV > 3× CAC**

### Customer Acquisition Cost

```
CAC = Total Sales & Marketing Spend / Number of New Customers
```

Include: salaries of sales and marketing team, ad spend, content production, software tools, commissions, and overhead. Be honest about what's included.

**CAC by channel is more useful than total CAC.** Your content marketing CAC might be $50 while your paid ads CAC is $500. Knowing this tells you where to invest more.

**Blended CAC** averages across all channels. Useful for investors, but not for operational decisions.

### Lifetime Value

```
LTV = Average Revenue Per Customer × Average Customer Lifetime
```

For SaaS:
```
LTV = ARPU × Gross Margin / Monthly Churn Rate

Example: $100/month ARPU, 80% margin, 3% monthly churn
LTV = $100 × 0.80 / 0.03 = $2,667
```

**Net Revenue Retention (NRR)** — the most important SaaS metric you're not tracking:

```
NRR = Starting MRR + Expansion - Contraction - Churn / Starting MRR
```

NRR > 100% means your existing customers are paying you more over time — they're upgrading, adding seats, or buying more products. This compounds incredibly well. Top SaaS companies have NRR of 120%+.

### Payback Period

How long to recover the CAC:

```
Payback Period = CAC / (Monthly Revenue per Customer × Gross Margin)

Example: CAC = $1,500, ARPU = $100/month, margin = 80%
Payback = $1,500 / ($100 × 0.80) = 18.75 months
```

**Healthy benchmarks:**

| Type | Payback |
|------|---------|
| Ideal SaaS | < 12 months |
| Acceptable SaaS | 12–24 months |
| Enterprise SaaS | 12–18 months (higher ACV) |
| SMB SaaS | < 6 months (lower ACV) |

### Gross Margin

```
Gross Margin = (Revenue - COGS) / Revenue
```

COGS (Cost of Goods Sold) for SaaS includes: hosting infrastructure, customer support, payment processing fees, and implementation services.

| Metric | Healthy SaaS |
|--------|-------------|
| Gross margin | > 70% |
| Rule of 40 | Revenue growth % + Profit margin % > 40 |
| CAC payback | < 12 months |
| LTV / CAC | > 3× |
| NRR | > 100% |

### Beyond the Basics

**Magic Number** — efficiency of sales spend:

```
Magic Number = (Quarterly New ARR × 4) / Prior Quarter S&M Spend
```

> 1.0 = efficient, 0.5–1.0 = okay, < 0.5 = inefficient

**Burn Multiple** — how much cash you burn per dollar of new ARR:

```
Burn Multiple = Net Cash Burn / Net New ARR
```

< 1× = efficient, 1–2× = normal for growth stage, > 3× = concerning

**LTV/CAC by cohort.** Track these metrics by customer cohort (month acquired, channel, plan type). Aggregate numbers hide problems. A declining LTV/CAC ratio is a warning sign.

## Glossary

| Term | Definition |
|------|------------|
| CAC | Customer Acquisition Cost |
| LTV | Lifetime Value of a customer |
| NRR | Net Revenue Retention — revenue retained from existing customers including upgrades |
| Payback period | Months to recover CAC |
| Gross margin | Revenue minus direct costs divided by revenue |
| ARPU | Average Revenue Per User |
| Churn | Rate of customer loss |

## Next Steps

- [Fundraising](fundraising.md) — investors will ask for these numbers
- [Go-to-Market](go-to-market.md) — how to acquire customers efficiently
