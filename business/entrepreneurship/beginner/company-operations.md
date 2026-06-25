# Company Operations & Analytics

## Description

Company operations is how a startup runs itself — the systems, processes, and metrics that turn vision into execution. For founders, building operational discipline early prevents chaos as the company grows and ensures you make decisions based on data, not intuition.

## Prerequisites

- [Fundraising](fundraising.md) — understanding financial metrics investors expect
- [Unit Economics](unit-economics.md) — the fundamental metrics of the business
- [Team & Culture](team-and-culture.md) — organizational structure and processes

## Table of Contents

- [Why Operations Matter](#why-operations-matter)
- [Metrics & KPIs](#metrics--kpis)
- [OKRs](#okrs)
- [Financial Operations](#financial-operations)
- [Board Reporting](#board-reporting)
- [Tools & Systems](#tools--systems)
- [Process Design](#process-design)
- [Risk Management](#risk-management)
- [Common Mistakes](#common-mistakes)

## Content / Material

### Why Operations Matter

Most startups fail from chaos, not competition. Poor operations — no clear metrics, no financial discipline, no decision-making frameworks — lead to running out of money, building the wrong things, and scaling dysfunction.

**The operations maturity curve:**

| Stage | Operations Approach | Characteristics |
|-------|--------------------|-----------------|
| **Survival** | None | No metrics, no process, founder does everything |
| **Reactive** | Ad hoc | Metrics in spreadsheets, process created after fires |
| **Proactive** | Systematic | Dashboards, cadences, documented processes |
| **Strategic** | Integrated | Operations drive strategy, data informs all decisions |

**Where to invest in operations:**

| Investment | Returns | When to Do It |
|------------|---------|---------------|
| Basic metrics dashboard | Prevents blind decisions | 5+ employees |
| Financial model | Prevents running out of money | Before raising |
| Board deck template | Saves time, improves communication | Before first board meeting |
| Process documentation | Reduces chaos, improves onboarding | 10+ employees |
| Hiring process | Reduces bad hires | Before first hire |
| OKR cadence | Aligns team, improves focus | 10+ employees |

### Metrics & KPIs

Metrics are how you know if you're winning. The right metrics tell you what's working, what's broken, and where to focus.

**The hierarchy of metrics:**

```
                           ┌───────────────┐
                           │  North Star    │
                           │   Metric       │
                           └───────┬───────┘
                                   │
              ┌────────────────────┼────────────────────┐
              │                    │                    │
         ┌────┴────┐        ┌─────┴─────┐        ┌────┴────┐
         │  Growth  │        │  Health    │        │  Finance │
         │  Metrics │        │  Metrics   │        │  Metrics │
         └────┬────┘        └─────┬─────┘        └────┬────┘
              │                    │                    │
    ┌─────────┼─────────┐  ┌──────┴──────┐  ┌─────────┼─────────┐
    │         │         │  │             │  │         │         │
  Signups  Activation  Viral  Retention  NPS  Revenue  Burn   CAC
```

**North star metric:**

The single metric that best captures customer value and business success. It should be:

- **Leading** (predicts future success)
- **Actionable** (you can influence it daily)
- **Customer-centric** (measures value delivered to customers)

| Company | North Star Metric |
|---------|-------------------|
| Airbnb | Nights booked |
| Spotify | Time spent listening |
| Slack | Messages sent |
| Uber | Rides completed |
| Facebook | Daily active users |
| Stripe | Payment volume |

**Growth metrics:**

| Metric | Formula | What It Tells You |
|--------|---------|-------------------|
| New signups | Count per period | Top-of-funnel health |
| Activation rate | % of signups who reach core value | Product-market fit signal |
| DAU/MAU | Daily / Monthly active users | Engagement (good > 20%) |
| Session frequency | Sessions per user per week | Stickiness |
| Viral coefficient | New users per existing user | Organic growth (needs > 1 to be viral) |
| CAC | Total sales + marketing / new customers | Acquisition efficiency |
| Payback period | CAC / monthly revenue per customer | Time to recover acquisition cost |

**Health metrics:**

| Metric | Formula | What It Tells You |
|--------|---------|-------------------|
| Monthly churn | Customers lost / customers at start | Retention (good < 3-5%) |
| NPS | Promoters - Detractors | Customer satisfaction (good > 30) |
| LTV | ARPU × average lifetime | Customer value |
| LTV/CAC | LTV / CAC | Unit economics (good > 3) |
| Gross margin | (Revenue - COGS) / Revenue | Business model health (good > 70%) |
| Net dollar retention | (Starting MRR + expansion - contraction - churn) / Starting MRR | Expansion efficiency (good > 100%) |

**Financial metrics:**

| Metric | Formula | What It Tells You |
|--------|---------|-------------------|
| MRR / ARR | Monthly / Annual Recurring Revenue | Revenue scale |
| Net burn | Revenue - Expenses | Cash consumption |
| Gross burn | Total expenses | Cost structure |
| Runway | Cash / Net burn | Months until out of money |
| ARPU | MRR / Total customers | Revenue per customer |
| Magic number | (New ARR in quarter) × 4 / (Sales & marketing spend in prior quarter) | Sales efficiency (good > 0.75) |

**Vanity vs actionable metrics:**

| Vanity Metric | Actionable Metric |
|---------------|-------------------|
| Total registered users | Active users this week |
| Website visitors | Trial signups |
| Total revenue | Net new ARR |
| Social media followers | Qualified leads |
| Press mentions | Customer conversion rate |

**Dashboard design principles:**

- Show the same metrics every time (consistency builds intuition)
- Include context (trends, targets, benchmarks)
- Start with the north star, layer detail below
- Review at the same cadence every week
- Make it visible to the whole company

### OKRs

Objectives and Key Results (OKRs) align the company around what matters. Objectives are qualitative goals. Key Results are quantitative measures of progress.

**OKR anatomy:**

```
Objective: Win the enterprise segment
  KR1: Close 5 enterprise deals this quarter
  KR2: Achieve 90% uptime SLA
  KR3: Ship SSO and audit log features
```

**OKR principles:**

| Principle | Explanation |
|-----------|-------------|
| **Less is more** | 3-5 objectives max per team. 3 KRs each |
| **Aspirational** | KRs should stretch — 70% completion is success |
| **Transparent** | Everyone in the company can see everyone's OKRs |
| **Aligned** | Team OKRs roll up to company OKRs |
| **Time-bound** | Quarterly for most companies |
| **Measurable** | Every KR must have a clear pass/fail criteria |

**Tying OKRs to metrics:**

The best OKRs connect directly to your KPI dashboard. If you're improving churn, your OKR includes a churn reduction KR. OKRs are how you make progress on the metrics that matter.

**Common OKR mistakes:**

- Too many OKRs (nothing is prioritized)
- Output-based KRs ("Launch feature X") instead of outcome-based ("10% increase in activation")
- Not revisiting OKRs throughout the quarter
- OKRs that don't connect to company goals
- Perfect is the enemy of good — start with imperfect OKRs, improve with practice

### Financial Operations

Financial operations (finops) is managing your startup's money — budgeting, forecasting, cash management, and reporting.

**Budgeting for startups:**

| Budget Category | % of Total (Typical SaaS) | Notes |
|-----------------|---------------------------|-------|
| Engineering | 40-60% | Salaries, infrastructure, tools |
| Sales & Marketing | 20-35% | Salaries, ads, events, content |
| G&A | 10-20% | Legal, accounting, office, insurance |
| R&D | 5-15% | New product development, experiments |

**The financial model:**

A financial model projects your revenue, expenses, and cash position over 12-24 months. It's essential for fundraising and managing runway.

**Components of a good financial model:**

| Component | What It Projects | Key Inputs |
|-----------|------------------|------------|
| **Revenue model** | Future revenue | Customer acquisition rate, pricing, churn |
| **Expense model** | Future costs | Headcount plan, infrastructure, marketing spend |
| **Cash flow** | Bank balance | Revenue + investment - expenses |
| **Scenario analysis** | Best/worst/base case | Variations on key assumptions |

**Building the financial model:**

1. **Start simple.** Revenue = customers × price. Expenses = headcount + infrastructure + tools
2. **Add complexity gradually.** Segment revenue by product line or customer type. Add COGS for infrastructure costs
3. **Use bottom-up inputs.** Top-down projections (we'll capture 1% of a $10B market) are meaningless. Bottom-up (we'll sell to 100 customers at $10K each) is realistic
4. **Run scenarios.** What if sales are 50% of projection? What if we hire 2 more engineers? What if churn doubles?
5. **Update monthly.** Compare actuals to projections and adjust

**Runway management:**

| Runway Remaining | Action Required |
|-----------------|-----------------|
| 18+ months | Normal operations, focus on growth |
| 12-18 months | Start fundraising planning |
| 6-12 months | Active fundraising or revenue acceleration |
| 3-6 months | Cost reduction, bridge round, or pivot |
| < 3 months | Emergency measures (layoffs, founder salary to zero, debt) |

**The burn multiple:**

A key efficiency metric for venture-backed startups:

```
Burn Multiple = Net Burn / Net New ARR
```

| Burn Multiple | Assessment |
|---------------|------------|
| < 1x | Highly efficient |
| 1-2x | Efficient |
| 2-3x | Normal for growth stage |
| 3-5x | Needs improvement |
| > 5x | Burning too fast |

**Financial controls for startups:**

| Control | Why It Matters |
|---------|----------------|
| **Spending approval limits** | Prevents uncontrolled spending |
| **Monthly financial review** | Catches problems early |
| **Audited financials** | Required for fundraising, potential acquirers |
| **Proper accounting (accrual basis)** | Accurate picture of business health |
| **Cash forecasting** | Prevents running out of money |
| **Fraud controls** | Prevents embezzlement (yes, it happens in startups) |

### Board Reporting

The board deck is the most important recurring presentation a CEO makes. It communicates progress, risks, and strategy to investors and board members.

**Board deck structure (monthly/quarterly):**

| Section | Content | Slide Count |
|---------|---------|-------------|
| **Executive summary** | Top 3 highlights, top 3 challenges, ask of the board | 1 |
| **Metrics** | North star, growth, health, financial KPIs | 2-3 |
| **Revenue and financials** | ARR, burn, runway, revenue by segment | 2-3 |
| **Product** | What shipped, what's coming, product-market fit metrics | 2-3 |
| **Go-to-market** | Pipeline, sales velocity, CAC, channel performance | 1-2 |
| **Team** | Headcount, key hires, attrition, culture metrics | 1-2 |
| **Competitive** | Landscape changes, wins/losses, positioning | 1 |
| **Risks** | Top 3 risks and mitigation plans | 1 |
| **Ask** | What you need from the board | 1 |

**The "ask of the board":**

Every board deck should include a clear ask: "Here's where we need help." Specific asks are more effective than general ones:

- "We need introductions to CTOs at these 5 target accounts"
- "We're hiring a VP Sales — who do you know?"
- "We're evaluating option C for our international expansion — what are we missing?"

**Board meeting rhythm:**

| Cadence | Format | Audience |
|---------|--------|----------|
| **Weekly update** | Email (5 bullets) | Full investor list |
| **Monthly board pack** | Deck (10-15 slides) | Board members |
| **Quarterly board meeting** | In-person or video (60-90 min) | Board + key executives |
| **Annual board retreat** | Offsite (half day to full day) | Board + leadership team |

**Good board communication principles:**

- **Bad news early.** Don't wait for the board meeting to share problems. Bad news doesn't get better with time.
- **Tell a story.** Don't just report numbers. Explain what's happening, why, and what you're doing about it.
- **Be concise.** Board members have limited time. Make every slide earn its place.
- **Focus on what changed.** If nothing changed since last month, say that. Don't rehash the same information.
- **Ask for help.** Board members can't help if they don't know what you need.

### Tools & Systems

The right tools reduce chaos and free up time for high-value work.

**Core operational stack for a startup:**

| Function | Tool Options | When to Invest |
|----------|-------------|----------------|
| **CRM** | HubSpot (free tier), Salesforce, Pipedrive | First customer |
| **Financials** | QuickBooks, Xero, Pilot | First revenue or funding |
| **HRIS** | Gusto, Rippling, BambooHR | First employee |
| **Project management** | Linear, Notion, Asana | 3+ employees |
| **Communication** | Slack, Discord | First employee |
| **Knowledge base** | Notion, Confluence, Guru | 5+ employees |
| **Analytics** | PostHog, Amplitude, Mixpanel | First 100 users |
| **Design** | Figma | First designer |
| **Dev tools** | GitHub, Vercel, AWS | Day 1 |

**Tool selection principles:**

- **Free tier first.** Start with free tools. Upgrade when you hit limits, not before.
- **One tool per function.** Don't have three project management tools. Consolidate.
- **Integrate where possible.** Tools that talk to each other reduce manual work.
- **Standardize early.** The cost of switching tools (migrating data, retraining team) is high. Pick well, but don't overthink it.
- **Eliminate, don't add.** Before adding a new tool, ask: "Can we stop doing this task altogether?"

**The Notion trap:**

Notion is powerful but can become a black hole of information. Pages multiply, structure is lost, and nobody can find anything. If you use Notion: establish page hierarchy early, assign page owners, and archive old content.

### Process Design

Process is the system for turning inputs into outputs reliably. Good process enables speed. Bad process creates bureaucracy.

**When to add process:**

| Signal | Process to Add |
|--------|----------------|
| Same mistake happens 3+ times | Document the correct approach |
| New person takes weeks to contribute | Create onboarding checklist |
| Decisions are inconsistent | Establish decision criteria |
| Information isn't shared | Set up recurring update cadence |
| Tasks fall through the cracks | Create tracking system |

**Process design principles:**

- **Document before you automate.** A spreadsheet that works is better than software nobody uses.
- **Write it down.** Document your process before hiring someone to do it. The document is the training material.
- **Start with output, work backward.** What outcome do you want? Then design the process to produce it.
- **Question every step.** "Why do we do this?" If nobody knows, eliminate it.
- **Review regularly.** Processes that made sense at 5 people may not at 20.

**The process maturity model:**

| Level | Description | Example |
|-------|-------------|---------|
| **Ad hoc** | Everyone does it differently | Each engineer deploys differently |
| **Standardized** | One documented way | Shared deployment instructions |
| **Enforced** | System prevents deviation | CI/CD pipeline with automated checks |
| **Optimized** | Continuously improved | Regular retrospectives improve the pipeline |

**Process = speed, not bureaucracy:**

The best processes remove decision fatigue. When you know exactly how to handle a customer complaint, you handle it faster. When there's a clear escalation path, incidents get resolved faster. Process is not the enemy of speed — chaos is.

**Key processes every startup needs:**

| Process | When to Create |
|---------|----------------|
| Hiring and onboarding | Before first hire |
| Budget approval | Before first significant vendor purchase |
| Incident response | Before first production system |
| Customer complaint handling | Before first paying customer |
| Data backup and recovery | Day 1 |
| Security incident response | Before storing customer data |
| Expense reporting | Before second employee |
| Performance reviews | Before 10 employees |

### Risk Management

Startups operate in uncertainty. Risk management identifies what could go wrong and prepares for it.

**Categories of startup risk:**

| Risk Category | Examples | Likelihood | Impact |
|---------------|----------|------------|--------|
| **Market risk** | No demand, wrong market, timing | High | Fatal |
| **Financial risk** | Run out of money, fraud | Medium | Fatal |
| **Product risk** | Technical failure, bugs, security breach | Medium | Serious |
| **Team risk** | Founder departure, key person loss, culture failure | Medium | Serious |
| **Competitive risk** | Competitor raises more, launches faster | Medium | Significant |
| **Regulatory risk** | New regulations, compliance failure | Low | Fatal |

**Insurance for startups:**

| Type | What It Covers | Typical Cost |
|------|----------------|--------------|
| **General liability** | Bodily injury, property damage | $500-2K/year |
| **Professional liability (E&O)** | Errors and omissions in your product | $2-10K/year |
| **Directors & Officers (D&O)** | Board member liability | $5-20K/year |
| **Cyber liability** | Data breach costs, legal fees | $2-15K/year |
| **Workers' compensation** | Employee injuries | $500-5K/year |

**Contingency planning:**

| Risk | Contingency |
|------|-------------|
| Co-founder leaves | Vesting schedule, buy-sell agreement |
| Key person leaves | Cross-train, document processes |
| Server outage | Multi-region, backup, disaster recovery plan |
| Economic downturn | Reduce burn rate, extend runway |
| Major customer churns | Customer concentration limits, diversification plan |

**The risk register:**

Maintain a simple risk register that tracks:

1. Risk description
2. Likelihood (1-5)
3. Impact (1-5)
4. Risk score (likelihood × impact)
5. Mitigation strategy
6. Owner
7. Status

Review the risk register quarterly. Risks change as the business evolves.

### Common Mistakes

**No metrics.** The founder runs on intuition alone. They don't know churn, activation rate, or unit economics. When things go wrong, they can't diagnose why.

**Dashboard overload.** The opposite problem — tracking 50 metrics and not knowing which ones matter. Focus on 5-10 key metrics. Go deep on the ones that predict your business health.

**Ignoring cash.** The company is growing but the bank account is shrinking. The founder doesn't look at cash until it's too late. Cash is oxygen — check it weekly.

**No documentation.** The founder is the only one who knows how things work. When they're unavailable, the company stops. Document critical processes.

**Over-hiring operational roles.** Before you have revenue, every hire should be revenue-generating or product-building. Operations hires come after product-market fit.

**Process before product.** A 20-person startup with a twice-yearly planning process, quarterly OKRs, monthly reviews, and weekly standups for every function. They spend more time planning than building. Process serves the product, not the other way around.

**No board communication.** The board hears from the CEO once a quarter. When things go wrong, they're surprised and lose confidence. Communicate with the board weekly or bi-weekly, even briefly.

**Party like it's 2021.** Over-hiring, overspending, assuming the next round will come. Run the company as if the next round won't come. Revenue is the only reliable funding source.

## Study Cases

### Case 1: The Dashboard That Saved a Startup

A SaaS startup was burning cash at alarming rate. The founders felt like they were growing but couldn't tell if they were healthy. They built a simple dashboard with 8 metrics: MRR, new customers, churn, CAC, LTV, burn, runway, and NPS. Within a month, they discovered their CAC was 3x higher than they thought and their churn was 8% monthly. They fixed the churn first (onboarding improvements), then the CAC (cut underperforming ad channels). Runway extended from 6 to 12 months without raising more money.

### Case 2: The Process That Created Chaos

A founder read about "scaling processes" and implemented a full project management system, weekly planning, quarterly OKRs, and bi-weekly performance reviews — at a 5-person startup. The team spent 20% of their time in meetings and process overhead. Output dropped. The founder scaled back to: one weekly standup, one shared document for priorities, and everything else ad hoc. Output recovered.

Lesson: Process should be the minimum needed to prevent chaos, not the maximum you can imagine.

## Glossary

| Term | Definition |
|------|------------|
| North star metric | The single metric that best captures customer value and business success |
| OKR | Objectives and Key Results — a goal-setting framework |
| MRR | Monthly Recurring Revenue |
| ARR | Annual Recurring Revenue |
| Burn multiple | Net burn divided by net new ARR — efficiency metric |
| Runway | Months until the company runs out of cash |
| CAC | Customer Acquisition Cost |
| LTV | Lifetime Value — total revenue from a customer over their lifetime |
| Board deck | A presentation to the board of directors covering company performance |
| Risk register | A document tracking identified risks, their likelihood, impact, and mitigation |

## Next Steps

- [Fundraising](fundraising.md) — financial metrics that investors evaluate
- [Crisis Management](crisis-management.md) — what to do when risks materialize
- [Founder Development](founder-development.md) — personal effectiveness for scaling leaders
