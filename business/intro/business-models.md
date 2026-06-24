# Business Models

## Description

A business model describes how a company creates, delivers, and captures value. Developers who understand business models can evaluate product viability, identify revenue opportunities, and build features that align with company strategy.

## Prerequisites

- [Why Business?](why-business.md)

## Table of Contents

- [What Is a Business Model?](#what-is-a-business-model)
- [Common Software Business Models](#common-software-business-models)
- [How to Evaluate a Business Model](#how-to-evaluate-a-business-model)
- [Business Models in Code](#business-models-in-code)

## Content / Material

### What Is a Business Model?

A business model answers four questions:

1. **Who** are your customers?
2. **What** value do you provide them?
3. **How** do you deliver and capture value?
4. **Why** will customers choose you over alternatives?

The Business Model Canvas is a popular template for mapping these dimensions — customer segments, value proposition, channels, customer relationships, revenue streams, key resources, key activities, key partnerships, and cost structure.

### Common Software Business Models

| Model | How It Works | Examples |
|-------|-------------|----------|
| **SaaS** | Recurring subscription, per-seat or usage-based | Salesforce, Slack, GitHub |
| **Marketplace** | Connect buyers and sellers, take a cut | Airbnb, Uber, Etsy |
| **Platform** | Enable third parties to build on your infrastructure | iOS, AWS, Shopify |
| **Freemium** | Free tier converts to paid premium | Dropbox, Figma, Notion |
| **Open-core** | Free open-source + paid enterprise features | GitLab, Redis, MongoDB |
| **Transactional** | Charge per transaction or volume | Stripe, Twilio, Plaid |
| **Advertising** | Free product funded by ad revenue | Google, Meta, TikTok |
| **DevOps / Infrastructure** | Tools and platforms for development teams | Datadog, Vercel, Sentry |

### How to Evaluate a Business Model

**Unit economics** tell you if the model works:

```
LTV (Lifetime Value) > CAC (Customer Acquisition Cost)
Payback period < 12 months
Gross margin > 70% (for SaaS)
```

**Recurring revenue** is the most valuable because it's predictable. Companies with high net revenue retention (NRR > 120%) grow without acquiring new customers — existing customers spend more over time.

**Network effects** make a business model defensible. Each new user makes the product more valuable for everyone else — creating a moat competitors can't easily cross.

### Business Models in Code

Business models are encoded in software architecture:

- **Multi-tenant SaaS** — single codebase, isolated data per customer
- **Usage-based billing** — metering, event tracking, consumption APIs
- **Marketplace** — payment splitting, escrow, dispute resolution
- **Freemium** — feature gating, tier enforcement, upgrade flows
- **Open-core** — dual licensing, enterprise features behind a license key

Understanding which business model your company uses helps you build the right features, the right way.

## Glossary

| Term | Definition |
|------|------------|
| Business model | How a company creates, delivers, and captures value |
| SaaS | Software as a Service — recurring subscription software |
| LTV | Lifetime Value — total revenue a customer generates |
| CAC | Customer Acquisition Cost — cost to acquire a customer |
| Network effect | A product becomes more valuable as more people use it |
| Gross margin | Revenue minus direct costs, divided by revenue |

## Next Steps

- [Why Finance?](../financial/intro/why-finance.md) — the financial mechanics behind business models
- [Pricing Strategies](../beginner/pricing-strategies.md) (future) — how to set prices for software products
