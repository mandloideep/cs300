---
title: "CSS Box Model, Colors & Typography"
weight: 2
---

## Introduction

Every element on a webpage is a **rectangular box**. Understanding the CSS Box Model is fundamental to controlling layout and spacing. This session also covers colors and typography - essential skills for making your pages visually appealing.

---

## The CSS Box Model

The box model describes how every HTML element is rendered as a rectangular box with four layers:

```
┌─────────────────────────────────────────────────┐
│                    MARGIN                        │
│   ┌─────────────────────────────────────────┐   │
│   │                BORDER                    │   │
│   │   ┌─────────────────────────────────┐   │   │
│   │   │            PADDING               │   │   │
│   │   │   ┌─────────────────────────┐   │   │   │
│   │   │   │                         │   │   │   │
│   │   │   │        CONTENT          │   │   │   │
│   │   │   │                         │   │   │   │
│   │   │   └─────────────────────────┘   │   │   │
│   │   │                                  │   │   │
│   │   └─────────────────────────────────┘   │   │
│   │                                          │   │
│   └─────────────────────────────────────────┘   │
│                                                  │
└─────────────────────────────────────────────────┘
```

From inside to outside:

1. **Content** - The actual text, images, etc.
2. **Padding** - Space inside the border, around the content
3. **Border** - The edge of the element
4. **Margin** - Space outside the border, between elements

---

## Content

The content area holds your actual text, images, or other elements.

### Setting Dimensions

```css
.box {
  width: 300px;
  height: 200px;
}
```

{{< callout type="warning" >}}
**Important:** By default, `width` and `height` set the size of the **content area only**. Padding and border are added on top of this!

```css
.box {
  width: 300px;
  padding: 20px;
  border: 5px solid black;
}
/* Total width = 300 + 20 + 20 + 5 + 5 = 350px! */
```

We'll fix this unexpected behavior later with `box-sizing`.
{{< /callout >}}

---

## Padding

Padding creates space **inside** the element, between the content and the border.

```css
.box {
  padding: 20px;
}
```

### Key Characteristics

- Background color **extends through** padding
- Creates "breathing room" inside elements
- Values cannot be negative

### Padding Shorthand

```css
/* All four sides */
padding: 20px;

/* Top/bottom | Left/right */
padding: 10px 20px;

/* Top | Left/right | Bottom */
padding: 10px 20px 15px;

/* Top | Right | Bottom | Left (clockwise) */
padding: 10px 20px 15px 25px;
```

{{< callout type="info" >}}
**Memory trick:** Think of a clock starting at 12: **T**op → **R**ight → **B**ottom → **L**eft (TRouBLe)
{{< /callout >}}

### Individual Sides

```css
padding-top: 10px;
padding-right: 20px;
padding-bottom: 15px;
padding-left: 25px;
```

### Common Use Cases

**Buttons:**

```css
.button {
  padding: 10px 20px; /* More horizontal padding */
}
```

**Cards:**

```css
.card {
  padding: 20px; /* Even spacing inside */
}
```

**Containers:**

```css
.container {
  padding: 0 15px; /* Side padding only */
}
```

---

## Border

The border sits between padding and margin, creating a visible edge.

### Border Properties

```css
.box {
  border-width: 2px;
  border-style: solid;
  border-color: black;
}
```

### Border Shorthand

```css
.box {
  border: 2px solid black;
  /* width | style | color */
}
```

### Border Styles

| Style    | Appearance  |
| -------- | ----------- |
| `solid`  | ─────────── |
| `dashed` | - - - - - - |
| `dotted` | · · · · · · |
| `double` | ═══════════ |
| `none`   | (no border) |

### Individual Sides

```css
.box {
  border-top: 2px solid red;
  border-bottom: 1px dashed gray;
  border-left: none;
  border-right: none;
}
```

### Border Radius (Rounded Corners)

```css
.box {
  border-radius: 10px; /* All corners */
  border-radius: 50%; /* Circle (with equal width/height) */
  border-radius: 10px 20px; /* Top-left/bottom-right | Top-right/bottom-left */
}
```

{{< callout type="info" >}}
**Tip:** Use `border-radius: 50%` on a square element to create a circle:

```css
.avatar {
  width: 100px;
  height: 100px;
  border-radius: 50%; /* Perfect circle */
}
```

{{< /callout >}}

---

## Margin

Margin creates space **outside** the element, between it and other elements.

```css
.box {
  margin: 20px;
}
```

### Key Characteristics

- Margin is **always transparent** (background doesn't show)
- Creates space **between** elements
- Can be negative (pulls elements closer)

### Margin Shorthand

Same pattern as padding:

```css
/* All four sides */
margin: 20px;

/* Top/bottom | Left/right */
margin: 10px 20px;

/* Top | Left/right | Bottom */
margin: 10px 20px 15px;

/* Top | Right | Bottom | Left */
margin: 10px 20px 15px 25px;
```

### Individual Sides

```css
margin-top: 10px;
margin-right: 20px;
margin-bottom: 15px;
margin-left: 25px;
```

### Centering with Margin

To center a block element horizontally:

```css
.centered {
  width: 600px; /* Must have a width */
  margin: 0 auto; /* auto on left and right */
}
```

`auto` tells the browser to split the remaining space equally.

### Margin Collapse

{{< callout type="warning" >}}
When two **vertical** margins touch, they **collapse** into a single margin (the larger one wins):

```css
.box1 {
  margin-bottom: 30px;
}

.box2 {
  margin-top: 20px;
}

/* Space between them = 30px (not 50px!) */
```

This only happens with **vertical** margins, not horizontal ones.
{{< /callout >}}

---

## Padding vs Margin

| Aspect              | Padding                | Margin                         |
| ------------------- | ---------------------- | ------------------------------ |
| **Location**        | Inside border          | Outside border                 |
| **Background**      | Shows background color | Always transparent             |
| **Effect**          | Makes element larger   | Creates space between elements |
| **Negative values** | Not allowed            | Allowed                        |
| **Collapse**        | Doesn't collapse       | Vertical margins collapse      |

### Analogies

**Picture frame:**

- Padding = The mat (colored border inside the frame)
- Margin = Wall space between frames

**Personal space:**

- Padding = Your coat's thickness
- Margin = How far you stand from others

### When to Use Which

**Use padding when:**

- You want background color to extend
- Creating space inside a button or card
- Adding breathing room around text

**Use margin when:**

- Separating elements from each other
- Centering block elements
- Creating gutters between items

---

## The box-sizing Fix

### The Problem

By default, `width` and `height` only set the **content** size. Padding and border are **added** to that:

```css
.box {
  width: 300px;
  padding: 20px;
  border: 5px solid black;
}
/* Actual width: 300 + 20 + 20 + 5 + 5 = 350px */
```

This makes sizing unpredictable!

### The Solution: border-box

```css
.box {
  box-sizing: border-box;
  width: 300px;
  padding: 20px;
  border: 5px solid black;
}
/* Actual width: 300px (padding and border included) */
```

With `border-box`, the `width` you set is the **total width** including padding and border.

{{< callout type="important" >}}
**Best Practice: Apply to Everything**

Add this to the top of every CSS file:

```css
*,
*::before,
*::after {
  box-sizing: border-box;
}
```

This applies `border-box` to all elements, making sizing intuitive.
{{< /callout >}}

### Universal Reset

{{< callout type="info" >}}
Many developers also reset default margins and padding:

```css
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
```

This gives you a clean slate to work with.
{{< /callout >}}

---

## Colors in CSS

CSS offers several ways to specify colors.

{{< tabs items="Named Colors,Hexadecimal,RGB,RGBA (Transparency)" >}}
{{< tab >}}
CSS has ~140 named colors:

```css
color: red;
color: blue;
color: tomato;
color: rebeccapurple;
color: coral;
color: lightgray;
```

**Pros:** Easy to remember
**Cons:** Limited selection
{{< /tab >}}
{{< tab >}}
Six-digit codes representing Red, Green, Blue values:

```css
color: #ff0000; /* Red */
color: #00ff00; /* Green */
color: #0000ff; /* Blue */
color: #000000; /* Black (no color) */
color: #ffffff; /* White (all colors) */
color: #808080; /* Gray */
```

**Format:** `#RRGGBB` where each pair is 00-FF (0-255 in hexadecimal)

**Shorthand:** When pairs repeat, you can shorten:

```css
#FF0000 → #F00
#AABBCC → #ABC
```

{{< /tab >}}
{{< tab >}}
Specify Red, Green, Blue as decimal values (0-255):

```css
color: rgb(255, 0, 0); /* Red */
color: rgb(0, 255, 0); /* Green */
color: rgb(0, 0, 255); /* Blue */
color: rgb(128, 128, 128); /* Gray */
```

{{< /tab >}}
{{< tab >}}
Add an alpha channel for opacity (0 = transparent, 1 = solid):

```css
color: rgba(255, 0, 0, 1); /* Solid red */
color: rgba(255, 0, 0, 0.5); /* 50% transparent red */
color: rgba(0, 0, 0, 0.3); /* 30% black (for shadows) */
```

{{< /tab >}}
{{< /tabs >}}

### Color Comparison

| Format | Example                  | Best For                   |
| ------ | ------------------------ | -------------------------- |
| Named  | `tomato`                 | Quick prototyping          |
| Hex    | `#FF6347`                | Most common, design tools  |
| RGB    | `rgb(255, 99, 71)`       | When you need to calculate |
| RGBA   | `rgba(255, 99, 71, 0.5)` | Transparency effects       |

### Color Tools

{{< callout type="info" >}}
Don't pick colors randomly - use tools:

- [Coolors](https://coolors.co/) - Generate color palettes
- [ColorHunt](https://colorhunt.co/) - Pre-made palettes
- [Adobe Color](https://color.adobe.com/) - Advanced color wheel
- Browser DevTools - Built-in color picker (click any color value)
  {{< /callout >}}

---

## Typography

Typography is the art of styling text. Good typography dramatically improves readability and aesthetics.

### font-family

Specifies which font to use:

```css
body {
  font-family: Arial, Helvetica, sans-serif;
}
```

**Always provide fallbacks** in case the first font isn't available.

### Generic Font Families

| Family       | Style                  | Example          |
| ------------ | ---------------------- | ---------------- |
| `serif`      | Has decorative strokes | Times New Roman  |
| `sans-serif` | Clean, no strokes      | Arial, Helvetica |
| `monospace`  | Fixed-width characters | Courier New      |

Always end with a generic family as the final fallback:

```css
font-family: "Roboto", Arial, sans-serif;
```

### Using Google Fonts

Google Fonts provides free, professional fonts.

{{% steps %}}

### Go to Google Fonts

Visit [fonts.google.com](https://fonts.google.com)

### Select a font and weights

Choose a font and select weights (Regular 400, Bold 700, etc.)

### Copy the link tag

Copy the `<link>` tag into your HTML `<head>`:

```html
<head>
  <!-- Google Font BEFORE your stylesheet -->
  <link
    href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap"
    rel="stylesheet"
  />
  <link
    rel="stylesheet"
    href="styles.css"
  />
</head>
```

### Use the font in CSS

```css
body {
  font-family: "Roboto", sans-serif;
}
```

{{% /steps %}}

{{< callout type="warning" >}}
**Important:** The Google Font link must come **before** your CSS file.
{{< /callout >}}

### font-size

Sets the size of text:

```css
p {
  font-size: 16px; /* Pixels - fixed size */
  font-size: 1rem; /* Relative to root - responsive */
  font-size: 1.2em; /* Relative to parent - cascades */
}
```

**For now, use `px`.** We'll cover `rem` and `em` later.

**Common sizes:**

- Body text: 14-16px
- h1: 32-48px
- h2: 24-32px
- h3: 18-24px

### font-weight

Controls how bold/light text appears:

```css
p {
  font-weight: normal; /* 400 */
  font-weight: bold; /* 700 */
}

h1 {
  font-weight: 300; /* Light */
  font-weight: 400; /* Normal */
  font-weight: 500; /* Medium */
  font-weight: 600; /* Semi-bold */
  font-weight: 700; /* Bold */
  font-weight: 900; /* Black */
}
```

{{< callout type="info" >}}
**Note:** The font must support the weight you're using. Google Fonts lets you select which weights to include.
{{< /callout >}}

### font-style

```css
p {
  font-style: normal;
  font-style: italic;
}
```

### line-height

Controls spacing between lines of text:

```css
p {
  line-height: 1.5; /* 1.5x the font size - recommended */
  line-height: 24px; /* Fixed value */
  line-height: 150%; /* Percentage of font size */
}
```

{{< callout type="important" >}}
**Best practice:** Use unitless values (1.4-1.6 for body text):

```css
body {
  line-height: 1.6; /* Good readability */
}
```

{{< /callout >}}

### letter-spacing

Space between characters:

```css
h1 {
  letter-spacing: 2px; /* Spread out */
  letter-spacing: -0.5px; /* Tighter */
}
```

### text-transform

Changes capitalization:

```css
.uppercase {
  text-transform: uppercase; /* ALL CAPS */
}

.lowercase {
  text-transform: lowercase; /* all lowercase */
}

.capitalize {
  text-transform: capitalize; /* First Letter Of Each Word */
}
```

### text-decoration

Adds lines to text:

```css
a {
  text-decoration: none; /* Remove underline */
  text-decoration: underline; /* Add underline */
  text-decoration: line-through; /* Strikethrough */
}
```

### Complete Typography Example

```css
body {
  font-family: "Open Sans", Arial, sans-serif;
  font-size: 16px;
  line-height: 1.6;
  color: #333;
}

h1 {
  font-family: "Playfair Display", Georgia, serif;
  font-size: 48px;
  font-weight: 700;
  letter-spacing: -1px;
  line-height: 1.2;
}

.subtitle {
  font-size: 18px;
  font-weight: 300;
  text-transform: uppercase;
  letter-spacing: 3px;
  color: #666;
}
```

---

## Using AI for CSS

AI tools can help you write CSS faster, but always verify and understand the output.

### Good Prompt Example

```
Create CSS for a profile card component:
- 300px wide
- White background (#ffffff)
- 20px padding on all sides
- 10px border radius
- Subtle shadow: 0 2px 10px rgba(0, 0, 0, 0.1)
- Center it on the page
- Use 'Roboto' font
- Name should be 24px, bold, dark gray (#333)
- Title should be 14px, regular, medium gray (#666)
```

### AI Workflow

1. Write a **specific** prompt with exact values
2. **Review** every property in the output
3. **Test** in browser
4. **Modify** as needed
5. **Understand** what each property does

{{< callout type="warning" >}}
**Remember:** You must be able to explain any code you submit.
{{< /callout >}}

---

## Viewing the Box Model in DevTools

Browser DevTools are invaluable for understanding and debugging CSS.

**To open:** Press F12 or right-click → Inspect

**Box model visualization:**

1. Select an element in the Elements panel
2. Look for the "Computed" tab
3. You'll see a visual diagram of margin, border, padding, and content

{{< callout type="info" >}}
**Tip:** Hover over the box model sections to see them highlighted on the page.
{{< /callout >}}

---

## Key Takeaways

{{< callout emoji="🎯" >}}

- Every element is a **box** with content, padding, border, and margin
- **Padding** is inside the border (background shows through)
- **Margin** is outside the border (always transparent)
- Use `box-sizing: border-box` to make width/height include padding and border
- `margin: 0 auto` centers block elements horizontally
- Vertical margins **collapse** (only the larger one applies)
- Colors: Use hex (`#RRGGBB`) or RGB - always pick from a palette
- Typography: Always include font fallbacks, use `line-height: 1.5` for readability
- Use browser DevTools to visualize and debug the box model
  {{< /callout >}}

---

## Additional Resources

### Box Model

{{< cards >}}
{{< card link="https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/The_box_model" title="MDN: Box Model" subtitle="Complete guide" icon="book-open" >}}
{{< card link="https://css-tricks.com/the-css-box-model/" title="CSS-Tricks: Box Model" subtitle="Visual explanation" icon="eye" >}}
{{< /cards >}}

### Colors

{{< cards >}}
{{< card link="https://coolors.co/" title="Coolors" subtitle="Color palette generator" icon="color-swatch" >}}
{{< card link="https://colorhunt.co/" title="ColorHunt" subtitle="Curated palettes" icon="collection" >}}
{{< card link="https://webaim.org/resources/contrastchecker/" title="WebAIM Contrast Checker" subtitle="Accessibility tool" icon="check-circle" >}}
{{< /cards >}}

### Typography

{{< cards >}}
{{< card link="https://fonts.google.com/" title="Google Fonts" subtitle="Free web fonts" icon="document-text" >}}
{{< card link="https://www.typewolf.com/" title="Typewolf" subtitle="Font pairing inspiration" icon="sparkles" >}}
{{< card link="https://practicaltypography.com/" title="Practical Typography" subtitle="Typography fundamentals" icon="academic-cap" >}}
{{< /cards >}}

### Tools

- **Browser DevTools** - Inspect box model, edit CSS live
- [MDN Box Model Diagram](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Box_Model/Introduction_to_the_CSS_box_model) - Visual reference
