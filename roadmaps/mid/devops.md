# Mid DevOps Engineer

## Description

What a mid-level DevOps engineer should know — containerization, CI/CD pipelines, cloud infrastructure, monitoring, and incident response.

## Prerequisites

- [Junior Backend Developer](../junior/backend.md) — scripting and server basics

## Learning Path

### Scripting & Automation

- `🔴 CRITICAL` Shell scripting — Bash or Zsh, pipes, redirection, cron
- `🔴 CRITICAL` Infrastructure as Code — Terraform or Pulumi
- `🟠 HIGH` Configuration management — Ansible, Chef, or Puppet
- `🟠 HIGH` Python or Go for automation tooling

### Containers & Orchestration

- `🔴 CRITICAL` Docker — Dockerfiles, multi-stage builds, docker-compose
- `🔴 CRITICAL` Container registries — Docker Hub, ECR, GCR
- `🔴 CRITICAL` Kubernetes basics — pods, deployments, services, configmaps, secrets
- `🟠 HIGH` Helm — packaging Kubernetes applications
- `🟠 HIGH` kustomize, kubectl workflows
- `🟢 LOW` Service mesh basics — Istio, Linkerd

### CI/CD

- `🔴 CRITICAL` CI/CD pipelines — GitHub Actions, GitLab CI, Jenkins
- `🔴 CRITICAL` Build, test, deploy stages
- `🔴 CRITICAL` Artifact management — storing and versioning builds
- `🟠 HIGH` Deployment strategies — blue/green, canary, rolling updates
- `🟠 HIGH` Feature flags — LaunchDarkly, Flagsmith

### Cloud Platforms

Pick **one** primary:
- `🔴 CRITICAL` **AWS** — EC2, S3, RDS, VPC, IAM
- `🔴 CRITICAL` **GCP** — Compute Engine, Cloud Storage, Cloud SQL, VPC, IAM
- `🔴 CRITICAL` **Azure** — VMs, Blob Storage, SQL Database, VNet, Entra ID
- `🟠 HIGH` Multi-cloud and cloud-agnostic strategies

### Monitoring & Observability

- `🔴 CRITICAL` Infrastructure monitoring — CPU, memory, disk, network
- `🔴 CRITICAL` Application monitoring — Prometheus, Grafana
- `🔴 CRITICAL` Centralized logging — ELK, Loki, CloudWatch Logs
- `🟠 HIGH` Alerting — Alertmanager, PagerDuty, OpsGenie
- `🟠 HIGH` Distributed tracing — Jaeger, OpenTelemetry

### Networking

- `🔴 CRITICAL` DNS — how it works, record types, troubleshooting
- `🔴 CRITICAL` Load balancing — ALB, Nginx, HAProxy
- `🔴 CRITICAL` Firewalls and security groups
- `🟠 HIGH` Reverse proxies — Nginx, Caddy, Traefik
- `🟠 HIGH` TLS/SSL — certificates, Let's Encrypt, cert-manager

### Security

- `🔴 CRITICAL` Identity and Access Management (IAM) — least privilege
- `🔴 CRITICAL` Secrets management — HashiCorp Vault, AWS Secrets Manager
- `🔴 CRITICAL` Vulnerability scanning — container scanning, dependency scanning
- `🟠 HIGH` Network policies and zero-trust models

### Incident Response

- `🔴 CRITICAL` On-call rotations and escalation policies
- `🔴 CRITICAL` Runbooks — documenting incident response steps
- `🟠 HIGH` Post-mortems — blameless culture, action items

## Next Steps

- [Senior DevOps Engineer](../senior/devops.md) — platform engineering, SRE, architecture
