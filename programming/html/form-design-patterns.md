# Accessible Forms

## Description

An accessible form works for everyone — keyboard-only users, screen reader users, users with motor disabilities, and users with cognitive impairments. Accessible forms are not a separate concern: they are simply well-designed forms that follow semantic HTML, associate labels correctly, and provide clear feedback.

## Prerequisites

- [Form Structure & Elements](form-basics.md) — form basics and controls
- [Input Types & Attributes](input-types.md) — input types and attributes
- [Form Validation](form-validation.md) — validation patterns

## Table of Contents

- [The Foundation: Semantic HTML](#the-foundation-semantic-html)
- [Label Every Control](#label-every-control)
- [Grouping Related Controls](#grouping-related-controls)
- [Error Messages](#error-messages)
- [Focus Management](#focus-management)
- [Keyboard Navigation](#keyboard-navigation)
- [Color and Contrast](#color-and-contrast)
- [Form Instructions](#form-instructions)
- [Autocomplete for Faster Input](#autocomplete-for-faster-input)
- [Multi-page Forms and Wizards](#multi-page-forms-and-wizards)
- [CAPTCHA Accessibility](#captcha-accessibility)
- [Testing Forms for Accessibility](#testing-forms-for-accessibility)
- [Common Mistakes](#common-mistakes)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Foundation: Semantic HTML

Accessible forms start with correct HTML. Every form control must:

1. Have a programmatically associated `<label>`
2. Use the correct `type` for the expected data
3. Include appropriate ARIA attributes when needed
4. Be navigable by keyboard

```html
<form action="/register" method="POST">
    <div>
        <label for="name">Full name</label>
        <input type="text" id="name" name="name" autocomplete="name">
    </div>

    <div>
        <label for="email">Email</label>
        <input type="email" id="email" name="email" autocomplete="email">
    </div>

    <button type="submit">Submit</button>
</form>
```

Semantic HTML is the foundation. ARIA fills in gaps — it does not replace proper HTML.

### Label Every Control

Every form control must have a label. The `<label>` element is the primary mechanism.

**Explicit labels** use `for` and `id`:

```html
<label for="search">Search</label>
<input type="search" id="search" name="q">
```

**Implicit labels** wrap the input:

```html
<label>
    I agree to the terms
    <input type="checkbox" name="agree" required>
</label>
```

**When a visible label is not possible** (search icons, symbol-only inputs), use `aria-label`:

```html
<button aria-label="Search the site">
    <svg><!-- search icon --></svg>
</button>
<input type="search" aria-label="Search" name="q">
```

**When additional context is needed**, use `aria-labelledby`:

```html
<h2 id="shipping-heading">Shipping Address</h2>

<div role="group" aria-labelledby="shipping-heading">
    <label for="street">Street</label>
    <input type="text" id="street" name="street">

    <label for="city">City</label>
    <input type="text" id="city" name="city">
</div>
```

**Do not rely on `placeholder` as a label.** Placeholder text disappears when the user types. Screen readers do not consistently announce placeholders.

### Grouping Related Controls

Use `<fieldset>` and `<legend>` to group related controls:

```html
<fieldset>
    <legend>Contact Method</legend>

    <input type="radio" name="contact" id="contact-email" value="email" checked>
    <label for="contact-email">Email</label>

    <input type="radio" name="contact" id="contact-phone" value="phone">
    <label for="contact-phone">Phone</label>

    <input type="radio" name="contact" id="contact-mail" value="mail">
    <label for="contact-mail">Mail</label>
</fieldset>
```

Screen readers announce the legend when entering the fieldset. This provides essential context for radio buttons and checkboxes.

Best practices:

- Use `<fieldset>` for every group of radio buttons and related checkboxes
- Keep legends concise but descriptive
- Do not nest fieldsets more than one level deep
- Style the `<fieldset>` border with CSS — the default browser rendering is often unattractive

### Error Messages

Error messages must be perceivable and understandable by all users.

**Associate errors with inputs using `aria-describedby`:**

```html
<div>
    <label for="email">Email address</label>
    <input type="email" id="email" name="email" required
           aria-describedby="email-error" aria-invalid="false">
    <span id="email-error" role="alert"></span>
</div>
```

**Use `role="alert"`** on the error container to trigger immediate announcement by screen readers.

**Update `aria-invalid`** dynamically:

```javascript
function showError(input, message) {
    const error = document.getElementById(input.id + '-error');
    error.textContent = message;
    input.setAttribute('aria-invalid', 'true');
}

function clearError(input) {
    const error = document.getElementById(input.id + '-error');
    error.textContent = '';
    input.setAttribute('aria-invalid', 'false');
}
```

**Inline errors are better than a summary list** — place the error next to the relevant field, not just at the top of the form. A summary at the top can supplement inline errors but should not replace them.

**Error messages must be specific and actionable:**

| Bad | Good |
|---|---|
| "Invalid input" | "Please enter a valid email address (e.g., name@example.com)" |
| "Error" | "Password must be at least 8 characters with one uppercase letter" |
| "Field required" | "Full name is required" |

**Use multiple methods to indicate errors:** color, icon, and text. Do not rely on color alone.

### Focus Management

**On submission, focus the first invalid field:**

```javascript
form.addEventListener('submit', function(event) {
    const firstInvalid = form.querySelector(':invalid');
    if (firstInvalid) {
        event.preventDefault();
        firstInvalid.focus();
    }
});
```

**Do not steal focus from the user** without a clear reason. Auto-focusing an error message can disorient screen reader users.

**When dynamically showing/hiding form sections, move focus to the new content:**

```javascript
function showSection(sectionId) {
    const section = document.getElementById(sectionId);
    section.style.display = 'block';
    section.querySelector('input, select, textarea, button').focus();
}
```

### Keyboard Navigation

**Ensure a logical tab order.** The default tab order (DOM order) should match the visual order. Do not use positive `tabindex` values:

```html
<!-- Do not do this -->
<input type="text" tabindex="3">
<input type="text" tabindex="1">
<input type="text" tabindex="2">
```

Use `tabindex="0"` to make an element focusable (e.g., a `<div>` role button) and `tabindex="-1"` to make it programmatically focusable but not in the tab order.

**Custom controls need keyboard handling:**

| Key | Action |
|---|---|
| Tab | Move to next focusable element |
| Shift+Tab | Move to previous focusable element |
| Enter / Space | Activate a button or toggle a checkbox |
| Arrow keys | Navigate within radio groups, select menus |
| Escape | Close a popup or dismiss an error |

**Provide visible focus indicators:**

```css
:focus-visible {
    outline: 2px solid #0056b3;
    outline-offset: 2px;
}
```

Never remove the focus outline without providing an alternative focus indicator.

### Color and Contrast

**Errors must not rely on color alone.** Use icons, text, or patterns in addition to color:

```css
.error-message {
    color: #d32f2f;
    background: url('error-icon.svg') no-repeat left center;
    padding-left: 24px;
}
```

**Ensure sufficient color contrast:**
- Text on background: at least 4.5:1 ratio (normal text) or 3:1 (large text)
- Error indicators: at least 3:1 against adjacent colors
- Focus indicators: at least 3:1 against the background

**Use a color palette that works for color-blind users:**
- Do not use red/green as the only differentiator for success/error
- Add icons, text labels, or patterns alongside color

### Form Instructions

**Provide clear instructions at the top of the form:**

```html
<form aria-labelledby="form-instructions">
    <h2 id="form-instructions">Contact Us</h2>
    <p>Fields marked with <span aria-hidden="true">*</span> are required.</p>
    <!-- form fields -->
</form>
```

**Use `aria-describedby` for field-level instructions:**

```html
<label for="password">Password</label>
<input type="password" id="password" name="password"
       aria-describedby="password-hint">
<span id="password-hint">Must be at least 8 characters with one number and one letter.</span>
```

**Indicate required fields clearly.** Use an asterisk `*` with `aria-hidden="true"` so screen readers do not announce it redundantly (the `required` attribute already conveys this):

```html
<label for="name">Full name <span aria-hidden="true">*</span></label>
<input type="text" id="name" name="name" required aria-required="true">
```

### Autocomplete for Faster Input

The `autocomplete` attribute lets browsers autofill form fields with saved user data. This is especially helpful for users with motor or cognitive disabilities.

```html
<input type="text" name="name" autocomplete="name">
<input type="email" name="email" autocomplete="email">
<input type="tel" name="phone" autocomplete="tel">
<input type="text" name="street" autocomplete="street-address">
<input type="text" name="city" autocomplete="address-level2">
<input type="text" name="state" autocomplete="address-level1">
<input type="text" name="zip" autocomplete="postal-code">
<input type="text" name="country" autocomplete="country-name">
<input type="password" name="password" autocomplete="current-password">
<input type="password" name="new-password" autocomplete="new-password">
```

Common `autocomplete` values:

| Field | `autocomplete` value |
|---|---|
| Full name | `name` |
| First name | `given-name` |
| Last name | `family-name` |
| Email | `email` |
| Phone | `tel` |
| Street address | `street-address` |
| City | `address-level2` |
| State / Province | `address-level1` |
| ZIP / Postal code | `postal-code` |
| Country | `country-name` |
| Credit card number | `cc-number` |
| Credit card expiry | `cc-exp` |
| Credit card CVC | `cc-csc` |
| Username | `username` |
| Current password | `current-password` |
| New password | `new-password` |

Do not disable autocomplete unless there is a strong security reason (e.g., one-time codes, security questions).

### Multi-page Forms and Wizards

Long forms should be broken into logical steps. For accessibility:

**Progress indicator:**

```html
<nav aria-label="Form progress">
    <ol>
        <li aria-current="step">Account Details</li>
        <li>Shipping Address</li>
        <li>Payment</li>
        <li>Review & Confirm</li>
    </ol>
</nav>
```

**Announce step changes to screen readers:**

```html
<h2 id="step-heading" tabindex="-1">Step 2: Shipping Address</h2>
```

After loading a new step, focus the heading:

```javascript
function showStep(stepNumber) {
    const heading = document.getElementById('step-heading');
    heading.focus();
}
```

**Preserve form data across steps.** Do not clear completed steps when the user navigates back.

**Allow users to review all data before final submission.** Include an edit link next to each section.

### CAPTCHA Accessibility

CAPTCHAs are inherently inaccessible. If you must use one:

- **Provide an audio alternative** alongside the visual challenge
- **Use hCaptcha or reCAPTCHA v3** (invisible) instead of visual puzzles
- **Always provide a fallback method** (e.g., email verification, SMS code)
- **Explain what the user needs to do** clearly

```html
<div class="captcha" aria-describedby="captcha-desc">
    <span id="captcha-desc">Complete the security check to verify you are human.</span>
    <!-- captcha widget -->
    <a href="/alternative-verify">Can't see the challenge? Verify via email instead.</a>
</div>
```

### Testing Forms for Accessibility

**Keyboard testing:**
1. Tab through the entire form — can you reach every control?
2. Is the focus order logical?
3. Can you operate all controls (buttons, selects, checkboxes, radios)?
4. Is the focus indicator clearly visible?

**Screen reader testing:**

- [VoiceOver](https://www.apple.com/accessibility/vision/) (macOS/iOS) — built into Apple devices
- [NVDA](https://www.nvaccess.org/) (Windows) — free open-source screen reader
- [JAWS](https://www.freedomscientific.com/products/software/jaws/) (Windows) — most common commercial screen reader

Check:
1. Are all labels announced?
2. Are legends announced for fieldset groups?
3. Are error messages announced when they appear?
4. Is the required state conveyed?
5. Are instructions available before the form?

**Automated testing tools:**
- [axe DevTools](https://www.deque.com/axe/) — browser extension
- [Lighthouse](https://developer.chrome.com/docs/lighthouse/) — built into Chrome DevTools
- [WAVE](https://wave.webaim.org/) — browser extension

### Common Mistakes

**Placeholder as label.** The most common form accessibility error. Screen readers do not consistently announce placeholders, and they disappear when the user types.

**No fieldset for radio buttons.** Screen readers cannot tell users how many radio options exist or when they move between them without fieldset.

**Relying on color alone for errors.** Color-blind users cannot distinguish red text from normal text.

**Removing focus outlines.** The single worst accessibility mistake for keyboard users.

**No `aria-describedby` for errors.** Error messages are visually associated but not programmatically associated.

**Positive `tabindex` values.** This overrides the natural DOM order and creates unpredictable tab flows.

**Not preserving data when navigating multi-step forms.** Users who need to go back and correct a field lose all progress.

**Disabling autocomplete.** Users with motor or cognitive disabilities rely on autofill for online forms.

## Glossary

| Term | Definition |
|------|------------|
| `aria-label` | Provides an accessible name when no visible label exists |
| `aria-labelledby` | References another element as the accessible name |
| `aria-describedby` | Associates a description (e.g., error message) with an input |
| `aria-required` | Indicates a field is required (screen reader) |
| `aria-invalid` | Indicates a field has a validation error |
| `role="alert"` | Causes the element to be announced immediately by screen readers |
| `aria-current` | Indicates the current step in a multi-step form |
| Focus indicator | Visual cue (outline) showing which element is focused |
| WCAG | Web Content Accessibility Guidelines |
| Fieldset | HTML element grouping related form controls |
| Legend | Caption for a fieldset |
| Keyboard navigation | Using Tab, Enter, Arrow keys to operate a form |
| Autocomplete | Browser feature to pre-fill form fields with saved data |
| Screen reader | Software that reads digital content aloud (NVDA, VoiceOver, JAWS) |
| Color contrast | The difference in luminance between text and background |
| CAPTCHA | Automated test to distinguish humans from bots |
| `tabindex` | Attribute controlling keyboard focus order |

## Quick References

- [MDN: Form Accessibility](https://developer.mozilla.org/en-US/docs/Learn/Accessibility/HTML) — accessible forms guide
- [MDN: ARIA Forms](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles/form_role) — ARIA form role
- [WAI: Form Instructions](https://www.w3.org/WAI/tutorials/forms/) — W3C accessibility guide
- [WAI: Multi-page Forms](https://www.w3.org/WAI/tutorials/forms/multi-page/) — wizard patterns
- [WebAIM: Creating Accessible Forms](https://webaim.org/techniques/forms/) — comprehensive guide
- [Deque: Accessible Form Design](https://www.deque.com/blog/accessible-form-design/) — best practices
- [WCAG 2.1: Forms](https://www.w3.org/TR/WCAG21/#input-assistance) — WCAG input assistance requirements
- [HTML Spec: Autocomplete](https://html.spec.whatwg.org/multipage/form-control-infrastructure.html#autofill) — autocomplete values
- [A11y Project: Forms Pattern](https://www.a11yproject.com/patterns/) — accessibility patterns

## Next Steps

- [Semantic HTML & Document Outline](semantic-html.md) — structural meaning in markup
- [HTML & ARIA](aria-basics.md) — accessible rich internet applications
- [Focus Management](focus-management.md) — controlling keyboard focus
