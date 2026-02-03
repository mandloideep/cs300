---
title: "HTML Basics"
weight: 1
toc: true
---

## Introduction

**HTML** (HyperText Markup Language) is the foundation of every website. It defines the **structure and content** of web pages - what elements are on the page and what they mean.

{{< callout type="important" >}}
HTML is **NOT** a programming language. It's a **markup language**. You don't write logic or calculations - you describe what content is and how it's organized.
{{< /callout >}}

### How Websites Work

{{% steps %}}

### Type a URL

You type a URL in your browser (e.g., `google.com`)

### Browser sends request

Your browser sends a request to a server

### Server responds

The server sends back HTML, CSS, and JavaScript files

### Browser renders

Your browser reads the HTML and displays the page

{{% /steps %}}

### The Three Languages of the Web

{{< cards >}}
{{< card title="HTML" subtitle="Structure & Content - The skeleton/bones" icon="code" >}}
{{< card title="CSS" subtitle="Appearance & Style - The skin/clothes" icon="color-swatch" >}}
{{< card title="JavaScript" subtitle="Behavior & Interactivity - The muscles/brain" icon="lightning-bolt" >}}
{{< /cards >}}

This course starts with HTML because everything else builds on top of it.

---

## HTML Document Structure

Every HTML document follows the same basic structure:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>My Page Title</title>
  </head>
  <body>
    <!-- Your visible content goes here -->
  </body>
</html>
```

### Breaking It Down

**`<!DOCTYPE html>`**

- Tells the browser this is an HTML5 document
- Must be the very first line
- Not technically a tag (it's a declaration)

**`<html>`**

- The root element that contains everything
- All other elements go inside this

**`<head>`**

- Contains metadata (information _about_ the page)
- Not visible on the page itself
- Includes: title, links to CSS files, meta tags

**`<title>`**

- Sets the text shown in the browser tab
- Important for SEO and bookmarks

**`<body>`**

- Contains all visible content
- Everything users see goes here

### Comments

Comments are notes for developers that browsers ignore:

```html
<!-- This is a comment - it won't appear on the page -->
```

Use comments to:

- Explain complex sections
- Temporarily hide code
- Leave notes for yourself or teammates

---

## Understanding Tags and Elements

### What is a Tag?

Tags are the building blocks of HTML. They use angle brackets:

```html
<tagname>content</tagname>
```

- **Opening tag:** `<tagname>`
- **Closing tag:** `</tagname>` (note the forward slash)
- **Content:** Whatever goes between the tags

### What is an Element?

An **element** is the complete package: opening tag + content + closing tag.

```html
<p>This is a paragraph element.</p>
```

### Self-Closing (Void) Elements

Some elements don't have content or closing tags:

```html
<img
  src="photo.jpg"
  alt="A photo"
/>
<br />
<input type="text" />
```

These are called **void elements** or **self-closing elements**.

---

## Text Content Elements

### Headings

HTML has six heading levels, from most important (`<h1>`) to least (`<h6>`):

```html
<h1>Main Title (use only once per page)</h1>
<h2>Section Heading</h2>
<h3>Subsection Heading</h3>
<h4>Sub-subsection</h4>
<h5>Smaller heading</h5>
<h6>Smallest heading</h6>
```

{{< callout type="important" >}}
Headings are about **hierarchy**, not size. Don't use `<h1>` just because you want big text - use CSS for that. Screen readers use headings to navigate, so proper hierarchy matters for accessibility.
{{< /callout >}}

{{< callout type="info" >}}
**Best Practice:** Use headings in order (don't skip from `<h1>` to `<h4>`).
{{< /callout >}}

### Paragraphs

The `<p>` tag defines a paragraph of text:

```html
<p>
  This is a paragraph. Browsers automatically add space above and below
  paragraphs.
</p>

<p>This is another paragraph. Each paragraph is a separate block of text.</p>
```

### Text Formatting

**Bold text:**

```html
<b>This text is bold</b> <strong>This text is bold AND important</strong>
```

**Italic text:**

```html
<i>This text is italic</i> <em>This text is italic AND emphasized</em>
```

{{< callout type="info" >}}
**When to use which?**

- `<b>` and `<i>` are purely visual
- `<strong>` and `<em>` have semantic meaning (important/emphasized)
- Screen readers may read `<strong>` and `<em>` differently
- When in doubt, use `<strong>` and `<em>`
  {{< /callout >}}

### Line Breaks

The `<br>` tag creates a line break (moves to next line):

```html
<p>Line one<br />Line two<br />Line three</p>
```

{{< callout type="warning" >}}
Don't use `<br>` to create spacing - use CSS margins instead. Use `<br>` only when you actually need a line break (like in addresses or poems).
{{< /callout >}}

---

## Links and Images

### Links (Anchor Tags)

The `<a>` tag creates clickable links:

```html
<a href="https://google.com">Go to Google</a>
```

**Attributes:**

- `href` - The URL to link to (required)
- `target="_blank"` - Opens in a new tab

```html
<a
  href="https://google.com"
  target="_blank"
  >Opens in new tab</a
>
```

### Types of Links

**External links** (to other websites):

```html
<a href="https://google.com">Google</a>
```

**Internal links** (to pages on your site):

```html
<a href="about.html">About Us</a> <a href="pages/contact.html">Contact</a>
```

**Page anchors** (to a section on the same page):

```html
<a href="#section2">Jump to Section 2</a>

<!-- Later in the page -->
<h2 id="section2">Section 2</h2>
```

### Images

The `<img>` tag displays images:

```html
<img
  src="photo.jpg"
  alt="Description of the image"
/>
```

**Attributes:**

- `src` - Path to the image file (required)
- `alt` - Alternative text description (required for accessibility)
- `width` and `height` - Dimensions in pixels (optional)

```html
<img
  src="images/logo.png"
  alt="Company Logo"
  width="200"
  height="100"
/>
```

{{< callout type="important" >}}
**Why `alt` text matters:**

- Screen readers read it aloud for visually impaired users
- Displays if the image fails to load
- Helps with SEO
- **Always include meaningful alt text!**
  {{< /callout >}}

### Image Paths

**Relative paths** (from your HTML file's location):

{{< filetree/container >}}
{{< filetree/folder name="project" >}}
{{< filetree/file name="index.html" >}}
{{< filetree/file name="photo.jpg" >}}
{{< filetree/folder name="images" >}}
{{< filetree/file name="photo.jpg" >}}
{{< /filetree/folder >}}
{{< /filetree/folder >}}
{{< /filetree/container >}}

```html
<img src="photo.jpg" />
<!-- Same folder -->
<img src="images/photo.jpg" />
<!-- In images subfolder -->
<img src="../photo.jpg" />
<!-- One folder up -->
```

**Absolute paths** (full URL):

```html
<img src="https://example.com/images/photo.jpg" />
```

---

## Lists

### Unordered Lists (Bullet Points)

```html
<ul>
  <li>First item</li>
  <li>Second item</li>
  <li>Third item</li>
</ul>
```

Displays as:

- First item
- Second item
- Third item

### Ordered Lists (Numbered)

```html
<ol>
  <li>First step</li>
  <li>Second step</li>
  <li>Third step</li>
</ol>
```

Displays as:

1. First step
2. Second step
3. Third step

### Nested Lists

Lists can contain other lists:

```html
<ul>
  <li>
    Fruits
    <ul>
      <li>Apple</li>
      <li>Banana</li>
    </ul>
  </li>
  <li>Vegetables</li>
</ul>
```

---

## Containers: div and span

### The `<div>` Element

`<div>` is a generic **block-level** container. It's used to group elements together:

```html
<div>
  <h2>Section Title</h2>
  <p>Section content goes here.</p>
</div>
```

**Block-level** means:

- Takes up the full width available
- Starts on a new line
- Can contain other block elements

### The `<span>` Element

`<span>` is a generic **inline** container. It's used to style part of text:

```html
<p>This is <span>highlighted</span> text.</p>
```

**Inline** means:

- Only takes up as much width as needed
- Doesn't start a new line
- Used within text content

### Block vs Inline

```
Block elements (div, p, h1-h6):
┌────────────────────────────┐
│        Full width          │
└────────────────────────────┘
┌────────────────────────────┐
│        Full width          │
└────────────────────────────┘

Inline elements (span, a, strong):
[element 1][element 2][element 3]
```

---

## Interactive Elements

### Buttons

```html
<button>Click Me</button>
```

{{< callout type="info" >}}
Buttons don't do anything by themselves - you need JavaScript to make them functional. For now, just know how to create them.
{{< /callout >}}

### Basic Inputs

```html
<input
  type="text"
  placeholder="Enter your name"
/>
<input
  type="password"
  placeholder="Enter password"
/>
<input
  type="email"
  placeholder="Enter email"
/>
```

The `type` attribute determines what kind of input it is. We'll cover forms in more detail in Session 2.

---

## Attributes

Attributes provide additional information about elements.

### Syntax

```html
<tagname attribute="value">content</tagname>
```

**Rules:**

- Attributes go in the opening tag
- Format: `name="value"` (with quotes)
- Multiple attributes are separated by spaces

```html
<a
  href="https://google.com"
  target="_blank"
  title="Go to Google"
  >Link</a
>
```

### Common Attributes

| Attribute | Used On     | Purpose           |
| --------- | ----------- | ----------------- |
| `href`    | `<a>`       | Link destination  |
| `src`     | `<img>`     | Image source      |
| `alt`     | `<img>`     | Alternative text  |
| `id`      | Any element | Unique identifier |
| `class`   | Any element | CSS class name    |
| `style`   | Any element | Inline CSS        |
| `title`   | Any element | Tooltip text      |

### id vs class

**`id`** - Unique identifier (use only once per page)

```html
<h1 id="main-title">Welcome</h1>
```

**`class`** - Reusable identifier (use on multiple elements)

```html
<p class="highlight">This is highlighted.</p>
<p class="highlight">This is also highlighted.</p>
```

---

## Putting It All Together

Here's a complete HTML page example:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>My First Page</title>
  </head>
  <body>
    <h1>Welcome to My Website</h1>

    <p>This is my <strong>first</strong> webpage!</p>

    <h2>About Me</h2>
    <p>I'm learning HTML in CS300.</p>

    <h2>My Favorite Things</h2>
    <ul>
      <li>Coding</li>
      <li>Coffee</li>
      <li>Learning new things</li>
    </ul>

    <h2>Contact</h2>
    <p>Visit my <a href="https://github.com">GitHub</a> profile.</p>

    <img
      src="photo.jpg"
      alt="My photo"
    />
  </body>
</html>
```

---

## Key Takeaways

{{< callout emoji="🎯" >}}

- HTML defines **structure and content**, not appearance (that's CSS)
- Every HTML document needs `<!DOCTYPE html>`, `<html>`, `<head>`, and `<body>`
- Tags describe **what content is**, not how it looks
- Use semantic elements appropriately (`<h1>` for main heading, not just big text)
- Always include `alt` text on images for accessibility
- `<div>` is a block container, `<span>` is an inline container
- Attributes provide extra information: `name="value"`
- Proper nesting matters: close tags in reverse order of opening
  {{< /callout >}}

---

## Additional Resources

### Official Documentation

{{< cards >}}
{{< card link="https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/HTML_basics" title="MDN: HTML Basics" subtitle="Comprehensive beginner guide" icon="book-open" >}}
{{< card link="https://developer.mozilla.org/en-US/docs/Web/HTML/Element" title="MDN: HTML Elements Reference" subtitle="Complete list of all HTML elements" icon="document-text" >}}
{{< /cards >}}

### Interactive Learning

{{< cards >}}
{{< card link="https://www.freecodecamp.org/learn/2022/responsive-web-design/" title="freeCodeCamp" subtitle="Free interactive course" icon="academic-cap" >}}
{{< card link="https://www.codecademy.com/learn/learn-html" title="Codecademy: Learn HTML" subtitle="Interactive lessons" icon="play" >}}
{{< /cards >}}

### Quick References

{{< cards >}}
{{< card link="https://www.w3schools.com/html/" title="W3Schools HTML Tutorial" subtitle="Examples and try-it-yourself editor" icon="code" >}}
{{< card link="https://htmlcheatsheet.com/" title="HTML Cheat Sheet" subtitle="Quick reference for common elements" icon="document-text" >}}
{{< /cards >}}

### Tools

- **VS Code** - Recommended code editor
- **Browser DevTools** (F12) - Inspect HTML structure of any webpage
