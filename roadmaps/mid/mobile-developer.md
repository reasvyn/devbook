# Mid Mobile Developer

## Description

What a mid-level mobile developer should know — architectural patterns, platform depth, offline-first design, performance optimization, testing, and publishing independently.

## Prerequisites

- [Junior Mobile Developer](../junior/mobile-developer.md)

## Learning Path

### Architecture & Design Patterns

- `🔴 CRITICAL` MVVM / MVI — separating concerns on mobile
- `🔴 CRITICAL` Repository pattern — abstracting data sources
- `🔴 CRITICAL` Dependency injection — Dagger / Hilt (Android), Swinject (iOS), riverpod (Flutter)
- `🔴 CRITICAL` Navigation — type-safe routing, deep linking
- `🟠 HIGH` Clean Architecture — use cases, layers, dependency rules
- `🟠 HIGH` Unidirectional data flow — state management at scale

### Platform Depth

**Android:**
- `🔴 CRITICAL` Coroutines and Flow — structured concurrency, state collection
- `🔴 CRITICAL` Jetpack Compose in production — side effects, performance, theming
- `🔴 CRITICAL` Room database — DAOs, migrations, relations
- `🟠 HIGH` WorkManager — background work, constraints
- `🟠 HIGH` Hilt — scoped dependencies, modules, qualifiers
- `🟡 MEDIUM` NDK and JNI basics

**iOS:**
- `🔴 CRITICAL` Swift concurrency — async/await, actors, Task
- `🔴 CRITICAL` SwiftUI in production — @StateObject, @Environment, performance
- `🔴 CRITICAL` Core Data / SwiftData — model layer, migrations, background context
- `🟠 HIGH` Combine framework — publishers, subscribers, operators
- `🟠 HIGH` Swift Package Manager — creating and consuming packages
- `🟡 MEDIUM` UIKit interoperability — bridging SwiftUI and UIKit

**Cross-Platform (if applicable):**
- `🔴 CRITICAL` Platform channels — calling native code from Flutter / RN
- `🟠 HIGH` Performance profiling — Frame rate, memory, bundle size
- `🟠 HIGH` Code sharing strategies — monorepo, shared logic layer

### Offline-First & Data Sync

- `🔴 CRITICAL` Local-first architecture — offline writes, sync queue
- `🔴 CRITICAL` Conflict resolution — last-write-wins, CRDTs, custom merge
- `🔴 CRITICAL` Background sync — periodic sync, push-triggered sync
- `🟠 HIGH` Optimistic UI — immediate feedback, rollback on failure
- `🟠 HIGH` Pagination — cursor-based, offset-based, infinite scroll

### Networking & APIs

- `🔴 CRITICAL` Retrofit / Ktor / URLSession — interceptors, logging, auth
- `🔴 CRITICAL` Authentication flows — login, token refresh, biometrics
- `🔴 CRITICAL` Certificate pinning — SSL / TLS security
- `🟠 HIGH` WebSocket — real-time communication in mobile
- `🟠 HIGH` Upload management — multipart, progress tracking, retry
- `🟡 MEDIUM` GraphQL on mobile — Apollo, queries, caching

### Performance

- `🔴 CRITICAL` Memory management — leaks, weak references, profiling
- `🔴 CRITICAL` Image optimization — caching, downsampling, disk storage
- `🔴 CRITICAL` UI performance — 60fps rendering, layout passes, overdraw
- `🟠 HIGH` Startup time — app cold start, lazy initialization
- `🟠 HIGH` Battery optimization — background work, network batching
- `🟡 MEDIUM` App size optimization — resource shrinking, ProGuard, bitcode

### Testing

- `🔴 CRITICAL` Unit testing — ViewModels, repositories, use cases
- `🔴 CRITICAL` UI testing — Compose / XCTest / Flutter Driver
- `🟠 HIGH` Snapshot testing — UI consistency across releases
- `🟠 HIGH` Integration testing — database, API, navigation flows
- `🟡 MEDIUM` Performance testing — benchmark tests, profiling in CI

### CI/CD & Quality

- `🔴 CRITICAL` CI for mobile — GitHub Actions, Bitrise, CircleCI
- `🔴 CRITICAL` Linting and static analysis — detekt, SwiftLint, ESLint
- `🔴 CRITICAL` Automated build pipeline — signed releases, versioning
- `🟠 HIGH` Code signing management — certificates, provisioning profiles
- `🟠 HIGH` Beta distribution — TestFlight, Firebase App Distribution
- `🟡 MEDIUM` Feature flags — gradual rollout, A/B testing

### Security

- `🔴 CRITICAL` Secure storage — EncryptedSharedPreferences, Keychain
- `🔴 CRITICAL` ProGuard / R8 — obfuscation, shrinking
- `🟠 HIGH` Root / jailbreak detection
- `🟠 HIGH` Network security config — cleartext traffic, proxy detection
- `🟡 MEDIUM` Runtime application self-protection (RASP) awareness

### Monitoring & Crash Reporting

- `🔴 CRITICAL` Crash reporting — Crashlytics, Sentry
- `🔴 CRITICAL` Analytics — Firebase, Mixpanel, Amplitude
- `🟠 HIGH` Remote logging — log collection from production
- `🟠 HIGH` Performance monitoring — Firebase Performance, New Relic

### Soft Skills

- `🔴 CRITICAL` Mentoring junior mobile developers — code review, pairing
- `🔴 CRITICAL` Platform-specific UX conventions — Material Design, HIG
- `🟠 HIGH` Collaboration with designers — component handoff, prototyping
- `🟠 HIGH` App review process — anticipating rejection reasons, appeal strategy
- `🟠 HIGH` Estimation — breaking mobile features into shippable increments

## Next Steps

- [Senior Full-Stack Developer](../senior/full-stack-developer.md) — broaden beyond mobile
- [Software Architect](../specialist/software-architect.md) — system design for mobile at scale
