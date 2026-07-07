# History of HTML

## Description

The story of HTML is the story of the web itself — from a humble document-sharing system at a Swiss physics lab to the most widely used markup language on Earth. Understanding this history helps developers appreciate why HTML works the way it does and where it is headed.

## Prerequisites

- [What Is HTML?](what-is-html.md) — basic understanding of HTML concepts

## Table of Contents

- [Before HTML: The Hypertext Dream](#before-html-the-hypertext-dream)
- [1990–1991: Birth at CERN](#19901991-birth-at-cern)
- [1993–1995: The First Browsers and HTML 2.0](#19931995-the-first-browsers-and-html-20)
- [1995–1999: The Browser Wars](#19951999-the-browser-wars)
- [HTML 3.2 and 4.01](#html-32-and-401)
- [2000–2004: XHTML and the XML Dream](#20002004-xhtml-and-the-xml-dream)
- [2004: The WHATWG Split](#2004-the-whatwg-split)
- [2004–2014: The HTML5 Era](#20042014-the-html5-era)
- [HTML5 in Practice](#html5-in-practice)
- [2014–2019: From Snapshot to Living Standard](#20142019-from-snapshot-to-living-standard)
- [2019–Present: The Living Standard](#2019present-the-living-standard)
- [Key Figures in HTML History](#key-figures-in-html-history)
- [Timeline Summary](#timeline-summary)
- [Lessons from History](#lessons-from-history)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Before HTML: The Hypertext Dream

HTML did not emerge from a vacuum. The concept of hypertext — linked documents that readers could navigate non-linearly — had been theorized for decades before the web existed.

**1945: Vannevar Bush and the Memex**

Vannevar Bush, an American engineer and science administrator, published a seminal article titled "As We May Think" in *The Atlantic Monthly*. He proposed the **Memex** (Memory Extender) — a hypothetical device that would store all of a person's books, records, and communications, and allow them to be accessed with "associative indexing." A user could create trails of linked information, much like modern hyperlinks. The Memex was never built, but it planted the seed of linked information systems.

**1960s: Ted Nelson and Project Xanadu**

Ted Nelson coined the terms "hypertext" and "hypermedia" in 1963. He envisioned **Project Xanadu**, a global hypertext publishing system where documents were connected by bidirectional links, users could see the original context of any quotation, and micropayments compensated authors automatically. Xanadu was ambitious to the point of impracticality — it was never completed — but Nelson's ideas deeply influenced later hypertext systems.

**1968: Douglas Engelbart and the Mother of All Demos**

Douglas Engelbart demonstrated a system called **NLS** (oNLine System) at the Fall Joint Computer Conference in San Francisco. In a 90-minute presentation, he showed: hypertext links, the computer mouse, video conferencing, collaborative editing, and a graphical user interface. Every major concept in personal computing was demonstrated that day. Engelbart's system used hypertext to organize information hierarchically and link related documents.

**1980s: HyperCard and SGML**

In 1987, Apple released **HyperCard** — a hypertext system for the Macintosh that let users create "stacks" of cards linked together. It was accessible to non-programmers and inspired a generation of developers. HyperCard's influence can be seen in modern web concepts like linking and interactive content.

Meanwhile, the publishing industry had developed **SGML** (Standard Generalized Markup Language, ISO 8879:1986) — a metalanguage for defining markup languages. SGML was powerful but complex. The full specification was over 500 pages. Tim Berners-Lee would later borrow SGML's syntax (angle-bracketed tags) for HTML, while dramatically simplifying it.

Other hypertext systems of the 1980s also contributed ideas that shaped HTML's design:

**Intermedia (1985).** Developed at Brown University's Institute for Research in Information and Scholarship, Intermedia was a hypertext system for education that ran on Unix workstations. It introduced bidirectional links — each link stored both its source and destination, allowing users to see all documents that referenced the current one. This concept of transclusion — showing content from one document embedded in another — later influenced the `<iframe>` element and modern content syndication patterns like RSS and embed APIs.

**NoteCards (1985).** Developed by Frank Halasz at Xerox PARC, NoteCards organized information into semantic units called "cards" connected by typed links. Each link could describe its relationship to the target (e.g., "supports", "contradicts", "example-of"). This typed-link model influenced the semantic web movement and the development of microformats, RDFa, and JSON-LD — standards that add machine-readable meaning to HTML documents.

**Guide (1986).** Created at the University of Kent, Guide was one of the first commercial hypertext systems for personal computers. It introduced "replacement links" — clicking a link would expand the linked content inline rather than navigating away from the current document. This interaction pattern reappeared in HTML5 with the `<details>` and `<summary>` elements, which provide native disclosure widgets without JavaScript.

**HyperTIES (1987).** Developed at the University of Maryland, HyperTIES pioneered "embedded menus" — clickable links placed directly within running text rather than in separate navigation panels. This design directly influenced HTML's anchor element: in HTML, a link is an inline element that can appear within a paragraph, not a separate navigation structure. Every `<a>` tag embedded in prose today traces its lineage to HyperTIES.

**The Common Ground.** What united these systems — and what HTML would inherit — was the principle that hypertext should be authorable, not just readable. HyperCard succeeded partly because non-programmers could create stacks with its scripting language, HyperTalk. Guide succeeded because writers could author documents without learning complex syntax. HTML's simplicity — a plain text file with angle brackets — made it the first hypertext system that anyone could use, on any computer, without purchasing specialized software.

### 1990–1991: Birth at CERN

In 1989, Tim Berners-Lee was working as a software engineer at CERN, the European Organization for Nuclear Research in Geneva, Switzerland. CERN had thousands of scientists from hundreds of institutions worldwide, all sharing data through incompatible systems. Berners-Lee saw the problem clearly: researchers could not easily share information across different computers, operating systems, and networks.

He proposed a solution in March 1989: a "hypertext project" that would allow information to be linked across the internet. His manager wrote "vague but exciting" on the proposal and gave him the go-ahead.

By the end of 1990, Berners-Lee had built all the components necessary for a working web:

- **HTML** — the markup language for creating documents
- **HTTP** — the protocol for transferring hypertext over the internet
- **URI/URL** — the addressing system for identifying resources
- **The first web server** — a NeXTcube running the httpd daemon
- **The first web browser** — called WorldWideWeb (later renamed Nexus), which was also a WYSIWYG editor

The first web page was published on August 6, 1991. It described what the World Wide Web was and how to set up a web server. Berners-Lee also posted a summary of the project to the alt.hypertext Usenet newsgroup, marking the first public announcement of the web.

The original HTML had a tiny vocabulary: headings (`<h1>` through `<h6>`), paragraphs (`<p>`), lists (`<ul>`, `<ol>`, `<li>`), links (`<a>`), and emphasis (`<em>`, `<strong>`). There were no images, no tables, no forms, no stylesheets. The web was purely about hypertext documents.

Berners-Lee and his colleague Robert Cailliau spent 1991 and 1992 evangelizing the web at conferences across Europe and the United States, demonstrating how hypertext could connect research documents across the internet. They carried a NeXTcube to conferences, connected it to the CERN server via telephone lines, and showed live hypertext navigation. The reception was enthusiastic but adoption was slow — the web was still a niche tool for high-energy physicists.

Most internet users at the time relied on other protocols: **Gopher** for hierarchical document browsing, **FTP** for file transfer, **Usenet** for threaded discussions, and **email** for direct communication. Gopher, developed at the University of Minnesota, was particularly popular because it presented a clean, menu-driven interface to distributed documents. The web's advantage — hypertext links embedded in documents rather than nested menu hierarchies — was not immediately obvious to users who had never experienced non-linear navigation.

The first web page at CERN also served as a directory of other web servers. Berners-Lee maintained this list by hand as new servers appeared. By the end of 1992, the list contained only about 50 entries — all at universities and research laboratories. The breakthrough that would bring the web to millions of users came the following year, when a graphical browser made hypertext intuitive and visually appealing.

### 1993–1995: The First Browsers and HTML 2.0

The web grew slowly at first — there were only about 50 web servers by the end of 1992. The turning point came with graphical browsers.

**1993: NCSA Mosaic**

Marc Andreessen and Eric Bina at the National Center for Supercomputing Applications (NCSA) created **Mosaic**, the first browser to display images inline with text (earlier browsers opened images in separate windows). Mosaic was also easier to install and use than its predecessors. It became an overnight sensation, with millions of downloads.

Mosaic introduced the `<img>` tag — a controversial addition at the time, since HTML was supposed to be about hypertext, not embedded media. The `<img>` tag won because users loved it.

**1994: Netscape Navigator**

Andreessen co-founded Netscape Communications and released **Netscape Navigator** in October 1994. It was faster, more polished, and more feature-rich than Mosaic. Netscape quickly became the dominant browser, commanding over 90% of the market within two years.

Netscape introduced proprietary HTML extensions: `<font>` for text styling, `<center>` for alignment, `<blink>` for animated text (universally hated), and `<table>` for layout (originally intended for tabular data). These extensions worked only in Netscape, forcing developers to choose: write for Netscape and reach the most users, or write standard HTML and reach everyone.

**1995: HTML 2.0**

The Internet Engineering Task Force (IETF) published **RFC 1866: HTML 2.0** in November 1995. This was the first standardized HTML specification. It codified the existing feature set: headings, paragraphs, lists, links, images, and basic forms. HTML 2.0 was modest — it documented what browsers already implemented rather than introducing new features — but it was a crucial step toward interoperability.

### 1995–1999: The Browser Wars

Microsoft recognized the web's growing importance and rushed to compete. In August 1995, Microsoft released **Internet Explorer 1.0**, bundled with Windows 95 Plus! Pack. It was a rebranded version of Spyglass Mosaic, and it was terrible.

**Internet Explorer 3.0 (1996)** changed everything. It was genuinely competitive with Netscape, supporting HTML tables, frames, and — most importantly — CSS and a scripting language (JScript, Microsoft's reverse-engineered JavaScript clone). IE3 was a credible alternative.

**Internet Explorer 4.0 (1997)** pushed further. Microsoft bet the company on the web, investing hundreds of millions of dollars. IE4 was bundled free with Windows, while Netscape still charged $49 for Navigator. This pricing asymmetry, combined with Microsoft's OS monopoly, began eroding Netscape's market share.

The browser wars created chaos for web developers. Both Netscape and Microsoft added competing, incompatible features:

| Feature | Netscape | Internet Explorer |
|---|---|---|
| Scripting | JavaScript | JScript |
| Dynamic HTML | Layers (via `<layer>`) | Dynamic HTML (via `document.all`) |
| Style sheets | JavaScript Style Sheets (JSSS) | CSS (partial) |
| Font embedding | Dynamic fonts (TrueDoc) | Embedded OpenType (EOT) |
| Frames | Supported | Supported (different behavior) |

Developers wrote two versions of every page or used browser detection to serve different code. The `&lt;!-- browser sniffing --&gt;` pattern became standard practice, and the web became fragmented.

### HTML 3.2 and 4.01

**HTML 3.2 (1997)** was published by the World Wide Web Consortium (W3C), which Tim Berners-Lee had founded in 1994 to steer web standards. HTML 3.2 was a pragmatic compromise — it incorporated widely-used browser extensions (tables, applets, text flow around images) while ignoring proprietary features that lacked broad support.

**HTML 4.0 (1997)** was a major leap forward. It introduced:

- **CSS integration** — the `style` attribute, `<link rel="stylesheet">`, and the `<style>` element
- **Scripting** — the `<script>` element for embedding and linking JavaScript
- **Accessibility** — the `lang` attribute, `<label>` for form controls, `<fieldset>` and `<legend>` for grouping
- **Internationalization** — the `dir` attribute for text direction (ltr/rtl)
- **Improved forms** — new form controls, the `<fieldset>` element, disabled/readonly attributes
- **Object embedding** — the `<object>` element (replacing the non-standard `<embed>` and `<applet>`)

**HTML 4.01 (1999)** was a minor revision that corrected errors in HTML 4.0. It became the definitive HTML standard for the next fifteen years. HTML 4.01 was stable, well-understood, and supported by every browser.

But the W3C had already moved on to a more ambitious project: XHTML.

### 2000–2004: XHTML and the XML Dream

In January 2000, the W3C published **XHTML 1.0** — a reformulation of HTML 4.01 as an XML application. The idea was elegant: take the familiar HTML vocabulary and make it conform to XML's stricter rules.

XHTML required:

- All tags must be closed (`<br/>` instead of `<br>`)
- All attributes must be quoted (`<div class="main">` not `<div class=main>`)
- All elements must be properly nested (`<b><i>text</i></b>` not `<b><i>text</b></i>`)
- Element and attribute names must be lowercase (`<Div>` was invalid)
- The document must be well-formed XML

The promise was seductive: XHTML documents could be parsed by XML tools, combined with other XML vocabularies (MathML, SVG), and processed with standard XML pipelines. Browsers could use standard XML parsers instead of the complex, error-prone HTML parsers.

In practice, XHTML failed for several reasons:

**XHTML served as text/html was a lie.** Most web servers served XHTML with a `Content-Type: text/html` header, which told browsers to parse it with the lenient HTML parser. The strict XML parsing rules were never applied. This meant XHTML documents with syntax errors were silently treated as broken HTML, defeating the purpose.

**XHTML served as application/xhtml+xml was impractical.** When served with the correct XML content type, Internet Explorer (still the dominant browser) refused to render it. Users saw a download dialog instead of a web page. Other browsers would show a cryptic XML parsing error for the smallest mistake — an unescaped ampersand in a URL would crash the entire page.

**The complexity was not worth the benefit.** Web developers had to remember to close `<br>` tags and quote attributes, but the payoff — XML tooling integration — was irrelevant to most of them. HTML 4.01 worked fine, and the strictness of XHTML made the web less forgiving, not better.

The XHTML experiment taught the web community an important lesson: strict technical purity is less valuable than practical interoperability.

### 2004: The WHATWG Split

By 2004, the W3C had abandoned HTML entirely. It was working on **XHTML 2.0**, which was not backward compatible with existing HTML. XHTML 2.0 removed familiar elements, introduced new ones, and broke from web reality.

A group of browser vendors — Apple, Mozilla, and Opera — protested that the W3C was ignoring the needs of web developers. When their proposal for evolving HTML was rejected by the W3C, they formed their own working group: the **Web Hypertext Application Technology Working Group (WHATWG)**.

The WHATWG's mission was pragmatic: improve HTML in ways that mattered to developers and users, without breaking the existing web. They worked on two specifications:

- **Web Applications 1.0** — which became HTML5
- **Web Forms 2.0** — which became the enhanced form features in HTML5

The split was a pivotal moment. The official standards body (W3C) was working on a pure, elegant, but irrelevant specification (XHTML 2.0). The renegade group (WHATWG) was working on a messy, practical, backward-compatible specification (HTML5) that reflected what browsers actually did.

### 2004–2014: The HTML5 Era

The WHATWG published the first HTML5 draft in 2008. It was a radically different approach to standardization:

**Backward compatibility above all.** Every valid HTML 4.01 document must also be a valid HTML5 document. Nothing breaks.

**Define browser behavior.** HTML5 specified exactly how browsers should parse malformed HTML (the "parse algorithm"). For the first time, all browsers could produce the same DOM tree from the same HTML, even if the HTML was technically invalid.

**New semantic elements.** `<header>`, `<nav>`, `<article>`, `<section>`, `<aside>`, `<footer>`, `<main>`, `<figure>`, `<figcaption>` — elements that describe structure rather than appearance.

**Native multimedia.** `<video>` and `<audio>` elements eliminated the need for Flash and other plugin-based media players. The `<canvas>` element enabled scriptable 2D graphics.

**Improved forms.** New input types (`email`, `url`, `tel`, `number`, `date`, `range`, `color`, `search`), new attributes (`placeholder`, `required`, `autofocus`, `pattern`), and the `<datalist>` element for autocomplete.

**New APIs.** The HTML5 specification included JavaScript APIs: `document.querySelector()`, `classList`, `localStorage`, `sessionStorage`, `history.pushState()`, and the Drag and Drop API.

**The parser algorithm.** HTML5 defined a detailed, step-by-step algorithm for parsing HTML. This was unprecedented. Previous specifications described what HTML should look like (normative grammar) but not how browsers should handle errors. HTML5's parser algorithm ensured that "tag soup" — malformed HTML — would produce the same DOM in every browser.

Key milestones during the HTML5 era:

| Year | Event |
|---|---|
| 2004 | WHATWG formed by Apple, Mozilla, Opera |
| 2008 | First HTML5 working draft published |
| 2009 | W3C abandoned XHTML 2.0, adopted HTML5 |
| 2011 | HTML5 reached Last Call status |
| 2012 | HTML5 became a Candidate Recommendation |
| 2014 | HTML5 became a W3C Recommendation |

### HTML5 in Practice

The transition from HTML 4.01 to HTML5 was smooth because HTML5 was designed for it. Developers could start using HTML5 features immediately, without breaking existing pages:

```html
<!DOCTYPE html>
```

This simple doctype replaced three lengthy doctypes from HTML 4.01:

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN" "http://www.w3.org/TR/html4/frameset.dtd">
```

Any of these could be replaced with `<!DOCTYPE html>` and the page would work identically while triggering standards mode.

Similarly, the `<meta charset="utf-8">` element replaced the verbose:

```html
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
```

This incremental approach — small, backward-compatible improvements — was the hallmark of HTML5 and the reason for its rapid adoption.

### 2014–2019: From Snapshot to Living Standard

After HTML5 became a W3C Recommendation in 2014, the WHATWG and W3C disagreed on the maintenance model:

- **WHATWG** advocated for a "living standard" — a continuously updated specification that never had version numbers. As browsers implemented new features, the standard was updated incrementally.
- **W3C** preferred versioned snapshots — periodic "HTML 5.x" releases that gave enterprises a stable target.

In 2019, the two organizations reached an agreement: the W3C would adopt the WHATWG Living Standard as the official HTML specification. The W3C would publish "review drafts" as snapshots, but development happens in the WHATWG repository.

This agreement ended the fork. There is now one HTML specification, maintained by the WHATWG, with contributions from all major browser vendors (Google, Apple, Mozilla, Microsoft).

### 2019–Present: The Living Standard

The HTML Living Standard is updated continuously. New features are proposed, discussed, implemented in browsers, and added to the specification on an ongoing basis. There is no "HTML 6" release planned — the standard evolves feature by feature.

Recent additions to the living standard include:

**Lazy loading.** The `loading="lazy"` attribute on `<img>` and `<iframe>` defers loading off-screen resources until the user scrolls near them.

```html
<img src="huge-photo.jpg" loading="lazy" alt="A large photo">
```

**The `<dialog>` element.** Native modal dialog support without JavaScript libraries.

```html
<dialog id="my-dialog">
    <p>This is a native dialog</p>
    <button onclick="this.closest('dialog').close()">Close</button>
</dialog>
```

**The `inert` attribute.** Marks an element as non-interactive, removing it from the tab order and accessibility tree.

```html
<div inert>
    <p>This content cannot be focused or interacted with.</p>
</div>
```

**Entry and exit animations.** The `@starting-style` CSS rule and the `transition-behavior` property work with the HTML `popover` and `<dialog>` elements.

**The `popover` attribute.** Declarative popovers that require no JavaScript.

```html
<button popovertarget="my-popover">Open Popover</button>
<div id="my-popover" popover>
    <p>Popover content here</p>
</div>
```

The living standard model means HTML is never "done." It evolves to meet the needs of developers and users, one feature at a time.

### Key Figures in HTML History

**Tim Berners-Lee** (born 1955) — invented the World Wide Web, HTML, HTTP, and the first browser. Founded the W3C. Continues to advocate for an open, decentralized web.

**Marc Andreessen** (born 1971) — co-created Mosaic, the first popular graphical browser. Co-founded Netscape. Later became a venture capitalist.

**Håkon Wium Lie** (born 1965) — proposed CSS while at CERN. Worked at Opera and advocated for web standards.

**Brendan Eich** (born 1961) — created JavaScript in ten days in 1995. Co-founded Mozilla and the Firefox browser.

**Ian Hickson** (born 1978) — the primary editor of the HTML5 specification for the WHATWG. Also wrote the CSS specifications and the Web Storage specification.

**John Allsopp** — wrote "A Dao of Web Design" (2000), which argued that the web should embrace flexibility rather than trying to control it. The article influenced the philosophical direction of HTML5.

### Timeline Summary

| Year | Event |
|---|---|
| 1945 | Vannevar Bush proposes the Memex |
| 1963 | Ted Nelson coins "hypertext" |
| 1968 | Douglas Engelbart demonstrates NLS |
| 1989 | Tim Berners-Lee proposes the World Wide Web |
| 1990 | First web browser and server created |
| 1991 | First web page published |
| 1993 | NCSA Mosaic released |
| 1994 | Netscape Navigator released |
| 1995 | HTML 2.0 standardized; Internet Explorer released |
| 1996 | CSS 1 published; IE 3.0 with CSS support |
| 1997 | HTML 3.2 (W3C); IE 4.0 released |
| 1999 | HTML 4.01 published |
| 2000 | XHTML 1.0 published; "Dao of Web Design" |
| 2004 | WHATWG formed |
| 2008 | First HTML5 draft |
| 2009 | W3C abandons XHTML 2.0 |
| 2014 | HTML5 becomes W3C Recommendation |
| 2019 | W3C and WHATWG agree on Living Standard |

### Lessons from History

**Backward compatibility is sacred.** Every attempt to "fix" HTML by breaking backward compatibility (XHTML, XHTML 2.0) has failed. HTML5 succeeded because it did not break existing pages.

**Standards follow implementation, not the reverse.** HTML5 formalized what browsers already did. The parser algorithm documented existing browser behavior rather than prescribing new rules. This pragmatic approach ensured that the specification matched reality.

**The web platform is shaped by competition.** The browser wars produced innovation and chaos. The period of IE dominance (2000–2008) produced stagnation — IE6 did not receive a major update for five years. Competition drives progress.

**Community matters more than formal standards bodies.** The WHATWG, formed by frustrated browser vendors, produced the most significant HTML revision in decades. The W3C's more formal process produced XHTML 2.0, which was irrelevant. The lesson: the people who implement browsers should define the standard.

**The web is resilient.** HTML has survived terrible code, malicious input, fragmented browsers, and well-intentioned redesigns. The lenient parsing model — the thing that makes HTML "messy" — is also what makes it unbreakable.

### Study Cases

**Case 1: The `<blink>` Element — When a Joke Ships to Production**

In 1995, Netscape engineer Lou Montulli implemented the `<blink>` element in a single evening as a joke after colleagues joked that the Netscape logo should blink. It was never intended as a serious feature — it was a prank that escaped into production and was copied by other browsers to maintain compatibility. The `<blink>` tag became universally despised by designers and users for causing nausea and readability problems, yet browsers continue to support it decades later. This case illustrates the "once shipped, never removed" reality of the web platform: even a joke feature becomes permanent if it gains adoption.

**Case 2: The `<img>` Tag — User Demand Overrides Specifier Vision**

When Marc Andreessen added the `<img>` tag to Mosaic in 1993, it violated the vision of HTML as a pure hypertext medium. The W3C had not approved it, and purists argued that images should be linked (click to view) rather than embedded (displayed inline). But users overwhelmingly preferred inline images — they made pages visually engaging and eliminated an extra click to see every picture. Andreessen made the pragmatic choice to ship what users wanted. The `<img>` tag won through user demand, not standards-body approval, and it became one of the most important elements in HTML history.

**Case 3: The Browser Lock-In Strategy of 1997**

By 1997, Netscape Navigator held over 80% market share, but Netscape charged $49 per copy while Microsoft bundled Internet Explorer for free with Windows. Microsoft's strategy was not technical superiority — IE 3.0 was roughly comparable to Netscape 3.0 — but distribution leverage. Pre-installation meant that most new computer users never needed to seek out an alternative browser. By 2000, IE had over 90% market share, and Netscape had been acquired by AOL and effectively abandoned. This case demonstrates how browser market dynamics, not technical merit, determined the outcome of the first browser war, and why the web platform must remain neutral and multi-vendor.

**Case 4: XHTML — Why Purity Lost to Pragmatism**

XHTML 1.0 was technically superior to HTML 4.01 in several dimensions: it was well-formed, extensible via XML namespaces, and compatible with off-the-shelf XML parsers and tooling. Yet it failed because strictness punished authors for mistakes that had no practical consequence in browsers. The real web — pages generated by content management systems, template engines, e-commerce platforms, and human hands — is not well-formed. An unescaped ampersand in a URL would crash an entire XHTML page served with the correct content type. XHTML's failure teaches that a specification must match how people actually work, not how specification authors believe they should work.

### Examples

**The First Web Page (August 1991)**

Tim Berners-Lee's original index page demonstrated the core HTML elements in their earliest form:

```html
<head><title>The World Wide Web project</title></head>
<body>
<h1>World Wide Web</h1>
The WorldWideWeb (W3) is a wide-area
<a href="WhatIs.html">hypermedia</a> information retrieval
initiative aiming to give universal access to a large universe
of documents.<p>
Everything there is online about W3 is linked directly or
indirectly to this document, including the
<a href="Summary.html">executive summary</a> of the project.
</body>
```

Notice the absence of a doctype, the use of bare text outside any paragraph element, and the inconsistent tag casing — `<head>` but `</HEAD>` in the original. The parser algorithm did not exist yet, so behavior varied between browsers.

**HTML 4.01 Transitional (Late 1990s)**

A typical late-1990s page combined semantic structure with presentational markup:

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
    "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type"
    content="text/html; charset=iso-8859-1">
<title>Welcome</title>
</head>
<body>
<font face="Arial" size="4" color="#333">Welcome</font><br><br>
<table width="100%" cellpadding="10">
<tr><td bgcolor="#eee">Navigation</td>
<td>Content area</td></tr>
</table>
</body>
</html>
```

The transitional doctype allowed deprecated presentational elements like `<font>` and `<center>`, while the table-based layout was borrowed from its original purpose (tabular data) and repurposed for page structure — a pattern that would not be fully replaced by CSS until the 2010s.

**HTML5 Modern (Current Era)**

A contemporary page is cleaner, semantic, and separates concerns:

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Welcome</title>
<link rel="stylesheet" href="styles.css">
</head>
<body>
<nav aria-label="Main navigation">
    <ul>
        <li><a href="/">Home</a></li>
        <li><a href="/about">About</a></li>
    </ul>
</nav>
<main>
    <h1>Welcome</h1>
    <p>Content goes here.</p>
</main>
</body>
</html>
```

The evolution from the first example to the last shows HTML's trajectory: from loose, presentation-focused, browser-dependent markup to structured, semantic, accessible, and interoperable code — without ever breaking backward compatibility with the first page published in 1991.

## Glossary

| Term | Definition |
|------|------------|
| Hypertext | Text that contains links to other documents, enabling non-linear navigation |
| SGML | Standard Generalized Markup Language — the complex metalanguage that inspired HTML's syntax |
| W3C | World Wide Web Consortium — founded by Tim Berners-Lee to develop web standards |
| WHATWG | Web Hypertext Application Technology Working Group — maintains the HTML Living Standard |
| XHTML | A reformulation of HTML as an XML application (2000) |
| HTML5 | The major revision of HTML that added semantic elements, multimedia, and APIs |
| Living Standard | A continuously updated specification with no version numbers |
| Browser wars | The period (1995-1999) when Netscape and Microsoft competed for browser dominance |
| Mosaic | The first popular graphical web browser (1993) |
| Netscape Navigator | The dominant browser of the mid-1990s |
| Internet Explorer | Microsoft's browser series that won the first browser wars |
| Memex | Vannevar Bush's hypothetical hypertext system (1945) |
| NLS | oNLine System — Douglas Engelbart's hypertext system (1968) |
| Project Xanadu | Ted Nelson's ambitious but unfinished global hypertext project |
| Parser algorithm | HTML5's defined method for handling malformed HTML deterministically |
| Backward compatibility | The ability of new specifications to work with existing content |
| Doctype | A declaration that tells the browser which rendering mode to use |
| Standards mode | Browser rendering mode that follows W3C specifications |
| Quirks mode | Browser rendering mode that emulates old browser bugs |
| Feature detection | Checking whether a browser supports a feature before using it |

## Quick References

- [A Brief History of HTML by MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/History_of_HTML) — concise official history
- [The HTML Living Standard](https://html.spec.whatwg.org/) — current specification maintained by WHATWG
- [W3C HTML History](https://www.w3.org/History.html) — historical documents from the early web
- [Tim Berners-Lee's Original Proposal](https://www.w3.org/History/1989/proposal.html) — the 1989 document that started it all
- [The Dao of Web Design by John Allsopp](https://alistapart.com/article/dao/) — influential article on web philosophy
- [Web Design: The First 100 Years](https://idlewords.com/talks/web_design_first_100_years.htm) — talk by Maciej Ceglowski on the web's evolution

## Next Steps

- [HTML: Introduction](index.md) — all introductory materials
- [What Is HTML?](what-is-html.md) — core concepts and syntax
- [HTML Philosophy](html-philosophy.md) — the design principles that shape HTML
- [HTML Elements & Attributes](../elements-and-attributes.md) — deep dive into elements
- [Semantic HTML](../semantic-html.md) — using HTML meaningfully
