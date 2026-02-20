---
title: "CSS Grid"
weight: 2
toc: true
---

## Introduction

**CSS Grid** is a two-dimensional layout system that lets you create complex layouts with rows AND columns simultaneously. While Flexbox excels at one-dimensional layouts (a row or a column), Grid is designed for laying out entire pages or complex UI sections.

### What Problems Does Grid Solve?

- **Two-dimensional layouts** - Control both rows and columns at once
- **Page structure** - Create headers, sidebars, main content, and footers easily
- **Responsive galleries** - Cards that automatically adjust to fill available space
- **Complex alignments** - Items that span multiple rows or columns

---

## Grid vs Flexbox

Understanding when to use each is crucial:

{{< tabs items="Comparison,When to Use Flexbox,When to Use Grid" >}}
  {{< tab >}}
  | Flexbox | Grid |
  |---------|------|
  | **One-dimensional** (row OR column) | **Two-dimensional** (rows AND columns) |
  | **Content-driven** - items determine layout | **Layout-driven** - you define the structure |
  | Best for **components** (navbars, cards) | Best for **page layouts** (overall structure) |
  | Items flow and wrap naturally | Items placed in defined cells |
  {{< /tab >}}
  {{< tab >}}
  **Use Flexbox for:**
  - Navigation menus
  - Button groups
  - Card content layout
  - Centering elements
  - Components where items should flow naturally
  {{< /tab >}}
  {{< tab >}}
  **Use Grid for:**
  - Page layouts (header, sidebar, main, footer)
  - Image galleries
  - Card grids
  - Any layout that needs precise row AND column control
  {{< /tab >}}
{{< /tabs >}}

### Analogy
- **Flexbox** = Arranging books on a shelf (one dimension)
- **Grid** = Arranging furniture in a room (two dimensions)

{{< callout type="info" >}}
**Pro tip:** You can (and should!) use both together - Grid for the page layout, Flexbox for components within the grid cells.
{{< /callout >}}

---

## Creating a Grid Container

### display: grid

Creates a grid container and enables grid layout.

```css
.container {
  display: grid;
}
```

### grid-template-columns

Defines the number and size of columns.

```css
.container {
  display: grid;
  grid-template-columns: 200px 200px 200px; /* Three 200px columns */
}
```

```
grid-template-columns: 200px 200px 200px

┌──────────┬──────────┬──────────┐
│  200px   │  200px   │  200px   │
│          │          │          │
└──────────┴──────────┴──────────┘
```

### grid-template-rows

Defines the number and size of rows.

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 100px 200px; /* First row 100px, second row 200px */
}
```

### gap

Sets spacing between grid cells (same as in Flexbox).

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 20px;           /* 20px gap between all cells */
  /* Or specify separately: */
  row-gap: 10px;
  column-gap: 20px;
}
```

---

## The fr Unit

The `fr` (fraction) unit represents a **fraction of the available space**. It's similar to `flex-grow` in Flexbox.

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr; /* Three equal columns */
}
```

```
grid-template-columns: 1fr 1fr 1fr (container width: 900px)

┌──────────┬──────────┬──────────┐
│  300px   │  300px   │  300px   │
│   (1fr)  │   (1fr)  │   (1fr)  │
└──────────┴──────────┴──────────┘
```

### Mixing fr with fixed units

```css
.container {
  display: grid;
  grid-template-columns: 250px 1fr 1fr; /* Fixed sidebar, two flexible columns */
}
```

```
grid-template-columns: 250px 1fr 1fr (container width: 1000px)

┌──────────┬────────────┬────────────┐
│  250px   │   375px    │   375px    │
│ (fixed)  │   (1fr)    │   (1fr)    │
└──────────┴────────────┴────────────┘
               remaining space split equally
```

### Common pattern: Sidebar layout

```css
.page-layout {
  display: grid;
  grid-template-columns: 250px 1fr; /* Fixed sidebar, flexible main content */
}
```

---

## The repeat() Function

Simplifies defining multiple columns or rows of the same size.

```css
/* Instead of: */
grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr;

/* Write: */
grid-template-columns: repeat(6, 1fr); /* Six equal columns */
```

### Syntax
```css
repeat(number-of-times, size)
```

### Examples

```css
grid-template-columns: repeat(3, 1fr);        /* Three equal columns */
grid-template-columns: repeat(4, 200px);      /* Four 200px columns */
grid-template-columns: repeat(3, 1fr 2fr);    /* Pattern: 1fr, 2fr, 1fr, 2fr, 1fr, 2fr */
```

---

## Understanding Grid Lines

This is the **most confusing concept** in Grid - but essential to understand.

{{< callout type="warning" >}}
**Grid lines are numbered starting at 1, not 0!**

Grid lines are the **lines between cells**, not the cells themselves.
{{< /callout >}}

```
        Line 1    Line 2    Line 3    Line 4
           ↓         ↓         ↓         ↓
           │         │         │         │
     ──────┼─────────┼─────────┼─────────┼────── Line 1
           │  Cell   │  Cell   │  Cell   │
           │   1     │   2     │   3     │
     ──────┼─────────┼─────────┼─────────┼────── Line 2
           │  Cell   │  Cell   │  Cell   │
           │   4     │   5     │   6     │
     ──────┼─────────┼─────────┼─────────┼────── Line 3
```

{{< callout type="important" >}}
**Key insight:** A grid with **3 columns** has **4 vertical lines**.
{{< /callout >}}

### Why lines, not cells?
Grid lines let you specify where items start and end, allowing items to span multiple cells.

---

## Positioning Grid Items

### grid-column

Specifies which column lines an item spans.

```css
.item {
  grid-column: 1 / 3; /* Start at line 1, end at line 3 (spans 2 columns) */
}
```

```
grid-column: 1 / 3

Line 1    Line 2    Line 3
   │         │         │
   │◄────────┼────────►│
   │    Item spans     │
   │    columns 1-2    │
```

### grid-row

Specifies which row lines an item spans.

```css
.item {
  grid-row: 1 / 3; /* Start at line 1, end at line 3 (spans 2 rows) */
}
```

### Using span

Instead of specifying end lines, use `span` to say how many cells to cover:

```css
.item {
  grid-column: 1 / span 2; /* Start at line 1, span 2 columns */
  /* Same as: grid-column: 1 / 3; */
}
```

### grid-area (Shorthand)

Combines row and column positioning:

```css
.item {
  grid-area: 1 / 1 / 3 / 4;
  /* row-start / column-start / row-end / column-end */
}
```

### Example: Header spanning full width

```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}

.header {
  grid-column: 1 / -1; /* -1 means "last line" */
}
```

```
┌─────────────────────────────────┐
│           Header                │ ← spans all columns (1 / -1)
├──────────┬──────────┬──────────┤
│   Item   │   Item   │   Item   │
└──────────┴──────────┴──────────┘
```

---

## Advanced Grid Features

### minmax()

Sets minimum and maximum sizes for tracks.

```css
.container {
  display: grid;
  grid-template-columns: minmax(200px, 1fr) 2fr;
  /* First column: at least 200px, but can grow */
}
```

### grid-auto-rows

Sets the size of automatically created rows (when items overflow the defined rows).

```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-auto-rows: 200px; /* Any new rows will be 200px tall */
}
```

With `minmax` for flexible heights:

```css
.container {
  grid-auto-rows: minmax(150px, auto); /* At least 150px, grows with content */
}
```

### auto-fit and auto-fill

Create responsive grids without media queries!

**auto-fit:** Columns stretch to fill available space
**auto-fill:** Creates as many columns as fit, even if empty

{{< callout type="important" >}}
**The "Magic Formula" for Responsive Grids:**

```css
.container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
}
```

This is powerful because:
- Each card is at least 200px wide
- Cards grow to fill available space
- Cards automatically wrap to new rows when needed
- **No media queries required!**
{{< /callout >}}

```
Wide screen:
┌────────┬────────┬────────┬────────┐
│  Card  │  Card  │  Card  │  Card  │
└────────┴────────┴────────┴────────┘

Narrow screen (same CSS):
┌────────────┬────────────┐
│    Card    │    Card    │
├────────────┼────────────┤
│    Card    │    Card    │
└────────────┴────────────┘
```

---

## Grid Template Areas

Define your layout visually using named areas (optional but very readable).

```css
.container {
  display: grid;
  grid-template-columns: 200px 1fr;
  grid-template-rows: auto 1fr auto;
  grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
}

.header  { grid-area: header; }
.sidebar { grid-area: sidebar; }
.main    { grid-area: main; }
.footer  { grid-area: footer; }
```

```
┌────────────────────────────┐
│          header            │
├─────────┬──────────────────┤
│         │                  │
│ sidebar │      main        │
│         │                  │
├─────────┴──────────────────┤
│          footer            │
└────────────────────────────┘
```

{{< callout type="info" >}}
This is very readable - you can literally see the layout structure in your CSS!
{{< /callout >}}

---

## Combining Grid and Flexbox

Use them together for the best results:
- **Grid** for the overall page structure
- **Flexbox** for components within grid cells

### Example: Netflix-style page layout

```css
/* Page layout with Grid */
.page {
  display: grid;
  grid-template-rows: auto 1fr auto; /* Header, main, footer */
  min-height: 100vh;
}

/* Navigation with Flexbox */
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

/* Movie cards grid */
.movie-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 20px;
}

/* Individual card with Flexbox */
.movie-card {
  display: flex;
  flex-direction: column;
}
```

---

## Common Patterns

### Basic Page Layout

```css
.page {
  display: grid;
  grid-template-columns: 250px 1fr;
  grid-template-rows: auto 1fr auto;
  min-height: 100vh;
}

.header {
  grid-column: 1 / -1; /* Span full width */
}

.footer {
  grid-column: 1 / -1; /* Span full width */
}
```

### Responsive Card Gallery

```css
.gallery {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1.5rem;
}
```

### Holy Grail Layout (Header, Footer, Sidebar, Main)

```css
.layout {
  display: grid;
  grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
  grid-template-columns: 250px 1fr;
  grid-template-rows: auto 1fr auto;
  min-height: 100vh;
}
```

### Sticky Footer (Content pushes footer down)

```css
.page {
  display: grid;
  grid-template-rows: auto 1fr auto; /* Header, main (grows), footer */
  min-height: 100vh;
}
```

---

## Troubleshooting & Common Mistakes

{{% details title="Items not spanning correctly?" closed="true" %}}
- Remember: grid lines are numbered starting at **1**, not 0
- A 3-column grid has **4** vertical lines
- Use `-1` to refer to the last line
{{% /details %}}

{{% details title="Grid items overlapping?" closed="true" %}}
- Check your `grid-column` and `grid-row` values
- Make sure items aren't assigned to the same cells
{{% /details %}}

{{% details title="Grid not taking full height?" closed="true" %}}
- Add `min-height: 100vh` to the grid container
- Use `1fr` for the main content row
{{% /details %}}

{{% details title="Responsive grid not working?" closed="true" %}}
- Use `auto-fit` with `minmax()` for automatic responsiveness
- Example: `repeat(auto-fit, minmax(200px, 1fr))`
{{% /details %}}

{{% details title="Want to debug?" closed="true" %}}
- Open browser DevTools (F12)
- Click on your grid container in the Elements panel
- Look for the "grid" badge - click it to show grid lines
- Chrome and Firefox have excellent Grid inspectors
{{% /details %}}

---

## Key Takeaways

{{< callout emoji="🎯" >}}
- Grid is **two-dimensional** - controls rows AND columns simultaneously
- Grid is **layout-first** - define the structure, then place items
- The `fr` unit is like `flex-grow` - distributes available space
- Grid **lines** are numbered 1, 2, 3... (not 0-indexed)
- `repeat(auto-fit, minmax(200px, 1fr))` is the magic formula for responsive grids
- Use **Grid for page layouts**, **Flexbox for components**
- You can (and should) use Grid and Flexbox together
- Browser DevTools have excellent Grid debugging tools
{{< /callout >}}

---

## Additional Resources

### Official Documentation

{{< cards >}}
  {{< card link="https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout" title="MDN: CSS Grid Layout" subtitle="Comprehensive reference" icon="book-open" >}}
  {{< card link="https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout/Basic_Concepts_of_Grid_Layout" title="MDN: Basic Concepts" subtitle="Beginner guide" icon="academic-cap" >}}
{{< /cards >}}

### Interactive Learning

{{< cards >}}
  {{< card link="https://cssgridgarden.com/" title="Grid Garden" subtitle="Learn Grid through a fun game" icon="play" >}}
  {{< card link="https://cssgrid-generator.netlify.app/" title="CSS Grid Generator" subtitle="Visual tool to generate Grid code" icon="code" >}}
{{< /cards >}}

### Visual References

{{< cards >}}
  {{< card link="https://css-tricks.com/snippets/css/complete-guide-grid/" title="CSS-Tricks: Guide to Grid" subtitle="Excellent visual reference" icon="eye" >}}
  {{< card link="https://gridbyexample.com/" title="Grid by Example" subtitle="Patterns and examples by Rachel Andrew" icon="collection" >}}
{{< /cards >}}

### Tools

- **Browser DevTools Grid Inspector** - Built into Chrome and Firefox
- [Firefox Grid Inspector](https://developer.mozilla.org/en-US/docs/Tools/Page_Inspector/How_to/Examine_grid_layouts) - Particularly powerful for debugging

### Video Resources

{{< cards >}}
  {{< card link="https://www.youtube.com/watch?v=uuOXPWCh-6o" title="CSS Grid in 100 Seconds" subtitle="Quick overview by Fireship" icon="play" >}}
  {{< card link="https://learncssgrid.com/" title="Learn CSS Grid" subtitle="Free interactive course" icon="academic-cap" >}}
{{< /cards >}}
