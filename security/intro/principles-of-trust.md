# Principles of Trust

## Description

Every security decision is ultimately about trust: who do you trust, how much, and for what? This document explores the philosophical and practical principles of trust in computing — from passwords to PKI to zero trust architectures.

## Prerequisites

- [Why Security Matters](why-security.md)
- [The Security Mindset](the-security-mindset.md)

## Table of Contents

- [What Is Trust in Computing](#what-is-trust-in-computing)
- [Authentication vs. Authorization](#authentication-vs-authorization)
- [Chains of Trust](#chains-of-trust)
- [Zero Trust](#zero-trust)

## Content / Material

### What Is Trust in Computing

Trust in computing means relying on a component, person, or system to behave as expected. Every trust decision involves three questions:

1. **Identity** — who is the entity?
2. **Integrity** — can I verify they haven't been tampered with?
3. **Scope** — what am I trusting them to do?

The moment you type a password, you're trusting: the OS not to log it, the network not to intercept it, the server to hash it correctly, and the database to store it securely.

### Authentication vs. Authorization

| Concept | Question | Example |
|---------|----------|---------|
| **Authentication (AuthN)** | Who are you? | Login with password + MFA |
| **Authorization (AuthZ)** | What can you do? | RBAC: user can read, admin can write |

Authentication establishes identity. Authorization establishes permissions. They are separate concerns — mixing them is a common source of vulnerabilities.

### Chains of Trust

Trust is rarely direct. The web trusts Certificate Authorities (CAs), which sign certificates for websites:

```
Browser → Trusted CA list → CA verifies domain → CA signs cert
                                                      ↓
                                               Website presents cert
                                                      ↓
                                               Browser trusts website
```

A chain of trust is only as strong as its weakest link. If a CA is compromised, every certificate they signed is untrustworthy.

### Zero Trust

Traditional security trusts everything inside the corporate network. Zero trust flips this: **never trust, always verify**.

| Traditional | Zero Trust |
|-------------|------------|
| Trust the network | Trust nothing — not the network, not devices, not users |
| Perimeter defense | Every request is authenticated and authorized |
| VPN for access | Micro-segmentation, least privilege |
| Implicit trust inside | Continuous verification |

Zero trust is a philosophy, not a product. It applies to cloud, on-prem, and hybrid architectures.

## Glossary

| Term | Definition |
|------|------------|
| Authentication | Verifying the identity of a user or system |
| Authorization | Determining what an authenticated entity is allowed to do |
| PKI | Public Key Infrastructure — a system for managing digital certificates |
| Zero trust | A security model that never implicitly trusts any entity |

## Next Steps

- [OWASP Top 10](../beginner/owasp-top-10.md) — the most critical web application security risks
