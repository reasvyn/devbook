# Form Structure & Elements

## Description

Forms are the primary mechanism for user input on the web — collecting data, submitting information, and powering interactive experiences like search, checkout, and contact pages. A well-structured form is easy to use, accessible, and secure.

## Prerequisites

- [Elements, Tags & Attributes](elements-and-attributes.md) — fundamental HTML building blocks
- [Links & Navigation](links.md) — form submission destinations

## Table of Contents

- [The `<form>` Element](#the-form-element)
- [Form Controls](#form-controls)
- [Labels and Accessibility](#labels-and-accessibility)
- [Form Structure Patterns](#form-structure-patterns)
- [Fieldset and Legend](#fieldset-and-legends)
- [Form Attributes](#form-attributes)
- [Form Submission](#form-submission)
- [GET vs POST](#get-vs-post)
- [Form Security Basics](#form-security-basics)
- [Common Mistakes](#common-mistakes)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The `<form>` Element

The `<form>` element wraps all form controls and defines how data is submitted:

```html
<form action="/submit" method="POST">
    <!-- form controls go here -->
</form>
```

Key attributes:

- **`action`** — URL where the form data is sent
- **`method`** — HTTP method for submission (`GET` or `POST`)
- **`enctype`** — encoding type for the data (`application/x-www-form-urlencoded` by default, `multipart/form-data` for file uploads)
- **`novalidate`** — disables browser validation
- **`target`** — where to display the response (same as link `target`)
- **`autocomplete`** — enables or disables autocomplete for the form

A form can contain any HTML elements, not just inputs. You can use headings, paragraphs, sections, and even tables inside a form to structure the layout.

### Form Controls

HTML provides a variety of form controls for different types of input:

**Text input — `<input type="text">`:**

```html
<input type="text" name="username" id="username" maxlength="50" placeholder="Enter your username">
```

**Password — `<input type="password">`:**

```html
<input type="password" name="password" id="password" autocomplete="current-password">
```

Characters are masked. Use `autocomplete` to allow password managers.

**Email — `<input type="email">`:**

```html
<input type="email" name="email" id="email" required>
```

Browsers validate email format automatically. On mobile, the keyboard shows the @ key.

**Number — `<input type="number">`:**

```html
<input type="number" name="quantity" id="quantity" min="1" max="100" step="1" value="1">
```

Provides up/down arrows (spinner) on desktop and a numeric keyboard on mobile.

**Checkbox — `<input type="checkbox">`:**

```html
<input type="checkbox" name="subscribe" id="subscribe" checked>
<label for="subscribe">Subscribe to newsletter</label>
```

Multiple checkboxes with the same `name` form a group. Each checked checkbox sends its value.

**Radio button — `<input type="radio">`:**

```html
<input type="radio" name="plan" id="plan-basic" value="basic" checked>
<label for="plan-basic">Basic</label>

<input type="radio" name="plan" id="plan-pro" value="pro">
<label for="plan-pro">Pro</label>
```

Only one radio in the same `name` group can be selected.

**Textarea — `<textarea>`:**

```html
<textarea name="message" id="message" rows="5" cols="40" maxlength="1000" placeholder="Enter your message"></textarea>
```

Unlike `<input>`, `<textarea>` can have multiple lines and has both opening and closing tags. Text between the tags is the default value.

**Select menu — `<select>`:**

```html
<select name="country" id="country">
    <option value="us">United States</option>
    <option value="ca">Canada</option>
    <option value="mx">Mexico</option>
</select>
```

Use `<optgroup>` to group options:

```html
<select name="department" id="department">
    <optgroup label="Engineering">
        <option value="frontend">Frontend</option>
        <option value="backend">Backend</option>
    </optgroup>
    <optgroup label="Design">
        <option value="ui">UI Design</option>
        <option value="ux">UX Research</option>
    </optgroup>
</select>
```

**File upload — `<input type="file">`:**

```html
<input type="file" name="avatar" id="avatar" accept="image/png, image/jpeg" multiple>
```

- `accept` — restrict file types (MIME types)
- `multiple` — allow multiple file selection
- Form must have `enctype="multipart/form-data"`

**Hidden input — `<input type="hidden">`:**

```html
<input type="hidden" name="csrf_token" value="abc123">
```

Hidden inputs are not visible to the user but are submitted with the form. Use them for tokens, IDs, and other metadata.

**Button — `<button>`:** (See separate section)

**Range — `<input type="range">`:**

```html
<input type="range" name="volume" id="volume" min="0" max="100" value="50">
```

Displays a slider. Use with JavaScript to show the current value.

**Date and time inputs:**

```html
<input type="date" name="birthday" id="birthday">
<input type="time" name="appointment" id="appointment">
<input type="datetime-local" name="meeting" id="meeting">
```

Browsers provide native date/time pickers. Fall back to text input on unsupported browsers.

**Color picker:**

```html
<input type="color" name="theme-color" id="theme-color" value="#0056b3">
```

Displays a native color picker. The value is always a 7-character hex color.

**Telephone — `<input type="tel">`:**

```html
<input type="tel" name="phone" id="phone" pattern="[0-9]{3}-[0-9]{3}-[0-9]{4}" placeholder="555-0100-1234">
```

On mobile, shows a telephone keypad. No automatic validation — use `pattern` for format validation.

**URL — `<input type="url">`:**

```html
<input type="url" name="website" id="website" placeholder="https://example.com">
```

Browsers validate URL format. On mobile, shows the keyboard with .com key.

### Labels and Accessibility

Every form control must have an associated `<label>`:

```html
<label for="email">Email address:</label>
<input type="email" name="email" id="email" required>
```

The `for` attribute matches the control's `id`. This creates an explicit association:

- Clicking the label focuses the input
- Screen readers announce the label when the input is focused
- The label provides a larger click target for checkboxes and radio buttons

**Implicit labeling** wraps the input inside the label:

```html
<label>
    Email address:
    <input type="email" name="email" required>
</label>
```

Explicit labeling with `for`/`id` is recommended for complex forms where label placement may differ from input placement.

**Placeholder is not a label.** The `placeholder` attribute provides a hint about the expected content, not a permanent label. Placeholders disappear when the user types, making the form unusable if the user has entered text and forgotten the field purpose.

```html
<!-- Bad: placeholder serves as the only visible label -->
<input type="email" placeholder="Email address">

<!-- Good: label is always visible -->
<label for="email">Email address</label>
<input type="email" id="email" placeholder="you@example.com">
```

**Required fields** should be indicated visually and programmatically:

```html
<label for="email">Email address <span aria-hidden="true">*</span></label>
<input type="email" id="email" required aria-required="true">
```

**Error messages** should be associated with their input:

```html
<label for="email">Email address</label>
<input type="email" id="email" aria-describedby="email-error" aria-invalid="true">
<span id="email-error" role="alert">Please enter a valid email address.</span>
```

### Form Structure Patterns

**Single column** — the most readable layout:

```html
<form>
    <div>
        <label for="name">Full name</label>
        <input type="text" id="name" name="name">
    </div>
    <div>
        <label for="email">Email address</label>
        <input type="email" id="email" name="email">
    </div>
    <div>
        <button type="submit">Submit</button>
    </div>
</form>
```

Each input-label pair should be wrapped in a block-level container (a `<div>`, `<p>`, or `<section>`).

**Two-column** — for shorter fields (city, state, zip):

```html
<div class="form-row">
    <div>
        <label for="city">City</label>
        <input type="text" id="city" name="city">
    </div>
    <div>
        <label for="state">State</label>
        <select id="state" name="state">
            <option value="CA">CA</option>
            <option value="NY">NY</option>
        </select>
    </div>
</div>
```

**Inline** — for checkboxes, radio buttons, and short inputs:

```html
<fieldset>
    <legend>Preferred contact method</legend>
    <label><input type="radio" name="contact" value="email"> Email</label>
    <label><input type="radio" name="contact" value="phone"> Phone</label>
    <label><input type="radio" name="contact" value="mail"> Mail</label>
</fieldset>
```

### Fieldset and Legend

The `<fieldset>` element groups related form controls, and `<legend>` provides a caption:

```html
<fieldset>
    <legend>Shipping Address</legend>
    <div>
        <label for="street">Street address</label>
        <input type="text" id="street" name="street">
    </div>
    <div>
        <label for="city">City</label>
        <input type="text" id="city" name="city">
    </div>
    <div>
        <label for="zip">ZIP code</label>
        <input type="text" id="zip" name="zip">
    </div>
</fieldset>
```

Screen readers announce the legend when navigating between fieldsets. This provides essential context for complex forms.

Best practices:

- Use `<fieldset>` for logical groups (shipping address, payment method, preferences)
- Keep the legend concise but descriptive
- Nesting fieldsets is allowed but rare — avoid more than one level of nesting
- Style `<fieldset>` with CSS to remove default border or add custom styling

### Form Attributes

Each form control supports attributes that control behavior and validation:

**`name`** — the key sent to the server. Required for data to be submitted:

```html
<input type="text" name="username">
<!-- Submits as: username=entered_value -->
```

**`value`** — the default or pre-filled value:

```html
<input type="text" name="city" value="San Francisco">
```

**`placeholder`** — a hint displayed inside the input when empty:

```html
<input type="text" name="search" placeholder="Search products...">
```

**`required`** — prevents form submission if the field is empty:

```html
<input type="email" name="email" required>
```

**`disabled`** — field exists but cannot be edited or submitted:

```html
<input type="text" name="readonly-field" value="Locked" disabled>
```

Disabled fields are not submitted with the form and are excluded from navigation.

**`readonly`** — field can be focused and copied but not edited:

```html
<input type="text" name="generated-slug" value="my-post" readonly>
```

Readonly fields are still submitted with the form and can be navigated with Tab.

**`maxlength` / `minlength`** — character limits:

```html
<input type="text" name="username" maxlength="20" minlength="3">
```

**`pattern`** — regex validation (client-side):

```html
<input type="text" name="zip" pattern="[0-9]{5}" title="5-digit ZIP code">
```

**`autocomplete`** — hints for browser autofill:

```html
<input type="text" name="name" autocomplete="name">
<input type="email" name="email" autocomplete="email">
<input type="password" name="password" autocomplete="current-password">
```

Valid autocomplete values include `name`, `email`, `tel`, `street-address`, `country`, `postal-code`, `cc-number`, `cc-exp`, and many more.

**`autofocus`** — focus the field on page load (use sparingly):

```html
<input type="text" name="search" autofocus>
```

Only one element per page should have `autofocus`. It should not be used on forms that appear lower on the page.

### Form Submission

A form is submitted when the user clicks a submit button or presses Enter in a text field.

**Submit button:**

```html
<button type="submit">Submit</button>
<!-- or -->
<input type="submit" value="Submit">
```

`<button>` is preferred because it can contain HTML (icons, formatting) and is easier to style.

**Reset button:**

```html
<button type="reset">Reset</button>
```

Resets the form to its initial values. Use with caution — it is easy to accidentally click.

**Button without type:**

```html
<button>Click me</button>
```

A `<button>` with no `type` attribute defaults to `type="submit"` inside a form. Always specify `type` explicitly to avoid unexpected submissions.

**Preventing default submission with JavaScript:**

```javascript
document.querySelector('form').addEventListener('submit', function(event) {
    event.preventDefault();
    // Handle the form data via fetch/XHR
});
```

This is the foundation of AJAX form submission.

### GET vs POST

The `method` attribute determines how form data is sent:

| Aspect | GET | POST |
|---|---|---|
| Data location | URL query string | Request body |
| Visibility | Visible in URL | Not visible in URL |
| Bookmarkable | Yes (contains state) | No |
| History | Included in browser history | Not included |
| Data size limit | ~2000 characters (browser dependent) | Much larger (up to 2GB server-dependent) |
| Data type | ASCII only | Any (text, binary, files) |
| Idempotent | Yes (safe to repeat) | No (may create side effects) |
| Caching | Can be cached | Not cached |
| Security | Not suitable for sensitive data | More secure (but still needs HTTPS) |

**Use GET for:**

- Search forms (results can be bookmarked)
- Read-only queries
- Idempotent operations

```html
<form action="/search" method="GET">
    <input type="text" name="q" placeholder="Search...">
    <button type="submit">Search</button>
</form>
<!-- Submits to: /search?q=search+term -->
```

**Use POST for:**

- Login and registration
- Data creation, update, deletion
- File uploads
- Any operation that changes server state

```html
<form action="/register" method="POST">
    <input type="text" name="username" required>
    <input type="password" name="password" required>
    <button type="submit">Register</button>
</form>
```

### Form Security Basics

**HTTPS** is mandatory for production forms. Without HTTPS, form data is sent in plain text and can be intercepted.

**CSRF protection** — include a server-generated token in every form:

```html
<input type="hidden" name="csrf_token" value="server-generated-unique-token">
```

Without CSRF protection, an attacker can trick a logged-in user into submitting a form on another site.

**Validate on both client and server.** Client-side validation (HTML5 + JavaScript) improves user experience. Server-side validation is the security boundary — never trust client input.

**Sanitize all input.** Never render user input as HTML without escaping. Use parameterized queries for database operations to prevent SQL injection.

**Set appropriate input types and patterns.** Using `type="email"`, `type="url"`, and `pattern` helps prevent malformed data.

**Rate limit submissions.** Prevent brute force attacks by limiting the number of submissions from a single IP or user.

**Use `autocomplete="off"` sparingly.** Only disable autocomplete for highly sensitive fields (e.g., 2FA codes, security questions). Disabling autocomplete on password fields hurts usability and does not improve security.

### Common Mistakes

**Missing labels.** Every input needs a `<label>`. Placeholder is not a substitute — it disappears when the user types.

**No `name` attribute.** Without `name`, the field is not submitted. This is one of the most common form bugs.

**Wrong `enctype` for file uploads.** File uploads require `enctype="multipart/form-data"`. Without it, the file is sent as text (corrupted).

**Button without type.** A `<button>` inside a form defaults to `type="submit"`, causing unexpected form submission when used as a regular button.

**GET for destructive operations.** Using GET for delete or update operations: a crawler or browser prefetch can trigger the action unintentionally.

**No CSRF token.** Forms that change server state need CSRF protection. Without it, cross-site request forgery is possible.

**Accessible error messages.** Error messages must be associated with their input using `aria-describedby` and announced with `role="alert"`.

**Too many fields.** Long forms have lower completion rates. Minimize the number of fields and use progressive disclosure (show fields step by step).

**No confirmation for destructive actions.** Before submitting a delete or irreversible action, ask for confirmation.

## Study Cases

**Case 1: The search form that clears on submission.**

A developer creates a search form with GET method but does not include the `name` attribute on the search input. Users type a query, press Enter, and the page reloads with no results. The URL is `/search` instead of `/search?q=query`.

The fix: add `name="q"` to the input.

```html
<form action="/search" method="GET">
    <input type="text" name="q" placeholder="Search...">
    <button type="submit">Search</button>
</form>
```

**Case 2: The button that submits the wrong form.**

A developer places a "Cancel" `<button>` inside a form without specifying `type`. Clicking Cancel refreshes the page with form data in the URL.

The fix: use `type="button"` for non-submit buttons.

```html
<button type="button" onclick="location.href='/cancel-page'">Cancel</button>
```

**Case 3: The file upload that fails silently.**

A developer adds a file input but forgets to set the form's `enctype`. The file is uploaded as a text string, arriving corrupted on the server.

The fix:

```html
<form action="/upload" method="POST" enctype="multipart/form-data">
    <input type="file" name="document">
    <button type="submit">Upload</button>
</form>
```

**Case 4: The inaccessible form with placeholders only.**

A developer uses only `placeholder` attributes for all inputs:

```html
<input type="text" placeholder="Full name">
<input type="email" placeholder="Email address">
```

Screen reader users see no label. Once the user starts typing, the placeholder disappears, and users cannot remember which field they are filling.

The fix: add proper `<label>` elements:

```html
<label for="name">Full name</label>
<input type="text" id="name" name="name" placeholder="John Smith">

<label for="email">Email address</label>
<input type="email" id="email" name="email" placeholder="john@example.com">
```

## Examples

**Example 1: Complete contact form.**

```html
<form action="/contact" method="POST">
    <div>
        <label for="name">Full name <span aria-hidden="true">*</span></label>
        <input type="text" id="name" name="name" required minlength="2" maxlength="100" autocomplete="name">
    </div>

    <div>
        <label for="email">Email address <span aria-hidden="true">*</span></label>
        <input type="email" id="email" name="email" required autocomplete="email">
    </div>

    <div>
        <label for="subject">Subject</label>
        <select id="subject" name="subject">
            <option value="">Select a subject</option>
            <option value="general">General Inquiry</option>
            <option value="support">Technical Support</option>
            <option value="billing">Billing Question</option>
        </select>
    </div>

    <div>
        <label for="message">Message <span aria-hidden="true">*</span></label>
        <textarea id="message" name="message" rows="6" required minlength="10" maxlength="2000"></textarea>
    </div>

    <div>
        <label>
            <input type="checkbox" name="subscribe" checked>
            Subscribe to our newsletter
        </label>
    </div>

    <button type="submit">Send Message</button>
</form>
```

**Example 2: Registration form with fieldset.**

```html
<form action="/register" method="POST">
    <fieldset>
        <legend>Account Information</legend>

        <div>
            <label for="reg-username">Username</label>
            <input type="text" id="reg-username" name="username" required minlength="3" maxlength="20" pattern="[a-zA-Z0-9_]+" autocomplete="username">
        </div>

        <div>
            <label for="reg-email">Email</label>
            <input type="email" id="reg-email" name="email" required autocomplete="email">
        </div>

        <div>
            <label for="reg-password">Password</label>
            <input type="password" id="reg-password" name="password" required minlength="8" autocomplete="new-password">
        </div>
    </fieldset>

    <fieldset>
        <legend>Profile</legend>

        <div>
            <label for="display-name">Display name</label>
            <input type="text" id="display-name" name="display_name" maxlength="50">
        </div>

        <div>
            <label for="avatar">Profile picture</label>
            <input type="file" id="avatar" name="avatar" accept="image/png, image/jpeg, image/webp">
        </div>
    </fieldset>

    <button type="submit">Create Account</button>
</form>
```

**Example 3: Login form with autocomplete.**

```html
<form action="/login" method="POST">
    <input type="hidden" name="csrf_token" value="abc123def456">

    <div>
        <label for="login-email">Email</label>
        <input type="email" id="login-email" name="email" required autocomplete="email" autofocus>
    </div>

    <div>
        <label for="login-password">Password</label>
        <input type="password" id="login-password" name="password" required autocomplete="current-password">
    </div>

    <div>
        <label>
            <input type="checkbox" name="remember" value="1">
            Remember me
        </label>
    </div>

    <button type="submit">Sign In</button>
</form>
```

**Example 4: Survey with radio buttons and checkboxes.**

```html
<form action="/survey" method="POST">
    <fieldset>
        <legend>How did you hear about us?</legend>
        <label><input type="radio" name="source" value="search" checked> Search engine</label>
        <label><input type="radio" name="source" value="social"> Social media</label>
        <label><input type="radio" name="source" value="friend"> Friend referral</label>
        <label><input type="radio" name="source" value="other"> Other</label>
    </fieldset>

    <fieldset>
        <legend>What features interest you? (select all that apply)</legend>
        <label><input type="checkbox" name="features" value="api"> API access</label>
        <label><input type="checkbox" name="features" value="analytics"> Analytics dashboard</label>
        <label><input type="checkbox" name="features" value="mobile"> Mobile app</label>
        <label><input type="checkbox" name="features" value="integrations"> Third-party integrations</label>
    </fieldset>

    <button type="submit">Submit</button>
</form>
```

## Glossary

| Term | Definition |
|------|------------|
| `<form>` | Container element for form controls |
| `<input>` | Versatile input control with many types |
| `<textarea>` | Multi-line text input |
| `<select>` | Dropdown menu |
| `<option>` | Individual option in a select menu |
| `<optgroup>` | Group of related options |
| `<label>` | Caption for a form control |
| `<fieldset>` | Group of related form controls |
| `<legend>` | Caption for a fieldset |
| `<button>` | Clickable button element |
| `action` | URL where form data is submitted |
| `method` | HTTP method for submission (GET or POST) |
| `enctype` | Encoding type for form data |
| `name` | Identifier sent as the key in form data |
| `required` | Field must be filled before submission |
| `disabled` | Field is present but cannot be used |
| `readonly` | Field content cannot be edited |
| `placeholder` | Hint text inside the input |
| `pattern` | Regex pattern for validation |
| `autocomplete` | Browser autofill hint |
| `autofocus` | Focus the field on page load |
| `csrf_token` | Hidden token preventing cross-site request forgery |
| `aria-describedby` | Associates a description with an input |
| `aria-invalid` | Indicates the input has an error |
| `aria-required` | Indicates the field is required (screen reader) |

## Quick References

- [MDN: Form Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/form) — complete reference
- [MDN: Input Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input) — all input types
- [MDN: Button Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button) — button reference
- [MDN: Fieldset Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/fieldset) — grouping controls
- [MDN: Label Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/label) — label reference
- [MDN: Form Accessibility](https://developer.mozilla.org/en-US/docs/Learn/Accessibility/HTML) — accessible forms guide
- [HTML Spec: Forms](https://html.spec.whatwg.org/multipage/forms.html) — official specification
- [WAI: Form Instructions](https://www.w3.org/WAI/tutorials/forms/) — W3C accessibility guide

## Next Steps

- [Input Types & Attributes](input-types.md) — deep dive into input capabilities
- [Form Validation](form-validation.md) — client and server-side validation
- [Accessible Forms](form-design-patterns.md) — advanced form design patterns
