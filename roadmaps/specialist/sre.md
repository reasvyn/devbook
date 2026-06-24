# Site Reliability Engineer (SRE)

## Description

What an SRE should know — reliability engineering, incident management, capacity planning, automation, and building systems that meet their SLOs.

## Prerequisites

- [Senior DevOps Engineer](../senior/devops.md) or equivalent deep ops/infra experience

## Learning Path

### Reliability Engineering

- `[must-know]` SLO/SLI/SLA design — defining meaningful targets
- `[must-know]` Error budgets — balancing reliability with feature velocity
- `[must-know]` Risk assessment — identifying failure modes, blast radius
- `[must-know]` Capacity planning — load testing, right-sizing, forecasting
- `[good-to-know]` Chaos engineering — Game Days, fault injection
- `[nice-to-have]` Formal methods — TLA+ for distributed systems verification

### Incident Management

- `[must-know]` Incident command system — roles (IC, comms, ops)
- `[must-know]` Declaring and resolving incidents — severity levels, timelines
- `[must-know]` Post-incident reviews — blameless culture, action items
- `[must-know]` On-call excellence — runbooks, escalation, fatigue management
- `[good-to-know]` Incident analysis — root cause vs contributing factors
- `[good-to-know]` Measuring MTTR, MTTD, and driving improvement

### Automation & Platform

- `[must-know]` Automating operational toil — identifying, measuring, eliminating
- `[must-know]` Infrastructure as Code — Terraform, Pulumi, Crossplane
- `[must-know]` GitOps — declarative deployments, drift detection
- `[good-to-know]` Self-healing systems — auto-remediation, Kubernetes operators
- `[good-to-know]` Progressive delivery — canary, blue/green, feature flags

### Observability

- `[must-know]` Distributed tracing — OpenTelemetry, sampling strategies
- `[must-know]` Metrics — RED method (Rate, Errors, Duration), USE method
- `[must-know]` Logging — structured, correlation IDs, log levels
- `[must-know]` Dashboard design — actionable, hierarchical, minimal
- `[good-to-know]` Alert fatigue — reducing noise, meaningful thresholds
- `[nice-to-have]` AIOps — anomaly detection, automated RCA

### Performance & Scalability

- `[must-know]` Performance testing strategy — load, stress, endurance
- `[must-know]` Bottleneck identification — CPU, memory, I/O, network, locks
- `[must-know]` Auto-scaling — horizontal, vertical, event-driven
- `[good-to-know]` CDN and edge computing optimization
- `[good-to-know]` Database performance — connection pooling, query optimization

### Security & Compliance

- `[must-know]` Zero-trust operations — secure deployment, least privilege
- `[must-know]` Disaster recovery — RPO/RTO, backup validation, failover drills
- `[good-to-know]` Supply chain security — SBOM, image signing, dependency scanning
- `[good-to-know]` Compliance automation — audit-ready evidence collection

### Culture & Leadership

- `[must-know]` Building reliability culture across engineering teams
- `[must-know]` Production readiness reviews — gates for launching services
- `[must-know]` Mentoring ops and dev engineers — spreading SRE practices
- `[good-to-know]` Vendor evaluation — observability, incident management platforms

## Next Steps

- [Software Architect](software-architect.md) — broader system architecture
- Principal Engineer — IC track beyond SRE
