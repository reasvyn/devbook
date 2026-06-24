# Why Security Matters

## Description

Security is not a feature — it's a requirement. Every developer has a responsibility to understand basic security principles to protect users, data, and systems from threats. This document covers the mindset and fundamentals of secure development.

## Prerequisites

- [Why Networks Matter](../../networks/intro/why-networks.md) — networking basics for understanding attack vectors

## Table of Contents

- [The Security Mindset](#the-security-mindset)
- [Core Security Principles](#core-security-principles)
- [Common Threats Overview](#common-threats-overview)
- [Security Is Everyone's Job](#security-is-everyones-job)

## Content / Material

### The Security Mindset

Security thinking means asking:

- "How could this be abused?"
- "What happens if this input is malicious?"
- "What data am I exposing?"
- "Who should have access to this?"

It's not paranoia — it's risk management.

### Core Security Principles

| Principle | Description |
|-----------|-------------|
| **Least privilege** | Give the minimum access necessary |
| **Defense in depth** | Multiple layers of security — don't rely on a single wall |
| **Fail securely** | Default to denying access on error |
| **Never trust user input** | Validate, sanitize, and escape everything |
| **Encrypt everywhere** | Data at rest and in transit must be encrypted |

### Common Threats Overview

| Threat | What It Is |
|--------|------------|
| **SQL Injection** | Malicious SQL in user input |
| **XSS** | Malicious scripts injected into web pages |
| **CSRF** | Forging requests on behalf of an authenticated user |
| **Authentication bypass** | Gaining access without valid credentials |
| **Data exposure** | Leaking sensitive data through misconfiguration |

### Security Is Everyone's Job

Security isn't just the security team's problem:

- **Frontend devs** — XSS, CSP, secure cookies
- **Backend devs** — input validation, auth, rate limiting
- **DevOps** — firewall rules, secrets management, image scanning
- **PMs** — threat modeling, compliance requirements

## Glossary

| Term | Definition |
|------|------------|
| XSS | Cross-Site Scripting — injecting client-side scripts into web pages |
| CSRF | Cross-Site Request Forgery — tricking a user into performing unwanted actions |
| Least privilege | A principle where entities are given only the minimum access needed |

## Next Steps

- [OWASP Top 10](../beginner/owasp-top-10.md) — the most critical web application security risks
- [Authentication & Authorization](../beginner/auth-basics.md) — sessions, tokens, OAuth2, JWT
