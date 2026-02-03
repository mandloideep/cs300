---
title: "Advanced HTML & Developer Tools"
weight: 2
toc: true
---

## Introduction

This session builds on HTML basics by covering **semantic HTML5 elements**, **forms**, **tables**, and essential developer tools like **Git** and **GitHub**. These topics will help you write more meaningful HTML and manage your code professionally.

---

## Semantic HTML5 Elements

**Semantic elements** describe the meaning of their content, not just their appearance. Instead of using `<div>` for everything, semantic elements tell browsers (and screen readers) what each section represents.

### Why Semantic HTML Matters

1. **Accessibility** - Screen readers use semantic elements to help visually impaired users navigate
2. **SEO** - Search engines understand your content better
3. **Maintainability** - Code is easier to read and understand
4. **Standardization** - Developers worldwide understand what each section means

### The Main Semantic Elements

```
┌─────────────────────────────────────────────┐
│                 <header>                     │
│  (Logo, site title, main navigation)        │
├─────────────────────────────────────────────┤
│                  <nav>                       │
│  (Navigation links)                         │
├─────────────────────────────────────────────┤
│                 <main>                       │
│  ┌─────────────────────┬─────────────────┐  │
│  │     <article>       │    <aside>      │  │
│  │  (Main content)     │  (Sidebar)      │  │
│  │                     │                 │  │
│  │    <section>        │                 │  │
│  │    <section>        │                 │  │
│  └─────────────────────┴─────────────────┘  │
├─────────────────────────────────────────────┤
│                 <footer>                     │
│  (Copyright, contact, additional links)     │
└─────────────────────────────────────────────┘
```

### Element Descriptions

**`<header>`**

- Contains introductory content or navigation
- Typically includes logo, site name, main nav
- Can be used for the page header OR section headers

```html
<header>
  <h1>My Website</h1>
  <nav><!-- navigation links --></nav>
</header>
```

**`<nav>`**

- Contains navigation links
- Use for main navigation, not every group of links

```html
<nav>
  <a href="/">Home</a>
  <a href="/about">About</a>
  <a href="/contact">Contact</a>
</nav>
```

**`<main>`**

- Contains the primary content of the page
- **Only ONE `<main>` per page**
- Should not include headers, footers, or sidebars

```html
<main>
  <h1>Article Title</h1>
  <p>Main content goes here...</p>
</main>
```

**`<section>`**

- Groups related content together
- Should typically have a heading
- Use when content forms a thematic group

```html
<section>
  <h2>Our Services</h2>
  <p>Description of services...</p>
</section>
```

**`<article>`**

- Self-contained content that could stand alone
- Examples: blog post, news article, forum post, product card
- Could be syndicated or shared independently

```html
<article>
  <h2>Blog Post Title</h2>
  <p>Posted on January 15, 2025</p>
  <p>Blog content here...</p>
</article>
```

**`<aside>`**

- Content tangentially related to main content
- Sidebars, pull quotes, advertisements
- Related links or additional information

```html
<aside>
  <h3>Related Articles</h3>
  <ul>
    <li><a href="#">Article 1</a></li>
    <li><a href="#">Article 2</a></li>
  </ul>
</aside>
```

**`<footer>`**

- Contains footer information
- Copyright, contact info, additional navigation
- Can be page footer OR section footer

```html
<footer>
  <p>&copy; 2025 My Company. All rights reserved.</p>
  <nav>
    <a href="/privacy">Privacy Policy</a>
    <a href="/terms">Terms of Service</a>
  </nav>
</footer>
```

### Complete Page Structure Example

```html
<!DOCTYPE html>
<html>
  <head>
    <title>My Website</title>
  </head>
  <body>
    <header>
      <h1>Site Name</h1>
      <nav>
        <a href="/">Home</a>
        <a href="/about">About</a>
      </nav>
    </header>

    <main>
      <article>
        <h2>Main Article</h2>
        <p>Content here...</p>
      </article>

      <aside>
        <h3>Sidebar</h3>
        <p>Related info...</p>
      </aside>
    </main>

    <footer>
      <p>&copy; 2025 My Website</p>
    </footer>
  </body>
</html>
```

---

## HTML Forms

Forms collect user input and send it to a server. They're essential for login pages, search bars, contact forms, surveys, and more.

### Basic Form Structure

```html
<form
  action="/submit"
  method="POST"
>
  <label for="username">Username:</label>
  <input
    type="text"
    id="username"
    name="username"
  />

  <button type="submit">Submit</button>
</form>
```

### Form Attributes

| Attribute | Purpose                       |
| --------- | ----------------------------- |
| `action`  | URL where form data is sent   |
| `method`  | HTTP method (`GET` or `POST`) |

- **GET** - Data visible in URL (for searches, filters)
- **POST** - Data hidden (for passwords, sensitive info)

### The Label-Input Connection

Labels and inputs must be connected for accessibility:

```html
<!-- Method 1: Using "for" and "id" -->
<label for="email">Email:</label>
<input
  type="text"
  id="email"
  name="email"
/>

<!-- Method 2: Nesting input inside label -->
<label>
  Email:
  <input
    type="text"
    name="email"
  />
</label>
```

**Why this matters:**

- Clicking the label focuses the input
- Screen readers announce the label when the input is focused
- **Always connect your labels!**

### Common Input Types

```html
<!-- Text input (default) -->
<input
  type="text"
  name="username"
  placeholder="Enter username"
/>

<!-- Password (hidden characters) -->
<input
  type="password"
  name="password"
  placeholder="Enter password"
/>

<!-- Email (validates format on mobile, shows @ keyboard) -->
<input
  type="email"
  name="email"
  placeholder="you@example.com"
/>

<!-- Number (numeric keyboard on mobile) -->
<input
  type="number"
  name="age"
  min="0"
  max="120"
/>

<!-- Checkbox (multiple selections) -->
<input
  type="checkbox"
  id="agree"
  name="agree"
/>
<label for="agree">I agree to the terms</label>

<!-- Radio buttons (single selection from group) -->
<input
  type="radio"
  id="opt1"
  name="choice"
  value="option1"
/>
<label for="opt1">Option 1</label>

<input
  type="radio"
  id="opt2"
  name="choice"
  value="option2"
/>
<label for="opt2">Option 2</label>
```

**Note:** Radio buttons with the same `name` form a group (only one can be selected).

### Textarea (Multi-line Text)

```html
<label for="message">Message:</label>
<textarea
  id="message"
  name="message"
  rows="4"
  cols="50"
  placeholder="Enter your message"
></textarea>
```

### Select Dropdown

```html
<label for="country">Country:</label>
<select
  id="country"
  name="country"
>
  <option value="">Select a country</option>
  <option value="us">United States</option>
  <option value="ca">Canada</option>
  <option value="uk">United Kingdom</option>
</select>
```

**When to use select vs radio buttons:**

- **Radio buttons** - 2-4 options (all visible)
- **Select dropdown** - 5+ options (saves space)

### Important Input Attributes

| Attribute     | Purpose                             | Example                     |
| ------------- | ----------------------------------- | --------------------------- |
| `name`        | Identifies data when sent to server | `name="email"`              |
| `id`          | Connects to label, must be unique   | `id="email"`                |
| `placeholder` | Hint text inside input              | `placeholder="Enter email"` |
| `required`    | Makes field mandatory               | `required`                  |
| `value`       | Pre-filled or submitted value       | `value="default"`           |
| `disabled`    | Prevents input                      | `disabled`                  |

### Complete Form Example

```html
<form
  action="/signup"
  method="POST"
>
  <div>
    <label for="name">Full Name:</label>
    <input
      type="text"
      id="name"
      name="name"
      required
    />
  </div>

  <div>
    <label for="email">Email:</label>
    <input
      type="email"
      id="email"
      name="email"
      required
    />
  </div>

  <div>
    <label for="password">Password:</label>
    <input
      type="password"
      id="password"
      name="password"
      required
    />
  </div>

  <div>
    <label for="bio">Bio:</label>
    <textarea
      id="bio"
      name="bio"
      rows="3"
    ></textarea>
  </div>

  <div>
    <input
      type="checkbox"
      id="newsletter"
      name="newsletter"
    />
    <label for="newsletter">Subscribe to newsletter</label>
  </div>

  <button type="submit">Sign Up</button>
</form>
```

---

## HTML Tables

Tables display **tabular data** - information organized in rows and columns. Think spreadsheets, schedules, comparison charts.

### Important Note

**Tables are for DATA, not layout.** Don't use tables to position elements on a page - use CSS for layout.

### Basic Table Structure

```html
<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Age</th>
      <th>City</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Alice</td>
      <td>25</td>
      <td>New York</td>
    </tr>
    <tr>
      <td>Bob</td>
      <td>30</td>
      <td>Los Angeles</td>
    </tr>
  </tbody>
</table>
```

### Table Elements

| Element   | Purpose                                       |
| --------- | --------------------------------------------- |
| `<table>` | Container for the entire table                |
| `<thead>` | Table header section                          |
| `<tbody>` | Table body section                            |
| `<tr>`    | Table row                                     |
| `<th>`    | Table header cell (bold, centered by default) |
| `<td>`    | Table data cell                               |

### Visual Structure

```
<table>
┌──────────────────────────────────┐
│ <thead>                          │
│ ┌────────┬────────┬────────────┐ │
│ │  <th>  │  <th>  │    <th>    │ │  ← Header row
│ └────────┴────────┴────────────┘ │
├──────────────────────────────────┤
│ <tbody>                          │
│ ┌────────┬────────┬────────────┐ │
│ │  <td>  │  <td>  │    <td>    │ │  ← Data row
│ ├────────┼────────┼────────────┤ │
│ │  <td>  │  <td>  │    <td>    │ │  ← Data row
│ └────────┴────────┴────────────┘ │
└──────────────────────────────────┘
```

### Why Separate thead and tbody?

1. **Accessibility** - Screen readers can identify headers
2. **Styling** - Easy to style header differently
3. **Printing** - Headers can repeat on each page
4. **Scrolling** - Fixed headers with scrolling body (with CSS)

---

## Meta Tags

Meta tags provide information about your page to browsers and search engines. They go in the `<head>` section.

### Essential Meta Tags

```html
<head>
  <!-- Character encoding (always include) -->
  <meta charset="UTF-8" />

  <!-- Responsive viewport (essential for mobile) -->
  <meta
    name="viewport"
    content="width=device-width, initial-scale=1.0"
  />

  <!-- Page description (for search results) -->
  <meta
    name="description"
    content="A brief description of your page for search engines"
  />

  <title>Page Title</title>
</head>
```

### What Each Does

**`charset="UTF-8"`**

- Tells browser how to interpret characters
- UTF-8 supports most languages and emojis
- Always include this first

**`viewport`**

- Makes your site work on mobile devices
- Without it, mobile browsers render at desktop width
- `width=device-width` - Match screen width
- `initial-scale=1.0` - Don't zoom in/out initially

**`description`**

- Appears in search engine results
- Keep it 150-160 characters
- Should describe the page content

---

## Accessibility Basics

Accessibility means making your website usable by everyone, including people with disabilities. It's also a **legal requirement** (ADA compliance).

### Quick Wins for Accessibility

**1. Always use alt text on images**

```html
<!-- Good -->
<img
  src="dog.jpg"
  alt="Golden retriever playing in the park"
/>

<!-- Bad -->
<img
  src="dog.jpg"
  alt="image"
/>
<img src="dog.jpg" />
<!-- Missing alt -->
```

**2. Use semantic elements**

```html
<!-- Good -->
<nav>...</nav>
<main>...</main>
<button>Click me</button>

<!-- Bad -->
<div class="nav">...</div>
<div class="main">...</div>
<div onclick="...">Click me</div>
```

**3. Connect labels to inputs**

```html
<!-- Good -->
<label for="email">Email:</label>
<input
  type="email"
  id="email"
/>

<!-- Bad -->
<span>Email:</span>
<input type="email" />
```

**4. Use proper heading hierarchy**

```html
<!-- Good -->
<h1>Main Title</h1>
<h2>Section</h2>
<h3>Subsection</h3>

<!-- Bad (skipping levels) -->
<h1>Main Title</h1>
<h4>Subsection</h4>
```

**5. Ensure sufficient color contrast**

- Text should be readable against its background
- Use tools like WebAIM Contrast Checker

---

## Using AI for HTML

AI tools can help you write HTML faster, but you must understand what they generate.

### The Right Approach

1. **Write a specific prompt** - Vague prompts give vague results
2. **Review the output** - Read every line
3. **Test it** - Open in browser, check it works
4. **Modify as needed** - Fix issues or customize
5. **Understand it** - Be able to explain every element

### Good vs Bad Prompts

**Bad prompt:**

> "Make me a form"

**Better prompt:**

> "Create an HTML contact form with fields for name, email, and message. Include labels for accessibility and a submit button."

**Best prompt:**

> "Create an HTML contact form with:
>
> - Name field (text input, required)
> - Email field (email input, required)
> - Message field (textarea, 5 rows)
> - Each input should have a label connected via for/id
> - Submit button at the bottom
>   Use semantic HTML."

### Important Rules

- **You must understand every line of code you submit**
- AI can make mistakes - always verify
- If you can't explain it, don't use it
- Video walkthroughs require you to explain your code
- AI is a tool, not a replacement for learning

---

## Git and GitHub Basics

**Git** and **GitHub** are essential tools for managing code professionally.

### Git vs GitHub

| Git                       | GitHub                             |
| ------------------------- | ---------------------------------- |
| Software on your computer | Website/service on the internet    |
| Tracks changes to files   | Hosts your Git repositories online |
| Works offline             | Requires internet                  |
| Free, open-source         | Free for public repos              |

**Analogy:** Git is like "Track Changes" in Word. GitHub is like Google Drive for your tracked documents.

### Key Terminology

| Term                  | Meaning                                  |
| --------------------- | ---------------------------------------- |
| **Repository (repo)** | A project folder tracked by Git          |
| **Commit**            | A snapshot of your changes               |
| **Push**              | Upload commits to GitHub                 |
| **Pull**              | Download changes from GitHub             |
| **Clone**             | Copy a repo from GitHub to your computer |

### Basic Workflow

```
1. Create repo on GitHub
         ↓
2. Clone to your computer
         ↓
3. Make changes to files
         ↓
4. Stage changes (git add)
         ↓
5. Commit changes (git commit)
         ↓
6. Push to GitHub (git push)
```

### Essential Git Commands

**Clone a repository:**

```bash
git clone https://github.com/username/repo-name.git
```

**Check status (what's changed):**

```bash
git status
```

**Stage files for commit:**

```bash
git add filename.html        # Add specific file
git add .                    # Add all changed files
```

**Commit changes:**

```bash
git commit -m "Add contact form to homepage"
```

**Push to GitHub:**

```bash
git push
```

### Writing Good Commit Messages

**Format:** Start with a verb, be specific

**Good examples:**

- "Add navigation bar to homepage"
- "Fix broken link in footer"
- "Update contact form validation"
- "Remove unused CSS styles"

**Bad examples:**

- "Updated stuff"
- "Fixed it"
- "Changes"
- "asdfasdf"

### Common Issues and Solutions

**"Permission denied" when pushing:**

- Make sure you're logged into GitHub
- Check that you have access to the repository

**"Nothing to commit":**

- You haven't made any changes, or
- You already committed the changes

**Changed files not showing:**

- Make sure you saved the files
- Run `git status` to check

---

## Key Takeaways

- **Semantic HTML** makes your code accessible, SEO-friendly, and maintainable
- Use `<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<aside>`, `<footer>` appropriately
- **Forms** collect user input - always connect labels to inputs
- **Tables** are for tabular data only, not layout
- **Meta tags** tell browsers and search engines about your page
- **Accessibility** is essential - use alt text, semantic elements, and proper labels
- **Git** tracks changes locally; **GitHub** hosts your code online
- AI is a tool - always understand and verify generated code

---

## Additional Resources

### Semantic HTML

- [MDN: Document and website structure](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Document_and_website_structure)
- [HTML5 Doctor](http://html5doctor.com/) - When to use each element

### Forms

- [MDN: Web forms](https://developer.mozilla.org/en-US/docs/Learn/Forms)
- [MDN: Input types](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input)

### Accessibility

- [WebAIM: Introduction to Web Accessibility](https://webaim.org/intro/)
- [A11Y Project Checklist](https://www.a11yproject.com/checklist/)
- [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/)

### Git and GitHub

- [GitHub: Git Handbook](https://guides.github.com/introduction/git-handbook/)
- [Git Cheat Sheet](https://education.github.com/git-cheat-sheet-education.pdf)
- [Learn Git Branching](https://learngitbranching.js.org/) - Interactive tutorial

### Tools

- **VS Code** - Code editor with Git integration
- **GitHub Desktop** - Visual Git client (easier than command line)
- **Browser DevTools** (F12) - Inspect any webpage's HTML
