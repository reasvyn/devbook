# Tables

## Description

HTML tables organize data into rows and columns, enabling clear presentation of structured information. Properly constructed tables are accessible to screen readers, sortable by users, and responsive across devices.

## Prerequisites

- [Elements, Tags & Attributes](elements-and-attributes.md) — fundamental HTML building blocks
- [Text Content: Headings, Paragraphs, Lists, Quotes & Code](text-content.md) — basic content elements

## Table of Contents

- [Table Structure](#table-structure)
- [Table Headers](#table-headers)
- [Spanning Rows and Columns](#spanning-rows-and-columns)
- [Caption](#caption)
- [Column Groups](#column-groups)
- [Table Sections](#table-sections)
- [Table Accessibility](#table-accessibility)
- [Table Styling](#table-styling)
- [Sorting Tables](#sorting-tables)
- [Responsive Tables](#responsive-tables)
- [Data Table vs Layout Table](#data-table-vs-layout-table)
- [Common Table Patterns](#common-table-patterns)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Table Structure

A table is defined by the `<table>` element and built row by row:

```html
<table>
    <tr>
        <td>Cell 1</td>
        <td>Cell 2</td>
        <td>Cell 3</td>
    </tr>
    <tr>
        <td>Cell 4</td>
        <td>Cell 5</td>
        <td>Cell 6</td>
    </tr>
</table>
```

| Cell 1 | Cell 2 | Cell 3 |
|---|---|---|
| Cell 4 | Cell 5 | Cell 6 |

- `<tr>` — table row
- `<td>` — table data (cell)

Each row must have the same number of cells for the table to display correctly (unless using colspan or rowspan).

### Table Headers

The `<th>` element defines header cells. Headers give meaning to columns or rows and are read aloud by screen readers:

```html
<table>
    <tr>
        <th>Name</th>
        <th>Age</th>
        <th>City</th>
    </tr>
    <tr>
        <td>Alice</td>
        <td>28</td>
        <td>New York</td>
    </tr>
    <tr>
        <td>Bob</td>
        <td>35</td>
        <td>London</td>
    </tr>
</table>
```

| Name | Age | City |
|---|---|---|
| Alice | 28 | New York |
| Bob | 35 | London |

The `scope` attribute clarifies which cells a header applies to:

```html
<th scope="col">Name</th>   <!-- Header for a column -->
<th scope="row">Alice</th>  <!-- Header for a row -->
```

| `scope` value | Meaning |
|---|---|
| `col` | Header applies to the column below |
| `row` | Header applies to the row to the right |
| `colgroup` | Header applies to a group of columns |
| `rowgroup` | Header applies to a group of rows |

Always use `scope` with `<th>` to ensure screen readers correctly associate headers with data cells. Without `scope`, screen readers assume column headers for `<th>` elements in the first row, but explicit scoping eliminates ambiguity.

### Spanning Rows and Columns

The `colspan` and `rowspan` attributes make a cell span multiple columns or rows:

```html
<table>
    <tr>
        <th colspan="2">Full Name</th>
        <th>Age</th>
    </tr>
    <tr>
        <td>Alice</td>
        <td>Johnson</td>
        <td>28</td>
    </tr>
    <tr>
        <td colspan="3">Bob Williams, 35</td>
    </tr>
</table>
```

- `colspan="2"` — cell takes two columns
- `rowspan="3"` — cell takes three rows

When using `colspan` or `rowspan`, adjust the number of cells in affected rows. A table with 3 columns and a `colspan="2"` on the first cell should have 2 cells in that row instead of 3.

```html
<!-- Incorrect: 4 cells in row 1, but row 2 has 4 cells too -->
<table>
    <tr>
        <td colspan="2">A</td>   <!-- Takes columns 1-2 -->
        <td>B</td>               <!-- Column 3 -->
        <td>C</td>               <!-- Column 4 -->
    </tr>
</table>

<!-- Correct: colspan="2" counts as 2 cells, so row has 3 logical cells -->
<table>
    <tr>
        <td colspan="2">A</td>   <!-- Takes columns 1-2 -->
        <td>B</td>               <!-- Column 3 -->
    </tr>
</table>
```

### Caption

The `<caption>` element provides a title or summary for the entire table:

```html
<table>
    <caption>Employee salaries for fiscal year 2026</caption>
    <tr>
        <th>Name</th>
        <th>Salary</th>
    </tr>
    <tr>
        <td>Alice</td>
        <td>$85,000</td>
    </tr>
</table>
```

The caption must be the first child of `<table>`. It is displayed above the table by default but can be positioned with CSS caption-side.

A caption should be concise but descriptive enough for a user to understand the table without reading every cell. Avoid captions like "Table 1" — instead, describe what the table contains.

### Column Groups

The `<colgroup>` and `<col>` elements define column properties:

```html
<table>
    <colgroup>
        <col style="background: #f0f0f0;">
        <col span="2" style="background: #e0e0e0;">
    </colgroup>
    <tr>
        <th>Name</th>
        <th>Age</th>
        <th>City</th>
    </tr>
    <!-- ... -->
</table>
```

- `<colgroup>` wraps the column definitions
- `<col>` defines a single column
- `span` — number of columns the `<col>` element covers

Properties set on `<col>` apply to every cell in that column. This is useful for column-specific styling, alignment, or width.

`<colgroup>` can also be used with `span` on `<colgroup>` directly:

```html
<colgroup span="2" style="background: #f0f0f0;">
<colgroup style="background: #e0e0e0;">
```

This creates two column groups: the first spans 2 columns, the second spans 1 column.

### Table Sections

Tables can be divided into `<thead>`, `<tbody>`, and `<tfoot>` sections:

```html
<table>
    <thead>
        <tr>
            <th>Product</th>
            <th>Q1</th>
            <th>Q2</th>
            <th>Q3</th>
            <th>Q4</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Widget A</td>
            <td>$10K</td>
            <td>$12K</td>
            <td>$15K</td>
            <td>$18K</td>
        </tr>
        <tr>
            <td>Widget B</td>
            <td>$8K</td>
            <td>$9K</td>
            <td>$11K</td>
            <td>$14K</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <td>Total</td>
            <td>$18K</td>
            <td>$21K</td>
            <td>$26K</td>
            <td>$32K</td>
        </tr>
    </tfoot>
</table>
```

- `<thead>` — header rows (one per table). Appears at the top of every printed page when the table spans multiple pages
- `<tbody>` — body rows. A table can have multiple `<tbody>` sections to group related rows
- `<tfoot>` — footer rows. Appears at the bottom of every printed page

These sections enable:

- **Scrolling** — scroll the `<tbody>` independently while the `<thead>` stays fixed
- **Printing** — header and footer repeat on each printed page
- **Grouping** — split data into logical groups within one table

When using these sections, the `<thead>` and `<tfoot>` must appear before `<tbody>` in source order, even though they display at the top and bottom.

### Table Accessibility

Tables present serious accessibility challenges when poorly constructed. Screen readers navigate tables cell by cell, announcing the associated header for each cell. If the markup is incorrect, the data becomes incomprehensible.

**Use `<th>` for all headers.** Data cells should use `<th>` with `scope`:

```html
<th scope="col">Name</th>
<th scope="row">Alice</th>
```

**Associate complex headers with `headers` attribute.** For tables with multiple levels of headers, use the `id` and `headers` attributes:

```html
<table>
    <thead>
        <tr>
            <th rowspan="2" id="name">Name</th>
            <th colspan="2" id="contact">Contact</th>
        </tr>
        <tr>
            <th id="email" headers="contact">Email</th>
            <th id="phone" headers="contact">Phone</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td headers="name">Alice</td>
            <td headers="contact email">alice@example.com</td>
            <td headers="contact phone">555-0101</td>
        </tr>
    </tbody>
</table>
```

The `headers` attribute lists the `id` values of all relevant `<th>` elements. This is verbose but necessary for complex tables with merged cells and multiple header levels.

**Use `<caption>` for a brief summary.** The caption provides context for the entire table.

**Use `scope` over `headers` when possible.** The `scope` attribute is simpler and supported by all screen readers. Only use `headers` for complex tables with multiple header levels.

**Avoid empty rows and columns.** Empty cells should contain a placeholder like `—` or `N/A` so screen readers announce something meaningful.

**Provide a summary for complex tables.** Add `aria-describedby` on the `<table>` element:

```html
<table aria-describedby="table-desc">
    <!-- ... -->
</table>
<p id="table-desc">This table shows quarterly sales by product for 2026.</p>
```

**Do not use tables for layout.** Using `<table>` for page layout (positioning content in rows and columns) is an antiquated practice that breaks screen reader navigation. Use CSS layout (Flexbox, Grid) instead.

### Table Styling

Tables require explicit CSS to look good — browsers render them without borders by default.

**Borders:**

```css
table, th, td {
    border: 1px solid #ccc;
    border-collapse: collapse;
}
```

The `border-collapse: collapse` property merges adjacent borders into a single border. Without it, cells have separate borders that double up between cells.

**Striped rows:**

```css
tbody tr:nth-child(even) {
    background: #f9f9f9;
}
```

**Hover effect:**

```css
tbody tr:hover {
    background: #e0f0ff;
}
```

**Text alignment:** left for text columns, right for numeric columns.

**Fixed headers with scrollable body:**

```css
thead, tbody {
    display: block;
}

tbody {
    max-height: 400px;
    overflow-y: auto;
}

thead tr, tbody tr {
    display: table;
    width: 100%;
    table-layout: fixed;
}
```

This pattern keeps the header visible while the body scrolls.

### Sorting Tables

JavaScript is required for client-side sorting, but HTML provides the semantic foundation. The typical approach:

1. Add `data-sort-type` attributes to `<th>` elements
2. Handle click events on `<th>` elements
3. Sort the rows in `<tbody>` using `Array.sort()`
4. Re-append the sorted rows

Example sortable table structure:

```html
<table id="sortable-table">
    <thead>
        <tr>
            <th data-sort-type="text" aria-sort="none">Name</th>
            <th data-sort-type="number" aria-sort="none">Age</th>
            <th data-sort-type="number" aria-sort="none">Salary</th>
        </tr>
    </thead>
    <tbody>
        <tr><td>Alice</td><td>28</td><td>$85,000</td></tr>
        <tr><td>Bob</td><td>35</td><td>$92,000</td></tr>
        <tr><td>Charlie</td><td>42</td><td>$105,000</td></tr>
    </tbody>
</table>
```

The `aria-sort` attribute communicates the current sort state to screen readers. Its values are:
- `none` — no sort applied
- `ascending` — sorted ascending
- `descending` — sorted descending

### Responsive Tables

Wide tables with many columns are problematic on narrow screens. Several strategies exist:

**Horizontal scroll:**

```html
<div style="overflow-x: auto;">
    <table>
        <!-- ... -->
    </table>
</div>
```

Wrap the table in a container with `overflow-x: auto`. This lets the user scroll the table horizontally. It is the simplest and most reliable approach.

**Stacked rows on small screens (CSS-only):**

```css
@media (max-width: 600px) {
    table, thead, tbody, tr, th, td {
        display: block;
    }

    thead tr {
        /* Hide the header row visually but keep it for screen readers */
        position: absolute;
        top: -9999px;
        left: -9999px;
    }

    td {
        position: relative;
        padding-left: 50%;
    }

    td::before {
        content: attr(data-label);
        position: absolute;
        left: 10px;
        font-weight: bold;
    }
}
```

This requires `data-label` attributes on each `<td>`:

```html
<tr>
    <td data-label="Name">Alice</td>
    <td data-label="Age">28</td>
    <td data-label="City">New York</td>
</tr>
```

**Prioritize columns:** Show only the most important columns on small screens and include a toggle to expand the full table.

**Use shorter labels:** Abbreviate column headers in the base markup and consider using CSS `text-overflow: ellipsis` for long content.

**Avoid very wide fixed-width columns.** Design tables so they can flex when space is limited.

### Data Table vs Layout Table

**Data tables** present structured data — the kind of content that belongs in a spreadsheet:

- Pricing plans with features
- Schedules and timetables
- Comparison matrices
- Financial reports
- Specifications

**Layout tables** used `<table>` for page layout before CSS was mature:

```html
<!-- DO NOT DO THIS -->
<table>
    <tr>
        <td id="sidebar">Navigation</td>
        <td id="main">Content</td>
    </tr>
</table>
```

Layout tables are harmful because:

- Screen readers announce "table with N rows and M columns" and navigate cell by cell
- They are not responsive without hacky CSS
- They add unnecessary markup complexity
- They break on print, screen readers, and text-only browsers

Use CSS Grid or Flexbox for layout. Use HTML tables only for tabular data.

### Common Table Patterns

**Pricing table (3 columns):**

```html
<table>
    <caption>Service pricing plans</caption>
    <thead>
        <tr>
            <th scope="col">Feature</th>
            <th scope="col">Basic</th>
            <th scope="col">Pro</th>
            <th scope="col">Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <th scope="row">Users</th>
            <td>1</td>
            <td>10</td>
            <td>Unlimited</td>
        </tr>
        <tr>
            <th scope="row">Storage</th>
            <td>5 GB</td>
            <td>50 GB</td>
            <td>1 TB</td>
        </tr>
        <tr>
            <th scope="row">Support</th>
            <td>Email</td>
            <td>Email + Chat</td>
            <td>24/7 Phone</td>
        </tr>
    </tbody>
</table>
```

**Schedule with merged cells:**

```html
<table>
    <caption>Conference schedule</caption>
    <thead>
        <tr>
            <th scope="col">Time</th>
            <th scope="col">Room A</th>
            <th scope="col">Room B</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <th scope="row">09:00-10:00</th>
            <td>Keynote</td>
            <td>Workshop: HTML Basics</td>
        </tr>
        <tr>
            <th scope="row">10:00-11:00</th>
            <td>CSS Layouts</td>
            <td rowspan="2">JavaScript Deep Dive</td>
        </tr>
        <tr>
            <th scope="row">11:00-12:00</th>
            <td>Accessible Forms</td>
        </tr>
    </tbody>
</table>
```

## Glossary

| Term | Definition |
|------|------------|
| `<table>` | The root element for tabular data |
| `<tr>` | Table row |
| `<td>` | Table data cell |
| `<th>` | Table header cell |
| `<caption>` | Title or summary of the table |
| `<thead>` | Header section, repeats on each printed page |
| `<tbody>` | Body section, can appear multiple times per table |
| `<tfoot>` | Footer section, repeats on each printed page |
| `<colgroup>` | Container for column definitions |
| `<col>` | Defines properties for one or more columns |
| `colspan` | Number of columns a cell spans |
| `rowspan` | Number of rows a cell spans |
| `scope` | Attribute indicating which cells a header applies to (col, row, colgroup, rowgroup) |
| `headers` | Attribute listing header cell IDs that apply to a data cell |
| `data-label` | Custom attribute used in responsive table CSS patterns |
| `border-collapse` | CSS property that merges adjacent cell borders |
| `aria-sort` | ARIA attribute indicating sort state (none, ascending, descending) |
| Layout table | A table used for visual layout (antipattern) |
| Data table | A table presenting structured data (correct use) |

## Quick References

- [MDN: Table Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/table) — complete reference
- [MDN: `<th>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/th) — header cell reference
- [MDN: Table Accessibility](https://developer.mozilla.org/en-US/docs/Learn/HTML/Tables/Advanced) — accessible tables guide
- [HTML Spec: Table Model](https://html.spec.whatwg.org/multipage/tables.html) — official specification
- [WAI Tutorials: Table Concepts](https://www.w3.org/WAI/tutorials/tables/) — W3C accessibility guide
- [CSS-Tricks: Responsive Table Patterns](https://css-tricks.com/responsive-table-layouts/) — practical responsive solutions

## Next Steps

- [Form Structure & Elements](form-basics.md) — user input collection
- [Semantic HTML & Document Outline](semantic-html.md) — structural meaning in markup
- [HTML & ARIA](aria-basics.md) — accessible rich internet applications
