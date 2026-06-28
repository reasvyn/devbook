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

## Study Cases

**Case 1: The form with placeholders as labels.**

A developer creates a login form using only placeholder attributes:

```html
<input type="email" placeholder="Email">
<input type="password" placeholder="Password">
<button>Sign In</button>
```

Screen reader users hear "Blank" for each input (no label). Users who start typing cannot see the field purpose.

The fix: add proper labels:

```html
<label for="login-email">Email</label>
<input type="email" id="login-email" placeholder="Email" autocomplete="email">

<label for="login-password">Password</label>
<input type="password" id="login-password" placeholder="Password" autocomplete="current-password">

<button type="submit">Sign In</button>
```

**Case 2: The missing fieldset for radio buttons.**

A developer places radio buttons in a form without grouping them:

```html
<label><input type="radio" name="plan" value="basic"> Basic</label>
<label><input type="radio" name="plan" value="pro"> Pro</label>
<label><input type="radio" name="plan" value="enterprise"> Enterprise</label>
```

Screen reader users hear "Basic radio button, Pro radio button, Enterprise radio button" without understanding the context of the group.

The fix: wrap in a fieldset with a legend:

```html
<fieldset>
    <legend>Choose your plan</legend>
    <label><input type="radio" name="plan" value="basic"> Basic</label>
    <label><input type="radio" name="plan" value="pro"> Pro</label>
    <label><input type="radio" name="plan" value="enterprise"> Enterprise</label>
</fieldset>
```

**Case 3: The error message that disappears too fast.**

A developer shows an error message that auto-hides after 2 seconds. A screen reader user submits the form, hears "alert — Invalid email", but by the time they navigate to the field the message has disappeared. They cannot recall the exact error.

The fix: error messages must remain visible until the field is corrected. Do not auto-hide error messages.

**Case 4: The password field with disabled autocomplete.**

A developer disables autocomplete on the password field for "security reasons":

```html
<input type="password" name="password" autocomplete="off">
```

Users who rely on password managers cannot log in. The security benefit is minimal (malware can still capture the password).

The fix: use the correct autocomplete value:

```html
<input type="password" name="password" autocomplete="current-password">
```

## Examples

**Example 1: Accessible contact form.**

```html
<form action="/contact" method="POST" aria-labelledby="contact-heading">
    <h2 id="contact-heading">Contact Us</h2>
    <p>Fields marked with <span aria-hidden="true">*</span> are required.</p>

    <div>
        <label for="contact-name">Full name <span aria-hidden="true">*</span></label>
        <input type="text" id="contact-name" name="name" required
               autocomplete="name" aria-required="true"
               aria-describedby="contact-name-error">
        <span id="contact-name-error" role="alert" class="error"></span>
    </div>

    <div>
        <label for="contact-email">Email <span aria-hidden="true">*</span></label>
        <input type="email" id="contact-email" name="email" required
               autocomplete="email" aria-required="true"
               aria-describedby="contact-email-error">
        <span id="contact-email-error" role="alert" class="error"></span>
    </div>

    <div>
        <label for="contact-subject">Subject</label>
        <select id="contact-subject" name="subject">
            <option value="">Select a subject...</option>
            <option value="general">General inquiry</option>
            <option value="support">Technical support</option>
            <option value="billing">Billing</option>
        </select>
    </div>

    <div>
        <label for="contact-message">Message <span aria-hidden="true">*</span></label>
        <textarea id="contact-message" name="message" rows="5" required
                  aria-describedby="contact-message-hint contact-message-error"
                  aria-required="true"></textarea>
        <span id="contact-message-hint">Max 2000 characters.</span>
        <span id="contact-message-error" role="alert" class="error"></span>
    </div>

    <button type="submit">Send Message</button>
</form>
```

**Example 2: Accessible registration form.**

```html
<form action="/register" method="POST" novalidate>
    <fieldset>
        <legend>Account Details</legend>

        <div>
            <label for="register-username">Username <span aria-hidden="true">*</span></label>
            <input type="text" id="register-username" name="username" required
                   minlength="3" maxlength="20" autocomplete="username"
                   aria-describedby="username-hint username-error"
                   aria-required="true">
            <span id="username-hint">3-20 characters, letters and numbers only.</span>
            <span id="username-error" role="alert" class="error"></span>
        </div>

        <div>
            <label for="register-email">Email <span aria-hidden="true">*</span></label>
            <input type="email" id="register-email" name="email" required
                   autocomplete="email" aria-required="true"
                   aria-describedby="email-error">
            <span id="email-error" role="alert" class="error"></span>
        </div>

        <div>
            <label for="register-password">Password <span aria-hidden="true">*</span></label>
            <input type="password" id="register-password" name="password" required
                   minlength="8" autocomplete="new-password" aria-required="true"
                   aria-describedby="password-hint password-error">
            <span id="password-hint">At least 8 characters with one uppercase letter and one number.</span>
            <span id="password-error" role="alert" class="error"></span>
        </div>
    </fieldset>

    <fieldset>
        <legend>Shipping Address</legend>

        <div>
            <label for="register-street">Street address</label>
            <input type="text" id="register-street" name="street" autocomplete="street-address">
        </div>

        <div>
            <label for="register-city">City</label>
            <input type="text" id="register-city" name="city" autocomplete="address-level2">
        </div>

        <div>
            <label for="register-zip">ZIP code</label>
            <input type="text" id="register-zip" name="zip" autocomplete="postal-code"
                   pattern="[0-9]{5}" inputmode="numeric"
                   aria-describedby="zip-hint">
            <span id="zip-hint">5-digit ZIP code (e.g., 94102).</span>
        </div>
    </fieldset>

    <div>
        <label>
            <input type="checkbox" name="newsletter" checked>
            Subscribe to our newsletter
        </label>
    </div>

    <button type="submit">Create Account</button>
</form>
```

**Example 3: Accessible payment form.**

```html
<form action="/pay" method="POST" aria-labelledby="payment-heading">
    <h2 id="payment-heading">Payment Information</h2>

    <fieldset>
        <legend>Card details</legend>

        <div>
            <label for="card-number">Card number <span aria-hidden="true">*</span></label>
            <input type="text" id="card-number" name="card-number" required
                   inputmode="numeric" autocomplete="cc-number"
                   pattern="[0-9\s]{13,19}" maxlength="19"
                   aria-describedby="card-error">
            <span id="card-error" role="alert" class="error"></span>
        </div>

        <div>
            <label for="card-expiry">Expiry date <span aria-hidden="true">*</span></label>
            <input type="text" id="card-expiry" name="card-expiry" required
                   placeholder="MM/YY" pattern="(0[1-9]|1[0-2])\/\d{2}"
                   autocomplete="cc-exp"
                   aria-describedby="expiry-error">
            <span id="expiry-error" role="alert" class="error"></span>
        </div>

        <div>
            <label for="card-cvc">CVC <span aria-hidden="true">*</span></label>
            <input type="text" id="card-cvc" name="card-cvc" required
                   inputmode="numeric" pattern="[0-9]{3,4}"
                   autocomplete="cc-csc"
                   aria-describedby="cvc-hint cvc-error">
            <span id="cvc-hint">3-digit code on the back of your card.</span>
            <span id="cvc-error" role="alert" class="error"></span>
        </div>
    </fieldset>

    <button type="submit">Pay Now</button>
</form>
```

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
