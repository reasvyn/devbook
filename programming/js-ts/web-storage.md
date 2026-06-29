# Web Storage & Cookies

## Description

Browsers provide three client-side storage mechanisms: localStorage, sessionStorage, and cookies. Each serves a different purpose — from persisting preferences across sessions to maintaining authentication state.

## Prerequisites

- [Values & Types](values-and-types.md) — JSON serialization, type coercion
- [Events](events.md) — storage event monitoring

## Table of Contents

- [The Three Storage Layers](#the-three-storage-layers)
- [localStorage](#localstorage)
- [sessionStorage](#sessionstorage)
- [Cookies](#cookies)
- [Storage Limits](#storage-limits)
- [JSON Serialization](#json-serialization)
- [The Storage Event](#the-storage-event)
- [IndexedDB Overview](#indexeddb-overview)
- [Security Considerations](#security-considerations)
- [When to Use What](#when-to-use-what)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Three Storage Layers

```
Feature          | localStorage | sessionStorage | Cookies
─────────────────|──────────────|────────────────|──────────
Persists after   | Yes          | No             | Configurable
  tab close      |              |                |
Capacity         | ~5-10 MB     | ~5-10 MB       | ~4 KB
Sent to server   | No           | No             | Yes (automatic)
Accessible from  | Same origin  | Same origin    | Configurable via path/domain
API type         | Synchronous  | Synchronous    | document.cookie (string)
Expiration       | Manual       | On tab close   | Via Max-Age or Expires
```

### localStorage

Data persists until explicitly deleted. Survives browser restarts.

**Basic operations:**

```javascript
// Store a value
localStorage.setItem("theme", "dark");
localStorage.setItem("userId", "42");

// Retrieve a value
const theme = localStorage.getItem("theme");    // "dark"
const missing = localStorage.getItem("nonexistent"); // null

// Remove a single key
localStorage.removeItem("theme");

// Clear all keys for this origin
localStorage.clear();

// Count keys
const count = localStorage.length;

// Access by index (keys in insertion order)
const firstKey = localStorage.key(0);
```

**Storing objects — serialize to JSON:**

```javascript
const user = {
    id: 42,
    name: "Alice",
    preferences: {
        theme: "dark",
        fontSize: 14
    }
};

// Save
localStorage.setItem("user", JSON.stringify(user));

// Load
const saved = JSON.parse(localStorage.getItem("user"));
console.log(saved.name);                        // "Alice"
console.log(saved.preferences.theme);           // "dark"
```

**Missing key returns null:**

```javascript
const data = localStorage.getItem("nonexistent");
if (data === null) {
    // Key does not exist
}

// This is different from:
localStorage.setItem("empty", "");              // ""
localStorage.getItem("empty") === "";           // true
localStorage.getItem("nonexistent") === null;   // true
```

### sessionStorage

Same API as localStorage, but data is cleared when the tab or window is closed:

```javascript
sessionStorage.setItem("sessionId", "abc123");
sessionStorage.getItem("sessionId");            // "abc123"

sessionStorage.removeItem("sessionId");
sessionStorage.clear();
```

**Tab isolation:** Each tab or window gets its own sessionStorage. Opening two tabs to the same site creates two separate storage areas.

```javascript
// Tab A
sessionStorage.setItem("draft", "Post content A");

// Tab B
sessionStorage.getItem("draft");                // null — separate storage
```

**Page session** persists across page reloads and restores within the same tab (e.g., after the browser crashes and restores tabs).

### Cookies

Cookies are small strings (max ~4 KB) sent with every HTTP request to the matching domain/path.

**Reading cookies:**

```javascript
document.cookie;   // "theme=dark; sessionId=abc123; lang=en-US"
```

The entire cookie string is semicolon-separated key=value pairs.

**Setting cookies:**

```javascript
document.cookie = "theme=dark";
document.cookie = "sessionId=abc123";
```

Note: `document.cookie` is a getter/setter. Setting it does not overwrite all cookies — it appends or updates one cookie.

**Cookie attributes — set via options in the value string:**

```javascript
document.cookie = "sessionId=abc123; Path=/; Domain=example.com; Max-Age=86400; Secure; HttpOnly; SameSite=Lax";
```

| Attribute | Purpose |
|-----------|---------|
| `Path=/` | Only send for URLs under this path |
| `Domain=example.com` | Include subdomains |
| `Max-Age=3600` | Lifetime in seconds (from now) |
| `Expires=Date` | Absolute expiration (alternative to Max-Age) |
| `Secure` | Only send over HTTPS |
| `HttpOnly` | Not accessible from JavaScript (set by server) |
| `SameSite=Lax|Strict|None` | CSRF protection control |

**Without Max-Age or Expires, the cookie is a session cookie:**

```javascript
// Session cookie — persists until tab is closed
document.cookie = "cartId=xyz789; Path=/; Secure";
```

**Reading a specific cookie value:**

```javascript
function getCookie(name) {
    const matches = document.cookie.match(
        new RegExp(`(?:^|; )${name.replace(/([.$?*|{}()\[\]\\\/+^])/g, '\\$1')}=([^;]*)`)
    );
    return matches ? decodeURIComponent(matches[1]) : null;
}

getCookie("theme");                             // "dark"
getCookie("nonexistent");                       // null
```

**Deleting a cookie — set Max-Age to 0 or a past date:**

```javascript
document.cookie = "theme=; Max-Age=0; Path=/";
```

### Storage Limits

```javascript
function testStorageLimit(storage = localStorage) {
    let testKey = "__storage_test__";
    let i = 0;

    try {
        for (i = 0; i < 10000; i++) {
            storage.setItem(testKey + i, "x".repeat(1024)); // 1 KB each
        }
    } catch (e) {
        if (e.name === "QuotaExceededError") {
            console.log(`Storage full at ~${i} KB`);
        }
    }

    // Clean up
    for (let j = 0; j < i; j++) {
        storage.removeItem(testKey + j);
    }
}
```

Typical storage limits:

- **localStorage/sessionStorage:** ~5 MB (varies by browser)
- **Cookies:** ~4 KB total per domain (including metadata)
- **IndexedDB:** Usually hundreds of MB or "unlimited" (with user prompt)

**QuotaExceededError handling:**

```javascript
try {
    localStorage.setItem("key", veryLargeString);
} catch (error) {
    if (error.name === "QuotaExceededError") {
        console.error("Storage is full — clear old data");
        cleanupOldData();
    }
}
```

### JSON Serialization

Web Storage only stores strings. All non-string data must be serialized.

```javascript
// Safe serialize/deserialize
function setJSON(key, value) {
    localStorage.setItem(key, JSON.stringify(value));
}

function getJSON(key) {
    const raw = localStorage.getItem(key);
    if (raw === null) return null;
    try {
        return JSON.parse(raw);
    } catch {
        // Corrupted data — remove it
        localStorage.removeItem(key);
        return null;
    }
}

// Usage
setJSON("userPreferences", { theme: "dark", fontSize: 14 });
const prefs = getJSON("userPreferences");
```

**What JSON cannot store:** `undefined`, `BigInt`, `Symbol`, functions, circular references, `Date` objects (serializes as string).

```javascript
const data = {
    date: new Date(),
    fn: () => {},
    undef: undefined
};

JSON.stringify(data);
// '{"date":"2026-06-29T...","fn":{},"undef":undefined}' — loses function, nulls undefined
```

### The Storage Event

When localStorage changes in one tab, other tabs of the same origin receive a `storage` event:

```javascript
// Tab A: listener
window.addEventListener("storage", (e) => {
    console.log(`Key "${e.key}" changed from "${e.oldValue}" to "${e.newValue}"`);
    console.log(`Origin: ${e.url}`);
    console.log(`Storage area:`, e.storageArea);   // localStorage object
});

// Tab B: triggers the event
localStorage.setItem("theme", "light");
```

The event only fires on **other tabs/windows** — not on the tab that made the change.

```javascript
// Cross-tab sync example
window.addEventListener("storage", (e) => {
    if (e.key === "theme") {
        applyTheme(e.newValue);
    }
});
```

### IndexedDB Overview

When localStorage is not enough (complex data, large blobs, structured queries), IndexedDB provides a full NoSQL database in the browser.

```javascript
// Open a database
const request = indexedDB.open("MyApp", 1);

request.onupgradeneeded = (event) => {
    const db = event.target.result;
    const store = db.createObjectStore("notes", {
        keyPath: "id",
        autoIncrement: true
    });
    store.createIndex("title", "title", { unique: false });
};

request.onsuccess = (event) => {
    const db = event.target.result;
    addNote(db, { title: "My note", body: "Content here" });
};

function addNote(db, note) {
    const tx = db.transaction("notes", "readwrite");
    const store = tx.objectStore("notes");
    store.add(note);
}
```

**When to use IndexedDB over localStorage:**

- Storing hundreds of MB of data
- Complex querying (indexes, ranges)
- Binary data (blobs, files)
- Structured data that needs ordering or filtering

### Security Considerations

**Never store sensitive data in localStorage or sessionStorage:**

```javascript
// DANGEROUS — XSS can steal this
localStorage.setItem("auth_token", "secret-token-123");

// Safer — HttpOnly cookie set by server
// Server: Set-Cookie: auth_token=secret-token-123; HttpOnly; Secure; SameSite=Strict
```

**Why localStorage is not secure:**

1. Any JavaScript running on the same origin can read it (XSS vulnerability)
2. No built-in encryption
3. Users cannot inspect/clear it as easily as cookies
4. Subdomain attacks can reach it

**Cookie security attributes:**

- `HttpOnly` — inaccessible from JavaScript, blocks XSS theft
- `Secure` — only sent over HTTPS, prevents network sniffing
- `SameSite=Strict` — prevents CSRF attacks from other sites

**Protecting storage data:**

```javascript
function setSecureItem(key, value) {
    try {
        const encrypted = btoa(JSON.stringify(value)); // base64 encode (not true encryption!)
        localStorage.setItem(key, encrypted);
    } catch {
        console.error("Failed to save data");
    }
}
```

Note: base64 is encoding, not encryption. For actual client-side encryption, use the Web Crypto API.

### When to Use What

| Use case | Solution |
|----------|----------|
| UI preferences (theme, layout) | localStorage |
| Shopping cart (no login needed) | localStorage |
| Draft content recovery | localStorage |
| Form input preservation | sessionStorage |
| Tab-specific wizard state | sessionStorage |
| Auth tokens (server-managed) | HttpOnly Secure cookie |
| Analytics/tracking identifiers | Cookie with SameSite |
| User-generated content cache | IndexedDB |
| Offline data for PWA | IndexedDB + Cache API |
| Large binary files | IndexedDB |

## Study Cases

### Case 1: Theme Persistence

```javascript
class ThemeManager {
    constructor() {
        this.storageKey = "theme";
        this.applyTheme(this.getSaved());
        document.querySelector("#theme-toggle")
            .addEventListener("click", () => this.toggle());
    }

    getSaved() {
        return localStorage.getItem(this.storageKey) || "light";
    }

    save(theme) {
        localStorage.setItem(this.storageKey, theme);
    }

    applyTheme(theme) {
        document.documentElement.setAttribute("data-theme", theme);
    }

    toggle() {
        const current = this.getSaved();
        const next = current === "light" ? "dark" : "light";
        this.save(next);
        this.applyTheme(next);

        // Notify other tabs
        localStorage.setItem("theme-changed", Date.now().toString());
    }
}

// Sync across tabs
window.addEventListener("storage", (e) => {
    if (e.key === "theme-changed") {
        themeManager.applyTheme(themeManager.getSaved());
    }
});
```

### Case 2: Session-Based Form Draft Recovery

```javascript
class FormDraft {
    constructor(formId) {
        this.form = document.querySelector(`#${formId}`);
        this.key = `draft:${formId}`;

        this.restore();
        this.form.addEventListener("input", () => this.save());
        this.form.addEventListener("submit", () => this.clear());
        window.addEventListener("beforeunload", () => this.save());
    }

    save() {
        const data = new FormData(this.form);
        const obj = {};
        for (const [key, value] of data) {
            obj[key] = value;
        }
        sessionStorage.setItem(this.key, JSON.stringify(obj));
    }

    restore() {
        const saved = sessionStorage.getItem(this.key);
        if (!saved) return;

        try {
            const data = JSON.parse(saved);
            for (const [key, value] of Object.entries(data)) {
                const field = this.form.querySelector(`[name="${key}"]`);
                if (field) field.value = value;
            }
        } catch {
            sessionStorage.removeItem(this.key);
        }
    }

    clear() {
        sessionStorage.removeItem(this.key);
    }
}

new FormDraft("post-form");
```

### Case 3: Cookie Consent Banner

```javascript
class CookieConsent {
    constructor() {
        this.consentKey = "cookie-consent";
        this.banner = document.querySelector("#cookie-banner");
    }

    needsConsent() {
        return localStorage.getItem(this.consentKey) === null;
    }

    acceptAll() {
        localStorage.setItem(this.consentKey, JSON.stringify({
            necessary: true,
            analytics: true,
            marketing: true,
            timestamp: Date.now()
        }));
        this.banner.hidden = true;
        loadAnalytics();
    }

    acceptNecessary() {
        localStorage.setItem(this.consentKey, JSON.stringify({
            necessary: true,
            analytics: false,
            marketing: false,
            timestamp: Date.now()
        }));
        this.banner.hidden = true;
    }

    getPreferences() {
        const data = localStorage.getItem(this.consentKey);
        return data ? JSON.parse(data) : null;
    }

    init() {
        if (this.needsConsent()) {
            this.banner.hidden = false;
            document.querySelector("#accept-all")
                .addEventListener("click", () => this.acceptAll());
            document.querySelector("#accept-necessary")
                .addEventListener("click", () => this.acceptNecessary());
        }
    }
}
```

## Examples

### Example 1: Simple Cache with Expiration

```javascript
function setWithExpiry(key, value, ttlMinutes) {
    const item = {
        value: value,
        expiry: Date.now() + ttlMinutes * 60 * 1000
    };
    localStorage.setItem(key, JSON.stringify(item));
}

function getWithExpiry(key) {
    const raw = localStorage.getItem(key);
    if (!raw) return null;

    const item = JSON.parse(raw);
    if (Date.now() > item.expiry) {
        localStorage.removeItem(key);
        return null;
    }
    return item.value;
}
```

### Example 2: Visit Counter

```javascript
function getVisitCount() {
    let count = parseInt(localStorage.getItem("visitCount") || "0", 10);
    count++;
    localStorage.setItem("visitCount", count.toString());
    return count;
}

console.log(`You have visited ${getVisitCount()} times`);
```

### Example 3: Referrer Tracking with Session

```javascript
function trackReferrer() {
    const ref = document.referrer;
    if (ref && !sessionStorage.getItem("referrer")) {
        sessionStorage.setItem("referrer", ref);
    }
}
```

### Example 4: Shopping Cart (localStorage)

```javascript
class Cart {
    constructor() {
        this.key = "shopping-cart";
        this.items = this.load();
    }

    load() {
        const data = localStorage.getItem(this.key);
        return data ? JSON.parse(data) : [];
    }

    save() {
        localStorage.setItem(this.key, JSON.stringify(this.items));
    }

    add(product) {
        const existing = this.items.find(i => i.id === product.id);
        if (existing) {
            existing.quantity++;
        } else {
            this.items.push({ ...product, quantity: 1 });
        }
        this.save();
    }

    remove(productId) {
        this.items = this.items.filter(i => i.id !== productId);
        this.save();
    }

    clear() {
        this.items = [];
        this.save();
    }

    total() {
        return this.items.reduce((sum, i) => sum + i.price * i.quantity, 0);
    }
}
```

### Example 5: Cookie-Based A/B Testing

```javascript
function getExperimentVariant(experimentName) {
    const cookieName = `exp_${experimentName}`;
    let variant = getCookie(cookieName);

    if (!variant) {
        variant = Math.random() < 0.5 ? "control" : "treatment";
        document.cookie = `${cookieName}=${variant}; Path=/; Max-Age=2592000`; // 30 days
    }

    return variant;
}

const variant = getExperimentVariant("checkout-button-color");
if (variant === "treatment") {
    document.querySelector("#checkout-btn").classList.add("variant");
}
```

## Glossary

| Term | Definition |
|------|------------|
| localStorage | Persistent client-side key-value storage that survives browser restarts |
| sessionStorage | Tab-scoped storage that clears when the tab closes |
| Cookie | Small string stored by the browser, sent with every HTTP request |
| Origin | Protocol + host + port that defines the scope of Web Storage |
| QuotaExceededError | Exception thrown when storage capacity is exceeded |
| IndexedDB | Browser-based NoSQL database for complex client-side storage |
| HttpOnly | Cookie flag that prevents JavaScript access |
| SameSite | Cookie flag that controls cross-site request behavior |
| CSRF | Cross-Site Request Forgery — attack that makes authenticated requests from other sites |
| XSS | Cross-Site Scripting — injecting malicious scripts into a page |
| Serialization | Converting data to a storable format (e.g., JSON.stringify) |
| Max-Age | Cookie lifetime in seconds from the moment it is set |
| Secure | Cookie flag that restricts transmission to HTTPS only |

## Quick References

- [MDN: Web Storage API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API) — complete storage reference
- [MDN: Document.cookie](https://developer.mozilla.org/en-US/docs/Web/API/Document/cookie) — cookie API details
- [MDN: IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API) — browser database
- [MDN: SameSite cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie/SameSite)
- [MDN: Storage Quota](https://developer.mozilla.org/en-US/docs/Web/API/StorageManager/estimate)

## Next Steps

- [Fetch API & HTTP](fetch-api.md) — combining storage with network requests
- [ES6+ Features](es6-plus.md) — modern JavaScript syntax for better data handling
- [Debugging with DevTools](debugging-devtools.md) — inspecting storage in the browser
