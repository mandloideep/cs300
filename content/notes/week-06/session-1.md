---
title: "DOM Manipulation & Events"
weight: 1
toc: true
---

## Introduction

In the previous weeks you learned **JavaScript fundamentals** -- variables, functions, conditionals, loops, arrays, and objects. All of that code ran in isolation: it processed data, performed calculations, and printed results to the console. But none of it changed what the user actually *sees* on the page.

This week, everything changes. You will learn how JavaScript connects to the **Document Object Model (DOM)** -- the live, in-memory representation of your HTML page. By manipulating the DOM, you can:

- Change text, images, and styles on the page without reloading
- Respond to user actions like clicks, typing, and form submissions
- Create new elements and remove existing ones dynamically
- Build truly **interactive** web pages

This is where web development gets exciting. The formula is simple:

```
  JavaScript + DOM = Interactive Web Pages
```

### How HTML Becomes the DOM

When the browser loads an HTML file, it does not work directly with the text. Instead, it **parses** the HTML and builds a tree structure in memory called the **DOM tree**. Every HTML element becomes a **node** in this tree, and JavaScript can read, modify, add, or remove any node.

```
  Your HTML File                        The DOM Tree (in memory)
  ─────────────                         ──────────────────────────
  <!DOCTYPE html>
  <html>                                       document
    <head>                                        │
      <title>My Page</title>                     html
    </head>                                     /    \
    <body>                                   head     body
      <h1>Hello</h1>                          │      /    \
      <p>World</p>                         title    h1     p
    </body>                                  │       │      │
  </html>                                "My Page" "Hello" "World"
```

Key points about the DOM:

- The DOM is a **live** representation -- changes to it instantly update the page
- Every HTML tag becomes an **element node**
- Text inside tags becomes a **text node**
- The root of the tree is the `document` object
- JavaScript accesses the DOM through the global `document` object

{{< callout type="info" >}}
Think of the DOM as a family tree. The `<html>` element is the root ancestor. `<head>` and `<body>` are its children. Elements inside `<body>` are grandchildren, and so on. This parent-child-sibling relationship is how the browser organizes your page.
{{< /callout >}}

### The document Object

The `document` object is your entry point to the DOM. It represents the entire HTML page and provides methods to find, create, and manipulate elements.

```javascript
// The document object is always available in browser JavaScript
console.log(document);          // The entire DOM
console.log(document.title);    // The <title> text
console.log(document.body);     // The <body> element
console.log(document.URL);      // The current page URL
```

Every DOM operation starts with `document`. You will see it hundreds of times.

---

## Selecting Elements

Before you can change anything on the page, you need to **find** the element you want to work with. This is called **selecting** or **querying** the DOM.

JavaScript provides several methods for selecting elements. The two most important ones use the same CSS selector syntax you already know from writing stylesheets.

### querySelector -- Select One Element

`document.querySelector()` returns the **first** element that matches a CSS selector. If no element matches, it returns `null`.

```javascript
// Select by tag name
const heading = document.querySelector("h1");

// Select by class name
const card = document.querySelector(".card");

// Select by ID
const nav = document.querySelector("#main-nav");

// Select by attribute
const emailInput = document.querySelector('input[type="email"]');

// Complex selector -- first <li> inside an element with class "menu"
const firstMenuItem = document.querySelector(".menu li");
```

{{< callout type="info" >}}
`querySelector` uses the exact same selector syntax as CSS. If you can select it in a stylesheet, you can select it with `querySelector`.
{{< /callout >}}

### querySelectorAll -- Select Multiple Elements

`document.querySelectorAll()` returns **all** elements that match a CSS selector, as a **NodeList** (an array-like collection).

```javascript
// Select all paragraphs
const paragraphs = document.querySelectorAll("p");

// Select all elements with the class "card"
const cards = document.querySelectorAll(".card");

// Select all links inside the nav
const navLinks = document.querySelectorAll("nav a");

// How many matches?
console.log(paragraphs.length); // e.g., 5
```

### Working with NodeLists

A **NodeList** looks like an array, but it is not exactly one. You can loop over it with `forEach`, but methods like `map` and `filter` are not directly available.

```javascript
const items = document.querySelectorAll(".item");

// Loop with forEach (works on NodeList)
items.forEach((item) => {
    console.log(item.textContent);
});

// Access by index
console.log(items[0]); // First item
console.log(items[2]); // Third item

// Check the count
console.log(items.length);
```

{{< callout type="warning" >}}
`querySelectorAll` always returns a NodeList, even if only one element matches (a NodeList with one item) or no elements match (an empty NodeList with `length` of 0). It never returns `null`.

`querySelector`, on the other hand, returns `null` when nothing matches. Always check for `null` before using the result.
{{< /callout >}}

### What Happens When No Match is Found

This is a critical distinction that will save you from many bugs:

```javascript
// querySelector: returns null if no match
const missing = document.querySelector(".nonexistent");
console.log(missing); // null

// This will crash!
// missing.textContent = "Hello"; // TypeError: Cannot set properties of null

// Safe approach: check first
if (missing) {
    missing.textContent = "Hello";
}

// querySelectorAll: returns empty NodeList if no match
const noItems = document.querySelectorAll(".nonexistent");
console.log(noItems);        // NodeList []
console.log(noItems.length); // 0

// forEach on an empty NodeList does nothing (no error)
noItems.forEach((item) => {
    console.log(item); // Never runs -- no items to iterate
});
```

### Comparing Selection Methods

{{< tabs items="querySelector (Recommended),getElementById (Legacy),getElementsByClassName (Legacy)" >}}
  {{< tab >}}

**`querySelector` and `querySelectorAll`** -- the modern approach.

```javascript
// By ID
const header = document.querySelector("#header");

// By class
const cards = document.querySelectorAll(".card");

// By tag
const paragraphs = document.querySelectorAll("p");

// Complex selectors
const activeItem = document.querySelector(".nav .item.active");
const oddRows = document.querySelectorAll("tr:nth-child(odd)");
```

**Why this is preferred:**
- Uses CSS selector syntax you already know
- One method handles all selector types
- Consistent return types (`Element` or `NodeList`)
- Works with any valid CSS selector, including pseudo-classes

  {{< /tab >}}
  {{< tab >}}

**`getElementById`** -- the classic way to select by ID.

```javascript
const header = document.getElementById("header");
// Note: NO # symbol -- just the ID name
```

**Differences from querySelector:**
- Only selects by ID (no classes, tags, or complex selectors)
- Does not use the `#` prefix -- just the bare ID name
- Returns `null` if not found (same as querySelector)
- Slightly faster, but the difference is negligible

```javascript
// These two are equivalent:
const a = document.querySelector("#header");
const b = document.getElementById("header");
```

  {{< /tab >}}
  {{< tab >}}

**`getElementsByClassName`** -- selects by class name.

```javascript
const cards = document.getElementsByClassName("card");
// Note: NO . prefix -- just the class name
```

**Important differences from querySelectorAll:**
- Returns a **live HTMLCollection**, not a static NodeList
- A live collection updates automatically when the DOM changes
- Does **not** have `forEach` -- you need a `for` loop or `Array.from()`

```javascript
const cards = document.getElementsByClassName("card");

// No forEach! Must convert to array first:
Array.from(cards).forEach((card) => {
    console.log(card.textContent);
});
```

{{< callout type="warning" >}}
`getElementsByClassName` returns a **live** collection that updates automatically when elements are added or removed. This can cause confusing bugs when you modify the DOM while looping. Stick with `querySelectorAll` to avoid this.
{{< /callout >}}

  {{< /tab >}}
{{< /tabs >}}

### Practical Example: Selecting Elements in a Real Page

Given this HTML:

```html
<nav id="main-nav">
    <ul class="nav-list">
        <li class="nav-item active">
            <a href="/">Home</a>
        </li>
        <li class="nav-item">
            <a href="/about">About</a>
        </li>
        <li class="nav-item">
            <a href="/contact">Contact</a>
        </li>
    </ul>
</nav>

<main>
    <h1 class="page-title">Welcome</h1>
    <div class="card-container">
        <div class="card">Card 1</div>
        <div class="card">Card 2</div>
        <div class="card">Card 3</div>
    </div>
</main>
```

Here are various ways to select elements:

```javascript
// Select the nav by ID
const nav = document.querySelector("#main-nav");

// Select the active nav item
const activeItem = document.querySelector(".nav-item.active");

// Select all nav links
const navLinks = document.querySelectorAll("#main-nav a");
console.log(navLinks.length); // 3

// Select the page title
const title = document.querySelector(".page-title");

// Select all cards
const cards = document.querySelectorAll(".card");

// Select the second card specifically
const secondCard = document.querySelector(".card:nth-child(2)");

// Select all list items inside the nav
const navItems = document.querySelectorAll("#main-nav .nav-item");
```

---

## Modifying Elements

Once you have selected an element, you can change its content, appearance, and attributes.

### Changing Text with textContent

`textContent` gets or sets the **plain text** inside an element. It is safe because it treats everything as text -- HTML tags are not rendered.

```javascript
const heading = document.querySelector("h1");

// Read the text
console.log(heading.textContent); // "Welcome"

// Change the text
heading.textContent = "Hello, World!";

// HTML tags are displayed as literal text, NOT rendered
heading.textContent = "<em>Hello</em>"; // Shows: <em>Hello</em>
```

### Changing HTML with innerHTML

`innerHTML` gets or sets the **HTML content** inside an element. Unlike `textContent`, it renders HTML tags.

```javascript
const container = document.querySelector(".container");

// Read the HTML
console.log(container.innerHTML);

// Set new HTML (tags are rendered)
container.innerHTML = "<h2>New Heading</h2><p>New paragraph</p>";

// Add to existing HTML
container.innerHTML += "<p>Another paragraph</p>";
```

{{< callout type="warning" >}}
**Security Warning: innerHTML and XSS Attacks**

Never use `innerHTML` with user-provided data. If a user types something like `<script>alert('hacked')</script>` and you insert it with `innerHTML`, that script could execute. This is called a **Cross-Site Scripting (XSS)** attack.

```javascript
// DANGEROUS -- never do this with user input!
const userInput = '<img src="x" onerror="alert(\'hacked\')">';
container.innerHTML = userInput; // This executes the attack!

// SAFE -- use textContent for user input
container.textContent = userInput; // Displays as harmless text
```

**Rule of thumb:** Use `textContent` for user-provided data. Use `innerHTML` only for content you control (like HTML you wrote yourself).
{{< /callout >}}

### textContent vs innerHTML Comparison

```html
<div id="demo">
    <strong>Hello</strong> World
</div>
```

```javascript
const demo = document.querySelector("#demo");

console.log(demo.textContent); // "Hello World" (just the text)
console.log(demo.innerHTML);   // "<strong>Hello</strong> World" (includes tags)
```

| Property | Returns | Renders HTML? | Safe for User Input? |
|----------|---------|---------------|---------------------|
| `textContent` | Plain text only | No | Yes |
| `innerHTML` | HTML string | Yes | **No** -- XSS risk |

### Changing CSS Classes with classList

The `classList` property provides methods to add, remove, toggle, and check CSS classes on an element. This is the **preferred way** to change an element's appearance because it keeps styling in CSS where it belongs.

```javascript
const card = document.querySelector(".card");

// Add a class
card.classList.add("highlighted");
// <div class="card highlighted">

// Remove a class
card.classList.remove("highlighted");
// <div class="card">

// Toggle a class (add if missing, remove if present)
card.classList.toggle("active");
// If "active" was missing, it's now added. If present, it's now removed.

// Check if a class exists
if (card.classList.contains("active")) {
    console.log("Card is active!");
}

// Add multiple classes at once
card.classList.add("featured", "large");

// Remove multiple classes at once
card.classList.remove("featured", "large");
```

**Why classList is better than directly setting className:**

```javascript
// BAD: Overwrites ALL existing classes
card.className = "active"; // Removes "card" class!

// GOOD: Adds "active" while keeping existing classes
card.classList.add("active"); // "card" class is preserved
```

### Practical Example: classList with CSS

```css
/* style.css */
.card {
    padding: 20px;
    border: 1px solid #ddd;
    border-radius: 8px;
    transition: all 0.3s ease;
}

.card.highlighted {
    border-color: #3b82f6;
    box-shadow: 0 4px 12px rgba(59, 130, 246, 0.3);
    background-color: #eff6ff;
}

.card.hidden {
    display: none;
}
```

```javascript
const card = document.querySelector(".card");

// Highlight the card
card.classList.add("highlighted");

// Hide the card
card.classList.add("hidden");

// Show the card again
card.classList.remove("hidden");
```

{{< callout type="important" >}}
**Best Practice:** Define visual states as CSS classes (`.active`, `.hidden`, `.highlighted`, `.error`) and use `classList` to switch between them. This keeps your JavaScript clean and your styles in CSS.
{{< /callout >}}

### Changing Inline Styles with the style Property

The `style` property lets you set inline CSS directly on an element. Property names use **camelCase** instead of kebab-case.

```javascript
const heading = document.querySelector("h1");

// Set individual styles
heading.style.color = "blue";
heading.style.fontSize = "2rem";         // CSS: font-size
heading.style.backgroundColor = "#f0f0f0"; // CSS: background-color
heading.style.marginTop = "20px";        // CSS: margin-top
heading.style.borderRadius = "8px";      // CSS: border-radius

// Read a style value
console.log(heading.style.color); // "blue"

// Remove an inline style (set to empty string)
heading.style.color = "";
```

**CSS Property to JavaScript Mapping:**

| CSS Property | JavaScript (camelCase) |
|-------------|----------------------|
| `font-size` | `fontSize` |
| `background-color` | `backgroundColor` |
| `margin-top` | `marginTop` |
| `border-radius` | `borderRadius` |
| `z-index` | `zIndex` |
| `text-align` | `textAlign` |

{{< callout type="info" >}}
**When to use `style` vs `classList`:**

- Use `classList` when you have predefined CSS classes -- this is cleaner and easier to maintain
- Use `style` for dynamic values that depend on calculations (like positioning based on mouse coordinates)
- `classList` is better for toggling states; `style` is better for computed values
{{< /callout >}}

### Changing Attributes with setAttribute and getAttribute

HTML elements have **attributes** like `src`, `href`, `alt`, `disabled`, `placeholder`, etc. You can read and change these with `getAttribute` and `setAttribute`.

```javascript
const image = document.querySelector("img");

// Read an attribute
console.log(image.getAttribute("src"));  // "photo.jpg"
console.log(image.getAttribute("alt"));  // "A photo"

// Change an attribute
image.setAttribute("src", "new-photo.jpg");
image.setAttribute("alt", "A new photo");

// Remove an attribute
image.removeAttribute("alt");

// Check if an attribute exists
console.log(image.hasAttribute("alt")); // false (we just removed it)
```

**Common attribute operations:**

```javascript
// Disable a button
const button = document.querySelector("button");
button.setAttribute("disabled", "");
// Or use the property directly:
button.disabled = true;

// Change a link's URL
const link = document.querySelector("a");
link.setAttribute("href", "https://example.com");
// Or: link.href = "https://example.com";

// Update an input's placeholder
const input = document.querySelector("input");
input.setAttribute("placeholder", "Enter your email");
// Or: input.placeholder = "Enter your email";
```

{{< callout type="info" >}}
Many common attributes can also be set as **properties** directly on the element object (e.g., `button.disabled = true` instead of `button.setAttribute("disabled", "")`). Both approaches work; the direct property is often more convenient for common attributes.
{{< /callout >}}

### Complete Before/After Example

Here is a complete example showing multiple modification techniques:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Modifying Elements Demo</title>
    <style>
        .profile-card {
            border: 1px solid #ddd;
            padding: 20px;
            border-radius: 8px;
            max-width: 300px;
            font-family: Arial, sans-serif;
        }
        .profile-card.vip {
            border-color: gold;
            background-color: #fffbeb;
        }
        .profile-card img {
            width: 80px;
            height: 80px;
            border-radius: 50%;
        }
        .badge {
            display: inline-block;
            padding: 2px 8px;
            background-color: gold;
            border-radius: 12px;
            font-size: 0.8rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="profile-card" id="user-card">
        <img src="default-avatar.png" alt="User avatar">
        <h2 class="user-name">New User</h2>
        <p class="user-bio">No bio yet.</p>
    </div>

    <script>
        // Select the elements
        const card = document.querySelector("#user-card");
        const avatar = card.querySelector("img");
        const name = card.querySelector(".user-name");
        const bio = card.querySelector(".user-bio");

        // Modify text content
        name.textContent = "Alice Johnson";
        bio.textContent = "Full-stack developer and coffee enthusiast.";

        // Modify attributes
        avatar.setAttribute("src", "alice-avatar.jpg");
        avatar.setAttribute("alt", "Alice Johnson's avatar");

        // Modify classes
        card.classList.add("vip");

        // Modify styles (for dynamic values)
        bio.style.fontStyle = "italic";

        // Add new HTML content
        card.innerHTML += '<span class="badge">VIP Member</span>';
    </script>
</body>
</html>
```

---

## Creating & Removing Elements

So far you have modified elements that already exist in the HTML. But what if you need to add entirely new elements -- like a new item in a todo list, a notification message, or a row in a table? JavaScript lets you **create elements from scratch** and add them to the DOM.

### The Create, Configure, Append Pattern

Creating and adding an element to the page follows a consistent three-step pattern:

{{% steps %}}

### Create the element

Use `document.createElement()` to create a new DOM element. At this point it exists in memory but is **not yet visible** on the page.

```javascript
const newParagraph = document.createElement("p");
// Creates: <p></p> (in memory, not on the page yet)
```

### Configure the element

Set the element's content, classes, attributes, and styles.

```javascript
newParagraph.textContent = "This is a new paragraph.";
newParagraph.classList.add("highlight");
newParagraph.setAttribute("id", "intro-text");
// Now it's: <p id="intro-text" class="highlight">This is a new paragraph.</p>
// Still in memory -- not on the page yet!
```

### Append it to the DOM

Use `appendChild()`, `append()`, or `prepend()` to insert the element into the page.

```javascript
const container = document.querySelector(".container");
container.appendChild(newParagraph);
// NOW it appears on the page inside .container
```

{{% /steps %}}

### createElement in Detail

`document.createElement()` accepts a tag name and returns a new element:

```javascript
const div = document.createElement("div");
const span = document.createElement("span");
const img = document.createElement("img");
const a = document.createElement("a");
const li = document.createElement("li");
const button = document.createElement("button");
```

The new element is completely empty and disconnected from the page. You configure it before inserting it.

### Methods for Adding Elements to the DOM

```javascript
const parent = document.querySelector(".parent");
const newChild = document.createElement("div");
newChild.textContent = "New element";

// appendChild -- adds as the last child
parent.appendChild(newChild);

// append -- more flexible, can add text and multiple items
parent.append(newChild);
parent.append("Some text");  // Can append plain text
parent.append(child1, child2, "text"); // Multiple items at once

// prepend -- adds as the first child
parent.prepend(newChild);

// before -- adds as a sibling before the reference element
const reference = document.querySelector(".reference");
reference.before(newChild);

// after -- adds as a sibling after the reference element
reference.after(newChild);
```

```
  parent.prepend(new)          parent.appendChild(new)
         │                              │
         ▼                              ▼
  ┌──────────────────────────────────────────┐
  │ parent                                   │
  │  ┌─────┐  ┌─────┐  ┌─────┐  ┌─────┐    │
  │  │ NEW │  │  A  │  │  B  │  │ NEW │    │
  │  └─────┘  └─────┘  └─────┘  └─────┘    │
  │  prepend   existing  existing  append    │
  └──────────────────────────────────────────┘
```

### Removing Elements

To remove an element from the DOM, call `remove()` on it:

```javascript
const element = document.querySelector(".unwanted");
element.remove();
// The element is gone from the page
```

You can also remove a child from its parent:

```javascript
const parent = document.querySelector(".parent");
const child = document.querySelector(".child");
parent.removeChild(child);
```

### Complete Example: Building a Todo List

This example demonstrates creating, configuring, appending, and removing elements:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Todo List</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 400px;
            margin: 50px auto;
            padding: 0 20px;
        }
        .todo-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px;
            margin: 5px 0;
            background-color: #f8f9fa;
            border-radius: 4px;
            border-left: 3px solid #3b82f6;
        }
        .delete-btn {
            background: #ef4444;
            color: white;
            border: none;
            padding: 4px 10px;
            border-radius: 4px;
            cursor: pointer;
        }
        .delete-btn:hover {
            background: #dc2626;
        }
        #todo-input {
            padding: 8px;
            width: 70%;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        #add-btn {
            padding: 8px 16px;
            background: #3b82f6;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <h1>My Todo List</h1>
    <div>
        <input type="text" id="todo-input" placeholder="Add a new task...">
        <button id="add-btn">Add</button>
    </div>
    <div id="todo-list"></div>

    <script>
        const input = document.querySelector("#todo-input");
        const addBtn = document.querySelector("#add-btn");
        const todoList = document.querySelector("#todo-list");

        function addTodo() {
            const text = input.value.trim();
            if (text === "") return; // Don't add empty items

            // Step 1: Create elements
            const todoItem = document.createElement("div");
            const todoText = document.createElement("span");
            const deleteBtn = document.createElement("button");

            // Step 2: Configure elements
            todoItem.classList.add("todo-item");
            todoText.textContent = text;
            deleteBtn.textContent = "Delete";
            deleteBtn.classList.add("delete-btn");

            // Add delete functionality
            deleteBtn.addEventListener("click", function () {
                todoItem.remove();
            });

            // Step 3: Assemble and append
            todoItem.appendChild(todoText);
            todoItem.appendChild(deleteBtn);
            todoList.appendChild(todoItem);

            // Clear the input
            input.value = "";
            input.focus();
        }

        addBtn.addEventListener("click", addTodo);

        // Also add on Enter key
        input.addEventListener("keydown", function (event) {
            if (event.key === "Enter") {
                addTodo();
            }
        });
    </script>
</body>
</html>
```

### Creating Multiple Elements in a Loop

A common pattern is creating several elements from an array of data:

```javascript
const fruits = ["Apple", "Banana", "Cherry", "Date", "Elderberry"];
const list = document.querySelector("#fruit-list"); // An existing <ul>

fruits.forEach((fruit) => {
    const li = document.createElement("li");
    li.textContent = fruit;
    list.appendChild(li);
});
```

### Using Document Fragments for Performance

When adding many elements, each `appendChild` causes the browser to re-render. A **DocumentFragment** lets you build a batch of elements off-screen, then insert them all at once:

```javascript
const data = ["Item 1", "Item 2", "Item 3", "Item 4", "Item 5"];
const list = document.querySelector("#my-list");

// Create a fragment (a lightweight container that is not rendered)
const fragment = document.createDocumentFragment();

data.forEach((text) => {
    const li = document.createElement("li");
    li.textContent = text;
    fragment.appendChild(li); // Add to fragment, not to the page
});

// One single DOM insertion
list.appendChild(fragment);
```

{{< callout type="info" >}}
For small numbers of elements (under 50), the performance difference is negligible. Use `DocumentFragment` when you are adding hundreds or thousands of items, like rendering a large dataset.
{{< /callout >}}

---

## Event Handling

An **event** is something that happens on the page -- a click, a key press, a form submission, the mouse moving, the page finishing loading. **Event handling** is writing JavaScript that responds to these events.

This is the core pattern that makes web pages interactive:

```
  Select an element  →  Listen for an event  →  Run a function

  "Find the button"  →  "When it's clicked"  →  "Do this thing"
```

### addEventListener -- The Standard Pattern

The `addEventListener` method attaches an event handler to an element:

```javascript
element.addEventListener("eventType", handlerFunction);
```

```javascript
const button = document.querySelector("#my-button");

button.addEventListener("click", function () {
    console.log("Button was clicked!");
});
```

When the button is clicked, the browser calls the function you provided. This function is called the **event handler** or **callback**.

### Named Functions vs Anonymous Functions

You can pass either a **named function** or an **anonymous function** as the handler:

{{< tabs items="Anonymous Function,Named Function,Arrow Function" >}}
  {{< tab >}}

**Anonymous function** -- defined inline, right where it is used.

```javascript
const button = document.querySelector("#btn");

button.addEventListener("click", function () {
    console.log("Clicked!");
});
```

- Quick and convenient for simple handlers
- Cannot be removed later (because it has no name to reference)
- Good for one-off handlers you will not need to remove

  {{< /tab >}}
  {{< tab >}}

**Named function** -- defined separately, then passed by name.

```javascript
function handleClick() {
    console.log("Clicked!");
}

const button = document.querySelector("#btn");
button.addEventListener("click", handleClick);

// Can be removed later:
button.removeEventListener("click", handleClick);
```

- Reusable -- the same handler can be attached to multiple elements
- Can be removed with `removeEventListener`
- Easier to debug (the function name appears in stack traces)

  {{< /tab >}}
  {{< tab >}}

**Arrow function** -- concise anonymous function.

```javascript
const button = document.querySelector("#btn");

button.addEventListener("click", () => {
    console.log("Clicked!");
});
```

- Same behavior as an anonymous function
- Shorter syntax
- Cannot be removed (same limitation as anonymous functions)

  {{< /tab >}}
{{< /tabs >}}

{{< callout type="important" >}}
**Best Practice:** Use named functions when you might need to remove the listener later or reuse the handler. Use anonymous/arrow functions for simple, one-off handlers.
{{< /callout >}}

### Common Event Types

Here are the events you will use most often:

| Event | Fires When | Common Use |
|-------|-----------|------------|
| `click` | Element is clicked | Buttons, cards, navigation |
| `dblclick` | Element is double-clicked | Text editing, special actions |
| `mouseover` | Mouse enters an element | Tooltips, hover effects |
| `mouseout` | Mouse leaves an element | Hiding tooltips |
| `mousedown` | Mouse button pressed | Drag start |
| `mouseup` | Mouse button released | Drag end |
| `keydown` | Key is pressed | Keyboard shortcuts, form input |
| `keyup` | Key is released | Input validation |
| `input` | Input value changes | Real-time search, character counters |
| `change` | Input value is committed | Dropdown selection, checkbox toggle |
| `submit` | Form is submitted | Form validation and processing |
| `focus` | Element gains focus | Highlighting active inputs |
| `blur` | Element loses focus | Validating input on exit |
| `scroll` | Page or element is scrolled | Infinite scroll, back-to-top button |
| `load` | Page or resource finishes loading | Initial setup, image loading |
| `DOMContentLoaded` | HTML parsing complete | Running setup code |

### The Event Object

When an event fires, the browser automatically passes an **event object** to your handler function. This object contains information about the event.

```javascript
const button = document.querySelector("#btn");

button.addEventListener("click", function (event) {
    console.log(event.type);    // "click"
    console.log(event.target);  // The element that was clicked
    console.log(event.clientX); // Mouse X position (from left of viewport)
    console.log(event.clientY); // Mouse Y position (from top of viewport)
    console.log(event.timeStamp); // When the event occurred
});
```

The parameter name is your choice, but `event`, `e`, or `evt` are conventional:

```javascript
// All of these work -- "event" is just the parameter name
button.addEventListener("click", function (event) { ... });
button.addEventListener("click", function (e) { ... });
button.addEventListener("click", (evt) => { ... });
```

**Key properties of the event object:**

| Property | Description |
|----------|------------|
| `event.type` | The type of event (`"click"`, `"keydown"`, etc.) |
| `event.target` | The element that triggered the event |
| `event.currentTarget` | The element the listener is attached to |
| `event.clientX` / `event.clientY` | Mouse position in the viewport |
| `event.pageX` / `event.pageY` | Mouse position in the document |
| `event.key` | The key that was pressed (for keyboard events) |
| `event.timeStamp` | When the event occurred (milliseconds) |

### event.target -- Which Element Was Interacted With

`event.target` is extremely useful. It tells you exactly which element the user interacted with:

```javascript
document.querySelector(".card-container").addEventListener("click", function (event) {
    console.log("You clicked on:", event.target);
    console.log("Tag name:", event.target.tagName); // "DIV", "P", "BUTTON", etc.
    console.log("Class list:", event.target.classList);
    console.log("Text:", event.target.textContent);
});
```

### Keyboard Events

For `keydown` and `keyup` events, the event object includes information about which key was pressed:

```javascript
document.addEventListener("keydown", function (event) {
    console.log("Key pressed:", event.key);   // "a", "Enter", "Escape", "ArrowUp"
    console.log("Key code:", event.code);     // "KeyA", "Enter", "Escape", "ArrowUp"

    if (event.key === "Escape") {
        console.log("Escape key pressed!");
    }

    // Check for modifier keys
    if (event.ctrlKey && event.key === "s") {
        event.preventDefault(); // Prevent browser's default save dialog
        console.log("Ctrl+S detected -- custom save!");
    }
});
```

### event.preventDefault -- Stopping Default Behavior

Some elements have built-in behavior. Forms submit and reload the page. Links navigate to a URL. `event.preventDefault()` stops this default behavior so you can handle it with JavaScript instead.

```javascript
// Prevent a form from reloading the page
const form = document.querySelector("form");
form.addEventListener("submit", function (event) {
    event.preventDefault(); // Stop the page reload!
    console.log("Form submitted via JavaScript");
    // Now handle the form data with JavaScript instead
});

// Prevent a link from navigating
const link = document.querySelector("a");
link.addEventListener("click", function (event) {
    event.preventDefault(); // Stop navigation
    console.log("Link clicked, but page did not navigate");
});
```

{{< callout type="warning" >}}
You will almost always need `event.preventDefault()` on form `submit` events. Without it, the browser reloads the page and you lose all your JavaScript state. This is one of the most common mistakes for beginners.
{{< /callout >}}

### Complete Event Handling Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Event Handling Demo</title>
    <style>
        body { font-family: Arial, sans-serif; padding: 20px; }
        .output { margin-top: 10px; padding: 10px; background: #f0f0f0; border-radius: 4px; }
        button { padding: 8px 16px; margin: 4px; cursor: pointer; }
        .active { background-color: #3b82f6; color: white; }
    </style>
</head>
<body>
    <h1>Event Demo</h1>

    <button id="click-btn">Click Me</button>
    <button id="hover-btn">Hover Over Me</button>

    <div class="output" id="output">Interact with the buttons above.</div>

    <h2>Type Something</h2>
    <input type="text" id="text-input" placeholder="Start typing...">
    <div class="output" id="type-output">Your text will appear here.</div>

    <script>
        const output = document.querySelector("#output");
        const typeOutput = document.querySelector("#type-output");

        // Click event
        const clickBtn = document.querySelector("#click-btn");
        clickBtn.addEventListener("click", function (event) {
            output.textContent = `Clicked at position (${event.clientX}, ${event.clientY})`;
            clickBtn.classList.toggle("active");
        });

        // Mouseover and mouseout events
        const hoverBtn = document.querySelector("#hover-btn");
        hoverBtn.addEventListener("mouseover", function () {
            output.textContent = "Mouse is over the button!";
            hoverBtn.style.backgroundColor = "#10b981";
            hoverBtn.style.color = "white";
        });
        hoverBtn.addEventListener("mouseout", function () {
            output.textContent = "Mouse left the button.";
            hoverBtn.style.backgroundColor = "";
            hoverBtn.style.color = "";
        });

        // Input event (fires on every keystroke)
        const textInput = document.querySelector("#text-input");
        textInput.addEventListener("input", function (event) {
            typeOutput.textContent = event.target.value || "Your text will appear here.";
        });

        // Keyboard event on the whole document
        document.addEventListener("keydown", function (event) {
            if (event.key === "Escape") {
                output.textContent = "Escape key pressed! Resetting...";
                clickBtn.classList.remove("active");
            }
        });
    </script>
</body>
</html>
```

---

## Event Propagation

When you click a button inside a `<div>` inside the `<body>`, which element receives the event? The answer is: **all of them**. Events do not just fire on the element you clicked -- they travel through the DOM tree. This is called **event propagation**.

### Event Bubbling

By default, events **bubble up** from the target element to the root of the document. When you click a `<button>` inside a `<div>`, the click event fires on the button first, then the div, then the body, then the html element, then the document.

```
  Event Bubbling (default behavior):

  ┌──────────────────────────────────────────┐
  │ document                         ④       │
  │  ┌────────────────────────────────────┐  │
  │  │ body                       ③      │  │
  │  │  ┌──────────────────────────────┐  │  │
  │  │  │ div.container        ②      │  │  │
  │  │  │  ┌────────────────────────┐  │  │  │
  │  │  │  │ button          ①     │  │  │  │
  │  │  │  │   CLICK HERE          │  │  │  │
  │  │  │  └────────────────────────┘  │  │  │
  │  │  └──────────────────────────────┘  │  │
  │  └────────────────────────────────────┘  │
  └──────────────────────────────────────────┘

  Event fires in order: ① button → ② div → ③ body → ④ document
```

You can see this in action:

```javascript
document.querySelector("button").addEventListener("click", function () {
    console.log("1. Button clicked");
});

document.querySelector(".container").addEventListener("click", function () {
    console.log("2. Container clicked (via bubbling)");
});

document.querySelector("body").addEventListener("click", function () {
    console.log("3. Body clicked (via bubbling)");
});

// Clicking the button logs:
// 1. Button clicked
// 2. Container clicked (via bubbling)
// 3. Body clicked (via bubbling)
```

### Event Capturing

There is also a **capturing phase** where the event travels DOWN from the document to the target, before bubbling back up. Capturing is rarely used, but you can enable it by passing `{ capture: true }` as the third argument:

```javascript
document.querySelector(".container").addEventListener("click", function () {
    console.log("Capturing phase: container");
}, { capture: true }); // Enable capturing
```

```
  Full event flow:

  Capturing Phase (top → down)     Bubbling Phase (bottom → up)
  ① document                       ⑥ document
  ② body                           ⑤ body
  ③ div.container                  ④ div.container
  ─── ─── ─── TARGET: button ─── ─── ───
```

{{< callout type="info" >}}
You will almost never need to use the capturing phase. It is covered here for completeness, and so you understand the full picture of event propagation. In practice, bubbling is what matters.
{{< /callout >}}

### event.stopPropagation -- Stopping Bubbling

Sometimes bubbling causes problems. For example, you have a click handler on a card and a delete button inside the card. Clicking the delete button also triggers the card's click handler. Use `event.stopPropagation()` to stop the event from bubbling further.

```javascript
const card = document.querySelector(".card");
const deleteBtn = document.querySelector(".card .delete-btn");

card.addEventListener("click", function () {
    console.log("Card clicked -- open details");
    // Opens the card detail view
});

deleteBtn.addEventListener("click", function (event) {
    event.stopPropagation(); // Prevent the card click from firing!
    console.log("Delete button clicked -- deleting card");
    card.remove();
});
```

Without `stopPropagation`, clicking the delete button would first run the delete handler, then bubble up and also run the card's click handler.

{{< callout type="warning" >}}
Use `event.stopPropagation()` sparingly. Overusing it can make debugging difficult because events stop flowing in unexpected ways. Only use it when you have a specific reason, like preventing a parent handler from triggering.
{{< /callout >}}

### Event Delegation

**Event delegation** is a powerful pattern that takes advantage of bubbling. Instead of adding event listeners to every individual child element, you add **one listener to the parent** and use `event.target` to determine which child was clicked.

**Without delegation (bad for many items):**

```javascript
// Adding a listener to every single button -- wasteful!
const buttons = document.querySelectorAll(".item-btn");
buttons.forEach((btn) => {
    btn.addEventListener("click", function () {
        console.log("Button clicked:", btn.textContent);
    });
});
// Problem: If new buttons are added later, they won't have listeners
```

**With delegation (efficient and handles new elements):**

```javascript
// One listener on the parent handles all child clicks
const buttonContainer = document.querySelector(".button-list");

buttonContainer.addEventListener("click", function (event) {
    // Check if the clicked element is a button
    if (event.target.matches(".item-btn")) {
        console.log("Button clicked:", event.target.textContent);
    }
});
// New buttons added later will automatically work!
```

**Why event delegation is better:**

1. **Performance** -- one listener instead of many
2. **Dynamic elements** -- works for elements added after the listener is set up
3. **Cleaner code** -- less event listener management

```
  Event Delegation Pattern:

  ┌────────────────────────────────────────────┐
  │  <ul class="list">  ← ONE listener here   │
  │  ┌──────────┐  ┌──────────┐  ┌──────────┐ │
  │  │  <li>    │  │  <li>    │  │  <li>    │ │
  │  │ Item 1   │  │ Item 2   │  │ Item 3   │ │
  │  └──────────┘  └──────────┘  └──────────┘ │
  │                                            │
  │  Click on any <li> → event bubbles to <ul> │
  │  → event.target tells you WHICH <li>       │
  └────────────────────────────────────────────┘
```

### Complete Event Delegation Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Event Delegation</title>
    <style>
        .color-list {
            list-style: none;
            padding: 0;
            display: flex;
            gap: 10px;
        }
        .color-item {
            padding: 20px 30px;
            border-radius: 8px;
            cursor: pointer;
            color: white;
            font-weight: bold;
            transition: transform 0.2s;
        }
        .color-item:hover { transform: scale(1.05); }
        .color-item.selected { outline: 3px solid black; outline-offset: 3px; }
        #result { margin-top: 20px; padding: 15px; background: #f0f0f0; border-radius: 4px; }
    </style>
</head>
<body>
    <h1>Pick a Color</h1>
    <ul class="color-list">
        <li class="color-item" data-color="red" style="background: #ef4444;">Red</li>
        <li class="color-item" data-color="blue" style="background: #3b82f6;">Blue</li>
        <li class="color-item" data-color="green" style="background: #22c55e;">Green</li>
        <li class="color-item" data-color="purple" style="background: #a855f7;">Purple</li>
    </ul>
    <div id="result">Click a color to select it.</div>

    <script>
        const colorList = document.querySelector(".color-list");
        const result = document.querySelector("#result");

        // ONE event listener on the parent handles all clicks
        colorList.addEventListener("click", function (event) {
            // Only respond if a color item was clicked
            if (event.target.matches(".color-item")) {
                // Remove 'selected' from all items
                document.querySelectorAll(".color-item").forEach((item) => {
                    item.classList.remove("selected");
                });

                // Add 'selected' to the clicked item
                event.target.classList.add("selected");

                // Read the data attribute
                const color = event.target.dataset.color;
                result.textContent = `You selected: ${color}`;
                result.style.borderLeft = `4px solid ${event.target.style.background}`;
            }
        });
    </script>
</body>
</html>
```

{{< callout type="info" >}}
The `data-*` attributes (like `data-color`) are custom attributes you can add to any HTML element. Access them in JavaScript with `element.dataset.color` (drop the `data-` prefix and use camelCase for multi-word names).
{{< /callout >}}

---

## Form Handling & Validation

Forms are one of the most important interactive elements on the web. Login forms, search bars, contact forms, checkout flows -- they all require JavaScript to validate input, provide feedback, and handle submission.

### Reading Input Values

Every form input has a `.value` property that gives you the current value:

```html
<input type="text" id="name-input" placeholder="Your name">
<input type="email" id="email-input" placeholder="Your email">
<textarea id="message-input" placeholder="Your message"></textarea>
<select id="topic-select">
    <option value="">Choose a topic</option>
    <option value="support">Support</option>
    <option value="feedback">Feedback</option>
    <option value="other">Other</option>
</select>
<input type="checkbox" id="agree-checkbox">
```

```javascript
const nameInput = document.querySelector("#name-input");
const emailInput = document.querySelector("#email-input");
const messageInput = document.querySelector("#message-input");
const topicSelect = document.querySelector("#topic-select");
const agreeCheckbox = document.querySelector("#agree-checkbox");

// Reading values
console.log(nameInput.value);       // Whatever the user typed
console.log(emailInput.value);      // Email text
console.log(messageInput.value);    // Textarea text
console.log(topicSelect.value);     // Selected option's value
console.log(agreeCheckbox.checked); // true or false (not .value!)
```

{{< callout type="warning" >}}
For checkboxes and radio buttons, use `.checked` (returns `true` or `false`), not `.value`. The `.value` of a checkbox is always its HTML `value` attribute, regardless of whether it is checked.
{{< /callout >}}

### Real-Time Validation with the input Event

The `input` event fires every time the value changes (on every keystroke). This is perfect for real-time validation and feedback:

```javascript
const emailInput = document.querySelector("#email-input");
const emailError = document.querySelector("#email-error");

emailInput.addEventListener("input", function (event) {
    const email = event.target.value;

    if (email === "") {
        emailError.textContent = "";
    } else if (!email.includes("@")) {
        emailError.textContent = "Please include an @ symbol.";
        emailError.style.color = "red";
    } else {
        emailError.textContent = "Looks good!";
        emailError.style.color = "green";
    }
});
```

### The change Event vs the input Event

| Event | Fires When | Best For |
|-------|-----------|----------|
| `input` | On every keystroke / value change | Real-time feedback, character counters |
| `change` | When input loses focus (blur) after value changed | Dropdowns, checkboxes, final validation |

```javascript
// input: fires on every keystroke
textInput.addEventListener("input", function (e) {
    console.log("Current value:", e.target.value);
});

// change: fires when the user leaves the input (or selects a dropdown option)
textInput.addEventListener("change", function (e) {
    console.log("Final value:", e.target.value);
});
```

### Handling Form Submission

When a form is submitted, the browser's default behavior is to reload the page. You must call `event.preventDefault()` to handle it with JavaScript instead:

```javascript
const form = document.querySelector("#contact-form");

form.addEventListener("submit", function (event) {
    event.preventDefault(); // CRITICAL: prevent page reload

    // Collect form data
    const name = document.querySelector("#name").value.trim();
    const email = document.querySelector("#email").value.trim();
    const message = document.querySelector("#message").value.trim();

    // Validate
    if (name === "" || email === "" || message === "") {
        alert("Please fill in all fields.");
        return;
    }

    // Process the data
    console.log("Form submitted:", { name, email, message });

    // You could send this to an API, display a success message, etc.
});
```

### Complete Contact Form with Validation

This is a full, working example you can copy and run:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Contact Form</title>
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body {
            font-family: Arial, sans-serif;
            max-width: 500px;
            margin: 40px auto;
            padding: 0 20px;
            color: #333;
        }
        h1 { margin-bottom: 20px; }
        .form-group {
            margin-bottom: 16px;
        }
        label {
            display: block;
            margin-bottom: 4px;
            font-weight: bold;
            font-size: 0.9rem;
        }
        input, textarea, select {
            width: 100%;
            padding: 10px;
            border: 2px solid #ddd;
            border-radius: 6px;
            font-size: 1rem;
            font-family: inherit;
            transition: border-color 0.2s;
        }
        input:focus, textarea:focus, select:focus {
            outline: none;
            border-color: #3b82f6;
        }
        .error-message {
            font-size: 0.85rem;
            margin-top: 4px;
            min-height: 20px;
        }
        .error-message.error { color: #ef4444; }
        .error-message.success { color: #22c55e; }
        input.input-error, textarea.input-error { border-color: #ef4444; }
        input.input-success, textarea.input-success { border-color: #22c55e; }
        .submit-btn {
            width: 100%;
            padding: 12px;
            background: #3b82f6;
            color: white;
            border: none;
            border-radius: 6px;
            font-size: 1rem;
            cursor: pointer;
            transition: background 0.2s;
        }
        .submit-btn:hover { background: #2563eb; }
        .submit-btn:disabled {
            background: #94a3b8;
            cursor: not-allowed;
        }
        .success-message {
            padding: 16px;
            background: #f0fdf4;
            border: 1px solid #22c55e;
            border-radius: 6px;
            color: #166534;
            display: none;
            margin-top: 16px;
        }
    </style>
</head>
<body>
    <h1>Contact Us</h1>

    <form id="contact-form" novalidate>
        <div class="form-group">
            <label for="name">Name</label>
            <input type="text" id="name" placeholder="Your full name">
            <div class="error-message" id="name-feedback"></div>
        </div>

        <div class="form-group">
            <label for="email">Email</label>
            <input type="email" id="email" placeholder="you@example.com">
            <div class="error-message" id="email-feedback"></div>
        </div>

        <div class="form-group">
            <label for="subject">Subject</label>
            <select id="subject">
                <option value="">Choose a subject...</option>
                <option value="general">General Inquiry</option>
                <option value="support">Technical Support</option>
                <option value="feedback">Feedback</option>
            </select>
            <div class="error-message" id="subject-feedback"></div>
        </div>

        <div class="form-group">
            <label for="message">Message</label>
            <textarea id="message" rows="5" placeholder="Your message..."></textarea>
            <div class="error-message" id="message-feedback"></div>
        </div>

        <button type="submit" class="submit-btn">Send Message</button>
    </form>

    <div class="success-message" id="success-message">
        Thank you! Your message has been sent successfully.
    </div>

    <script>
        const form = document.querySelector("#contact-form");
        const nameInput = document.querySelector("#name");
        const emailInput = document.querySelector("#email");
        const subjectSelect = document.querySelector("#subject");
        const messageInput = document.querySelector("#message");
        const successMessage = document.querySelector("#success-message");

        // Helper: show feedback for a field
        function showFeedback(inputElement, feedbackId, message, isError) {
            const feedback = document.querySelector(`#${feedbackId}`);
            feedback.textContent = message;
            feedback.className = "error-message " + (isError ? "error" : "success");
            inputElement.className = isError ? "input-error" : "input-success";
        }

        // Helper: clear feedback for a field
        function clearFeedback(inputElement, feedbackId) {
            const feedback = document.querySelector(`#${feedbackId}`);
            feedback.textContent = "";
            feedback.className = "error-message";
            inputElement.className = "";
        }

        // Real-time name validation
        nameInput.addEventListener("input", function () {
            const value = nameInput.value.trim();
            if (value === "") {
                clearFeedback(nameInput, "name-feedback");
            } else if (value.length < 2) {
                showFeedback(nameInput, "name-feedback", "Name must be at least 2 characters.", true);
            } else {
                showFeedback(nameInput, "name-feedback", "Looks good!", false);
            }
        });

        // Real-time email validation
        emailInput.addEventListener("input", function () {
            const value = emailInput.value.trim();
            if (value === "") {
                clearFeedback(emailInput, "email-feedback");
            } else if (!value.includes("@") || !value.includes(".")) {
                showFeedback(emailInput, "email-feedback", "Please enter a valid email.", true);
            } else {
                showFeedback(emailInput, "email-feedback", "Valid email!", false);
            }
        });

        // Subject validation on change
        subjectSelect.addEventListener("change", function () {
            if (subjectSelect.value === "") {
                showFeedback(subjectSelect, "subject-feedback", "Please select a subject.", true);
            } else {
                showFeedback(subjectSelect, "subject-feedback", "Selected!", false);
            }
        });

        // Real-time message validation
        messageInput.addEventListener("input", function () {
            const value = messageInput.value.trim();
            if (value === "") {
                clearFeedback(messageInput, "message-feedback");
            } else if (value.length < 10) {
                showFeedback(messageInput, "message-feedback",
                    `Message too short (${value.length}/10 characters).`, true);
            } else {
                showFeedback(messageInput, "message-feedback",
                    `Good length (${value.length} characters).`, false);
            }
        });

        // Form submission
        form.addEventListener("submit", function (event) {
            event.preventDefault();

            const name = nameInput.value.trim();
            const email = emailInput.value.trim();
            const subject = subjectSelect.value;
            const message = messageInput.value.trim();

            // Final validation
            let isValid = true;

            if (name.length < 2) {
                showFeedback(nameInput, "name-feedback", "Name is required.", true);
                isValid = false;
            }
            if (!email.includes("@") || !email.includes(".")) {
                showFeedback(emailInput, "email-feedback", "Valid email is required.", true);
                isValid = false;
            }
            if (subject === "") {
                showFeedback(subjectSelect, "subject-feedback", "Please select a subject.", true);
                isValid = false;
            }
            if (message.length < 10) {
                showFeedback(messageInput, "message-feedback", "Message must be at least 10 characters.", true);
                isValid = false;
            }

            if (!isValid) return;

            // Success! In a real app, you would send this data to an API
            console.log("Form data:", { name, email, subject, message });
            form.style.display = "none";
            successMessage.style.display = "block";
        });
    </script>
</body>
</html>
```

---

## Common Patterns

These are practical, copy-pasteable patterns you will use frequently when building interactive pages.

### Toggle Show/Hide

Show or hide an element by toggling a CSS class:

```css
/* style.css */
.hidden {
    display: none;
}
```

```html
<button id="toggle-btn">Toggle Details</button>
<div id="details">
    <p>Here are the details you can show or hide.</p>
</div>
```

```javascript
const toggleBtn = document.querySelector("#toggle-btn");
const details = document.querySelector("#details");

toggleBtn.addEventListener("click", function () {
    details.classList.toggle("hidden");

    // Update button text based on visibility
    if (details.classList.contains("hidden")) {
        toggleBtn.textContent = "Show Details";
    } else {
        toggleBtn.textContent = "Hide Details";
    }
});
```

### Dark Mode Toggle

A complete dark mode implementation using `classList.toggle` on the `<body>`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Dark Mode Toggle</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: Arial, sans-serif;
            padding: 40px;
            background-color: #ffffff;
            color: #333333;
            transition: background-color 0.3s, color 0.3s;
        }

        /* Dark mode styles */
        body.dark-mode {
            background-color: #1a1a2e;
            color: #e0e0e0;
        }

        .card {
            background-color: #f8f9fa;
            border: 1px solid #ddd;
            padding: 20px;
            border-radius: 8px;
            margin: 20px 0;
            transition: background-color 0.3s, border-color 0.3s;
        }

        body.dark-mode .card {
            background-color: #16213e;
            border-color: #444;
        }

        .theme-toggle {
            padding: 10px 20px;
            border: 2px solid #333;
            border-radius: 20px;
            background: none;
            cursor: pointer;
            font-size: 1rem;
            color: inherit;
            transition: border-color 0.3s;
        }

        body.dark-mode .theme-toggle {
            border-color: #e0e0e0;
        }
    </style>
</head>
<body>
    <button class="theme-toggle" id="theme-btn">Switch to Dark Mode</button>

    <h1>My Website</h1>
    <div class="card">
        <h2>Welcome</h2>
        <p>This page supports dark mode. Click the button above to toggle.</p>
    </div>

    <script>
        const themeBtn = document.querySelector("#theme-btn");

        themeBtn.addEventListener("click", function () {
            document.body.classList.toggle("dark-mode");

            if (document.body.classList.contains("dark-mode")) {
                themeBtn.textContent = "Switch to Light Mode";
            } else {
                themeBtn.textContent = "Switch to Dark Mode";
            }
        });
    </script>
</body>
</html>
```

### Mobile Navigation Menu

A hamburger menu that opens and closes on mobile:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mobile Nav</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: Arial, sans-serif; }

        .navbar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px 20px;
            background-color: #1a1a2e;
            color: white;
        }

        .nav-links {
            list-style: none;
            display: flex;
            gap: 20px;
        }

        .nav-links a {
            color: white;
            text-decoration: none;
        }

        .hamburger {
            display: none;
            background: none;
            border: none;
            color: white;
            font-size: 1.5rem;
            cursor: pointer;
        }

        @media (max-width: 768px) {
            .hamburger {
                display: block;
            }

            .nav-links {
                display: none;
                flex-direction: column;
                position: absolute;
                top: 55px;
                left: 0;
                right: 0;
                background-color: #1a1a2e;
                padding: 20px;
                gap: 15px;
            }

            .nav-links.open {
                display: flex;
            }
        }
    </style>
</head>
<body>
    <nav class="navbar">
        <div class="logo">MySite</div>
        <button class="hamburger" id="hamburger">&#9776;</button>
        <ul class="nav-links" id="nav-links">
            <li><a href="/">Home</a></li>
            <li><a href="/about">About</a></li>
            <li><a href="/projects">Projects</a></li>
            <li><a href="/contact">Contact</a></li>
        </ul>
    </nav>

    <script>
        const hamburger = document.querySelector("#hamburger");
        const navLinks = document.querySelector("#nav-links");

        hamburger.addEventListener("click", function () {
            navLinks.classList.toggle("open");

            // Toggle between hamburger and close icon
            if (navLinks.classList.contains("open")) {
                hamburger.textContent = "\u2715"; // X symbol
            } else {
                hamburger.textContent = "\u2630"; // Hamburger symbol
            }
        });

        // Close menu when a link is clicked
        navLinks.addEventListener("click", function (event) {
            if (event.target.matches("a")) {
                navLinks.classList.remove("open");
                hamburger.textContent = "\u2630";
            }
        });
    </script>
</body>
</html>
```

### Tab Switching

A tabbed content interface built entirely with JavaScript:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Tabs</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: Arial, sans-serif; padding: 40px; max-width: 600px; margin: 0 auto; }

        .tab-buttons {
            display: flex;
            border-bottom: 2px solid #ddd;
        }

        .tab-btn {
            padding: 10px 24px;
            border: none;
            background: none;
            cursor: pointer;
            font-size: 1rem;
            color: #666;
            border-bottom: 2px solid transparent;
            margin-bottom: -2px;
            transition: color 0.2s, border-color 0.2s;
        }

        .tab-btn:hover { color: #333; }

        .tab-btn.active {
            color: #3b82f6;
            border-bottom-color: #3b82f6;
            font-weight: bold;
        }

        .tab-panel {
            display: none;
            padding: 20px 0;
        }

        .tab-panel.active {
            display: block;
        }
    </style>
</head>
<body>
    <h1>Product Info</h1>

    <div class="tab-buttons">
        <button class="tab-btn active" data-tab="description">Description</button>
        <button class="tab-btn" data-tab="specs">Specifications</button>
        <button class="tab-btn" data-tab="reviews">Reviews</button>
    </div>

    <div class="tab-panel active" id="description">
        <h2>Product Description</h2>
        <p>This is an amazing product that will change your life.</p>
    </div>

    <div class="tab-panel" id="specs">
        <h2>Specifications</h2>
        <ul>
            <li>Weight: 2.5 lbs</li>
            <li>Dimensions: 10 x 5 x 3 inches</li>
            <li>Material: Aluminum</li>
        </ul>
    </div>

    <div class="tab-panel" id="reviews">
        <h2>Customer Reviews</h2>
        <p><strong>Alice:</strong> Great product! 5 stars.</p>
        <p><strong>Bob:</strong> Good value for the price. 4 stars.</p>
    </div>

    <script>
        const tabButtons = document.querySelector(".tab-buttons");

        // Event delegation -- one listener for all tab buttons
        tabButtons.addEventListener("click", function (event) {
            if (!event.target.matches(".tab-btn")) return;

            const targetTab = event.target.dataset.tab;

            // Deactivate all buttons and panels
            document.querySelectorAll(".tab-btn").forEach((btn) => {
                btn.classList.remove("active");
            });
            document.querySelectorAll(".tab-panel").forEach((panel) => {
                panel.classList.remove("active");
            });

            // Activate the clicked button and its panel
            event.target.classList.add("active");
            document.querySelector(`#${targetTab}`).classList.add("active");
        });
    </script>
</body>
</html>
```

### Back-to-Top Button

A button that appears when you scroll down and smoothly scrolls back to the top:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Back to Top</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            padding: 20px;
            max-width: 700px;
            margin: 0 auto;
        }

        .back-to-top {
            position: fixed;
            bottom: 30px;
            right: 30px;
            padding: 12px 16px;
            background-color: #3b82f6;
            color: white;
            border: none;
            border-radius: 50%;
            font-size: 1.2rem;
            cursor: pointer;
            opacity: 0;
            visibility: hidden;
            transition: opacity 0.3s, visibility 0.3s;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
        }

        .back-to-top.visible {
            opacity: 1;
            visibility: visible;
        }

        .back-to-top:hover {
            background-color: #2563eb;
        }

        /* Lots of content to make the page scroll */
        section { margin: 40px 0; }
    </style>
</head>
<body>
    <h1>Long Page Example</h1>

    <section><h2>Section 1</h2><p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris.</p></section>
    <section><h2>Section 2</h2><p>Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident.</p></section>
    <section><h2>Section 3</h2><p>Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam.</p></section>
    <section><h2>Section 4</h2><p>Nemo enim ipsam voluptatem quia voluptas sit aspernatur aut odit aut fugit, sed quia consequuntur magni dolores eos.</p></section>
    <section><h2>Section 5</h2><p>At vero eos et accusamus et iusto odio dignissimos ducimus qui blanditiis praesentium voluptatum deleniti atque corrupti.</p></section>
    <section><h2>Section 6</h2><p>Et harum quidem rerum facilis est et expedita distinctio. Nam libero tempore, cum soluta nobis est eligendi optio cumque nihil impedit.</p></section>
    <section><h2>Section 7</h2><p>Temporibus autem quibusdam et aut officiis debitis aut rerum necessitatibus saepe eveniet ut et voluptates repudiandae sint et molestiae non recusandae.</p></section>
    <section><h2>Section 8</h2><p>Itaque earum rerum hic tenetur a sapiente delectus, ut aut reiciendis voluptatibus maiores alias consequatur aut perferendis doloribus asperiores repellat.</p></section>

    <button class="back-to-top" id="back-to-top" title="Back to top">&uarr;</button>

    <script>
        const backToTopBtn = document.querySelector("#back-to-top");

        // Show/hide button based on scroll position
        window.addEventListener("scroll", function () {
            if (window.scrollY > 300) {
                backToTopBtn.classList.add("visible");
            } else {
                backToTopBtn.classList.remove("visible");
            }
        });

        // Smooth scroll to top when clicked
        backToTopBtn.addEventListener("click", function () {
            window.scrollTo({
                top: 0,
                behavior: "smooth"
            });
        });
    </script>
</body>
</html>
```

### Character Counter for a Textarea

Shows remaining characters and changes color as you approach the limit:

```html
<div class="form-group">
    <label for="bio">Bio (max 200 characters)</label>
    <textarea id="bio" maxlength="200" placeholder="Tell us about yourself..."></textarea>
    <div id="char-count">0 / 200 characters</div>
</div>

<style>
    #char-count {
        font-size: 0.85rem;
        color: #666;
        margin-top: 4px;
    }
    #char-count.warning { color: #f59e0b; }
    #char-count.danger { color: #ef4444; font-weight: bold; }
</style>
```

```javascript
const bio = document.querySelector("#bio");
const charCount = document.querySelector("#char-count");
const maxChars = 200;

bio.addEventListener("input", function () {
    const current = bio.value.length;
    const remaining = maxChars - current;

    charCount.textContent = `${current} / ${maxChars} characters`;

    // Change color based on how close to the limit
    charCount.classList.remove("warning", "danger");
    if (remaining <= 20) {
        charCount.classList.add("danger");
    } else if (remaining <= 50) {
        charCount.classList.add("warning");
    }
});
```

### Accordion / FAQ Section

An accordion where clicking a question reveals its answer:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>FAQ Accordion</title>
    <style>
        body { font-family: Arial, sans-serif; max-width: 600px; margin: 40px auto; padding: 0 20px; }

        .faq-item {
            border: 1px solid #ddd;
            border-radius: 8px;
            margin-bottom: 8px;
            overflow: hidden;
        }

        .faq-question {
            width: 100%;
            padding: 16px;
            background: #f8f9fa;
            border: none;
            text-align: left;
            font-size: 1rem;
            font-weight: bold;
            cursor: pointer;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .faq-question:hover { background: #e9ecef; }

        .faq-answer {
            max-height: 0;
            overflow: hidden;
            transition: max-height 0.3s ease, padding 0.3s ease;
            padding: 0 16px;
        }

        .faq-item.open .faq-answer {
            max-height: 200px;
            padding: 16px;
        }

        .faq-item.open .faq-question .arrow {
            transform: rotate(180deg);
        }

        .arrow {
            transition: transform 0.3s;
        }
    </style>
</head>
<body>
    <h1>Frequently Asked Questions</h1>

    <div class="faq-list">
        <div class="faq-item">
            <button class="faq-question">
                What is the DOM?
                <span class="arrow">&#9660;</span>
            </button>
            <div class="faq-answer">
                <p>The DOM (Document Object Model) is a tree-like representation of your HTML page that JavaScript can interact with to make dynamic changes.</p>
            </div>
        </div>

        <div class="faq-item">
            <button class="faq-question">
                What is event bubbling?
                <span class="arrow">&#9660;</span>
            </button>
            <div class="faq-answer">
                <p>Event bubbling is when an event fired on a child element propagates up through its parent elements toward the document root.</p>
            </div>
        </div>

        <div class="faq-item">
            <button class="faq-question">
                When should I use event delegation?
                <span class="arrow">&#9660;</span>
            </button>
            <div class="faq-answer">
                <p>Use event delegation when you have many similar child elements that need the same event handler, or when elements are added dynamically after page load.</p>
            </div>
        </div>
    </div>

    <script>
        const faqList = document.querySelector(".faq-list");

        // Event delegation: one listener for all FAQ items
        faqList.addEventListener("click", function (event) {
            const question = event.target.closest(".faq-question");
            if (!question) return;

            const faqItem = question.closest(".faq-item");

            // Close all other items (optional: for single-open behavior)
            document.querySelectorAll(".faq-item").forEach((item) => {
                if (item !== faqItem) {
                    item.classList.remove("open");
                }
            });

            // Toggle the clicked item
            faqItem.classList.toggle("open");
        });
    </script>
</body>
</html>
```

---

## DOMContentLoaded & Script Placement

### Why Script Placement Matters

JavaScript runs as soon as the browser encounters it. If your script is in the `<head>` and tries to select elements in the `<body>`, those elements do not exist yet -- and your code will fail silently (returning `null`).

```html
<!-- PROBLEM: Script runs before <h1> exists -->
<head>
    <script>
        const heading = document.querySelector("h1");
        console.log(heading); // null! The <h1> hasn't been parsed yet
        heading.textContent = "Hello"; // TypeError: Cannot set properties of null
    </script>
</head>
<body>
    <h1>Welcome</h1>
</body>
```

There are three solutions to this problem:

### Solution 1: Place Script at End of Body (Simplest)

Put your `<script>` tag right before the closing `</body>` tag. By the time the browser reaches it, all HTML elements are available.

```html
<body>
    <h1>Welcome</h1>
    <p>Content here...</p>

    <!-- Script runs after all elements above are parsed -->
    <script src="script.js"></script>
</body>
```

{{< callout type="important" >}}
This is the approach used in all the examples in these notes and is the simplest, most reliable method. When in doubt, put your script at the end of the body.
{{< /callout >}}

### Solution 2: The defer Attribute

Adding `defer` to a `<script>` tag in the `<head>` tells the browser to download the script immediately but wait to execute it until the HTML is fully parsed.

```html
<head>
    <!-- Downloads immediately, runs after HTML is parsed -->
    <script src="script.js" defer></script>
</head>
<body>
    <h1>Welcome</h1>
</body>
```

**Advantages of `defer`:**
- The script starts downloading earlier (while HTML is being parsed)
- Multiple deferred scripts run in order
- The page is not blocked while the script downloads

### Solution 3: DOMContentLoaded Event

You can put your script anywhere and wrap your code in a `DOMContentLoaded` event listener. This event fires when the HTML is fully parsed, regardless of where the script is placed.

```html
<head>
    <script>
        document.addEventListener("DOMContentLoaded", function () {
            // This code runs AFTER all HTML is parsed
            const heading = document.querySelector("h1");
            heading.textContent = "Hello!"; // Works perfectly
        });
    </script>
</head>
<body>
    <h1>Welcome</h1>
</body>
```

### Which Approach Should You Use?

{{< tabs items="Script at End of Body,defer Attribute,DOMContentLoaded" >}}
  {{< tab >}}

**Script at end of `<body>`** -- the simplest approach.

```html
<body>
    <!-- All your HTML content -->
    <script src="script.js"></script>
</body>
```

**When to use:** This is the default choice for this course. Simple, reliable, and easy to understand.

  {{< /tab >}}
  {{< tab >}}

**`defer` attribute** -- the modern professional approach.

```html
<head>
    <script src="script.js" defer></script>
</head>
```

**When to use:** Professional projects and frameworks. Allows the browser to start downloading the script earlier for better performance.

  {{< /tab >}}
  {{< tab >}}

**`DOMContentLoaded`** -- the event-based approach.

```javascript
document.addEventListener("DOMContentLoaded", function () {
    // Your code here
});
```

**When to use:** When you cannot control where the script tag is placed, or when you need to ensure the DOM is ready in a script that could be loaded in different ways.

  {{< /tab >}}
{{< /tabs >}}

### DOMContentLoaded vs load

| Event | Fires When | Use Case |
|-------|-----------|----------|
| `DOMContentLoaded` | HTML is fully parsed (DOM tree is built) | Most common -- for DOM manipulation |
| `load` | HTML, CSS, images, and all resources are fully loaded | When you need images/resources ready |

```javascript
// Fires when HTML is parsed (fast)
document.addEventListener("DOMContentLoaded", function () {
    console.log("DOM is ready!");
});

// Fires when EVERYTHING is loaded, including images (slow)
window.addEventListener("load", function () {
    console.log("Everything is loaded, including images!");
});
```

---

## Traversing the DOM

Sometimes you need to navigate from one element to a related element -- a parent, a sibling, or a child. The DOM provides properties for traversing the tree.

### Parent, Children, and Siblings

```javascript
const item = document.querySelector(".item");

// Parent
item.parentElement;           // The direct parent element

// Children
item.children;                // HTMLCollection of child elements
item.firstElementChild;       // First child element
item.lastElementChild;        // Last child element
item.children.length;         // Number of child elements

// Siblings
item.nextElementSibling;      // Next sibling element
item.previousElementSibling;  // Previous sibling element
```

```
  DOM Tree Relationships:

  ┌─────────────────────────────┐
  │  <ul> (parent)              │
  │  ┌─────┐ ┌─────┐ ┌─────┐  │
  │  │<li> │→│<li> │→│<li> │  │
  │  │  A  │ │  B  │ │  C  │  │
  │  │     │←│     │←│     │  │
  │  └─────┘ └─────┘ └─────┘  │
  └─────────────────────────────┘

  If B is selected:
  B.parentElement        → <ul>
  B.nextElementSibling   → C
  B.previousElementSibling → A
```

### closest -- Finding the Nearest Ancestor

`element.closest(selector)` walks up the DOM tree and returns the first ancestor (including the element itself) that matches the selector. This is incredibly useful for event delegation.

```javascript
// Given deeply nested HTML:
// <div class="card">
//   <div class="card-body">
//     <button class="delete-btn">Delete</button>
//   </div>
// </div>

deleteBtn.addEventListener("click", function (event) {
    // Find the nearest .card ancestor
    const card = event.target.closest(".card");
    card.remove(); // Remove the entire card
});
```

### Practical Example: DOM Traversal

```html
<ul class="todo-list">
    <li class="todo-item">
        <span>Buy groceries</span>
        <button class="complete-btn">Done</button>
    </li>
    <li class="todo-item">
        <span>Clean house</span>
        <button class="complete-btn">Done</button>
    </li>
</ul>
```

```javascript
const todoList = document.querySelector(".todo-list");

todoList.addEventListener("click", function (event) {
    if (event.target.matches(".complete-btn")) {
        // Navigate from the button to the parent <li>
        const todoItem = event.target.closest(".todo-item");

        // Cross out the text
        const textSpan = todoItem.querySelector("span");
        textSpan.style.textDecoration = "line-through";
        textSpan.style.color = "#999";

        // Disable the button
        event.target.disabled = true;
        event.target.textContent = "Completed";
    }
});
```

---

## Troubleshooting & Common Mistakes

{{% details title="TypeError: Cannot read properties of null" closed="true" %}}
This is the **most common DOM error**. It means `querySelector` returned `null` because it could not find the element.

**Common causes:**
1. Your script runs before the DOM is ready (script in `<head>` without `defer`)
2. A typo in the selector (class name, ID, or tag name)
3. The element does not exist in the HTML

```javascript
// WRONG: element does not exist
const btn = document.querySelector("#my-buton"); // Typo! Should be "button"
btn.addEventListener("click", handleClick); // TypeError!

// FIX: Check the selector and ensure the element exists
const btn = document.querySelector("#my-button");
if (btn) {
    btn.addEventListener("click", handleClick);
} else {
    console.error("Button not found! Check the selector.");
}
```
{{% /details %}}

{{% details title="Forgetting event.preventDefault() on forms" closed="true" %}}
Without `preventDefault()`, form submissions reload the page and all your JavaScript state is lost.

```javascript
// WRONG: page reloads, form data is lost
form.addEventListener("submit", function (event) {
    console.log("Processing..."); // Runs, then page reloads immediately
});

// FIX: prevent the default behavior
form.addEventListener("submit", function (event) {
    event.preventDefault(); // Stop the reload!
    console.log("Processing..."); // Runs and the page stays
});
```
{{% /details %}}

{{% details title="Using innerHTML when textContent is safer" closed="true" %}}
If you use `innerHTML` with user input, you open yourself to XSS attacks.

```javascript
const userInput = document.querySelector("#search").value;

// DANGEROUS: if userInput contains HTML/script tags, they will execute
resultsDiv.innerHTML = userInput;

// SAFE: textContent treats everything as plain text
resultsDiv.textContent = userInput;
```
{{% /details %}}

{{% details title="style.background-color vs style.backgroundColor (camelCase)" closed="true" %}}
CSS properties with hyphens must be written in **camelCase** in JavaScript.

```javascript
// WRONG: hyphenated property names don't work
element.style.background-color = "red";    // SyntaxError!
element.style.font-size = "16px";          // SyntaxError!

// FIX: use camelCase
element.style.backgroundColor = "red";     // Correct
element.style.fontSize = "16px";           // Correct
element.style.borderRadius = "8px";        // Correct
element.style.marginTop = "20px";          // Correct
```
{{% /details %}}

{{% details title="Selecting elements before the DOM is ready" closed="true" %}}
If your script is in the `<head>` without `defer`, elements have not been parsed yet.

```html
<!-- WRONG: script runs before body is parsed -->
<head>
    <script>
        const btn = document.querySelector("#btn"); // null!
    </script>
</head>

<!-- FIX 1: Move script to end of body -->
<body>
    <button id="btn">Click</button>
    <script>
        const btn = document.querySelector("#btn"); // Works!
    </script>
</body>

<!-- FIX 2: Use defer -->
<head>
    <script src="app.js" defer></script>
</head>

<!-- FIX 3: Use DOMContentLoaded -->
<head>
    <script>
        document.addEventListener("DOMContentLoaded", function () {
            const btn = document.querySelector("#btn"); // Works!
        });
    </script>
</head>
```
{{% /details %}}

{{% details title="NodeList is not an Array" closed="true" %}}
`querySelectorAll` returns a **NodeList**, which supports `forEach` but not `map`, `filter`, or `reduce`. Convert it to an array first if you need those methods.

```javascript
const items = document.querySelectorAll(".item");

// forEach works on NodeList
items.forEach((item) => console.log(item));

// map does NOT work on NodeList
// items.map((item) => item.textContent); // TypeError!

// FIX: Convert to array first
const itemsArray = Array.from(items);
const texts = itemsArray.map((item) => item.textContent); // Works!

// Or use the spread operator:
const textsAlt = [...items].map((item) => item.textContent); // Also works!
```
{{% /details %}}

{{% details title="Event listener does not fire" closed="true" %}}
If your event listener seems to do nothing, check these common causes:

1. **Wrong selector** -- the element was not found (check for typos)
2. **Script order** -- the script runs before the element exists in the DOM
3. **Wrong event type** -- using `"change"` when you meant `"input"`, or `"click"` on a form instead of `"submit"`
4. **Calling the function instead of passing it** -- parentheses `()` call the function immediately

```javascript
// WRONG: handleClick() is called immediately, not when clicked
button.addEventListener("click", handleClick());

// FIX: pass the function reference (no parentheses)
button.addEventListener("click", handleClick);
```
{{% /details %}}

{{% details title="Checkbox uses .checked, not .value" closed="true" %}}
For checkboxes and radio buttons, use `.checked` to get the boolean state:

```javascript
const checkbox = document.querySelector("#agree");

// WRONG: .value gives the HTML value attribute, not the checked state
console.log(checkbox.value); // "on" (always, regardless of checked state)

// FIX: use .checked for boolean state
console.log(checkbox.checked); // true or false
```
{{% /details %}}

---

## Key Takeaways

{{< callout emoji="🎯" >}}
- The **DOM** is the browser's in-memory tree of your HTML. JavaScript interacts with this tree, not the HTML file directly.
- The fundamental pattern is **Select, Listen, Modify** -- find an element, listen for an event, then change something.
- Use **`querySelector`** and **`querySelectorAll`** for selecting elements -- they use CSS selector syntax you already know.
- **`textContent`** is safe for user input. **`innerHTML`** renders HTML tags and has XSS risks.
- Use **`classList`** (add, remove, toggle, contains) to manage CSS classes -- keep styles in CSS.
- The **Create, Configure, Append** pattern is how you add new elements to the page.
- **`addEventListener`** is how you make pages interactive -- it listens for events like clicks, typing, and form submissions.
- The **event object** gives you information about the event -- `event.target`, `event.type`, `event.key`, etc.
- **`event.preventDefault()`** stops default browser behavior -- essential for forms.
- Events **bubble up** the DOM tree. Use **event delegation** to efficiently handle events on many child elements.
- Place scripts at the **end of `<body>`** or use the **`defer`** attribute so the DOM is ready when your code runs.
- When something does not work, check the **console for errors** -- `null` errors almost always mean the element was not found.
{{< /callout >}}

---

## Additional Resources

### Official Documentation

{{< cards >}}
  {{< card link="https://javascript.info/document" title="javascript.info: DOM" subtitle="Comprehensive DOM tutorial with interactive examples" icon="book-open" >}}
  {{< card link="https://javascript.info/events" title="javascript.info: Events" subtitle="Complete guide to browser events" icon="book-open" >}}
  {{< card link="https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction" title="MDN: DOM Introduction" subtitle="Official DOM documentation" icon="document-text" >}}
  {{< card link="https://developer.mozilla.org/en-US/docs/Web/Events" title="MDN: Event Reference" subtitle="Complete list of all browser events" icon="document-text" >}}
{{< /cards >}}

### Interactive Learning

{{< cards >}}
  {{< card link="https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Client-side_web_APIs/Manipulating_documents" title="MDN: Manipulating Documents" subtitle="Step-by-step DOM manipulation tutorial" icon="academic-cap" >}}
  {{< card link="https://javascript.info/introduction-browser-events" title="javascript.info: Browser Events" subtitle="Interactive event handling tutorial" icon="academic-cap" >}}
{{< /cards >}}

### Further Reading

{{< cards >}}
  {{< card link="https://javascript.info/event-delegation" title="Event Delegation Pattern" subtitle="Deep dive into event delegation" icon="link" >}}
  {{< card link="https://developer.mozilla.org/en-US/docs/Web/API/Event" title="MDN: Event Interface" subtitle="Complete Event object reference" icon="document-text" >}}
{{< /cards >}}
