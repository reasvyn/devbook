# Time Value of Money

## Description

The time value of money (TVM) is the single most important concept in finance — a dollar today is worth more than a dollar tomorrow because it can earn returns. Developers encounter TVM in equity compensation, project ROI, and pricing decisions.

## Prerequisites

- [Why Finance?](why-finance.md)

## Table of Contents

- [The Core Idea](#the-core-idea)
- [Present Value & Future Value](#present-value--future-value)
- [Discount Rates](#discount-rates)
- [TVM in Developer Decisions](#tvm-in-developer-decisions)

## Content / Material

### The Core Idea

Money has a time dimension. $100 today can be invested at 10% to become $110 next year. Therefore, **$100 today is not the same as $100 next year** — it's worth more.

```
Future Value = Present Value × (1 + rate)ⁿ
Present Value = Future Value / (1 + rate)ⁿ
```

Where `n` is the number of periods and `rate` is the discount rate (return you could earn elsewhere).

### Present Value & Future Value

**Future Value** asks: if I invest $1,000 today at 8%, how much will I have in 5 years?

```
FV = 1,000 × (1.08)⁵ = $1,469.33
```

**Present Value** asks: what is $1,000 received in 5 years worth today, discounted at 8%?

```
PV = 1,000 / (1.08)⁵ = $680.58
```

The farther in the future money arrives, the less it's worth today.

### Discount Rates

The discount rate reflects:

- **Opportunity cost** — what else you could do with the money
- **Risk** — riskier future cash flows deserve a higher discount rate
- **Inflation** — money loses purchasing power over time

| Use Case | Typical Rate |
|----------|-------------|
| Risk-free (govt bonds) | 4–5% |
| Startup discount rate | 25–40% |
| Engineering project ROI hurdle | 15–25% |

Higher risk → higher discount rate → lower present value.

### TVM in Developer Decisions

**Equity compensation:** 1,000 options vesting over 4 years are worth less than 1,000 options today. The later tranches are discounted.

**Build vs buy:** A SaaS tool costs $12k/year. Building it costs $60k once. You need to compare money spent today vs money spent over time — TVM tells you which is cheaper.

**Cloud spend:** Paying $100k upfront for reserved instances vs $12k/month on-demand is a TVM problem.

**Technical debt:** Fixing a bug today costs $5k. Ignoring it costs $50k in 2 years. Discounted at 20%, the $50k future cost has a present value of ~$34.7k — still more than fixing it today.

## Glossary

| Term | Definition |
|------|------------|
| Time value of money | The principle that money available now is worth more than the same amount later |
| Present value (PV) | The current value of a future amount of money |
| Future value (FV) | The value of a current asset at a specified future date |
| Discount rate | The rate used to calculate present value of future cash flows |
| Net present value (NPV) | The sum of present values of all cash flows (positive and negative) |

## Next Steps

- [Risk & Return](../../investment/intro/risk-and-return.md) — the trade-off at the heart of investing
- [Why Business?](../../business/intro/why-business.md) — applying finance to company-building
