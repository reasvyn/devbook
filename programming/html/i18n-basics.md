# HTML Internationalization (i18n)

## Description

Internationalization (i18n) in HTML prepares web content for global audiences by handling language declarations, text direction, character encoding, date and number formats, and locale-specific cultural conventions.

## Prerequisites

- [Document Structure](document-structure.md) — HTML document structure and `<head>` element
- [Meta Tags & SEO](meta-tags.md) — meta elements for document metadata

## Table of Contents

- [What is Internationalization?](#what-is-internationalization)
- [Declaring Language](#declaring-language)
- [Text Direction](#text-direction)
- [Character Encoding](#character-encoding)
- [Date and Time Formats](#date-and-time-formats)
- [Number and Currency Formats](#number-and-currency-formats)
- [Locale-Aware Sorting](#locale-aware-sorting)
- [Pluralization](#pluralization)
- [Translation and Content Management](#translation-and-content-management)
- [Images and Icons in i18n](#images-and-icons-in-i18n)
- [Forms and i18n](#forms-and-i18n)
- [URL and Routing](#url-and-routing)
- [Testing Internationalization](#testing-internationalization)
- [Accessibility and i18n](#accessibility-and-i18n)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What is Internationalization?

Internationalization (i18n) is the design and development of HTML content that can adapt to different languages, regions, and cultural conventions without requiring engineering changes.

**i18n vs l10n:**

- **Internationalization (i18n)** — preparing the codebase for localization (the letter i, 18 letters, n)
- **Localization (l10n)** — adapting content to a specific locale (translations, formatting)

**Core i18n concerns in HTML:**

| Aspect | HTML mechanism | Example |
|---|---|---|
| Language | `lang` attribute | `lang="fr"` for French |
| Text direction | `dir` attribute | `dir="rtl"` for Arabic |
| Character encoding | `<meta charset>` | `UTF-8` |
| Date/time format | `<time>` with `datetime` | Machine-readable dates |
| Number format | JavaScript `Intl` API | `Intl.NumberFormat` |
| Locale-aware strings | Template systems | Translation keys |

**The `<html>` element as the i18n root:**

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
<head>
    <meta charset="UTF-8">
    <title>Internationalization Guide</title>
</head>
<body>
    <!-- Content -->
</body>
</html>
```

### Declaring Language

**The `lang` attribute** declares the natural language of an element's content:

```html
<html lang="en">
```

The primary language is declared on the `<html>` element and can be overridden on any descendant:

```html
<p>The French word for "hello" is <span lang="fr">bonjour</span>.</p>
```

**Language tags** follow BCP 47 format: `language-script-region-variant`

| Tag | Meaning |
|---|---|
| `en` | English |
| `en-US` | American English |
| `en-GB` | British English |
| `fr` | French |
| `fr-CA` | Canadian French |
| `zh-Hans` | Simplified Chinese |
| `zh-Hant-TW` | Traditional Chinese (Taiwan) |
| `ar-SA` | Arabic (Saudi Arabia) |
| `pt-BR` | Brazilian Portuguese |
| `de-AT` | German (Austria) |

**Why `lang` matters:**

- **Screen readers** use `lang` to switch pronunciation rules and voice
- **Search engines** use `lang` to serve content to the right audience
- **Spell checkers** use `lang` to apply correct dictionaries
- **Browsers** use `lang` for date/time pickers and number formats
- **Translation tools** use `lang` to detect content language

**Screen reader example:**

```html
<p lang="fr">Je voudrais un café, s'il vous plaît.</p>
```

A French screen reader pronounces this correctly. An English screen reader with `lang="en"` would mispronounce it.

**Setting `lang` on changed language:**

```html
<p>He said, <q lang="de"><span lang="de">Guten Tag!</span></q> as he entered.</p>
```

**Language and HTTP headers:**

The server can also declare language via the `Content-Language` header:

```http
Content-Language: en-US
```

The HTML `lang` attribute takes precedence over the HTTP header.

**Language negotiation** — the server chooses content based on the `Accept-Language` HTTP header:

```http
Accept-Language: fr-CH, fr;q=0.9, en;q=0.8, de;q=0.7, *;q=0.5
```

The browser sends the user's language preferences. The server returns the best match.

### Text Direction

**The `dir` attribute** declares the base text direction:

```html
<html dir="ltr">  <!-- Left-to-right (default) -->
<html dir="rtl">  <!-- Right-to-left -->
```

**Direction values:**

| Value | Meaning | Used by |
|---|---|---|
| `ltr` | Left-to-right | English, French, Chinese, etc. |
| `rtl` | Right-to-left | Arabic, Hebrew, Persian, Urdu |
| `auto` | Auto-detect from first strong character | User-generated content |

**RTL page structure:**

```html
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <title>مرحبا بالعالم</title>
</head>
<body>
    <h1>الصفحة الرئيسية</h1>
    <p>هذا نص عربي.</p>
</body>
</html>
```

**Override direction on inline content:**

```html
<p dir="rtl">هذا النص بالعربية</p>
<p>The English word <span dir="ltr">"hello"</span> within Arabic context.</p>
```

**CSS and direction:**

```css
/* Direction-aware properties */
margin-inline-start: 10px;   /* left in LTR, right in RTL */
padding-inline-end: 10px;    /* right in LTR, left in RTL */
border-inline-start: 1px solid;

/* Logical properties adapt to dir */
inset-inline-start: 0;
```

Use logical CSS properties (`margin-inline-start`, `padding-inline-end`) instead of physical (`margin-left`, `padding-right`) for RTL compatibility.

**The `<bdo>` element** overrides bidirectional algorithm:

```html
<bdo dir="rtl">This text is forced right-to-left.</bdo>
```

**The `<bdi>` element** isolates bidirectional text:

```html
<p>User <bdi>أحمد</bdi> logged in.</p>
```

`<bdi>` prevents user-generated content from reversing surrounding text direction.

**Direction and the `<html>` element:**

Always set `dir` on `<html>` for RTL pages. Browser defaults are LTR, and auto-detection may fail on the first render.

### Character Encoding

**`<meta charset="UTF-8">`** declares the character encoding:

```html
<head>
    <meta charset="UTF-8">
</head>
```

UTF-8 is the only encoding you should use. It supports every character in Unicode, including:
- Latin script: `A`, `é`, `ñ`
- CJK: `中`, `文`
- Arabic: `العربية`
- Emoji: `🎉`, `🚀`
- Math symbols: `∑`, `∞`, `≠`

**Why UTF-8 is essential for i18n:**

```html
<!-- Correct: UTF-8 encodes all languages -->
<p>English: Hello</p>
<p>日本語: こんにちは</p>
<p>Русский: Здравствуйте</p>
<p>العربية: مرحبا</p>
```

**HTML entities as fallback (rarely needed with UTF-8):**

```html
<p>&quot; &amp; &lt; &gt; &copy; &mdash;</p>
```

Named entities are optional with UTF-8. Use them only for HTML special characters (`<`, `>`, `&`, `"`) that would break parsing.

**Encoding declaration must be in the first 1024 bytes** of the document for the browser to detect it correctly.

### Date and Time Formats

**The `<time>` element** provides a machine-readable date for any locale:

```html
<time datetime="2026-06-28">June 28, 2026</time>
```

For localization, render the human-readable portion in the correct locale and use the `datetime` attribute for the standard machine format:

```html
<!-- English (US) -->
<time datetime="2026-06-28">June 28, 2026</time>

<!-- English (UK) -->
<time datetime="2026-06-28">28 June 2026</time>

<!-- French -->
<time datetime="2026-06-28">28 juin 2026</time>

<!-- Japanese -->
<time datetime="2026-06-28">2026年6月28日</time>

<!-- ISO 8601 (always valid) -->
<time datetime="2026-06-28">2026-06-28</time>
```

**JavaScript `Intl.DateTimeFormat` for dynamic formatting:**

```javascript
const date = new Date('2026-06-28');
const formats = {
    'en-US': new Intl.DateTimeFormat('en-US'),     // 6/28/2026
    'en-GB': new Intl.DateTimeFormat('en-GB'),     // 28/06/2026
    'de-DE': new Intl.DateTimeFormat('de-DE'),     // 28.6.2026
    'ja-JP': new Intl.DateTimeFormat('ja-JP'),     // 2026/6/28
};
```

**Locale-specific options:**

```javascript
const formatter = new Intl.DateTimeFormat('en-US', {
    weekday: 'long',
    year: 'numeric',
    month: 'long',
    day: 'numeric',
});
console.log(formatter.format(new Date()));  // "Sunday, June 28, 2026"
```

**Time zones:**

```javascript
const formatter = new Intl.DateTimeFormat('en-US', {
    timeZone: 'America/New_York',
    timeZoneName: 'short',
    hour: 'numeric',
    minute: 'numeric',
});
console.log(formatter.format(date));  // "3:45 PM EDT"
```

**HTML `datetime` attribute must use ISO 8601 format:**

```
YYYY-MM-DDTHH:mm:ss.sssZ
2026-06-28T15:30:00Z        → UTC
2026-06-28T15:30:00+02:00   → With timezone offset
2026-06-28                  → Date only
2026-06                     → Year and month only
2026                        → Year only
```

### Number and Currency Formats

**JavaScript `Intl.NumberFormat`** handles locale-specific number formatting:

```javascript
const number = 1234567.89;

new Intl.NumberFormat('en-US').format(number);    // "1,234,567.89"
new Intl.NumberFormat('de-DE').format(number);    // "1.234.567,89"
new Intl.NumberFormat('fr-FR').format(number);    // "1 234 567,89"
new Intl.NumberFormat('ar-SA').format(number);    // "١٬٢٣٤٬٥٦٧٫٨٩"
```

**Currency formatting:**

```javascript
new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD',
}).format(1234.56);  // "$1,234.56"

new Intl.NumberFormat('de-DE', {
    style: 'currency',
    currency: 'EUR',
}).format(1234.56);  // "1.234,56 €"

new Intl.NumberFormat('ja-JP', {
    style: 'currency',
    currency: 'JPY',
}).format(1234);  // "￥1,234"
```

**Significant digits:**

```javascript
new Intl.NumberFormat('en-US', {
    minimumSignificantDigits: 3,
    maximumSignificantDigits: 5,
}).format(0.0012345);  // "0.0012345"
```

**Unit formatting:**

```javascript
new Intl.NumberFormat('en-US', {
    style: 'unit', unit: 'kilometer-per-hour',
}).format(100);  // "100 km/h"
```

**Percentage formatting:**

```javascript
new Intl.NumberFormat('fr-FR', {
    style: 'percent',
}).format(0.25);  // "25 %"
```

### Locale-Aware Sorting

Use `Intl.Collator` for language-aware string comparison:

```javascript
const collator = new Intl.Collator('de-DE');

const words = ['Österreich', 'Ostsee', 'Ofen'];
words.sort(collator.compare);
// In German: ['Ofen', 'Ostsee', 'Österreich']
// With default sort: ['Ofen', 'Ostsee', 'Österreich'] — different order
```

**Collator options:**

```javascript
const collator = new Intl.Collator('en', {
    sensitivity: 'base',   // Ignore case and accents
    ignorePunctuation: true,
    numeric: true,
});

collator.compare('a', 'A');  // 0 (equal with base sensitivity)
collator.compare('100', '20');  // 1 (100 > 20 with numeric: true)
```

**Sensitivity levels:**

| Sensitivity | Case | Accent | Example matches |
|---|---|---|---|
| `base` | Ignores | Ignores | a = A = á = Á |
| `accent` | Ignores | Considers | a = A, a ≠ á |
| `case` | Considers | Ignores | a ≠ A, a = á |
| `variant` | Considers | Considers | a ≠ A, a ≠ á |

### Pluralization

Pluralization rules differ dramatically across languages. English has two forms (singular/plural), Arabic has six, Russian has four.

**JavaScript `Intl.PluralRules`:**

```javascript
const pluralRules = new Intl.PluralRules('en-US');

pluralRules.select(0);  // "other"  ("0 items")
pluralRules.select(1);  // "one"   ("1 item")
pluralRules.select(2);  // "other"  ("2 items")
```

**Handling plural forms with ICU message syntax (in template systems):**

```javascript
// Using Intl.PluralRules with a mapping
function pluralize(count, locale) {
    const rules = new Intl.PluralRules(locale);
    const forms = {
        'en': {
            one: 'item',
            other: 'items',
        },
        'ar': {
            one: 'عنصر',
            two: 'عنصران',
            few: 'عناصر',
            many: 'عنصرًا',
            other: 'عنصر',
        },
    };

    const form = forms[locale][rules.select(count)];
    return `${count} ${form}`;
}

pluralize(1, 'en');  // "1 item"
pluralize(2, 'en');  // "2 items"
pluralize(2, 'ar');  // "2 عنصران"
```

**Plural categories by language:**

English has two forms (singular/plural), Russian has four (one/few/many/other), Arabic has six (zero/one/two/few/many/other). Always handle the `other` category as fallback.

### Translation and Content Management

**Separate content from code.** Never hardcode display strings in templates:

```html
<!-- Bad: hardcoded English -->
<button>Save Changes</button>

<!-- Good: translation key -->
<button data-i18n="save_changes">Save Changes</button>
```

**JSON-based translation files:**

```json
{
    "en": {
        "save_changes": "Save Changes",
        "cancel": "Cancel",
        "welcome": "Welcome, {name}!"
    },
    "fr": {
        "save_changes": "Enregistrer les modifications",
        "cancel": "Annuler",
        "welcome": "Bienvenue, {name}!"
    }
}
```

**Simple JavaScript translation function:**

```javascript
function t(key, params = {}) {
    const locale = document.documentElement.lang || 'en';
    let template = translations[locale]?.[key] || key;
    for (const [k, v] of Object.entries(params)) {
        template = template.replace(`{${k}}`, v);
    }
    return template;
}
```

Use a mature i18n library (i18next, FormatJS, react-intl) for production work.

### Images and Icons in i18n

**Text in images is not translatable.** Never embed text in images:

```html
<!-- Bad: text in image cannot be translated -->
<img src="hero-banner-en.png" alt="Buy One Get One Free">

<!-- Good: text on top of image with CSS -->
<div class="hero">
    <img src="hero-banner.jpg" alt="">
    <h1 data-i18n="hero_headline">Buy One Get One Free</h1>
</div>
```

**Locale-specific images:**

```html
<picture>
    <source srcset="/images/flag-{locale}.jpg" media="(min-width: 768px)">
    <img src="/images/flag-{locale}.jpg" alt="Flag">
</picture>
```

Be aware of cultural differences: a thumbs-up gesture (👍) is positive in Western cultures but offensive in some Middle Eastern countries. Test images and icons across target locales.

### Forms and i18n

**Locale-aware form inputs:**

```html
<!-- Localized placeholder -->
<input type="tel" placeholder="+1 (555) 123-4567" data-i18n-placeholder="phone_us">
<input type="tel" placeholder="+44 20 7123 4567" data-i18n-placeholder="phone_uk">

<!-- Postal code patterns -->
<input type="text" name="postal_code"
       pattern="[0-9]{5}" title="Five-digit US ZIP code">
```

**Locale-aware validation:**

```javascript
function validatePostalCode(value, locale) {
    const patterns = {
        'en-US': /^\d{5}(-\d{4})?$/,
        'en-GB': /^[A-Z]{1,2}\d{1,2}[A-Z]?\s?\d[A-Z]{2}$/,
        'fr-FR': /^\d{5}$/,
        'ja-JP': /^\d{3}-\d{4}$/,
    };
    const pattern = patterns[locale] || patterns['en-US'];
    return pattern.test(value);
}
```

**Name fields — do not assume family name order.** In Japan and Hungary, the family name comes first. Consider a single `name` field or clearly labeled separate fields.

**Address forms vary by country.** US has state + ZIP, UK has county + postcode, Japan has prefecture + ward + block. Adapt form layouts per locale rather than using a single template.

**Phone numbers** vary by country in format and length. Use a library like libphonenumber-js for validation.

### URL and Routing

**Locale-specific URLs:**

```
/en/docs/html
/fr/docs/html
/ja/docs/html
```

or with subdomains:

```
en.example.com
fr.example.com
ja.example.com
```

**Canonical URL with language targeting:**

```html
<link rel="canonical" href="https://example.com/docs/html">

<!-- hreflang for language variants -->
<link rel="alternate" href="https://example.com/en/docs/html" hreflang="en">
<link rel="alternate" href="https://example.com/fr/docs/html" hreflang="fr">
<link rel="alternate" href="https://example.com/ja/docs/html" hreflang="ja">
<link rel="alternate" href="https://example.com/docs/html" hreflang="x-default">
```

`hreflang` tells search engines which URL to serve for which language.

**URL slug localization:**

| Route | English slug | Japanese slug |
|---|---|---|
| `/docs/html` | `/docs/html` | `/docs/html` (if term is standard) |
| `/blog/getting-started` | `/blog/getting-started` | `/blog/はじめに` |

Translated slugs improve SEO but require URL routing support.

### Testing Internationalization

**Manual testing checklist:**

| Check | How |
|---|---|
| Language attribute | `html[lang]` matches content language |
| Text direction | RTL content renders from right, layout flips |
| Character encoding | All characters display correctly |
| Date format | Matches locale conventions |
| Number format | Decimal and thousand separators correct |
| Currency | Symbol, position, and decimal places correct |
| Sorting | Language-specific ordering (e.g., ö vs o) |
| Pluralization | All plural forms tested (not just singular/plural) |

**Automated checks:**

```javascript
const html = document.documentElement;
if (!html.lang) console.error('Missing lang attribute');
if (!html.dir && /^(ar|he|fa|ur)/.test(html.lang)) {
    console.error('RTL language but missing dir attribute');
}
```

**Testing RTL layout:**
1. Set `dir="rtl"` on `<html>` and verify text aligns right
2. Check that UI elements are mirrored (back button points right)
3. Test mixed LTR/RTL content does not break layout
4. Ensure scrolling works in the correct direction

**Locale fallback chain:**

If a translation key is missing, fall back to a parent locale (`fr-CA → fr → en`). Always have a fallback locale (typically `en`) with all keys.

### Accessibility and i18n

**Language switching and screen readers:**

When the user switches language, screen readers must switch voices:

```html
<button lang="fr" onclick="switchLang('fr')">Français</button>
```

The `lang` attribute on the button ensures the screen reader pronounces "Français" correctly, even on an English page.

**Text spacing and i18n:**

Some languages require more spacing:
- German compounds: "Donaudampfschifffahrtsgesellschaftskapitän"
- Thai and Burmese: no spaces between words

```css
/* Ensure text containers can accommodate longer strings */
.text-container {
    min-height: 3em;  /* Allow for text expansion */
}

/* For languages with different line heights */
[lang="th"] {
    line-height: 1.8;  /* Thai needs more line height */
}
```

**Text expansion in translation:**

English to other languages typically expands by:

| Language | Expansion |
|---|---|
| German | +30–35% |
| French | +15–20% |
| Spanish | +15–25% |
| Russian | +10–15% |
| Japanese | -10–50% (can be shorter) |
| Chinese | -20–50% (can be much shorter) |

Design layouts that accommodate 30–50% text expansion without breaking.

## Study Cases

### Multi-Language Blog with E-Commerce Checkout

A site supporting en-US, fr-FR, ar-SA (RTL):

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
<head>
    <meta charset="UTF-8">
    <title data-i18n="blog_title">DevBook Blog</title>
    <link rel="alternate" hreflang="en" href="/en/blog/post1">
    <link rel="alternate" hreflang="fr" href="/fr/blog/post1">
    <link rel="alternate" hreflang="ar" href="/ar/blog/post1">
</head>
<body>
    <nav aria-label="Language selector">
        <ul>
            <li><a href="/en/blog/post1" hreflang="en" lang="en">English</a></li>
            <li><a href="/fr/blog/post1" hreflang="fr" lang="fr">Français</a></li>
            <li><a href="/ar/blog/post1" hreflang="ar" lang="ar">العربية</a></li>
        </ul>
    </nav>

    <article>
        <h1 data-i18n="post_title">Getting Started with HTML</h1>
        <p data-i18n="post_date">
            <time datetime="2026-06-28">June 28, 2026</time>
        </p>
    </article>
</body>
</html>
```

For Arabic locale, `dir="rtl"` is set on `<html>` and logical CSS handles layout mirroring. Currency formatting uses `Intl.NumberFormat` with locale-appropriate currency symbols and separator styles. Address forms adapt per locale: US shows state+ZIP, Japan shows prefecture+ward+block.

## Examples

### Example 1: Search with locale-aware sorting

```html
<label for="search">Search</label>
<input type="search" id="search">
<ul id="results"></ul>
```

```javascript
const items = ['Österreich', 'Ostsee', 'Über'];
const collator = new Intl.Collator(document.documentElement.lang);
items.sort(collator.compare);  // Handles umlauts and accents correctly
```

### Example 2: Right-to-left layout with logical CSS

```html
<div dir="rtl">
    <div class="card">
        <img src="icon.png" alt="">
        <p>بطاقة نصية عربية</p>
    </div>
</div>
```

```css
.card {
    display: flex;
    gap: 1rem;
    padding-inline: 1rem;     /* 1rem left in LTR, right in RTL */
    border-inline-start: 3px solid blue;
}
```

The `*-inline-start` and `*-inline-end` properties automatically mirror based on `dir`.

## Glossary

| Term | Definition |
|---|---|
| i18n | Internationalization — preparing code for multiple languages and locales |
| l10n | Localization — adapting content to a specific locale |
| Locale | A combination of language and region (e.g., en-US, fr-CA) |
| BCP 47 | Standard format for language tags (e.g., zh-Hans-CN) |
| RTL | Right-to-left script direction (Arabic, Hebrew, Persian) |
| LTR | Left-to-right script direction (most European languages) |
| UTF-8 | Universal character encoding supporting all Unicode characters |
| `Intl` | JavaScript API for locale-aware formatting (dates, numbers, collation) |
| Plural category | Grammatical number category (one, few, many, other) |
| hreflang | HTML attribute telling search engines which URL for which language |
| Bidirectional text | Text containing both LTR and RTL content in the same paragraph |

## Quick References

- [W3C Internationalization](https://www.w3.org/International/) — official i18n resources and best practices
- [MDN Intl API](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl) — JavaScript internationalization API
- [BCP 47 Language Tags](https://www.iana.org/assignments/language-subtag-registry/language-subtag-registry) — registered language subtags
- [ICU Message Format](https://unicode-org.github.io/icu/userguide/format_parse/messages/) — cross-platform pluralization and message formatting
- [libphonenumber-js](https://www.npmjs.com/package/libphonenumber-js) — phone number formatting per country
- [Logical CSS Properties](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_logical_properties_and_values) — direction-aware CSS properties

## Next Steps

- [Meta Tags & SEO](meta-tags.md) — search engine and social media metadata
- [Semantic HTML & Document Outline](semantic-html.md) — semantic structure fundamentals