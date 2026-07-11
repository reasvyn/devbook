# Form Validation

## Description

Form validation ensures the data submitted by users meets expected formats, ranges, and requirements. HTML5 provides built-in validation attributes, while JavaScript enables custom validation logic. Both client-side and server-side validation are essential for a robust form experience.

## Prerequisites

- [Form Structure & Elements](form-basics.md) — form basics and controls
- [Input Types & Attributes](input-types.md) — input types and validation-related attributes

## Table of Contents

- [Why Validate?](#why-validate)
- [HTML5 Built-in Validation](#html5-built-in-validation)
- [The Constraint Validation API](#the-constraint-validation-api)
- [Custom Validation with JavaScript](#custom-validation-with-javascript)
- [Styling Validation States](#styling-validation-states)
- [Real-time Validation](#real-time-validation)
- [Form Submission Validation](#form-submission-validation)
- [Server-side Validation](#server-side-validation)
- [Validation Strategy](#validation-strategy)
- [Common Validation Patterns](#common-validation-patterns)
- [Accessibility of Validation Messages](#accessibility-of-validation-messages)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Why Validate?

Validation serves two different purposes:

**User experience** — catching errors early helps users correct mistakes before submission. Inline validation messages, real-time feedback, and clear error descriptions make forms less frustrating.

**Security** — client-side validation is a convenience, not a security boundary. Server-side validation is the security boundary. All form data must be validated and sanitized on the server, because client-side validation can be bypassed.

Validation prevents:

- Empty required fields
- Incorrect formats (email, phone, ZIP code)
- Values outside allowed ranges
- Malicious input (SQL injection, XSS)
- Duplicate or inconsistent data

### HTML5 Built-in Validation

HTML5 provides validation through attributes on form controls. Browsers handle the validation automatically on form submission.

**`required`** — field must have a value:

```html
<input type="text" name="username" required>
```

The browser prevents form submission and shows a validation message.

**`type` validation** — some input types have built-in format rules:

```html
<input type="email" name="email">          <!-- Must contain @ and domain -->
<input type="url" name="website">           <!-- Must have protocol -->
<input type="number" name="age">            <!-- Must be a valid number -->
```

**`min` / `max`** — value boundaries:

```html
<input type="number" name="age" min="18" max="120">
<input type="date" name="delivery" min="2026-06-28" max="2026-12-31">
```

**`minlength` / `maxlength`** — character count limits:

```html
<input type="text" name="username" minlength="3" maxlength="20">
```

**`pattern`** — regex validation:

```html
<input type="text" name="zip" pattern="[0-9]{5}(-[0-9]{4})?" title="ZIP code: 12345 or 12345-6789">
```

The `title` attribute provides a description of the expected format. This description is shown in the browser's validation tooltip.

**`step`** — increment granularity:

```html
<input type="number" name="rating" step="0.5" min="1" max="5">
```

A value of 1.3 would be invalid because it is not a multiple of 0.5.

### The Constraint Validation API

JavaScript can interact with HTML5 validation through the Constraint Validation API:

**`checkValidity()`** — returns `true` if the element passes all constraints:

```javascript
const input = document.getElementById('email');
if (input.checkValidity()) {
    console.log('Valid');
} else {
    console.log('Invalid');
}
```

**`reportValidity()`** — checks validity and shows the browser's validation UI:

```javascript
input.reportValidity();
```

**`setCustomValidity()`** — sets a custom error message or marks the field as invalid:

```javascript
input.setCustomValidity('');         // Clear custom error (mark as valid)
input.setCustomValidity('Invalid');  // Mark as invalid with message
```

**`validity`** — a `ValidityState` object with boolean properties:

| Property | Meaning |
|---|---|
| `valueMissing` | Required field is empty |
| `typeMismatch` | Value does not match the input type (e.g., email format) |
| `patternMismatch` | Value does not match the `pattern` regex |
| `tooLong` | Value exceeds `maxlength` |
| `tooShort` | Value is shorter than `minlength` |
| `rangeUnderflow` | Value is less than `min` |
| `rangeOverflow` | Value exceeds `max` |
| `stepMismatch` | Value does not match the `step` granularity |
| `badInput` | Browser cannot parse the value |
| `customError` | A custom validity message was set |
| `valid` | All constraints pass |

```javascript
const validity = input.validity;

if (validity.valueMissing) {
    // Show "This field is required"
} else if (validity.typeMismatch) {
    // Show "Please enter a valid email"
} else if (validity.tooShort) {
    // Show "Must be at least N characters"
}
```

**`validationMessage`** — the browser's localized validation message:

```javascript
console.log(input.validationMessage);
```

### Custom Validation with JavaScript

**`setCustomValidity()`** is the primary tool for custom validation:

```javascript
const password = document.getElementById('password');
const confirm = document.getElementById('confirm');

function validatePassword() {
    if (password.value !== confirm.value) {
        confirm.setCustomValidity('Passwords do not match');
    } else {
        confirm.setCustomValidity('');
    }
}

password.addEventListener('input', validatePassword);
confirm.addEventListener('input', validatePassword);
```

When `setCustomValidity` is called with a non-empty string, the element becomes invalid. Calling it with an empty string makes the element valid (assuming other constraints pass).

**Custom validation on the whole form:**

```javascript
const form = document.getElementById('registration');

form.addEventListener('submit', function(event) {
    const username = document.getElementById('username');
    const forbidden = ['admin', 'root', 'test'];

    if (forbidden.includes(username.value.toLowerCase())) {
        username.setCustomValidity('This username is reserved');
        username.reportValidity();
        event.preventDefault();
    }
});
```

**Debouncing real-time validation** — avoid validating on every keystroke for expensive checks:

```javascript
let debounceTimer;

username.addEventListener('input', function() {
    clearTimeout(debounceTimer);
    debounceTimer = setTimeout(function() {
        // Check username availability via API
        fetch('/check-username?q=' + username.value)
            .then(response => response.json())
            .then(data => {
                if (data.taken) {
                    username.setCustomValidity('Username is taken');
                    username.reportValidity();
                } else {
                    username.setCustomValidity('');
                }
            });
    }, 300); // Wait 300ms after last keystroke
});
```

### Styling Validation States

CSS pseudo-classes style elements based on their validation state:

```css
/* Valid inputs */
input:valid {
    border-color: #28a745;
}

/* Invalid inputs */
input:invalid {
    border-color: #dc3545;
}

/* Required inputs that are empty */
input:required {
    border-left: 3px solid #ffc107;
}

/* Optional inputs */
input:optional {
    border-left: 3px solid #ccc;
}

/* Placeholder shown */
input:placeholder-shown {
    border-color: #ccc;
}

/* Focus + invalid */
input:focus:invalid {
    outline-color: #dc3545;
}

/* Disabled browser tooltip */
input:invalid {
    /* The browser shows its own tooltip on submit; cannot be removed with CSS alone */
}
```

**Showing validation messages on interaction:**

```css
/* Hide validation message by default */
.validation-message {
    display: none;
    color: #dc3545;
    font-size: 0.875em;
}

/* Show when the input is invalid and not focused (after first blur) */
input:not(:focus):invalid ~ .validation-message {
    display: block;
}
```

### Real-time Validation

There are three timing strategies for showing validation feedback:

**On blur** (when the field loses focus) — the most common approach:

```javascript
input.addEventListener('blur', function() {
    if (!input.checkValidity()) {
        showError(input, input.validationMessage);
    }
});
```

**On input** (on every keystroke) — provides immediate feedback but can be distracting:

```javascript
input.addEventListener('input', function() {
    if (input.checkValidity()) {
        hideError(input);
    } else {
        showError(input, input.validationMessage);
    }
});
```

**On form submission** — the browser default:

```html
<form>
    <input type="email" required>
    <button type="submit">Submit</button>
</form>
```

The browser shows validation messages for all invalid fields on the first submission attempt.

**Combined approach — validate on blur initially, then on input after first interaction:**

```javascript
let wasValidated = false;

input.addEventListener('blur', function() {
    if (!wasValidated) {
        wasValidated = true;
        validateField(input);
    }
});

input.addEventListener('input', function() {
    if (wasValidated) {
        validateField(input);
    }
});

function validateField(input) {
    if (input.checkValidity()) {
        hideError(input);
    } else {
        showError(input, input.validationMessage);
    }
}
```

### Form Submission Validation

**Preventing default submission with full validation check:**

```javascript
form.addEventListener('submit', function(event) {
    if (!form.checkValidity()) {
        event.preventDefault();
        // Focus the first invalid field
        const firstInvalid = form.querySelector(':invalid');
        if (firstInvalid) {
            firstInvalid.focus();
        }
    }
});
```

**HTML5 `novalidate` attribute** — disable browser validation for custom JavaScript handling:

```html
<form action="/submit" method="POST" novalidate>
    <!-- All validation is handled by JavaScript -->
</form>
```

```javascript
form.addEventListener('submit', function(event) {
    event.preventDefault();
    // Custom validation logic
    if (validateForm()) {
        form.submit(); // Or use fetch
    }
});
```

**Combined approach — use HTML5 validation attributes and enhance with JavaScript:**

```html
<form action="/submit" method="POST" novalidate>
    <!-- HTML5 attributes define constraints -->
    <input type="email" name="email" required>
    <input type="password" name="password" required minlength="8">

    <!-- JavaScript handles custom validation and presentation -->
</form>
```

Using `novalidate` with HTML5 attributes gives you the best of both worlds: declarative constraints in markup and full control over the validation UI.

### Server-side Validation

Client-side validation is a convenience. Server-side validation is mandatory.

**Never trust client input.** An attacker can:

- Submit the form with `curl` or Postman, bypassing the browser entirely
- Disable JavaScript
- Modify HTML attributes using browser devtools
- Intercept and modify network requests

**Server-side validation must check:**

- All `required` fields are present
- Data types and formats are correct (email format, number range)
- String lengths are within limits
- Values are in allowed sets (e.g., country codes)
- File types and sizes are allowed
- No SQL injection, XSS, or other malicious content

**Always sanitize output.** Even valid data must be escaped when rendered in HTML to prevent XSS.

### Validation Strategy

A complete validation strategy has three layers:

| Layer | Responsibility | Location |
|---|---|---|
| HTML5 attributes | Basic constraints (required, type, min/max, pattern) | Markup |
| JavaScript | Custom logic, async checks, enhanced UX | Client |
| Server | All validation, security checks, sanitization | Server |

**Recommended workflow:**

1. Define constraints with HTML5 attributes in the markup
2. Enhance with JavaScript for custom validation and better error display
3. Set `novalidate` on the form for full control over the UI
4. Validate on blur initially, then on input after first interaction
5. Focus the first invalid field on submission
6. Validate everything again on the server

### Common Validation Patterns

**Password strength indicator:**

```html
<div>
    <label for="pw">Password</label>
    <input type="password" id="pw" name="password" minlength="8" required
           pattern="(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,}"
           title="8+ chars, upper, lower, and number">
    <div id="pw-strength" class="password-strength"></div>
</div>
```

```javascript
pw.addEventListener('input', function() {
    const val = pw.value;
    const strength = document.getElementById('pw-strength');

    let score = 0;
    if (val.length >= 8) score++;
    if (val.match(/[a-z]/)) score++;
    if (val.match(/[A-Z]/)) score++;
    if (val.match(/\d/)) score++;
    if (val.match(/[^a-zA-Z0-9]/)) score++;

    const levels = ['Weak', 'Fair', 'Good', 'Strong', 'Very Strong'];
    strength.textContent = levels[score] || '';
    strength.className = 'password-strength level-' + score;
});
```

**Confirm email / confirm password:**

```javascript
const email = document.getElementById('email');
const confirmEmail = document.getElementById('confirm-email');

function matchEmails() {
    if (email.value !== confirmEmail.value) {
        confirmEmail.setCustomValidity('Emails do not match');
    } else {
        confirmEmail.setCustomValidity('');
    }
}

email.addEventListener('input', matchEmails);
confirmEmail.addEventListener('input', matchEmails);
```

**Conditional validation (show/hide fields):**

```javascript
const contactMethod = document.querySelectorAll('input[name="contact"]');
const phoneGroup = document.getElementById('phone-group');

contactMethod.forEach(radio => {
    radio.addEventListener('change', function() {
        if (this.value === 'phone') {
            phoneGroup.style.display = 'block';
            phoneGroup.querySelector('input').required = true;
        } else {
            phoneGroup.style.display = 'none';
            phoneGroup.querySelector('input').required = false;
        }
    });
});
```

**Async username availability:**

```javascript
let debounceTimer;
const username = document.getElementById('username');
const usernameStatus = document.getElementById('username-status');

username.addEventListener('input', function() {
    clearTimeout(debounceTimer);

    if (username.value.length < 3) {
        usernameStatus.textContent = '';
        return;
    }

    debounceTimer = setTimeout(function() {
        usernameStatus.textContent = 'Checking...';
        fetch('/api/check-username?username=' + encodeURIComponent(username.value))
            .then(r => r.json())
            .then(data => {
                if (data.available) {
                    usernameStatus.textContent = '✓ Available';
                    usernameStatus.className = 'available';
                    username.setCustomValidity('');
                } else {
                    usernameStatus.textContent = '✗ Taken';
                    usernameStatus.className = 'taken';
                    username.setCustomValidity('Username is taken');
                }
            });
    }, 300);
});
```

### Accessibility of Validation Messages

Validation messages must be announced by screen readers. Use `aria-describedby` and `role="alert"`:

```html
<div>
    <label for="email">Email address</label>
    <input type="email" id="email" name="email" required
           aria-describedby="email-error" aria-invalid="false">
    <span id="email-error" role="alert" class="error-message"></span>
</div>
```

```javascript
function showError(input, message) {
    const errorElement = document.getElementById(input.id + '-error');
    if (errorElement) {
        errorElement.textContent = message;
        input.setAttribute('aria-invalid', 'true');
    }
}

function hideError(input) {
    const errorElement = document.getElementById(input.id + '-error');
    if (errorElement) {
        errorElement.textContent = '';
        input.setAttribute('aria-invalid', 'false');
    }
}
```

Key accessibility requirements:

- Error messages must be associated with the input via `aria-describedby`
- `role="alert"` causes screen readers to announce the message immediately
- `aria-invalid` indicates the field has an error
- Do not hide error messages with `display: none` — screen readers will not announce them. Use a visually-hidden class if needed
- Color alone should not indicate error — include an icon or text label
- The error message should tell the user what is wrong and how to fix it

## Glossary

| Term | Definition |
|------|------------|
| `checkValidity()` | Returns true if the element passes all constraints |
| `reportValidity()` | Checks validity and shows the browser's validation UI |
| `setCustomValidity()` | Sets a custom error message or clears the custom error |
| `validity` | ValidityState object with properties for each constraint type |
| `validationMessage` | The browser's localized validation message |
| `novalidate` | Form attribute that disables browser validation |
| `ValidityState` | Object containing boolean properties for each validation constraint |
| `valueMissing` | Required field is empty |
| `typeMismatch` | Value does not match the input type |
| `patternMismatch` | Value does not match the pattern regex |
| `tooLong` / `tooShort` | Value is too long or too short |
| `rangeUnderflow` / `rangeOverflow` | Value is outside the min/max range |
| `stepMismatch` | Value does not match step granularity |
| `customError` | Custom validity message was set |
| `aria-describedby` | Associates an error message with an input for screen readers |
| `aria-invalid` | Indicates the input has an error |
| `role="alert"` | Causes screen readers to announce content immediately |
| Server-side validation | Validation performed on the server (security boundary) |
| Client-side validation | Validation performed in the browser (user experience) |
| Debounce | Technique to delay execution until after a pause in events |

## Quick References

- [MDN: Constraint Validation API](https://developer.mozilla.org/en-US/docs/Web/API/Constraint_validation) — complete API reference
- [MDN: Form Validation Guide](https://developer.mozilla.org/en-US/docs/Learn/Forms/Form_validation) — comprehensive guide
- [MDN: ValidityState](https://developer.mozilla.org/en-US/docs/Web/API/ValidityState) — validity state properties
- [MDN: Using `aria-describedby`](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-describedby) — accessible error messages
- [HTML Spec: Forms — Validation](https://html.spec.whatwg.org/multipage/form-control-infrastructure.html#the-constraint-validation-api) — official specification
- [WAI: Validating Input](https://www.w3.org/WAI/tutorials/forms/validation/) — accessibility guide

## Next Steps

- [Accessible Forms](form-design-patterns.md) — advanced form design patterns
- [Semantic HTML & Document Outline](semantic-html.md) — structural meaning in markup
- [HTML & ARIA](aria-basics.md) — accessible rich internet applications
