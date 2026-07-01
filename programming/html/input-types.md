# Input Types & Attributes

## Description

HTML5 introduced a rich set of input types and attributes that enable native form validation, specialized keyboards on mobile, and better user experiences without JavaScript. Understanding the full range of input capabilities helps developers choose the right control for every data collection scenario.

## Prerequisites

- [Form Structure & Elements](form-basics.md) — form basics and controls

## Table of Contents

- [Overview of Input Types](#overview-of-input-types)
- [Text-Based Types](#text-based-types)
- [Numeric Types](#numeric-types)
- [Date and Time Types](#date-and-time-types)
- [Choice Types](#choice-types)
- [Specialized Types](#specialized-types)
- [Global Attributes](#global-attributes)
- [Input-Specific Attributes](#input-specific-attributes)
- [Datalist](#datalist)
- [Output Element](#output-element)
- [Progress and Meter](#progress-and-meter)
- [Input Mode](#input-mode)
- [Common Patterns](#common-patterns)
- [Browser Support Considerations](#browser-support-considerations)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Overview of Input Types

HTML defines over 20 input types via the `type` attribute on `<input>`. Each type controls the browser's rendering, validation behavior, and on-screen keyboard on mobile devices.

| Type | Purpose | Mobile keyboard |
|---|---|---|
| `text` | General text | Standard |
| `password` | Masked text | Standard |
| `email` | Email address | @ and .com keys |
| `tel` | Telephone number | Numeric keypad |
| `url` | Website URL | .com and / keys |
| `number` | Numeric values | Numeric keypad |
| `search` | Search queries | Standard with search action |
| `range` | Slider | No keyboard (touch/drag) |
| `color` | Color picker | Native color picker |
| `date` | Date (yyyy-mm-dd) | Date picker |
| `time` | Time (hh:mm) | Time picker |
| `datetime-local` | Date + time | Combined picker |
| `month` | Month (yyyy-mm) | Month picker |
| `week` | Week (yyyy-Www) | Week picker |
| `checkbox` | Binary choice | Tappable checkbox |
| `radio` | Single selection from group | Tappable radio |
| `file` | File upload | File picker |
| `hidden` | Non-visible data | None |
| `image` | Image as submit button | None |
| `submit` | Form submission | None |
| `reset` | Form reset | None |
| `button` | Generic button | None |

### Text-Based Types

**`text`** — the default type for any `<input>` without an explicit type:

```html
<input type="text" name="username" maxlength="50" placeholder="Enter your username">
```

Browsers render all text-based types similarly, but the differences in behavior are important.

**`search`** — semantically indicates a search field:

```html
<input type="search" name="q" placeholder="Search products...">
```

In some browsers, `type="search"` adds:
- A clear (×) button when the field has content
- Different default styling (rounded corners on Safari)
- Search-specific autocomplete behavior

**`email`** — accepts one or more email addresses:

```html
<!-- Single email -->
<input type="email" name="email" required>

<!-- Multiple emails (comma-separated) -->
<input type="email" name="recipients" multiple>
```

Built-in validation requires at least the `@` character and a domain. The `multiple` attribute allows comma-separated addresses.

**`tel`** — accepts a telephone number:

```html
<input type="tel" name="phone" pattern="[0-9]{3}-[0-9]{3}-[0-9]{4}" placeholder="555-010-1234">
```

Browsers do not validate telephone numbers (formats vary globally). Use `pattern` for format validation. On mobile, this type triggers a numeric keypad.

**`url`** — accepts a URL:

```html
<input type="url" name="website" placeholder="https://example.com">
```

Built-in validation requires a protocol (http:// or https://). On mobile, the keyboard shows .com, /, and other URL-specific keys.

**`password`** — masks the input characters:

```html
<input type="password" name="password" autocomplete="current-password" minlength="8">
```

Characters are replaced with dots or asterisks. Always include `autocomplete` for password manager compatibility. Never limit password length on the client side.

### Numeric Types

**`number`** — accepts numeric values with optional constraints:

```html
<input type="number" name="quantity" min="1" max="100" step="1" value="1">
```

| Attribute | Effect |
|---|---|
| `min` | Minimum allowed value |
| `max` | Maximum allowed value |
| `step` | Increment/decrement step (default: 1) |
| `value` | Initial value |

The browser provides up/down arrows (spinner) on desktop. Invalid text entries cause the input to be empty on form submission.

**`range`** — a slider control:

```html
<input type="range" name="volume" min="0" max="100" step="5" value="50">
```

- The visual appearance varies by browser and OS
- The current value is not shown by default — use JavaScript to display it
- `min`, `max`, `step` work the same as with `number`
- The `value` is always submitted as a string (e.g., "50")

### Date and Time Types

HTML5 date/time inputs provide native date pickers:

```html
<label for="birthday">Date of birth</label>
<input type="date" name="birthday" id="birthday" min="1900-01-01" max="2026-12-31">

<label for="appt-time">Appointment time</label>
<input type="time" name="appt-time" id="appt-time" min="09:00" max="17:00">

<label for="meeting">Meeting start</label>
<input type="datetime-local" name="meeting" id="meeting">

<label for="expiry">Expiry month</label>
<input type="month" name="expiry" id="expiry">

<label for="camp-week">Camp week</label>
<input type="week" name="camp-week" id="camp-week">
```

| Type | Format | Example |
|---|---|---|
| `date` | yyyy-mm-dd | 2026-06-28 |
| `time` | hh:mm | 14:30 |
| `datetime-local` | yyyy-mm-ddThh:mm | 2026-06-28T14:30 |
| `month` | yyyy-mm | 2026-06 |
| `week` | yyyy-Www | 2026-W26 |

Browser support varies. On desktop, Chrome and Firefox provide native date pickers. Safari requires a workaround or polyfill for some types. On mobile, all major browsers provide native pickers.

The value is always submitted in the standard format, regardless of the user's locale. For example, a user in the US who picks June 28, 2026 still submits `2026-06-28`.

### Choice Types

**`checkbox`** — binary (on/off) choice:

```html
<input type="checkbox" name="subscribe" id="subscribe" value="newsletter" checked>
<label for="subscribe">Subscribe to newsletter</label>
```

- If checked, submits `subscribe=newsletter`
- If unchecked, nothing is submitted for that field
- Multiple checkboxes with the same `name` but different `value` attributes form a group

**`radio`** — single selection from a group:

```html
<fieldset>
    <legend>Preferred contact method</legend>
    <input type="radio" name="contact" id="contact-email" value="email" checked>
    <label for="contact-email">Email</label>

    <input type="radio" name="contact" id="contact-phone" value="phone">
    <label for="contact-phone">Phone</label>

    <input type="radio" name="contact" id="contact-mail" value="mail">
    <label for="contact-mail">Mail</label>
</fieldset>
```

- All radio buttons in a group share the same `name` attribute
- Selecting one deselects the others
- The `value` of the selected radio is submitted
- One radio should be pre-selected with `checked`

### Specialized Types

**`file`** — file upload:

```html
<input type="file" name="avatar" accept="image/png, image/jpeg" multiple>
```

- `accept` — MIME type filter
- `multiple` — allow multiple files
- Requires `enctype="multipart/form-data"` on the `<form>`

Selected files are available in JavaScript via `input.files` (a `FileList`).

**`color`** — color picker:

```html
<input type="color" name="theme-color" value="#0056b3">
```

The value is always a 7-character hex color (e.g., `#ff0000`). The appearance depends on the browser — Chrome and Firefox show a native color picker.

**`hidden`** — invisible data carrier:

```html
<input type="hidden" name="csrf_token" value="abc123def456">
<input type="hidden" name="user_id" value="42">
```

Hidden inputs are not rendered but are submitted with the form. Use them for tokens, session identifiers, and other metadata the server needs.

**`image`** — graphical submit button:

```html
<input type="image" src="submit-button.png" alt="Submit" width="200" height="50">
```

Works like `type="submit"` but uses an image. The clicked coordinates are submitted as `x` and `y` parameters.

### Global Attributes

These attributes work on most input types:

**`name`** — the key submitted to the server:

```html
<input name="username">
```

**`value`** — the default value:

```html
<input type="text" name="city" value="San Francisco">
```

**`placeholder`** — hint text:

```html
<input type="text" placeholder="Enter your username">
```

**`required`** — prevents submission if empty:

```html
<input type="email" required>
```

**`disabled`** — field is visible but not interactive or submitted:

```html
<input type="text" disabled>
```

**`readonly`** — field can be focused but not edited; still submitted:

```html
<input type="text" readonly value="Read-only content">
```

**`autofocus`** — focus on page load (use once per page):

```html
<input type="search" autofocus>
```

**`autocomplete`** — browser autofill hints:

```html
<input type="email" autocomplete="email">
```

**`tabindex`** — tab order override:

```html
<input type="text" tabindex="1">
```

Avoid setting `tabindex` values greater than 0; let the DOM order determine tab sequence.

### Input-Specific Attributes

**`min` / `max`** — value boundaries for numeric and date inputs:

```html
<input type="number" min="0" max="100">
<input type="date" min="2026-01-01" max="2026-12-31">
```

**`step`** — increment step:

```html
<input type="number" step="0.01">  <!-- Currency: 0.01 -->
<input type="range" step="5">      <!-- 5-point increments -->
```

**`minlength` / `maxlength`** — character count limits:

```html
<input type="text" minlength="3" maxlength="20">
```

**`pattern`** — regex validation:

```html
<input type="text" name="zip" pattern="[0-9]{5}" title="5-digit ZIP code (e.g., 94102)">
```

The `title` attribute should describe the expected format for screen readers and validation messages.

**`multiple`** — allow multiple values:

```html
<input type="email" multiple>
<input type="file" multiple>
```

**`accept`** — file type restriction:

```html
<input type="file" accept=".pdf,.doc,.docx">
```

**`size`** — visible width in characters:

```html
<input type="text" size="30">
```

This sets the visual width, not the data limit. Use CSS `width` for layout control.

**`spellcheck`** — enable or disable spell checking:

```html
<input type="text" spellcheck="true">
<input type="search" spellcheck="false">
```

### Datalist

The `<datalist>` element provides a list of suggested values for an input, without restricting the user to those values:

```html
<label for="browser">Choose a browser:</label>
<input list="browsers" id="browser" name="browser">

<datalist id="browsers">
    <option value="Chrome">
    <option value="Firefox">
    <option value="Safari">
    <option value="Edge">
    <option value="Opera">
</datalist>
```

The user can type freely or select from the suggestions. This is different from `<select>`, which restricts the user to predefined options.

A datalist can be applied to any text-based input type:

```html
<label for="country">Country</label>
<input type="text" id="country" name="country" list="countries">

<datalist id="countries">
    <option value="United States">
    <option value="Canada">
    <option value="Mexico">
    <!-- ... -->
</datalist>
```

The `<option>` elements in a datalist can have both `value` and `label` attributes. If both are present, the label is shown in the dropdown and the value is inserted into the input when selected.

### Output Element

The `<output>` element displays the result of a calculation or user action:

```html
<form oninput="result.value = parseInt(a.value) + parseInt(b.value)">
    <input type="range" id="a" name="a" value="50"> +
    <input type="number" id="b" name="b" value="50"> =
    <output name="result" for="a b">100</output>
</form>
```

- `for` — space-separated list of input IDs that contribute to the output
- The element is semantically marked as output, which benefits screen readers
- Use `<output>` for live calculations, range displays, and dynamic feedback

### Progress and Meter

`<progress>` shows task completion progress:

```html
<label for="upload-progress">Upload progress:</label>
<progress id="upload-progress" value="70" max="100">70%</progress>
```

- `value` — current progress
- `max` — total (defaults to 1.0 if omitted)
- The text between tags is the fallback for non-supporting browsers

`<meter>` displays a scalar measurement within a known range:

```html
<label for="disk-usage">Disk usage:</label>
<meter id="disk-usage" value="0.7" min="0" max="1" low="0.5" high="0.8" optimum="0.2">70%</meter>
```

- `min` / `max` — range bounds
- `low` / `high` — threshold boundaries
- `optimum` — the preferred value
- Color changes based on where the value falls relative to thresholds (green, yellow, red)

### Input Mode

The `inputmode` attribute hints at the type of data the user might enter, controlling the on-screen keyboard on mobile:

```html
<input type="text" inputmode="numeric" pattern="[0-9]*">  <!-- Numeric keypad for text field -->
<input type="text" inputmode="email">                      <!-- Email keyboard layout -->
<input type="text" inputmode="url">                        <!-- URL keyboard layout -->
<input type="text" inputmode="tel">                        <!-- Telephone keypad -->
<input type="text" inputmode="decimal">                    <!-- Decimal numeric keypad -->
<input type="text" inputmode="search">                     <!-- Search keyboard -->
```

| `inputmode` | Keyboard layout |
|---|---|
| `text` | Default text keyboard |
| `decimal` | Numeric with decimal point |
| `numeric` | Digits 0-9 |
| `tel` | Telephone keypad |
| `search` | Text keyboard with search action |
| `email` | Text keyboard with @ key |
| `url` | Text keyboard with / and .com |

Use `inputmode` when you need a specific keyboard layout but don't want the automatic validation of the corresponding `type`. For example, a credit card field might use `type="text"` with `inputmode="numeric"` to get the numeric keypad without number-specific validation.

### Common Patterns

**Credit card number:**

```html
<label for="cc-number">Card number</label>
<input type="text" id="cc-number" name="cc-number" inputmode="numeric" pattern="[0-9\s]{13,19}" autocomplete="cc-number" maxlength="19" placeholder="1234 5678 9012 3456">
```

**Star rating (using range):**

```html
<label for="rating">Rating</label>
<input type="range" id="rating" name="rating" min="1" max="5" step="1" value="3">
```

**Password confirmation:**

```html
<label for="password">Password</label>
<input type="password" id="password" name="password" minlength="8" autocomplete="new-password">

<label for="confirm">Confirm password</label>
<input type="password" id="confirm" name="confirm" autocomplete="new-password">
```

**Search with suggestions:**

```html
<label for="search-field">Search</label>
<input type="search" id="search-field" name="q" list="suggestions" autocomplete="off">
<datalist id="suggestions">
    <option value="HTML">
    <option value="CSS">
    <option value="JavaScript">
    <option value="Accessibility">
</datalist>
```

**Quantity selector:**

```html
<label for="qty">Quantity</label>
<input type="number" id="qty" name="qty" min="1" max="99" step="1" value="1">
```

### Browser Support Considerations

While most input types are well-supported, some have caveats:

- **`date`, `time`, `datetime-local`** — Safari on desktop falls back to a text input. On iOS Safari, date pickers are supported in iOS 14.5+
- **`color`** — supported in all modern browsers. Safari added support in Safari 12.1
- **`range`** — supported everywhere, but styling is limited. Use vendor prefixes for custom appearance
- **`search`** — the clear button appearance varies by browser. Safari always shows it; Chrome shows it on hover

Fallback strategy: do not rely on native date pickers for critical flows on all browsers. Consider a JavaScript date picker library if full browser support is required.

## Glossary

| Term | Definition |
|------|------------|
| Input type | The `type` attribute on `<input>` that determines behavior and rendering |
| `datalist` | Provides suggested values for an input without restricting choices |
| `output` | Displays the result of a calculation or user action |
| `progress` | Shows task completion progress (determinate or indeterminate) |
| `meter` | Displays a scalar measurement within a known range |
| `inputmode` | Controls the mobile keyboard layout |
| `placeholder` | Hint text that disappears when the user types |
| `pattern` | Regex for client-side validation |
| `autocomplete` | Browser autofill hint |
| `multiple` | Allows multiple values for email and file inputs |
| `accept` | Restricts file types in a file input |
| `step` | Increment step for numeric and range inputs |
| `min` / `max` | Minimum and maximum values |
| `minlength` / `maxlength` | Character count limits |
| `readonly` | Content cannot be edited but is submitted |
| `disabled` | Field is visible but not interactive or submitted |

## Quick References

- [MDN: Input Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input) — complete reference for all types
- [MDN: Input Attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input#attributes) — all attributes
- [MDN: Datalist](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/datalist) — suggestion list reference
- [MDN: Output Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/output) — output reference
- [MDN: Inputmode](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/inputmode) — inputmode attribute
- [MDN: Autocomplete](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/autocomplete) — autocomplete values
- [HTML Spec: Input Types](https://html.spec.whatwg.org/multipage/input.html) — official specification
- [Can I Use: Input Types](https://caniuse.com/?search=input%20type) — browser support tables

## Next Steps

- [Form Validation](form-validation.md) — client and server-side validation
- [Accessible Forms](form-design-patterns.md) — accessible form design patterns
- [Semantic HTML & Document Outline](semantic-html.md) — structural meaning in markup
