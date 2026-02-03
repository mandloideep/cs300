---
title: "CSS Basics & Selectors"
weight: 1
---

## Introduction

**CSS** (Cascading Style Sheets) controls how HTML elements look. While HTML defines the structure and content of a page, CSS defines the presentation - colors, fonts, spacing, layout, and more.

### Why CSS?

Before CSS existed (1996), styling was done with HTML attributes like `<font color="red">`. This was:

- Messy - style mixed with content
- Repetitive - same styles repeated everywhere
- Hard to maintain - changing a color meant editing every page

CSS separates **content** (HTML) from **presentation** (CSS), making websites easier to build and maintain.

### What CSS Can Do

- Colors and backgrounds
- Fonts and text styling
- Spacing (margins, padding)
- Layouts (positioning, flexbox, grid)
- Animations and transitions
- Responsive designs (different styles for different screen sizes)

---

## CSS Syntax

Every CSS rule follows this pattern:

```css
selector {
  property: value;
}
```

### Breaking It Down

```css
p {
  color: blue;
  font-size: 16px;
}
```

| Part                  | What It Is                            | Example                             |
| --------------------- | ------------------------------------- | ----------------------------------- |
| **Selector**          | What to style                         | `p`                                 |
| **Property**          | What aspect to change                 | `color`                             |
| **Value**             | What to change it to                  | `blue`                              |
| **Declaration**       | Property + value pair                 | `color: blue;`                      |
| **Declaration block** | Everything inside `{ }`               | `{ color: blue; font-size: 16px; }` |
| **Rule**              | Complete selector + declaration block | The entire example above            |

### Syntax Rules

- Properties and values are separated by a colon `:`
- Each declaration ends with a semicolon `;`
- Multiple declarations go inside curly braces `{ }`
- Whitespace doesn't matter (but proper formatting helps readability)

### Comments

```css
/* This is a CSS comment */

p {
  color: blue; /* This makes text blue */
}
```

---

## Three Ways to Add CSS

{{< tabs items="Inline CSS,Internal CSS,External CSS (Recommended)" >}}
{{< tab >}}
CSS written directly on an element using the `style` attribute:

```html
<p style="color: blue; font-size: 18px;">This is blue text.</p>
```

**Pros:**

- Quick for testing
- Highest specificity (overrides other styles)

**Cons:**

- Mixes content with presentation
- Can't reuse styles
- Hard to maintain

**Use for:** Quick debugging, HTML emails, one-off overrides
{{< /tab >}}
{{< tab >}}
CSS written in a `<style>` tag in the document's `<head>`:

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      p {
        color: blue;
        font-size: 18px;
      }
    </style>
  </head>
  <body>
    <p>This is blue text.</p>
  </body>
</html>
```

**Pros:**

- Styles stay with the HTML file
- Good for single-page sites or prototypes

**Cons:**

- Styles can't be shared between pages
- Mixes concerns (structure and style in same file)

**Use for:** Single-page sites, prototyping, specific page overrides
{{< /tab >}}
{{< tab >}}
CSS written in a separate `.css` file and linked to HTML:

**styles.css:**

```css
p {
  color: blue;
  font-size: 18px;
}
```

**index.html:**

```html
<!DOCTYPE html>
<html>
  <head>
    <link
      rel="stylesheet"
      href="styles.css"
    />
  </head>
  <body>
    <p>This is blue text.</p>
  </body>
</html>
```

**Pros:**

- Complete separation of concerns
- One file styles multiple pages
- Browser caches the CSS file (faster loading)
- Easier to maintain

**Cons:**

- Extra HTTP request (minor)

**Use for:** All real projects - this is the professional standard
{{< /tab >}}
{{< /tabs >}}

### The `<link>` Tag

```html
<link
  rel="stylesheet"
  href="styles.css"
/>
```

- `rel="stylesheet"` - Tells browser this is a stylesheet
- `href="styles.css"` - Path to the CSS file
- Goes in the `<head>` section
- Common file names: `styles.css`, `style.css`, `main.css`

---

## CSS Selectors

Selectors determine which HTML elements your CSS rules apply to.

### Element Selector

Selects all elements of a specific type:

```css
p {
  color: gray;
}

h1 {
  font-size: 32px;
}
```

This styles **all** `<p>` and `<h1>` elements on the page.

**Use for:** Default styles that apply to all elements of that type

### Class Selector

Selects elements with a specific class attribute. Uses a dot (`.`) prefix:

```css
.highlight {
  background-color: yellow;
}

.large-text {
  font-size: 24px;
}
```

```html
<p class="highlight">This has a yellow background.</p>
<p class="large-text">This is larger text.</p>
<p class="highlight large-text">This has both styles!</p>
```

**Key points:**

- Class names start with `.` in CSS (not in HTML)
- One element can have multiple classes (space-separated)
- Same class can be used on multiple elements
- Use lowercase with hyphens: `my-class` not `myClass`

**Use for:** Reusable styles applied to multiple elements

### ID Selector

Selects the element with a specific ID. Uses a hash (`#`) prefix:

```css
#main-title {
  color: navy;
  font-size: 48px;
}
```

```html
<h1 id="main-title">Welcome to My Site</h1>
```

**Key points:**

- ID names start with `#` in CSS (not in HTML)
- **Each ID should be unique** - only one element per ID on a page
- Higher specificity than classes

**Use for:** Unique elements, JavaScript hooks, anchor links

### Class vs ID: When to Use Each

| Class                     | ID                            |
| ------------------------- | ----------------------------- |
| Reusable                  | Unique                        |
| Multiple elements         | One element per page          |
| Lower specificity         | Higher specificity            |
| **Preferred for styling** | Better for JavaScript/anchors |

{{< callout type="important" >}}
**Best Practice:** Use classes for styling, IDs for JavaScript and anchor links.
{{< /callout >}}

### Selector Comparison

| Selector Type | CSS Syntax   | HTML Syntax         | Example                  |
| ------------- | ------------ | ------------------- | ------------------------ |
| Element       | `p`          | `<p>`               | Style all paragraphs     |
| Class         | `.classname` | `class="classname"` | Style specific elements  |
| ID            | `#idname`    | `id="idname"`       | Style one unique element |

---

## Combining Selectors

### Grouping (Comma)

Apply the same styles to multiple selectors:

```css
h1,
h2,
h3 {
  color: navy;
  font-family: Georgia, serif;
}
```

This is equivalent to writing three separate rules.

### Descendant Selector (Space)

Select elements that are inside other elements:

```css
nav a {
  color: white;
  text-decoration: none;
}
```

This selects `<a>` elements that are **inside** `<nav>` elements (at any depth).

```html
<nav>
  <a href="/">Home</a>
  <!-- Styled -->
  <a href="/about">About</a>
  <!-- Styled -->
</nav>

<a href="/other">Other</a>
<!-- NOT styled -->
```

### Combining Class and Element

```css
p.intro {
  font-size: 20px;
}
```

This selects `<p>` elements that ALSO have the class `intro`.

```html
<p class="intro">This is styled.</p>
<!-- Styled -->
<p>This is not.</p>
<!-- NOT styled -->
<div class="intro">Nor this.</div>
<!-- NOT styled (not a p) -->
```

---

## The Cascade

CSS stands for **Cascading** Style Sheets. "Cascade" refers to how styles flow down and combine, with some styles overriding others.

### Where Styles Come From (Priority Order)

1. **Browser defaults** - Built-in styles (lowest priority)
2. **External stylesheets** - Your `.css` files
3. **Internal styles** - `<style>` tag in HTML
4. **Inline styles** - `style=""` attribute (highest priority)

Later sources override earlier sources.

### When Styles Conflict

```css
p {
  color: blue;
}

p {
  color: red;
}
```

The text will be **red** because the second rule comes later.

---

## Specificity

When multiple rules target the same element, **specificity** determines which wins. More specific selectors override less specific ones.

### Specificity Hierarchy

From lowest to highest:

1. **Element selectors** (`p`, `h1`, `div`)
2. **Class selectors** (`.highlight`, `.nav-link`)
3. **ID selectors** (`#main-title`, `#header`)
4. **Inline styles** (`style=""` attribute)

{{< callout type="info" >}}
**A Simple Mental Model**

| Selector Type | "Points" |
| ------------- | -------- |
| Element       | 1        |
| Class         | 10       |
| ID            | 100      |
| Inline        | 1000     |

Higher points win. (This is simplified but works for most cases.)
{{< /callout >}}

### Examples

```css
p {
  color: blue;
} /* 1 point */
.intro {
  color: green;
} /* 10 points - wins over element */
#main {
  color: red;
} /* 100 points - wins over class */
```

```html
<p
  class="intro"
  id="main"
>
  What color am I?
</p>
<!-- Answer: RED (ID has highest specificity) -->
```

### When Specificity Is Equal

If two rules have the same specificity, the **last one wins**:

```css
.highlight {
  color: yellow;
}
.highlight {
  color: orange;
} /* This one wins */
```

### Avoid `!important`

{{< callout type="warning" >}}

```css
p {
  color: blue !important; /* DON'T DO THIS */
}
```

`!important` overrides everything, but:

- Makes debugging difficult
- Creates specificity "wars"
- Hard to override later

**Only use as a last resort** (like overriding third-party styles).
{{< /callout >}}

---

## Basic CSS Properties

Here are some common properties to get started:

### Text Color

```css
p {
  color: blue;
  color: #0066cc;
  color: rgb(0, 102, 204);
}
```

### Background Color

```css
div {
  background-color: lightgray;
  background-color: #f0f0f0;
}
```

### Font Size

```css
p {
  font-size: 16px;
  font-size: 1.2em;
  font-size: 1rem;
}
```

For now, use `px` (pixels). We'll cover other units later.

### Font Family

```css
body {
  font-family: Arial, Helvetica, sans-serif;
}
```

Always provide fallbacks in case the first font isn't available.

### Text Alignment

```css
h1 {
  text-align: center;
}

p {
  text-align: left; /* default */
  text-align: right;
  text-align: justify;
}
```

---

## Common Mistakes

{{< callout type="error" >}}
**Using `=` instead of `:`**

```css
/* Wrong */
p {
  color = blue;
}

/* Correct */
p {
  color: blue;
}
```

{{< /callout >}}

{{< callout type="error" >}}
**Forgetting semicolons**

```css
/* Wrong - second property won't work */
p {
  color: blue
  font-size: 16px;
}

/* Correct */
p {
  color: blue;
  font-size: 16px;
}
```

{{< /callout >}}

{{< callout type="error" >}}
**Using dot/hash in HTML**

```html
<!-- Wrong -->
<p class=".highlight">Text</p>
<h1 id="#main">Title</h1>

<!-- Correct -->
<p class="highlight">Text</p>
<h1 id="main">Title</h1>
```

The dot (`.`) and hash (`#`) are **only used in CSS**, not in HTML attributes.
{{< /callout >}}

{{< callout type="error" >}}
**Misspelling property names**

```css
/* Wrong */
p {
  colour: blue; /* British spelling won't work */
  font-colour: blue; /* Made-up property */
}

/* Correct */
p {
  color: blue;
}
```

CSS properties use American English spellings.
{{< /callout >}}

---

## Key Takeaways

{{< callout emoji="🎯" >}}

- CSS controls **how things look**; HTML controls **what things are**
- Use **external stylesheets** for all real projects
- CSS syntax: `selector { property: value; }`
- Three selector types: **element** (`p`), **class** (`.classname`), **ID** (`#idname`)
- Use **classes for styling**, IDs for JavaScript/anchors
- **Specificity** determines which styles win: ID > Class > Element
- When specificity is equal, **last rule wins**
- Avoid `!important` - it makes CSS hard to maintain
  {{< /callout >}}

---

## Additional Resources

### Official Documentation

{{< cards >}}
{{< card link="https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/CSS_basics" title="MDN: CSS Basics" subtitle="Great starting point" icon="book-open" >}}
{{< card link="https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors" title="MDN: CSS Selectors" subtitle="Complete selector reference" icon="document-text" >}}
{{< /cards >}}

### Interactive Learning

{{< cards >}}
{{< card link="https://flukeout.github.io/" title="CSS Diner" subtitle="Learn selectors through a game" icon="play" >}}
{{< card link="https://www.freecodecamp.org/learn/2022/responsive-web-design/" title="freeCodeCamp CSS" subtitle="Free interactive course" icon="academic-cap" >}}
{{< /cards >}}

### References

{{< cards >}}
{{< card link="https://www.w3schools.com/css/" title="W3Schools CSS Tutorial" subtitle="Examples with try-it editor" icon="code" >}}
{{< card link="https://css-tricks.com/" title="CSS-Tricks" subtitle="Articles and guides on CSS techniques" icon="light-bulb" >}}
{{< card link="https://specificity.keegan.st/" title="Specificity Calculator" subtitle="Check selector specificity" icon="calculator" >}}
{{< /cards >}}

### Tools

- **Browser DevTools** (F12) - Inspect and modify CSS in real-time
- **VS Code** - Code editor with CSS autocomplete and color preview
