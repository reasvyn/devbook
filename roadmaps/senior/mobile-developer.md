# Senior Mobile Developer

## Description

What a senior mobile developer should know — platform architecture, cross-platform strategy, performance at scale, release governance, and technical leadership across mobile org.

## Prerequisites

- [Mid Mobile Developer](../mid/mobile-developer.md)

## Learning Path

### Mobile Architecture

- `🔴 CRITICAL` Architecture across teams — modularization, feature-first packaging
- `🔴 CRITICAL` Multi-module builds — build time optimization, dependency graphs
- `🔴 CRITICAL` Cross-platform strategy — when to share code, when to go native
- `🔴 CRITICAL` White-label and multi-brand apps — build flavors, product flavors
- `🟠 HIGH` Dynamic features — on-demand delivery, app bundles
- `🟡 MEDIUM` Kotlin Multiplatform / C++ shared logic

### Performance at Scale

- `🔴 CRITICAL` Startup optimization — app startup phases, cold/warm/hot
- `🔴 CRITICAL` Memory profiling — heap dumps, allocation tracking, leak detection
- `🔴 CRITICAL` Frame rate optimization — GPU profiling, layout inspector
- `🔴 CRITICAL` Battery impact — power profiles, background work auditing
- `🟠 HIGH` Network performance — request coalescing, pre-fetching, HTTP/3
- `🟠 HIGH` Database performance — query optimization, indexing, vacuuming
- `🟡 MEDIUM` Benchmarking framework — macrobenchmark, MetricKit

### Advanced Platform Features

**Android:**
- `🔴 CRITICAL` Compose internals — recomposition, stability, strong skipping
- `🔴 CRITICAL` Custom views and canvas drawing
- `🔴 CRITICAL` Performance tooling — Systrace, Perfetto, profiler
- `🟠 HIGH` Accessibility — TalkBack, custom actions, accessibility tree
- `🟠 HIGH` Large screens — foldables, tablets, multi-window
- `🟡 MEDIUM` Machine Learning Kit, CameraX, ARCore

**iOS:**
- `🔴 CRITICAL` SwiftUI internals — identity, lifetime, performance
- `🔴 CRITICAL` UIKit interop — bridging, performance considerations
- `🔴 CRITICAL` Performance tooling — Instruments, os_log, signposts
- `🟠 HIGH` Accessibility — VoiceOver, dynamic type, reduced motion
- `🟠 HIGH` iPadOS — multitasking, drag and drop, keyboard shortcuts
- `🟡 MEDIUM` Siri Intents, Widgets, Live Activities

### Release Governance & Quality

- `🔴 CRITICAL` Release strategy — phased rollouts, staged rollbacks
- `🔴 CRITICAL` CI/CD for mobile at scale — matrix builds, cache optimization
- `🔴 CRITICAL` Code signing and provisioning automation
- `🟠 HIGH` A/B testing — feature flags, experiment frameworks
- `🟠 HIGH` Monitoring — crash rate, ANR rate, freeze rate, app store ratings
- `🟠 HIGH` App size governance — size budgets, diff tracking, asset optimization

### Testing Strategy

- `🔴 CRITICAL` Testing pyramid for mobile — unit, integration, UI, manual
- `🔴 CRITICAL` Flaky test elimination — deterministic tests, test isolation
- `🔴 CRITICAL` Screenshot testing — baseline management, diff thresholds
- `🟠 HIGH` Monkey / fuzz testing — random input, edge cases
- `🟠 HIGH` Beta testing programs — internal, closed, open tracks
- `🟡 MEDIUM` Performance regression testing — CI-integrated benchmarks

### Security

- `🔴 CRITICAL` Mobile threat modeling — device compromise, network attacks
- `🔴 CRITICAL` Obfuscation and anti-tampering — ProGuard, R8, DX protection
- `🔴 CRITICAL` Secure network communication — pinning, mTLS, DoH
- `🟠 HIGH` App attestation — Play Integrity, DeviceCheck, App Attest
- `🟠 HIGH` Data privacy — encryption at rest, key management

### Monitoring & Observability

- `🔴 CRITICAL` Crash aggregation and prioritization — grouping, impact analysis
- `🔴 CRITICAL` User-centric metrics — startup time, crash-free rate, adoption
- `🔴 CRITICAL` Remote config — feature toggles, A/B parameters
- `🟠 HIGH` Analytics architecture — event design, funnel analysis
- `🟠 HIGH` Logging strategy — structured logging, log levels, sampling

### Technical Leadership

- `🔴 CRITICAL` Mobile platform strategy — framework decisions, SDK lifecycle
- `🔴 CRITICAL` Cross-team alignment — iOS/Android parity, shared timelines
- `🔴 CRITICAL` Mentoring — growing mid and junior mobile developers
- `🟠 HIGH` Hiring — designing mobile interview loops, rubric creation
- `🟠 HIGH` Vendor evaluation — analytics, crash reporting, CI tools
- `🟠 HIGH` RFCs and technical proposals — documenting mobile architecture decisions

## Next Steps

- [Software Architect](../specialist/software-architect.md) — platform architecture, org-wide impact
- [Engineering Manager](../specialist/engineering-manager.md) — people leadership track
