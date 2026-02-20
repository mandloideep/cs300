---
title: "Assignment 2: Responsive Page Guide"
weight: 2
toc: true
---

## Introduction

This assignment asks you to build a **fully responsive single-page website** that looks great on phones, tablets, and desktops. Responsive design is not optional in modern web development -- over 60% of web traffic comes from mobile devices, and Google ranks mobile-friendly sites higher in search results.

Building on Assignment 1 (a single component), you are now scaling up to a **full page layout** with five distinct sections. This is where layout tools like Flexbox and Grid become essential.

**What you will learn:**

- How to implement a mobile-first CSS approach
- How media queries adapt your layout at different screen sizes
- How to use Flexbox and/or CSS Grid for real page layouts
- How to make images, navigation, and content respond to any viewport

---

## Before You Start

{{% steps %}}

### Review Flexbox and Grid

Make sure you are comfortable with the basics of Flexbox (one-dimensional layout) and CSS Grid (two-dimensional layout). Review the Week 3 and Week 4 student notes if needed.

### Understand Mobile-First

Mobile-first means your **default CSS** (no media queries) is designed for the smallest screen. You then add `min-width` media queries to enhance the layout for larger screens. This is the opposite of building for desktop and then "fixing" mobile.

### Research Layout Inspiration

Open any modern website and resize your browser window from wide to narrow. Watch how the layout adapts. Notice how navigation collapses, columns stack, and spacing changes. You are not copying designs -- you are observing patterns.

### Sketch Your Layout at Three Sizes

Draw quick wireframes of your page at:

- **Mobile (320px)** -- everything stacks vertically
- **Tablet (768px)** -- some elements go side-by-side
- **Desktop (1024px)** -- full layout with maximum use of space

### Set Up Your Repository

Create a public GitHub repo named `cs300-assignment-2`. Create a feature branch to work on (e.g., `feature/responsive-page`). Deploy to GitHub Pages.

{{% /steps %}}

{{< callout type="important" >}}
The most common mistake is building for desktop first, then trying to "fix" mobile. That approach leads to fighting your own CSS. Always start with mobile styles as your default.
{{< /callout >}}

---

## Understanding the Requirements

| Criteria           | Points | What This Means                                            |
| ------------------ | ------ | ---------------------------------------------------------- |
| Mobile Layout      | 12     | Single-column, readable, no horizontal scroll at 320-480px |
| Tablet Layout      | 10     | Appropriate multi-column adjustments at 768px              |
| Desktop Layout     | 10     | Polished full layout with max-width container at 1024px+   |
| Flexbox/Grid Usage | 8      | Proper use of modern layout tools (no floats)              |
| Visual Design      | 5      | Consistent colors, fonts, spacing throughout               |
| Code Quality       | 5      | Clean, mobile-first CSS organization                       |

{{< callout type="warning" >}}
You need at least **two breakpoints**: 768px for tablet and 1024px for desktop. Your mobile styles should work **without any media queries** -- they are the default.
{{< /callout >}}

---

## Breaking Down the Five Sections

Your page needs five sections. Here is how to think about each one:

{{< tabs items="Header/Nav,Hero Section,Features Section,Content Section,Footer" >}}
{{< tab >}}
**Header and Navigation**

**What it contains:** A logo or site name, plus at least 4 navigation links.

**Mobile behavior:** Navigation links stack vertically or are hidden behind a hamburger menu. The logo sits at the top.

**Desktop behavior:** Navigation links display horizontally in a row. The logo and links sit on the same line, typically with `justify-content: space-between`.

**Layout tool:** Flexbox is ideal for navigation -- it handles horizontal alignment and spacing naturally.

**Think about:** How does the user access navigation on a small phone screen? Stacked links or a toggle menu?
{{< /tab >}}
{{< tab >}}
**Hero Section**

**What it contains:** A large headline, supporting text, a call-to-action (CTA) button, and a background image or color.

**Mobile behavior:** Everything stacks vertically. The headline should still be large but scaled down. The CTA button should be full-width or nearly full-width for easy tapping.

**Desktop behavior:** More generous spacing, larger text, potentially a side-by-side layout with an image.

**Layout tool:** Flexbox for centering content, or Grid if you want a two-column hero (text on one side, image on the other).

**Think about:** The hero is the first thing visitors see. Make the headline bold and the CTA button obvious.
{{< /tab >}}
{{< tab >}}
**Features/Services Section**

**What it contains:** At least 3 feature cards, each with an icon or image, a title, and a description.

**Mobile behavior:** Cards stack vertically, one per row.

**Tablet behavior:** Cards display 2 per row.

**Desktop behavior:** Cards display 3 per row (or all in one row).

**Layout tool:** CSS Grid with `repeat(auto-fit, minmax(250px, 1fr))` handles this beautifully with minimal media queries. Alternatively, use Flexbox with `flex-wrap: wrap`.

**Think about:** This is the section where Grid or Flexbox will shine. The transition from 1 column to 2 to 3 should feel natural.
{{< /tab >}}
{{< tab >}}
**Content Section** (choose one type)

**Options:** About section with image and text, a gallery/portfolio grid, or a testimonials section.

**Mobile behavior:** Content stacks vertically. Images are full-width.

**Desktop behavior:** Text and image sit side-by-side. Gallery items display in a multi-column grid.

**Layout tool:** Flexbox for a simple side-by-side layout. Grid for a gallery.

**Think about:** This section adds depth to your page. Choose whichever option interests you most -- all three work well for demonstrating responsive design.
{{< /tab >}}
{{< tab >}}
**Footer**

**What it contains:** Site links, social media icons or links, and copyright text.

**Mobile behavior:** Everything stacks vertically and is centered.

**Desktop behavior:** Footer content spreads across the width, often in 2-3 columns.

**Layout tool:** Flexbox or Grid, depending on how many columns you want.

**Think about:** The footer should be simple and consistent. It anchors the bottom of the page.
{{< /tab >}}
{{< /tabs >}}

---

## Recommended Approach

{{% steps %}}

### Write All HTML First

Create the complete page structure with all five sections before writing any CSS. Use semantic elements: `<header>`, `<nav>`, `<main>`, `<section>`, `<footer>`. Fill in real content (not "Lorem ipsum") so you can see how the layout works with actual text and images.

### Style for Mobile First

Write your default CSS for the smallest screen (320px). Everything should stack vertically. Focus on typography, colors, and spacing. **Do not write any media queries yet.** This is your baseline.

### Add the Tablet Breakpoint

Add `@media (min-width: 768px) { }` and make adjustments: navigation goes horizontal, feature cards go side-by-side, content sections gain more breathing room. Only change what needs to change -- most of your mobile styles will carry forward.

### Add the Desktop Breakpoint

Add `@media (min-width: 1024px) { }` for the final enhancements: add a `max-width` container to prevent content from stretching too wide on large monitors, increase spacing, and finalize the multi-column layouts.

### Test at Every Width

Open browser DevTools (F12) and drag the viewport width from 320px all the way to 1200px. Watch for anything that breaks or looks awkward. Fix issues as you find them.

### Deploy and Create Your Pull Request

Push your feature branch, deploy via GitHub Pages, and create a PR to `main` with a description and your live URL.

{{% /steps %}}

---

## Key Concepts to Review

### Mobile-First Philosophy

Starting with the smallest screen forces you to prioritize content. When you have only 320px of width, you decide what truly matters. The desktop version then adds to this foundation -- it never takes away.

The technical benefit: mobile devices load only the CSS they need (the defaults). They do not download and override desktop styles. This means faster load times on slower mobile connections.

### Media Queries with min-width

The mobile-first pattern uses `min-width` queries. Think of it as "at this width AND above, apply these extra styles":

- No media query = mobile (default)
- `@media (min-width: 768px)` = tablet and above
- `@media (min-width: 1024px)` = desktop and above

Each breakpoint builds on the previous one. Styles cascade upward.

### Flexbox vs Grid: When to Use Which

{{< tabs items="Use Flexbox,Use Grid,Use Both" >}}
{{< tab >}}
**Flexbox** works best for:

- Navigation bars (horizontal alignment)
- Centering content (both axes)
- Simple side-by-side layouts
- Anything that flows in **one direction**

Flexbox is your default tool for most components.
{{< /tab >}}
{{< tab >}}
**Grid** works best for:

- Page-level layouts (header, main, sidebar, footer)
- Card grids that wrap responsively
- Anything that needs precise **row AND column** control

Grid is powerful for the overall page structure and card layouts.
{{< /tab >}}
{{< tab >}}
**Use both together:**

- Grid for the overall page layout
- Flexbox for components within grid areas (navbar, card content, footer links)

This combination is the industry standard approach.
{{< /tab >}}
{{< /tabs >}}

### The Viewport Meta Tag

{{< callout type="error" >}}
You **must** include this in your HTML `<head>`:

```html
<meta
  name="viewport"
  content="width=device-width, initial-scale=1.0"
/>
```

Without it, mobile browsers zoom out to show the desktop version, and your media queries will not work. This is the most commonly forgotten requirement.
{{< /callout >}}

### Responsive Images

Images must never overflow their containers on small screens. The standard pattern is `max-width: 100%` with `height: auto`. This ensures images scale down on small screens while maintaining their aspect ratio.

---

## Project Structure

{{< filetree/container >}}
{{< filetree/folder name="cs300-assignment-2" >}}
{{< filetree/file name="index.html" >}}
{{< filetree/folder name="css" >}}
{{< filetree/file name="styles.css" >}}
{{< /filetree/folder >}}
{{< filetree/folder name="images" >}}
{{< filetree/file name="(responsive images)" >}}
{{< /filetree/folder >}}
{{< filetree/file name="README.md" >}}
{{< /filetree/folder >}}
{{< /filetree/container >}}

---

## Responsive Design Checklist

Before submitting, test every item:

- [ ] Viewport meta tag is in the HTML `<head>`
- [ ] No horizontal scrolling at any viewport width
- [ ] Text is readable at all sizes (not too small on mobile)
- [ ] Buttons and links are tappable on mobile (minimum 44x44px)
- [ ] Images do not overflow their containers
- [ ] Navigation works on all devices (stacked on mobile, horizontal on desktop)
- [ ] Layout adjusts at both breakpoints (768px and 1024px)
- [ ] Mobile styles are the default (only `min-width` media queries used)
- [ ] A `max-width` container prevents content from stretching too wide on large screens

---

## Video Explanation Guide

{{% steps %}}

### Demo at Three Sizes

Show your page at mobile (375px), tablet (768px), and desktop (1024px+). Use DevTools responsive mode to switch between sizes.

### Walk Through Your CSS

Explain your CSS structure: where are the mobile defaults, the tablet breakpoint, and the desktop breakpoint? Show the media queries.

### Explain Your Approach

How did you implement mobile-first? What Flexbox or Grid techniques did you use? What was challenging about making it responsive?

{{% /steps %}}

{{< callout type="info" >}}
Upload your video directly to D2L. Typical length is 4-6 minutes. Show the responsive behavior clearly by resizing the browser.
{{< /callout >}}

---

## Troubleshooting & Common Mistakes

{{% details title="My page has horizontal scrolling on mobile" closed="true" %}}
Something is wider than the viewport. Common causes:

- An element with a fixed `width` in pixels (use `max-width: 100%` instead)
- Padding or borders adding to the width (add `box-sizing: border-box` to your elements)
- An image that is wider than its container
- A `pre` or `code` block with long lines

Use DevTools to find the overflowing element: inspect the page and look for anything extending beyond the right edge.
{{% /details %}}

{{% details title="My media queries are not working" closed="true" %}}
Check these in order:

1. **Viewport meta tag** -- is it in your HTML `<head>`?
2. **min-width vs max-width** -- for mobile-first, use `min-width`
3. **Order in CSS** -- media queries must come **after** the default styles they override
4. **Syntax** -- check for typos in `@media (min-width: 768px) { }`
5. **Specificity** -- styles inside the media query must be at least as specific as the defaults
   {{% /details %}}

{{% details title="My images are overflowing their containers" closed="true" %}}
Add this to your CSS (works on all images):

```css
img {
  max-width: 100%;
  height: auto;
}
```

This ensures images scale down on small screens while maintaining their aspect ratio. If an image still overflows, check if a parent element has a fixed width.
{{% /details %}}

{{% details title="My navigation does not collapse on mobile" closed="true" %}}
For a simple approach: use Flexbox with `flex-direction: column` on mobile (stacked links) and switch to `flex-direction: row` on desktop via a media query.

For a hamburger menu: this requires JavaScript to toggle visibility, which is optional for this assignment. Stacked links are perfectly acceptable.
{{% /details %}}

{{% details title="My layout looks broken between breakpoints" closed="true" %}}
This is called a "no man's land" -- the viewport is between your defined breakpoints. Solutions:

- Use relative units (`%`, `rem`, `vw`) instead of fixed pixels so elements flex between breakpoints
- Use Grid's `auto-fit` with `minmax()` for cards -- this avoids the problem entirely
- Add an additional breakpoint if needed, but 2-3 breakpoints usually suffice
  {{% /details %}}

{{% details title="The page looks different on GitHub Pages than locally" closed="true" %}}
Common causes:

- File paths are case-sensitive on GitHub Pages (but not on Mac). Check capitalization in all file references
- CSS or images not committed to the repo (run `git status` to check)
- Browser cache showing old version (hard refresh with Ctrl+Shift+R / Cmd+Shift+R)
  {{% /details %}}

---

## Key Takeaways

{{< callout emoji="🎯" >}}

- **Mobile-first is the standard** -- write mobile styles as defaults, enhance with `min-width` queries
- **Two breakpoints cover 90% of cases:** 768px (tablet) and 1024px (desktop)
- **Flexbox for components, Grid for layouts** -- use both together for the best results
- **The viewport meta tag is mandatory** -- without it, responsive design does not work on real devices
- **Test by dragging** the viewport width from 320px to 1200px to catch every layout issue
- **Use relative units** (`rem`, `%`, `vw`) instead of fixed pixels for responsive sizing
- **Images need** `max-width: 100%` and `height: auto` to prevent overflow
- **Create a Pull Request** with a description and live URL for submission
  {{< /callout >}}

---

## Additional Resources

### Video Tutorials

{{< cards >}}
{{< card link="https://www.youtube.com/watch?v=0ohtVzCSHqs" title="Kevin Powell: Are You Writing Responsive CSS Wrong?" subtitle="Mobile-first approach explained" icon="play" >}}
{{< card link="https://www.youtube.com/watch?v=yU7jJ3NbPdA" title="Kevin Powell: Learn CSS Media Queries" subtitle="Practical media query tutorial" icon="play" >}}
{{< card link="https://www.youtube.com/watch?v=p0bGHP-PXD4" title="Traversy Media: Build a Responsive Website" subtitle="Full responsive build walkthrough" icon="play" >}}
{{< /cards >}}

### Official Documentation

{{< cards >}}
{{< card link="https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Responsive_Design" title="MDN: Responsive Design" subtitle="Official responsive design guide" icon="book-open" >}}
{{< card link="https://css-tricks.com/a-complete-guide-to-css-media-queries/" title="CSS-Tricks: Media Queries" subtitle="Comprehensive media query reference" icon="book-open" >}}
{{< card link="https://css-tricks.com/snippets/css/a-guide-to-flexbox/" title="CSS-Tricks: Guide to Flexbox" subtitle="Visual Flexbox reference" icon="document-text" >}}
{{< card link="https://css-tricks.com/snippets/css/complete-guide-grid/" title="CSS-Tricks: Guide to Grid" subtitle="Visual Grid reference" icon="document-text" >}}
{{< /cards >}}

### Interactive Tools

{{< cards >}}
{{< card link="https://flexboxfroggy.com/" title="Flexbox Froggy" subtitle="Learn Flexbox through a fun game" icon="play" >}}
{{< card link="https://cssgridgarden.com/" title="Grid Garden" subtitle="Learn Grid through a fun game" icon="play" >}}
{{< card link="https://responsively.app/" title="Responsively App" subtitle="View multiple viewport sizes at once" icon="eye" >}}
{{< /cards >}}
