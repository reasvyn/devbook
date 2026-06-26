# Junior Mobile Developer

## Description

What a junior mobile developer should know — platform fundamentals, UI development, navigation, data persistence, API integration, and publishing basics for Android or iOS.

## Prerequisites

- Basic programming knowledge — variables, functions, control flow, OOP concepts

## Learning Path

### Choose a Platform

Pick **one** primary path (or both over time):

**Android (Kotlin):**
- `🔴 CRITICAL` Kotlin basics — variables, functions, classes, null safety
- `🔴 CRITICAL` Android Studio — project structure, build system (Gradle), emulator
- `🔴 CRITICAL` Activities and Fragments — lifecycle, navigation
- `🔴 CRITICAL` Layouts — XML, ConstraintLayout, LinearLayout, RecyclerView
- `🔴 CRITICAL` Jetpack Compose basics — composables, state, modifiers

**iOS (Swift):**
- `🔴 CRITICAL` Swift basics — variables, functions, classes, optionals
- `🔴 CRITICAL` Xcode — project structure, Interface Builder, simulator
- `🔴 CRITICAL` View Controllers — lifecycle, segues, navigation controllers
- `🔴 CRITICAL` Auto Layout — constraints, stack views, safe area
- `🔴 CRITICAL` SwiftUI basics — views, state, modifiers, previews

### Cross-Platform (Alternative)

- `🔴 CRITICAL` React Native — components, navigation, native modules
- `🔴 CRITICAL` Flutter — widgets, state management, material design
- `🟠 HIGH` Expo (React Native) — managed workflow, OTA updates

### UI Development

- `🔴 CRITICAL` Building screens — buttons, text inputs, lists, images
- `🔴 CRITICAL` Navigation — stack, tab, drawer navigation patterns
- `🔴 CRITICAL` Handling user input — forms, validation, keyboard handling
- `🔴 CRITICAL` Responsive layout — different screen sizes and orientations
- `🟠 HIGH` Custom styling — themes, typography, colors
- `🟠 HIGH` Animations — transitions, gestures, basic motion

### Data & Persistence

- `🔴 CRITICAL` Local storage — SharedPreferences / UserDefaults
- `🟠 HIGH` SQLite / Room / Core Data — structured local persistence
- `🟠 HIGH` File storage — reading/writing local files
- `🟡 MEDIUM` State management patterns for mobile

### Networking

- `🔴 CRITICAL` [HTTP requests](../../networks/http-api/http-methods-and-status.md) — Retrofit (Android), URLSession (iOS)
- `🔴 CRITICAL` [JSON parsing](../../networks/http-api/headers-body-and-json.md) — serialization/deserialization
- `🟠 HIGH` [REST API integration](../../networks/http-api/rest-api-design.md) — GET, POST, PUT, DELETE
- `🟠 HIGH` Handling network errors — timeouts, no connection, retry
- `🟡 MEDIUM` Image loading libraries — Glide, Picasso, SDWebImage

### Version Control

- `🔴 CRITICAL` [Git basics](../../software/version-control/git-basics.md) — add, commit, push, pull, branch, merge
- `🔴 CRITICAL` Pull requests — creating and responding to review feedback
- `🟠 HIGH` .gitignore — platform-specific exclusions

### Platform-Specific Essentials

**Android:**
- `🔴 CRITICAL` Android Manifest — permissions, activities, intents
- `🔴 CRITICAL` Debugging — Logcat, Android Studio debugger
- `🟠 HIGH` Material Design guidelines

**iOS:**
- `🔴 CRITICAL` Info.plist — permissions, configurations
- `🔴 CRITICAL` Debugging — Xcode debugger, breakpoints, Instruments basics
- `🟠 HIGH` Human Interface Guidelines

### Testing

- `🟠 HIGH` Testing on physical devices vs emulators/simulators
- `🟡 MEDIUM` Writing unit tests for business logic
- `🟢 LOW` UI testing basics

### Publishing

- `🔴 CRITICAL` Generating signed builds — APK/AAB for Android, IPA for iOS
- `🟠 HIGH` Google Play Console — store listing, testing tracks
- `🟠 HIGH` Apple App Store Connect — certificates, provisioning profiles
- `🟡 MEDIUM` Understanding app review guidelines

### Soft Skills

- `🔴 CRITICAL` Testing on real devices — catching platform-specific issues
- `🔴 CRITICAL` Reading platform documentation — developer.android.com, developer.apple.com
- `🟠 HIGH` Understanding platform conventions — back navigation, gestures, permissions
- `🟠 HIGH` Communicating platform constraints in planning

## Next Steps

- [Mid Backend Developer](../mid/backend-developer.md) — deepen API and server-side skills
- [Senior Full-Stack Developer](../senior/full-stack-developer.md) — architecture for mobile at scale
