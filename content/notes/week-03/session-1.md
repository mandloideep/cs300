---
title: "CSS Flexbox"
weight: 1
---

## Introduction

**Flexbox** (Flexible Box Layout) is a CSS layout system designed to make it easy to arrange elements in rows or columns. Before Flexbox, creating layouts required using floats, positioning tricks, and complex CSS hacks. Flexbox solves common layout problems elegantly.

### What Problems Does Flexbox Solve?

- **Vertical centering** - Centering an element vertically used to be surprisingly difficult
- **Equal height columns** - Making sibling elements the same height regardless of content
- **Flexible spacing** - Distributing space evenly between items
- **Responsive layouts** - Elements that adapt to available space without media queries

---

## The Flex Model

Flexbox works with two types of elements:

### Flex Container

The parent element with `display: flex` applied. It controls how its children are laid out.

### Flex Items

The direct children of a flex container. These are the elements being arranged.

```html
<div class="container">
  <!-- Flex Container -->
  <div class="item">1</div>
  <!-- Flex Item -->
  <div class="item">2</div>
  <!-- Flex Item -->
  <div class="item">3</div>
  <!-- Flex Item -->
</div>
```

```css
.container {
  display: flex; /* This makes the div a flex container */
}
```

{{< callout type="important" >}}
Only **direct children** become flex items. Grandchildren and deeper nested elements are not affected by the container's flex properties.
{{< /callout >}}

---

## Understanding Axes

Flexbox operates along two axes. This is the **most important concept** to understand.

### Main Axis

The primary axis along which flex items are placed. By default, this runs **horizontally** (left to right).

### Cross Axis

The axis perpendicular to the main axis. By default, this runs **vertically** (top to bottom).

```
Default axes (flex-direction: row):

         Main Axis →
    ┌─────────────────────────┐
    │  ┌───┐  ┌───┐  ┌───┐   │  ↓
    │  │ 1 │  │ 2 │  │ 3 │   │  Cross
    │  └───┘  └───┘  └───┘   │  Axis
    └─────────────────────────┘
```

{{< callout type="info" >}}
**Memory Trick:** "**J**ustify works on the **M**ain axis (both have letters that go below the baseline: j, y, g). **A**lign works on the **C**ross axis."

Or simply: **J**ustify = horizontal (like text justification), **A**lign = vertical
{{< /callout >}}

---

## Flex Container Properties

These properties are applied to the parent (flex container).

### display: flex

Creates a flex container and enables flexbox layout.

```css
.container {
  display: flex;
}
```

### flex-direction

Sets the direction of the main axis.

| Value            | Description             |
| ---------------- | ----------------------- |
| `row`            | Left to right (default) |
| `row-reverse`    | Right to left           |
| `column`         | Top to bottom           |
| `column-reverse` | Bottom to top           |

```css
.container {
  display: flex;
  flex-direction: column; /* Items stack vertically */
}
```

```
flex-direction: row          flex-direction: column
┌──────────────────┐         ┌──────────────────┐
│ ┌──┐ ┌──┐ ┌──┐  │         │ ┌──────────────┐ │
│ │1 │ │2 │ │3 │  │         │ │      1       │ │
│ └──┘ └──┘ └──┘  │         │ └──────────────┘ │
└──────────────────┘         │ ┌──────────────┐ │
                             │ │      2       │ │
                             │ └──────────────┘ │
                             │ ┌──────────────┐ │
                             │ │      3       │ │
                             │ └──────────────┘ │
                             └──────────────────┘
```

### justify-content

Controls how items are distributed along the **main axis**.

| Value           | Description                                  |
| --------------- | -------------------------------------------- |
| `flex-start`    | Items packed at the start (default)          |
| `flex-end`      | Items packed at the end                      |
| `center`        | Items centered                               |
| `space-between` | Equal space between items, no space at edges |
| `space-around`  | Equal space around each item                 |
| `space-evenly`  | Truly equal space everywhere                 |

```css
.container {
  display: flex;
  justify-content: space-between;
}
```

```
justify-content: flex-start    justify-content: center
┌──────────────────────┐       ┌──────────────────────┐
│┌──┐┌──┐┌──┐         │       │     ┌──┐┌──┐┌──┐    │
│└──┘└──┘└──┘         │       │     └──┘└──┘└──┘    │
└──────────────────────┘       └──────────────────────┘

justify-content: space-between justify-content: space-evenly
┌──────────────────────┐       ┌──────────────────────┐
│┌──┐     ┌──┐    ┌──┐│       │  ┌──┐  ┌──┐  ┌──┐  │
│└──┘     └──┘    └──┘│       │  └──┘  └──┘  └──┘  │
└──────────────────────┘       └──────────────────────┘
```

### align-items

Controls how items are aligned along the **cross axis**.

| Value        | Description                               |
| ------------ | ----------------------------------------- |
| `stretch`    | Items stretch to fill container (default) |
| `flex-start` | Items aligned to the start of cross axis  |
| `flex-end`   | Items aligned to the end of cross axis    |
| `center`     | Items centered on cross axis              |
| `baseline`   | Items aligned by their text baseline      |

```css
.container {
  display: flex;
  align-items: center; /* Vertically centers items */
}
```

```
align-items: flex-start       align-items: center
┌──────────────────────┐      ┌──────────────────────┐
│┌──┐ ┌────┐ ┌──┐     │      │                      │
│└──┘ │    │ └──┘     │      │┌──┐ ┌────┐ ┌──┐     │
│     │    │          │      │└──┘ │    │ └──┘     │
│     └────┘          │      │     └────┘          │
└──────────────────────┘      └──────────────────────┘
```

### flex-wrap

Controls whether items wrap to new lines when they don't fit.

| Value          | Description                          |
| -------------- | ------------------------------------ |
| `nowrap`       | All items stay on one line (default) |
| `wrap`         | Items wrap to new lines              |
| `wrap-reverse` | Items wrap in reverse order          |

```css
.container {
  display: flex;
  flex-wrap: wrap; /* Items wrap when container is too narrow */
}
```

### gap

Sets spacing between flex items. This is the **recommended way** to add space between items (better than using margins).

```css
.container {
  display: flex;
  gap: 20px; /* Same gap in both directions */
  /* Or specify separately: */
  row-gap: 10px; /* Vertical gap */
  column-gap: 20px; /* Horizontal gap */
}
```

{{< callout type="important" >}}
**Why use gap instead of margins?**

- Gap only creates space _between_ items, not around them
- Cleaner code - no need for `:first-child` or `:last-child` exceptions
- Works consistently in both directions
  {{< /callout >}}

---

## Flex Item Properties

These properties are applied to the children (flex items).

### flex-grow

Determines how much an item should grow relative to other items when there's extra space.

```css
.item {
  flex-grow: 1; /* Item will grow to fill available space */
}
```

```
All items flex-grow: 0 (default)    All items flex-grow: 1
┌────────────────────────────┐      ┌────────────────────────────┐
│┌──┐ ┌──┐ ┌──┐              │      │┌────────┐┌────────┐┌──────┐│
│└──┘ └──┘ └──┘              │      │└────────┘└────────┘└──────┘│
└────────────────────────────┘      └────────────────────────────┘

Item 2 has flex-grow: 2, others have flex-grow: 1
┌────────────────────────────┐
│┌─────┐┌──────────┐┌──────┐│
│└─────┘└──────────┘└──────┘│
└────────────────────────────┘
  (1)      (2x)       (1)
```

### flex-shrink

Determines how much an item should shrink relative to other items when there isn't enough space.

```css
.item {
  flex-shrink: 0; /* Item will not shrink */
}
```

- Default value is `1` (items shrink equally)
- Set to `0` to prevent an item from shrinking

### flex-basis

Sets the initial size of an item before growing or shrinking.

```css
.item {
  flex-basis: 200px; /* Start at 200px, then grow/shrink */
}
```

- Similar to `width` but specific to flexbox
- Use `auto` to use the item's content size

### The flex Shorthand

Combines `flex-grow`, `flex-shrink`, and `flex-basis` into one property.

```css
.item {
  flex: 1; /* flex-grow: 1, flex-shrink: 1, flex-basis: 0% */
  flex: 0 0 200px; /* Don't grow, don't shrink, start at 200px */
  flex: 1 0 auto; /* Grow, don't shrink, use content size */
}
```

**Common values:**

- `flex: 1` - Item grows to fill space, commonly used
- `flex: 0 0 auto` - Item stays at its content size
- `flex: 0 0 200px` - Item stays fixed at 200px

### order

Changes the visual order of items without changing the HTML.

```css
.item-3 {
  order: -1; /* Moves this item to the beginning */
}
```

- Default is `0`
- Lower numbers appear first
- Useful for responsive layouts

### align-self

Overrides the container's `align-items` for a specific item.

```css
.special-item {
  align-self: flex-end; /* This item aligns to the bottom */
}
```

---

## Common Patterns

### Centering an Element (Horizontally and Vertically)

The classic problem that Flexbox makes trivial:

```css
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh; /* Full viewport height */
}
```

### Navigation Bar

```html
<nav class="navbar">
  <div class="logo">Logo</div>
  <ul class="nav-links">
    <li><a href="#">Home</a></li>
    <li><a href="#">About</a></li>
    <li><a href="#">Contact</a></li>
  </ul>
</nav>
```

```css
.navbar {
  display: flex;
  justify-content: space-between; /* Logo on left, links on right */
  align-items: center;
  padding: 1rem 2rem;
}

.nav-links {
  display: flex;
  gap: 2rem;
  list-style: none;
}
```

### Card Row with Equal Heights

```css
.card-container {
  display: flex;
  gap: 20px;
}

.card {
  flex: 1; /* Cards grow equally */
  display: flex;
  flex-direction: column;
}

.card-content {
  flex: 1; /* Content area grows to fill card */
}
```

### Sticky Footer Layout

```css
body {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

main {
  flex: 1; /* Main content grows to push footer down */
}

footer {
  /* Footer stays at bottom */
}
```

---

## Troubleshooting & Common Mistakes

{{% details title="Items not centering?" %}}

- Check that the container has a defined height
- Make sure you're using both `justify-content` and `align-items`
  {{% /details %}}

{{% details title="Items overflowing the container?" %}}

- Add `flex-wrap: wrap` to allow wrapping
- Or set `flex-shrink: 1` on items to let them shrink
  {{% /details %}}

{{% details title="Confusing justify-content with align-items?" %}}

- **justify-content** = main axis (horizontal by default)
- **align-items** = cross axis (vertical by default)
- Remember: `flex-direction` changes which axis is which!
  {{% /details %}}

{{% details title="Gap not working?" %}}

- Make sure the container has `display: flex`
- Check browser support (gap in Flexbox is widely supported in modern browsers)
  {{% /details %}}

{{% details title="Item not respecting width?" %}}

- Flex items can shrink by default
- Use `flex-shrink: 0` to prevent shrinking
- Or use `flex: 0 0 [width]` as a shorthand
  {{% /details %}}

---

## Key Takeaways

{{< callout emoji="🎯" >}}

- Flexbox is **one-dimensional** - it handles either rows OR columns (use Grid for two-dimensional layouts)
- The **main axis** is determined by `flex-direction`
- `justify-content` = main axis, `align-items` = cross axis
- Use **gap** instead of margins for spacing between items
- The `flex` shorthand (`flex: 1`) is commonly used for items that should grow
- Flexbox is ideal for **component-level layouts** (navbars, cards, form elements)
  {{< /callout >}}

---

## Additional Resources

### Official Documentation

{{< cards >}}
{{< card link="https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout" title="MDN: CSS Flexible Box Layout" subtitle="Comprehensive reference" icon="book-open" >}}
{{< card link="https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Flexbox" title="MDN: Flexbox" subtitle="Beginner-friendly guide" icon="academic-cap" >}}
{{< /cards >}}

### Interactive Learning

{{< cards >}}
{{< card link="https://flexboxfroggy.com/" title="Flexbox Froggy" subtitle="Learn Flexbox through a fun game" icon="play" >}}
{{< card link="http://www.flexboxdefense.com/" title="Flexbox Defense" subtitle="Tower defense game using Flexbox" icon="shield-check" >}}
{{< /cards >}}

### Visual References

{{< cards >}}
{{< card link="https://css-tricks.com/snippets/css/a-guide-to-flexbox/" title="CSS-Tricks: Guide to Flexbox" subtitle="Excellent visual reference" icon="eye" >}}
{{< card link="https://yoksel.github.io/flex-cheatsheet/" title="Flexbox Cheatsheet" subtitle="Interactive property explorer" icon="document-text" >}}
{{< /cards >}}

### Tools

- Use your browser's DevTools (F12) - the Flexbox inspector helps visualize the layout
