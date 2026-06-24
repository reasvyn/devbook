# Risk & Return

## Description

Risk and return are the two sides of every investment coin. Higher returns require accepting higher uncertainty. Developers face this trade-off in code deployments, architectural choices, and career decisions.

## Prerequisites

- [Why Investment?](why-investment.md)

## Table of Contents

- [The Risk-Return Relationship](#the-risk-return-relationship)
- [Types of Risk](#types-of-risk)
- [Measuring Risk](#measuring-risk)
- [Risk in Software Decisions](#risk-in-software-decisions)

## Content / Material

### The Risk-Return Relationship

Risk and return are directly linked. **There is no high return without high risk.** If someone promises guaranteed 20% returns, they are either lying or confused.

| Asset Class | Typical Annual Return | Risk Level |
|-------------|---------------------|------------|
| Cash / savings | 1–3% | Very low |
| Government bonds | 3–5% | Low |
| Corporate bonds | 5–7% | Medium |
| Public stocks | 7–10% (long-term avg) | High |
| Venture capital / startups | 0–30%+ (high variance) | Very high |

The premium you earn over a risk-free rate (govt bonds) is called the **risk premium**.

### Types of Risk

| Risk | Description | Dev Analogy |
|------|-------------|-------------|
| **Market risk** | The whole market goes down | A recession reduces everyone's ad revenue |
| **Specific risk** | One company fails | Your startup's largest customer churns |
| **Inflation risk** | Money loses purchasing power | Cloud costs rise 20% year over year |
| **Liquidity risk** | Can't sell when you need to | Your startup equity has no buyer |
| **Concentration risk** | Too much in one asset | 100% of your net worth is in your employer's stock |

### Measuring Risk

**Standard deviation** — how much returns fluctuate around the average. Higher σ = more volatile = riskier.

**Beta** — how much an asset moves relative to the market. β=1 means it moves with the market; β=1.5 means it's 50% more volatile.

**Sharpe ratio** — (return − risk-free rate) / standard deviation. How much return you get per unit of risk. Higher is better.

**Drawdown** — peak-to-trough decline. A 50% loss requires a 100% gain to recover.

### Risk in Software Decisions

| Decision | Risk | Mitigation |
|----------|------|------------|
| Rewriting the system | Months of zero feature delivery | Incremental migration, feature flags |
| Choosing a new framework | Unknown bugs, ecosystem immaturity | Proof of concept, gradual adoption |
| Single cloud provider | Vendor lock-in, outage exposure | Multi-cloud strategy, portable abstractions |
| No automated tests | Regression risk grows over time | Test pyramid, CI gating |
| Betting on one product feature | Wasted effort if users don't want it | Rapid prototyping, user testing |

Risk management in engineering mirrors investing: **identify, measure, mitigate, monitor**.

## Glossary

| Term | Definition |
|------|------------|
| Risk | The uncertainty of outcomes, especially negative ones |
| Return | The gain or loss from an investment |
| Risk premium | Extra return expected for taking on additional risk |
| Standard deviation | A measure of return volatility |
| Diversification | Reducing risk by holding multiple uncorrelated assets |

## Next Steps

- [Portfolio Theory](../beginner/portfolio-theory.md) (future) — how to construct an optimal portfolio
- [Business Models](../../business/intro/business-models.md) — evaluating companies as investment opportunities
