# The Security Mindset

## Description

Security is not a checklist — it's a way of thinking. The security mindset means constantly asking "how could this be abused?" and designing systems that are resilient even when components fail or are attacked. This document explores the adversarial thinking that separates secure systems from vulnerable ones.

## Prerequisites

- [Why Security Matters](why-security.md)

## Table of Contents

- [Adversarial Thinking](#adversarial-thinking)
- [Attack Surface](#attack-surface)
- [Defense in Depth](#defense-in-depth)
- [Assume Breach](#assume-breach)

## Content / Material

### Adversarial Thinking

Most developers think in terms of how a system *should* be used. Security thinkers also consider how it could be *misused*:

- "This endpoint returns user data" → "What if someone requests someone else's data?"
- "This input is validated on the client" → "What if someone sends a raw request?"
- "This password is hashed" → "What if the hash database is leaked?"

Adversarial thinking is not paranoia — it's risk management based on realistic threat models.

### Attack Surface

The attack surface is the sum of all entry points into your system:

- API endpoints
- User input fields
- File uploads
- Authentication mechanisms
- Third-party integrations
- Error messages (they can leak information)

Every feature adds to the attack surface. The goal is not zero attack surface (that would mean no functionality) — it's minimizing the attack surface while maximizing security controls at the remaining entry points.

### Defense in Depth

Never rely on a single security control. If one layer fails, others should still protect you:

```
Layer 1: Firewall
Layer 2: WAF (Web Application Firewall)
Layer 3: Authentication
Layer 4: Authorization
Layer 5: Input validation
Layer 6: Output encoding
Layer 7: Least privilege in database
Layer 8: Encryption at rest
```

A castle doesn't have just one wall — it has moats, ramparts, gates, guards, and inner keeps.

### Assume Breach

"Assume breach" is the principle that you design your system as if an attacker is already inside:

- Encrypt data at rest — even if someone steals the database.
- Use short-lived tokens — even if an access token is compromised, it expires.
- Implement least privilege — even if a service is compromised, the damage is contained.
- Audit and log everything — even if you don't detect the breach immediately, you can investigate afterward.

## Glossary

| Term | Definition |
|------|------------|
| Attack surface | The total set of entry points through which an attacker can interact with a system |
| Defense in depth | A strategy using multiple layers of security controls |
| Assume breach | A design philosophy assuming an attacker is already present |
| Threat model | A structured analysis of potential threats to a system |

## Next Steps

- [Principles of Trust](principles-of-trust.md) — trust models, zero trust, and identity
