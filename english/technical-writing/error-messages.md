# Error Messages & User-Facing Text

## Description

Error messages are the most visible form of technical writing. Every user sees them when something goes wrong. A well-crafted error message tells the user what happened, why it happened, and how to fix it. This document covers the principles, grammar, and process for writing user-facing text that reduces frustration and drives resolution.

## Prerequisites

- [Sentence Structure](../grammar-and-style/sentence-structure.md) — constructing clear, grammatical error messages depends on understanding sentence structure
- [Conciseness](../grammar-and-style/conciseness.md) — error messages must be brief; every unnecessary word adds cognitive load under stress

## Table of Contents

- [Why Error Messages Matter](#why-error-messages-matter)
- [The Three-Part Error Message](#the-three-part-error-message)
- [Principles of Good Error Messages](#principles-of-good-error-messages)
- [Error Message Tone and Grammar](#error-message-tone-and-grammar)
- [Error Codes vs. Plain Language](#error-codes-vs-plain-language)
- [Log Messages vs. User-Facing Errors](#log-messages-vs-user-facing-errors)
- [Notification Copy: Success, Warning, Error, Info](#notification-copy-success-warning-error-info)
- [In-Context Messages: Tooltips, Validation, Empty States](#in-context-messages-tooltips-validation-empty-states)
- [Localization of Error Messages](#localization-of-error-messages)
- [Testing Error Messages with Users](#testing-error-messages-with-users)
- [Error Message Style Guide](#error-message-style-guide)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Why Error Messages Matter

Error messages are the moment when users are most frustrated and most receptive to guidance. A bad error message makes the user feel stuck. A good one turns a failure into a minor detour.

**Error messages are documentation.** Unlike a tutorial or reference guide, error messages arrive in context. The user is already in the middle of an action. The error message is their lifeline.

**Error messages define product quality.** Users judge software quality by how it handles failure. A cryptic error code or a generic "Something went wrong" erodes trust. A clear, actionable message builds confidence.

**Error messages are a UX surface.** They appear in dialogs, toasts, inline validation, API responses, and system logs. Each surface has different constraints.

**Cost of bad error messages.** Poor error messages increase support tickets, reduce task completion rates, and damage brand perception. Clear error messages can reduce support volume by up to 40%.

### The Three-Part Error Message

Every error message should answer three questions:

```text
What happened?     →  The problem, in plain language
Why did it happen? →  The cause, with context
How do I fix it?   →  The action the user can take
```

**Three-part template:**

```text
Could not save your file. (what happened)
The disk is full and has no free space. (why)
Free up space on the disk and try again, or save to a different location. (how to fix)
```

**Short-form (when space is limited):**

```text
Disk full. Free space and try again.
```

**Minimal form (inline validation):**

```text
Email address is required.
```

The third part — the fix — is the most important and the most frequently omitted. Whenever possible, tell the user what to do next.

**When to omit parts:**

```text
What happened only:    "Connection lost."
  → Acceptable when the cause is obvious and no action is possible.

What + Why:            "Could not connect to the database because the server is offline."
  → Acceptable when the fix requires administrator action.

All three parts:       "Could not save. Your document exceeds the 10 MB limit. Remove some images or split the file."
  → Best practice for most errors.
```

### Principles of Good Error Messages

**Be specific.**

```text
Too vague:   Error saving document.
Better:      Could not save "report.docx". The file is open in another application.
```

**Put the user in control.**

```text
Passive:     A timeout error occurred during the upload process.
Actionable:  The upload timed out. Check your network connection and try again.
```

**Avoid blaming the user.**

```text
Blaming:     Invalid input entered. Please correct your mistake.
Neutral:     The date format is not recognized. Use YYYY-MM-DD format.
```

**Use the user's language.**

```text
Internal:    SQL constraint violation on column 'user_email'.
User:        This email address is already registered. Sign in instead.
```

**Prioritize the fix.**

```text
Technical:   EOFException while reading configuration: unexpected end of stream.
User:        The configuration file is missing the closing bracket. Add '}' at the end.
```

**Provide a path forward.**

```text
Dead end:    Authentication failed.
Path:        Authentication failed. Check your password or reset it at https://example.com/reset.
```

**Consider the context.**

```text
Toast (3 seconds):   "File saved."
Inline:              "Password must be at least 8 characters."
Full page:           "We cannot find this page. It may have been moved or deleted."
Dialog:              "You have unsaved changes." [Save] [Discard] [Cancel]
```

### Error Message Tone and Grammar

**Use plain language.**

```text
Technical:   The TLS handshake failed due to a certificate authority validation error.
Plain:       Could not establish a secure connection. The server's certificate is not valid.
```

**Use active voice.**

```text
Passive:     The file could not be saved because a lock was acquired by another process.
Active:      Could not save the file. Another process has locked it.
```

**Write in present tense.**

```text
Past:        The connection was lost.
Present:     Connection lost.
```

**Use contractions sparingly.**

```text
Formal:      Cannot save the file.
Friendly:    Can't save the file.
```

**Capitalize and punctuate consistently.**

```text
Sentence case:   "Could not connect to the server."
```

**Avoid exclamation marks and "please."**

```text
Avoid:      Password is incorrect!
Better:     Password is incorrect.
Avoid:      Please try again later.
Better:     Try again later.
```

**Write for international audiences.**

```text
Idiom:      Something went haywire.
Plain:      An unexpected error occurred.
```

### Error Codes vs. Plain Language

Error codes are useful for support and debugging. Users need the plain-language explanation first.

**Showing both:**

```text
User message:   "Could not connect to the database. Check that your database server is running."
Technical info: "Error code: ECONNREFUSED (connection refused by host)"
```

**When to use error codes:**

```text
Use error codes when:
- The user might need to contact support
- The error has many possible causes
- Multiple systems reference the same error

Do not rely on error codes when:
- The message is the only communication channel
- Users are non-technical
- The fix is obvious from context
```

**Formatting error codes:**

```text
HTTP status:      "404 Not Found"
Precise codes:    "Error 0x80004005: Unspecified error"
Human format:     "Error: EACCES (Permission denied) while opening /var/log/app.log"
```

**Machine-parsable vs. human-readable.**

```text
{"error": {"code": "RATE_LIMIT_EXCEEDED", "message": "Too many requests. Try again in 60 seconds."}}
```

### Log Messages vs. User-Facing Errors

Log messages serve developers and operators. User-facing errors serve the end user.

**Comparison:**

```text
Log entry:
[2026-06-29 14:23:01] ERROR [db.connection] SequelizeConnectionError:
  connect ECONNREFUSED 127.0.0.1:5432. Retry attempt 3/5.
  User_id: a7f3c2e, Session: 9b1d4f8

User-facing message:
Could not connect to the database.
Make sure your database server is running on port 5432.
```

**What to keep out of user-facing messages:**

```text
- Stack traces and line numbers
- Internal IP addresses and hostnames
- Database query strings
- Authentication tokens or session IDs
- Internal user IDs and system paths
```

**Structured logging example:**

```json
{
  "timestamp": "2026-06-29T14:23:01Z",
  "level": "ERROR",
  "component": "auth-service",
  "event": "LOGIN_FAILED",
  "userMessage": "Email or password is incorrect.",
  "debugInfo": { "failureReason": "INVALID_PASSWORD", "attemptCount": 3 }
}
```

**Graceful degradation.**

```text
Log:   Uncaught TypeError: Cannot read property 'data' of undefined
User:  An unexpected error occurred. Please reload the page and try again.
       If the problem persists, contact support with reference ID: ERR-A7F3C2E.
```

### Notification Copy: Success, Warning, Error, Info

**Success notifications:**

```text
Tone: Positive, confident
Length: Short (1-10 words)
Examples: "File saved." "Project published." "Settings updated." "Password changed."
```

**Warning notifications:**

```text
Tone: Cautious, informative
Length: Short to medium (5-25 words)
Examples:
  "You have unsaved changes. Do you want to save before closing?"
  "Your session will expire in 5 minutes."
  "This action cannot be undone. Are you sure?"
```

**Error notifications:**

```text
Tone: Neutral, direct
Length: Medium (10-40 words)
Examples:
  "Could not send email. The recipient address was not found. Check the address and try again."
  "Payment failed. Your card was declined. Try a different payment method."
  "Upload failed. The file exceeds the 50 MB limit. Compress the file and try again."
```

**Info notifications:**

```text
Tone: Neutral, helpful
Length: Short to medium (5-20 words)
Examples:
  "New version 3.2 is available. Restart to update."
  "Your trial expires in 7 days."
  "The maintenance window starts at 2:00 AM UTC."
```

**Placement and duration:**

```text
Toast / snackbar:     3-5 seconds, max 80 chars, optional dismiss
Inline notification:  Persistent, max 150 chars, adjacent to form field
Dialog / modal:       User-dismissed, title + body + up to 3 actions
Banner:               Persistent until dismissed, max 120 chars
```

### In-Context Messages: Tooltips, Validation, Empty States

**Tooltips and help text:**

```text
Format: 20-50 chars for tooltips, 50-150 chars for help text

Tooltip: "Download a CSV of your transaction history."
Help:    "Use 8 or more characters with a mix of letters, numbers, and symbols."
```

**Inline validation:**

```text
Format validation:
  "Enter a valid email address."
  "Phone number must be 10 digits."
  "URL must start with https://."

Requirement validation:
  "This field is required."
  "Password must be at least 8 characters."

Comparison validation:
  "Passwords do not match."
  "End date must be after start date."
```

**Empty states:**

```text
Format: Title (optional) + description + action

Examples:
  "No projects yet. Create your first project to get started."
  "No results found. Try adjusting your search or filters."
  "Your inbox is empty. Messages from your team will appear here."
```

**Confirmation dialogs:**

```text
Title:   "Delete project?"
Body:    "This will permanently delete 'Design System' and all its files. This action cannot be undone."
Buttons: [Cancel] [Delete project]

Button labels should be specific: [Delete project] not [OK]
```

### Localization of Error Messages

**String concatenation problems:**

```text
Bad (concatenated):
  "The file " + filename + " is too large. Maximum size is " + maxSize + "."
  Problem: Word order differs across languages.

Good (template with placeholders):
  "The file {filename} is too large. Maximum size is {maxSize}."
```

**Pluralization:**

```text
Bad: "You have " + count + " notifications."

Good (ICU format):
  "You have {count, plural, one {# notification} other {# notifications}}."
```

**Text expansion allowance:**

```text
English: "Save" (4 chars) → German: "Speichern" (9 chars) = 225% expansion
Plan for minimum 30% expansion in button labels and UI elements.
```

**What not to localize:**

```text
- Error codes: "ERR_ACCESS_DENIED"
- Technical keywords: "JSON", "API", "HTTPS"
- Brand names and product names
- File paths and command-line arguments
- Placeholder contents (user-provided data)
```

**Localization workflow:**

```text
1. Developer writes message in English with placeholders
2. String is extracted to translation files
3. Translator provides translations
4. CI/CD checks for missing translations and placeholder mismatch
5. Runtime loads the correct locale

Checklist:
- [ ] Placeholders preserved and in correct position
- [ ] Number/gender agreement is correct
- [ ] Technical terms consistent with glossary
- [ ] Tone matches the product voice in the target language
- [ ] Message fits the UI layout constraints
```

### Testing Error Messages with Users

**Comprehension testing:**

```text
Ask users:
- "What went wrong?"
- "Why did it happen?"
- "What would you do next?"

Success: User can answer all three questions correctly.
```

**A/B testing:**

```text
Version A: "Something went wrong. Please try again."
Version B: "Could not connect to the server. Check your internet connection."

Measure: Task completion rate, time to recovery, support tickets.
```

**Support ticket analysis:**

```text
Review tickets for recurring patterns:
- Users reporting an error code without understanding it
- Users asking "What do I do now?"
- Multiple users making the same incorrect assumption

Each pattern indicates an error message that needs improvement.
```

**Error state mapping:**

```text
For each critical user action, map the error states:

Action: "Upload a profile photo"
Error states:
  1. File too large → Show max size, compress option
  2. Wrong format → Show accepted formats (JPG, PNG)
  3. Upload timeout → Check network, retry option
  4. Server error → Graceful message with retry
  5. Permission denied → Check account status
```

**Review checklist:**

```text
- [ ] Does the message answer "what happened?"
- [ ] Does it answer "why did it happen?"
- [ ] Does it answer "how do I fix it?"
- [ ] Is the language appropriate for the audience?
- [ ] Is it free of jargon and internal terminology?
- [ ] Does it fit the UI surface constraints?
- [ ] Have placeholders and pluralization been handled?
- [ ] Has it been tested with users?
- [ ] Is there a corresponding log entry?
- [ ] Has it been reviewed by a technical writer?
```

### Error Message Style Guide

A project-level error message style guide ensures consistency.

```text
Error Messages: Style Guide Excerpt

1. STRUCTURE
   Every error message follows the three-part format:
     [What happened] + [Why] + [How to fix]
   Omit parts only when the omitted information is obvious or irrelevant.

2. TONE
   - Neutral and factual. Do not apologize or blame.
   - Use active voice. Prefer present tense.
   - No exclamation marks. No "please."
   - No humor, wordplay, or cultural references.

3. FORMATTING
   - Sentence case for all messages.
   - End with a period if the message is a complete sentence.
   - Bold for UI elements: "Select **Save**."
   - Code font for technical terms: "Enter `/api/v1/users`."
   - Error codes in parentheses at the end: "(ERR_ACCESS_DENIED)"

4. BUTTON LABELS
   - Verb-based: "Try again", "Save as", "Go to settings"
   - Specific, not generic: "Delete project" not "OK"
   - Maximum 3 words.

5. PLACEHOLDERS
   - Use curly brace notation: "The file {name} was not found."
   - Include type information for translators: "{count, plural, ...}"
   - Never concatenate strings.

6. LOCALIZATION
   - All user-facing strings pass through the i18n system.
   - No hard-coded user-facing text outside translation files.
   - Allow 30% expansion for all UI elements.
```

## Glossary

| Term | Definition |
|------|------------|
| Error code | A machine-readable identifier for a specific error condition |
| Inline validation | Error messages displayed next to form fields as the user types or submits |
| Toast notification | A transient message that appears temporarily and auto-dismisses |
| Empty state | The content displayed when a list or area has no data |
| Confirmation dialog | A modal that asks the user to confirm a potentially irreversible action |
| Placeholder | A token in a string template replaced with dynamic content at runtime |
| ICU message format | A standard syntax for expressing locale-aware messages with pluralization rules |
| RTL | Right-to-left writing direction used by Arabic, Hebrew, and other scripts |
| Text expansion | The increase in character count when text is translated |
| String concatenation | Building a string by joining parts together (problematic for localization) |
| User-facing text | Any text displayed to the end user in the application UI |
| Log entry | A structured record of an event intended for developers and operators |
| i18n | Internationalization — designing software for easy adaptation to different languages |
| l10n | Localization — adapting software for a specific language and region |
| Support ticket | A record of a user's request for help, often triggered by unclear error messages |
| Reference ID | A unique identifier attached to an error event for support and debugging |

## Quick References

- [Microsoft Style Guide: Error Messages](https://learn.microsoft.com/en-us/style-guide/procedures-instructions/error-messages) — Microsoft's comprehensive guidelines for error message writing
- [Google Material Design: Errors](https://material.io/design/communication/errors.html) — Google's guidance on error states and messaging
- [Apple HIG: Error Handling](https://developer.apple.com/design/human-interface-guidelines/error-handling) — Apple's Human Interface Guidelines for error states
- [ICU Message Format](https://unicode-org.github.io/icu/userguide/format_parse/messages/) — standard for internationalized messages
- [UserTesting: Error Messages Research](https://www.usertesting.com/blog/error-messages) — research on effective error messaging
- [Google Developer Docs: Error Messages](https://developers.google.com/style/error-messages) — Google's error message style guidance

## Next Steps

- [API Reference Documentation](api-docs.md) — applying error message principles to API response documentation
- [Tutorials & Getting Started Guides](tutorials.md) — handling errors that users encounter during guided walkthroughs
