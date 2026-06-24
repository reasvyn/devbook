# Site Reliability Engineer (SRE)

## Description

What an SRE should know — reliability engineering, incident management, capacity planning, automation, and building systems that meet their SLOs.

## Prerequisites

- [Senior DevOps Engineer](../senior/devops.md) or equivalent deep ops/infra experience

## Learning Path

### Reliability Engineering

- `🔴 CRITICAL` SLO/SLI/SLA design — defining meaningful targets
- `🔴 CRITICAL` Error budgets — balancing reliability with feature velocity
- `🔴 CRITICAL` Risk assessment — identifying failure modes, blast radius
- `🔴 CRITICAL` Capacity planning — load testing, right-sizing, forecasting
- `🟠 HIGH` Chaos engineering — Game Days, fault injection
- `🟢 LOW` Formal methods — TLA+ for distributed systems verification

### Incident Management

- `🔴 CRITICAL` Incident command system — roles (IC, comms, ops)
- `🔴 CRITICAL` Declaring and resolving incidents — severity levels, timelines
- `🔴 CRITICAL` Post-incident reviews — blameless culture, action items
- `🔴 CRITICAL` On-call excellence — runbooks, escalation, fatigue management
- `🟠 HIGH` Incident analysis — root cause vs contributing factors
- `🟠 HIGH` Measuring MTTR, MTTD, and driving improvement

### Automation & Platform

- `🔴 CRITICAL` Automating operational toil — identifying, measuring, eliminating
- `🔴 CRITICAL` Infrastructure as Code — Terraform, Pulumi, Crossplane
- `🔴 CRITICAL` GitOps — declarative deployments, drift detection
- `🟠 HIGH` Self-healing systems — auto-remediation, Kubernetes operators
- `🟠 HIGH` Progressive delivery — canary, blue/green, feature flags

### Observability

- `🔴 CRITICAL` Distributed tracing — OpenTelemetry, sampling strategies
- `🔴 CRITICAL` Metrics — RED method (Rate, Errors, Duration), USE method
- `🔴 CRITICAL` Logging — structured, correlation IDs, log levels
- `🔴 CRITICAL` Dashboard design — actionable, hierarchical, minimal
- `🟠 HIGH` Alert fatigue — reducing noise, meaningful thresholds
- `🟢 LOW` AIOps — anomaly detection, automated RCA

### Performance & Scalability

- `🔴 CRITICAL` Performance testing strategy — load, stress, endurance
- `🔴 CRITICAL` Bottleneck identification — CPU, memory, I/O, network, locks
- `🔴 CRITICAL` Auto-scaling — horizontal, vertical, event-driven
- `🟠 HIGH` CDN and edge computing optimization
- `🟠 HIGH` Database performance — connection pooling, query optimization

### Security & Compliance

- `🔴 CRITICAL` Zero-trust operations — secure deployment, least privilege
- `🔴 CRITICAL` Disaster recovery — RPO/RTO, backup validation, failover drills
- `🟠 HIGH` Supply chain security — SBOM, image signing, dependency scanning
- `🟠 HIGH` Compliance automation — audit-ready evidence collection

### Culture & Leadership

- `🔴 CRITICAL` Building reliability culture across engineering teams
- `🔴 CRITICAL` Production readiness reviews — gates for launching services
- `🔴 CRITICAL` Mentoring ops and dev engineers — spreading SRE practices
- `🟠 HIGH` Vendor evaluation — observability, incident management platforms

## Next Steps

- [Software Architect](software-architect.md) — broader system architecture
- Principal Engineer — IC track beyond SRE
