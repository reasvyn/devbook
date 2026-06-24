# Incorporation & Legal

## Description

Every software business begins with legal foundations — choosing the right entity, splitting equity with co-founders, protecting intellectual property, and understanding the contracts that govern your relationships.

## Prerequisites

- [Business Models](../../intro/business-models.md) — how software businesses are structured

## Table of Contents

- [Entity Types](#entity-types)
- [Incorporation Process](#incorporation-process)
- [Co-founder Equity](#co-founder-equity)
- [Intellectual Property](#intellectual-property)
- [Key Contracts](#key-contracts)

## Content / Material

### Entity Types

Your business structure determines taxation, liability, fundraising ability, and governance. Choose carefully — converting later is expensive.

| Entity | Best For | Taxation | Fundraising |
|--------|----------|----------|-------------|
| **Sole Proprietorship** | Freelancers, solo devs | Personal income only | Impossible |
| **LLC** | Bootstrapped businesses, small teams | Pass-through, flexible | Difficult (no stock) |
| **C-Corp (Delaware)** | VC-backed startups | Double taxation | Standard for VCs |
| **S-Corp** | US small businesses, <100 shareholders | Pass-through | Limited |

For venture-backed software startups, **Delaware C-Corp** is the industry standard. Investors require it because it provides clear shareholder structures, familiar legal frameworks, and unlimited share classes.

### Incorporation Process

1. **Name reservation** — check availability and trademark conflicts
2. **File certificate of incorporation** — Delaware Division of Corporations (online, ~$90)
3. **Draft bylaws** — governance rules for board meetings, shareholder rights
4. **Issue founder shares** — common stock with vesting (4-year, 1-year cliff)
5. **File 83(b) election** — US tax election for early-stage stock (within 30 days of issuance)
6. **Assign IP** — all founder IP must be assigned to the company
7. **Open bank account** — EIN from IRS, business bank account
8. **Register for taxes** — state and local business licenses

**Stripe Atlas, Clerky, Gust** — these services handle incorporation for software startups at ~$500. They're worth it for first-time founders.

### Co-founder Equity

Equity splits are the most common source of early startup conflict. Avoid equal splits by default — differentiate by role, experience, and commitment.

**Vesting.** All equity should vest. Standard terms:
- **4-year vesting schedule** — shares earned monthly over 4 years
- **1-year cliff** — no equity vests in the first year; after 12 months, 25% vests in one lump
- **Acceleration** — some or all unvested equity vests on acquisition (change of control)

**Equity ranges for first hires:**

| Role | Typical Equity |
|------|---------------|
| Co-founder (CEO/CTO) | 20–50% each (split among founders) |
| First engineer (employee #1) | 1–3% |
| Early engineer (#2–5) | 0.5–1.5% |
| Early non-engineer | 0.2–1% |

### Intellectual Property

Your company's IP is often its most valuable asset. Protect it from day one.

**IP Assignment Agreement** — every founder and employee must sign this. It assigns all work product (code, designs, inventions) to the company. Without it, a departing founder could claim ownership of the codebase.

**Provisional patent** — if you have a novel invention, file a provisional patent application ($65–$260) to establish priority. You have 12 months to file the full non-provisional application.

**Trademarks** — register your company name and logo to prevent others from using confusingly similar marks. Use the USPTO trademark database to check availability.

**Trade secrets** — source code, algorithms, customer lists, and pricing are trade secrets. Protect them with NDAs, access controls, and employment agreements.

### Key Contracts

| Contract | Purpose |
|----------|---------|
| **Founder Agreement** | Equity split, roles, vesting, IP assignment, decision-making |
| **Stock Plan** | Documents equity grants for employees and advisors |
| **Employment Agreement** | At-will employment, IP assignment, confidentiality |
| **Consulting Agreement** | For contractors — IP assignment, scope, payment terms |
| **NDA** | Non-disclosure agreement — protects confidential information |
| **Customer Terms of Service** | Governs how customers use your product |
| **DPA** | Data Processing Agreement — required for GDPR compliance |
| **SaaS Agreement** | Subscription terms, SLAs, support commitments |

## Glossary

| Term | Definition |
|------|------------|
| C-Corp | A corporation where shareholders are taxed separately from the entity |
| Vesting | Earning equity over time (typically 4 years with 1-year cliff) |
| 83(b) election | US tax election to pay tax on stock value at grant, not vesting |
| IP Assignment | Legal transfer of intellectual property rights to the company |
| Cap table | Spreadsheet showing who owns what percentage of the company |

## Next Steps

- [Fundraising](fundraising.md) — raising capital after incorporation
- [Regulatory & Compliance](regulatory-and-compliance.md) — ongoing legal obligations
