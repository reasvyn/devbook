# Mid DevOps Engineer

## Description

What a mid-level DevOps engineer should know — containerization, CI/CD pipelines, cloud infrastructure, monitoring, and incident response.

## Prerequisites

- [Junior Backend Developer](../junior/backend.md) — scripting and server basics

## Learning Path

### Scripting & Automation

- `[must-know]` Shell scripting — Bash or Zsh, pipes, redirection, cron
- `[must-know]` Infrastructure as Code — Terraform or Pulumi
- `[good-to-know]` Configuration management — Ansible, Chef, or Puppet
- `[good-to-know]` Python or Go for automation tooling

### Containers & Orchestration

- `[must-know]` Docker — Dockerfiles, multi-stage builds, docker-compose
- `[must-know]` Container registries — Docker Hub, ECR, GCR
- `[must-know]` Kubernetes basics — pods, deployments, services, configmaps, secrets
- `[good-to-know]` Helm — packaging Kubernetes applications
- `[good-to-know]` kustomize, kubectl workflows
- `[nice-to-have]` Service mesh basics — Istio, Linkerd

### CI/CD

- `[must-know]` CI/CD pipelines — GitHub Actions, GitLab CI, Jenkins
- `[must-know]` Build, test, deploy stages
- `[must-know]` Artifact management — storing and versioning builds
- `[good-to-know]` Deployment strategies — blue/green, canary, rolling updates
- `[good-to-know]` Feature flags — LaunchDarkly, Flagsmith

### Cloud Platforms

Pick **one** primary:
- `[must-know]` **AWS** — EC2, S3, RDS, VPC, IAM
- `[must-know]` **GCP** — Compute Engine, Cloud Storage, Cloud SQL, VPC, IAM
- `[must-know]` **Azure** — VMs, Blob Storage, SQL Database, VNet, Entra ID
- `[good-to-know]` Multi-cloud and cloud-agnostic strategies

### Monitoring & Observability

- `[must-know]` Infrastructure monitoring — CPU, memory, disk, network
- `[must-know]` Application monitoring — Prometheus, Grafana
- `[must-know]` Centralized logging — ELK, Loki, CloudWatch Logs
- `[good-to-know]` Alerting — Alertmanager, PagerDuty, OpsGenie
- `[good-to-know]` Distributed tracing — Jaeger, OpenTelemetry

### Networking

- `[must-know]` DNS — how it works, record types, troubleshooting
- `[must-know]` Load balancing — ALB, Nginx, HAProxy
- `[must-know]` Firewalls and security groups
- `[good-to-know]` Reverse proxies — Nginx, Caddy, Traefik
- `[good-to-know]` TLS/SSL — certificates, Let's Encrypt, cert-manager

### Security

- `[must-know]` Identity and Access Management (IAM) — least privilege
- `[must-know]` Secrets management — HashiCorp Vault, AWS Secrets Manager
- `[must-know]` Vulnerability scanning — container scanning, dependency scanning
- `[good-to-know]` Network policies and zero-trust models

### Incident Response

- `[must-know]` On-call rotations and escalation policies
- `[must-know]` Runbooks — documenting incident response steps
- `[good-to-know]` Post-mortems — blameless culture, action items

## Next Steps

- [Senior DevOps Engineer](../senior/devops.md) — platform engineering, SRE, architecture
