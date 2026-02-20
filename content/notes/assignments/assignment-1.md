---
title: "Assignment 1: HTML/CSS Component Guide"
weight: 1
toc: true
---

## Introduction

This assignment asks you to build a single, polished **card component** using HTML and CSS. Card components are everywhere on the web -- profile cards on social media, product cards on shopping sites, pricing cards on SaaS pages, and testimonial cards on marketing sites. Building one from scratch teaches you the fundamentals that every front-end developer uses daily.

**What you will learn:**

- How to structure content with semantic HTML5 elements
- How the CSS box model controls spacing and layout
- How to use different CSS selectors effectively
- How to create smooth, professional hover effects
- How to organize a project with proper file structure

---

## Before You Start

{{% steps %}}

### Review Your Tools

Make sure you have VS Code, Git, and a modern browser (Chrome or Firefox) installed. You will also need a GitHub account.

### Pick Your Component Option

You have four choices: **Profile Card**, **Product Card**, **Pricing Card**, or **Testimonial Card**. Pick the one that excites you most. Think about which one you can imagine using in a real project or portfolio.

### Sketch Your Design on Paper

Before writing any code, spend 2-3 minutes sketching your card on paper. Draw the layout, label each section, and note what colors and fonts you want. This small step saves hours of indecision while coding.

### Choose Your Colors and Font

Pick a color palette (2-4 colors) and one or two fonts before you start coding. Having these decisions made upfront keeps your design consistent.

### Set Up Your GitHub Repository

Create a new public repository named `cs300-assignment-1` on GitHub. Clone it to your computer. This is where all your code will live.

{{% /steps %}}

{{< callout type="important" >}}
Start with a sketch. The biggest mistake students make is jumping straight into code without a plan. Even 2 minutes of sketching saves hours of confusion.
{{< /callout >}}

---

## Understanding the Requirements

Here is what each rubric category is worth and what the instructor is looking for:

| Criteria          | Points | What This Means                                                    |
| ----------------- | ------ | ------------------------------------------------------------------ |
| HTML Structure    | 10     | Semantic elements, valid HTML, heading hierarchy, 5+ elements      |
| CSS Styling       | 10     | Box model usage, 3+ selector types, external stylesheet, web fonts |
| Visual Design     | 10     | Consistent colors, fonts, spacing. Professional appearance         |
| Hover Effects     | 5      | Smooth transitions on interactive elements                         |
| Code Quality      | 5      | Clean indentation, meaningful class names, organized code          |
| Video Explanation | 10     | Answer all 6 required questions clearly                            |

{{< callout type="warning" >}}
The video is worth **10 points** (20% of the grade). Do not skip or rush it. Practice once before recording.
{{< /callout >}}

---

## How to Think About Your Component

{{< tabs items="Profile Card,Product Card,Pricing Card,Testimonial Card" >}}
{{< tab >}}
**Profile Card** -- Think LinkedIn or social media profiles.

**Content to include:** avatar image, person's name and role/title, a short bio, and social media links.

**HTML elements to consider:** A container element for the card, `<figure>` for the image, heading elements for the name, a paragraph for the bio, and a list or group of links for social media.

**CSS focus areas:** Circular image (border-radius), centered layout, clean typography, and hover effects on the social links.

**Hover idea:** Social media links could change color, gain a background, or scale up slightly on hover.
{{< /tab >}}
{{< tab >}}
**Product Card** -- Think Amazon or any e-commerce site.

**Content to include:** product image, product name, price (with optional sale styling), a short description, and an "Add to Cart" button.

**HTML elements to consider:** An image element, heading for the product name, a paragraph or span for the price, a description paragraph, and a button element.

**CSS focus areas:** Card shadow, image sizing, price styling (consider a strikethrough for sale prices), and a button that invites clicking.

**Hover idea:** The entire card could lift with a shadow, or the button could change background color and grow slightly.
{{< /tab >}}
{{< tab >}}
**Pricing Card** -- Think subscription plan pages (Spotify, Netflix, etc.).

**Content to include:** plan name (Basic, Pro, Enterprise), price, a list of at least 5 features, and a call-to-action button. One card should be "featured" with different styling.

**HTML elements to consider:** Heading for the plan name, a price element, an unordered list for features, and a button. If building multiple cards, consider how to visually distinguish the "featured" one.

**CSS focus areas:** Feature list with checkmarks or icons, the featured card standing out (different border, background, or scale), and button hover states.

**Hover idea:** Cards could scale up slightly or gain a more prominent shadow. The button should visually respond to hover.
{{< /tab >}}
{{< tab >}}
**Testimonial Card** -- Think review sections on business websites.

**Content to include:** a quote or testimonial text, author photo, author name and title, company name, and a star rating or similar visual element.

**HTML elements to consider:** A `<blockquote>` for the testimonial, `<figure>` for the photo, heading elements for the author info, and a visual rating (could use Unicode stars or styled elements).

**CSS focus areas:** Quote styling (consider large quotation marks as a design element), author info layout (photo next to name), star rating styling, and overall card polish.

**Hover idea:** The card could gain a subtle border or shadow change. The star rating could brighten or the quote marks could shift color.
{{< /tab >}}
{{< /tabs >}}

---

## Recommended Approach

{{% steps %}}

### Build the HTML Structure First

Write the complete HTML before touching any CSS. Use semantic elements -- ask yourself: "If CSS disappeared, would this content still make sense and be readable?" This forces you to think about content and meaning before appearance.

### Apply CSS One Layer at a Time

Do not try to style everything at once. Work in this order:

1. **Typography** -- fonts, sizes, line heights
2. **Colors** -- background, text color, borders
3. **Spacing** -- margin, padding (the box model)
4. **Layout** -- centering the card, sizing
5. **Hover effects** -- add last, once the card looks good statically

This order matters because each layer builds on the previous one, and debugging is easier when you change one thing at a time.

### Add Your Hover Effect

Once the card looks polished in its default state, add the hover interaction. Remember that the `transition` property goes on the **base state** (not the `:hover` state) so the animation works in both directions.

### Validate Your HTML

Run your HTML through the W3C Validator to catch any errors. Fix them before submitting.

### Commit Your Progress

Make multiple git commits as you work. Your history should tell the story of how you built this -- not a single "final commit."

{{% /steps %}}

{{< callout type="important" >}}
Commit after each major step. Your git history should show your building process: "Add HTML structure" then "Add typography and colors" then "Add spacing and layout" then "Add hover effects."
{{< /callout >}}

---

## Key Concepts to Review

### Semantic HTML5

Semantic elements describe the **meaning** of your content, not just how it looks. Using `<header>`, `<main>`, `<footer>`, `<figure>`, `<blockquote>`, `<nav>`, and `<section>` makes your code readable and accessible.

Ask yourself for each element: "Does this tag describe what this content IS?" A person's name is a heading. A bio is a paragraph. A collection of links is a navigation or list.

### CSS Box Model

Every HTML element is a box with four layers. Understanding these layers is the foundation of all CSS layout:

```
┌─────────────────── Margin ────────────────────┐
│  ┌────────────── Border ───────────────────┐  │
│  │  ┌────────── Padding ───────────────┐   │  │
│  │  │                                  │   │  │
│  │  │        Content Area              │   │  │
│  │  │                                  │   │  │
│  │  └──────────────────────────────────┘   │  │
│  └─────────────────────────────────────────┘  │
└───────────────────────────────────────────────┘
```

- **Content** -- the text, image, or other content inside the element
- **Padding** -- space between the content and the border (inside the box)
- **Border** -- the visible edge of the box
- **Margin** -- space outside the border (between this box and other boxes)

Use browser DevTools (F12 > Elements tab > Computed panel) to visualize the box model on any element.

### CSS Selectors

You need at least 3 different selector types. Here are the most common:

| Selector Type | Example       | What It Selects                    |
| ------------- | ------------- | ---------------------------------- |
| Element       | `h2`          | All `<h2>` elements                |
| Class         | `.card-title` | Elements with `class="card-title"` |
| Descendant    | `.card p`     | `<p>` elements inside `.card`      |
| Pseudo-class  | `.btn:hover`  | `.btn` when hovered                |
| Grouped       | `h1, h2, h3`  | Multiple elements at once          |

### Hover Effects and Transitions

A hover effect changes a CSS property when the user moves their mouse over an element. The `transition` property animates that change smoothly instead of snapping instantly.

Key concept: put the `transition` property on the **element's normal state**, not on the `:hover` state. This way the animation is smooth going both in and out of the hover.

Think of it like a door: the hinge (transition) is on the door itself, not on the act of pushing it.

### Web Fonts

Google Fonts provides free fonts you can add to any project. The process is:

1. Browse fonts at Google Fonts
2. Select the font weights you need
3. Copy the `<link>` tag into your HTML `<head>`
4. Use the font name in your CSS `font-family`

Stick to 1-2 fonts maximum. One for headings, one for body text, or just one for everything.

---

## Project Structure

{{< filetree/container >}}
{{< filetree/folder name="cs300-assignment-1" >}}
{{< filetree/file name="index.html" >}}
{{< filetree/folder name="css" >}}
{{< filetree/file name="styles.css" >}}
{{< /filetree/folder >}}
{{< filetree/folder name="images" >}}
{{< filetree/file name="(your images here)" >}}
{{< /filetree/folder >}}
{{< filetree/file name="README.md" >}}
{{< /filetree/folder >}}
{{< /filetree/container >}}

{{< callout type="info" >}}
This structure separates concerns: HTML content, CSS presentation, and image assets each have their own place. This is how professional projects are organized.
{{< /callout >}}

---

## Video Explanation Guide

Your video is worth 10 points. You must answer these 6 questions:

{{% steps %}}

### Demo Your Component

Show it working in the browser, including hover effects. Resize the window if your card responds to different sizes.

### Explain Your HTML Structure

Walk through your HTML file. Why did you choose these semantic elements? How is your heading hierarchy organized?

### Explain Box Model Usage

Point to specific examples where you used margin, padding, and/or border. What spacing decisions did you make and why?

### Walk Through 3 CSS Selectors

Show at least 3 different selector types and explain why you chose each one.

### Explain Your Hover Effect

How does it work? What CSS property changes on hover? How does the transition make it smooth?

### Describe One Challenge

What was difficult and how did you solve it? Being honest about challenges shows understanding.

{{% /steps %}}

{{< callout type="info" >}}
Practice your video once before recording. Aim for 5-8 minutes. Speak naturally and show your screen. Upload directly to D2L (not YouTube or Loom).
{{< /callout >}}

---

## Submission Checklist

Before submitting, verify everything:

- [ ] GitHub repo is public and named `cs300-assignment-1`
- [ ] `index.html` uses 5+ semantic HTML5 elements
- [ ] CSS is in an external stylesheet only (no inline styles)
- [ ] 3+ different CSS selector types used
- [ ] At least one hover effect with a smooth transition
- [ ] Google Font (or web font) included
- [ ] Good color contrast (test with a contrast checker)
- [ ] HTML passes the W3C Validator with no errors
- [ ] README includes your name, component choice, live URL, and design notes
- [ ] GitHub Pages deployment is working
- [ ] Video answers all 6 required questions
- [ ] Multiple git commits showing progress (not just one final commit)

---

## Troubleshooting & Common Mistakes

{{% details title="My image is not showing up" closed="true" %}}
Check your file path. If your image is in the `images/` folder, the path should be `images/photo.jpg` (relative to `index.html`). File names are **case-sensitive** -- `Photo.jpg` and `photo.jpg` are different files. Also verify the file actually exists in your repository.
{{% /details %}}

{{% details title="My CSS is not applying" closed="true" %}}
Check three things:

1. The `<link>` tag in your HTML `<head>` -- does the `href` path match your CSS file location? It should be `css/styles.css`
2. Your selector names -- do they match exactly (class names are case-sensitive)?
3. Specificity conflicts -- a more specific selector might be overriding yours. Use DevTools to inspect the element and see which styles are applied
   {{% /details %}}

{{% details title="My hover effect snaps instead of animating" closed="true" %}}
The `transition` property must be on the **base state**, not the `:hover` state. The transition tells the browser "when any of my properties change, animate them over this duration."

If you put `transition` inside `:hover`, it only animates going IN to the hover state, not coming back out.
{{% /details %}}

{{% details title="The W3C validator shows errors" closed="true" %}}
Common issues:

- Missing `alt` attribute on images
- Unclosed tags (every opening tag needs a closing tag, or use self-closing syntax)
- Nesting errors (e.g., `<p>` inside `<p>` is not allowed)
- Missing `lang` attribute on the `<html>` tag

Fix errors one at a time, starting from the top. Sometimes one error causes cascading errors below it.
{{% /details %}}

{{% details title="My fonts look different on GitHub Pages vs locally" closed="true" %}}
Make sure the Google Fonts `<link>` tag is in your HTML `<head>`, not in your CSS file or body. Also check that your `font-family` CSS value exactly matches the font name from Google Fonts, including capitalization.
{{% /details %}}

{{% details title="I do not know what to talk about in my video" closed="true" %}}
Follow the 6 required questions one by one. Open your code in VS Code on one side and the browser on the other. Walk through each question sequentially. You do not need to be perfect -- just demonstrate that you understand what you built and why.
{{% /details %}}

---

## Key Takeaways

{{< callout emoji="🎯" >}}

- **Sketch before you code** -- planning saves time and leads to better designs
- **HTML first, CSS second** -- get the structure right before adding appearance
- **The box model is everything** -- margin, padding, and border control all spacing in CSS
- **Use semantic elements** that describe the content's meaning, not just how it looks
- **Smooth hover effects** require the `transition` property on the base state, not the `:hover` state
- **3+ selector types** are required -- element, class, descendant, pseudo-class, or grouped
- **Commit often** -- your git history should show how you built the component step by step
- **The video matters** (10 points) -- practice once, answer all 6 questions, demonstrate understanding
  {{< /callout >}}

---

## Additional Resources

### Official Documentation

{{< cards >}}
{{< card link="https://developer.mozilla.org/en-US/docs/Web/HTML/Element" title="MDN: HTML Elements" subtitle="Complete reference for all HTML elements" icon="book-open" >}}
{{< card link="https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_box_model" title="MDN: CSS Box Model" subtitle="Understanding margin, padding, border" icon="book-open" >}}
{{< card link="https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_selectors" title="MDN: CSS Selectors" subtitle="All selector types explained" icon="document-text" >}}
{{< /cards >}}

### Video Tutorials

{{< cards >}}
{{< card link="https://www.youtube.com/watch?v=rIO5326FgPE" title="Kevin Powell: CSS Box Model" subtitle="Visual explanation of the box model" icon="play" >}}
{{< card link="https://www.youtube.com/watch?v=l1mER1bV0N0" title="Kevin Powell: CSS Selectors" subtitle="Practical selector examples" icon="play" >}}
{{< card link="https://www.youtube.com/watch?v=ceNMP-aQkQ4" title="Web Dev Simplified: CSS Hover Effects" subtitle="Creating smooth hover interactions" icon="play" >}}
{{< /cards >}}

### Tools

{{< cards >}}
{{< card link="https://fonts.google.com/" title="Google Fonts" subtitle="Free web fonts for your project" icon="code" >}}
{{< card link="https://coolors.co/" title="Coolors" subtitle="Color palette generator" icon="eye" >}}
{{< card link="https://webaim.org/resources/contrastchecker/" title="WebAIM Contrast Checker" subtitle="Test color accessibility" icon="eye" >}}
{{< card link="https://validator.w3.org/" title="W3C HTML Validator" subtitle="Check your HTML for errors" icon="code" >}}
{{< card link="https://unsplash.com/" title="Unsplash" subtitle="Free high-quality images" icon="eye" >}}
{{< /cards >}}
