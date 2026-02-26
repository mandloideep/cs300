---
title: "Advanced DOM Patterns & Data Persistence"
weight: 2
toc: true
---

## Introduction

In Session 1, you learned the fundamental DOM pattern: **Select** an element, **Listen** for an event, **Modify** the page. You used `querySelector`, `addEventListener`, and methods like `classList.toggle` and `textContent` to build interactive features. That pattern is the foundation of everything you will build in this course.

But Session 1's approach has a limitation. When you add event listeners to elements that already exist on the page, **new elements created later with JavaScript do not get those listeners**. If you build a task list and add new tasks dynamically, clicking a delete button on a new task does nothing -- because the event listener was set up before the task existed.

This session introduces five patterns that solve this problem and take your DOM skills to the next level:

1. **Event Delegation** -- Handle events on elements that do not exist yet
2. **DOM Traversal** -- Navigate from one element to related elements (parent, children, siblings)
3. **Data Attributes** -- Store custom metadata directly on HTML elements
4. **Template Literals for Dynamic HTML** -- Build HTML strings with embedded data
5. **localStorage** -- Save data that survives page refreshes

Together, these patterns let you build features like dynamic lists, shopping carts, task managers, and persistent dark mode toggles -- the kinds of interactive features that make your Group Project 1 feel like a real application.

```
  Session 1 Pattern:           Session 2 Patterns:
  ─────────────────           ──────────────────────
  Select → Listen → Modify    + Event Delegation
                              + DOM Traversal
                              + Data Attributes
                              + Template Literals
                              + localStorage
```

{{< callout type="info" >}}
These five patterns are the building blocks for every dynamic feature in GP1. A shopping cart, a task list, an FAQ accordion, a dark mode toggle with persistence -- all of them combine two or more of these patterns.
{{< /callout >}}

---

## Event Delegation

### The Problem: New Elements Don't Get Listeners

Consider this code that adds a click-to-delete handler on every `.task-item`:

```javascript
// Add a click listener to each existing task item
const items = document.querySelectorAll(".task-item");
items.forEach(item => {
    item.addEventListener("click", () => item.remove());
});
```

This works perfectly for items already on the page. But now add a new task with JavaScript:

```javascript
const newTask = document.createElement("div");
newTask.className = "task-item";
newTask.textContent = "New task";
document.querySelector("#task-list").appendChild(newTask);
```

Click the new task -- **nothing happens**. The new element was not there when `querySelectorAll` ran, so it never got a listener.

You might think: "I will add a listener every time I create a new element." That works, but it is messy, error-prone, and gets complicated fast. There is a much better solution.

### The Solution: Listen on the Parent

Instead of attaching listeners to each child element, attach **one listener to the parent**. When a child is clicked, the event **bubbles up** through the DOM tree to the parent, where your listener catches it. This is called **event delegation**.

```javascript
const taskList = document.querySelector("#task-list");

// ONE listener on the parent -- handles ALL child clicks
taskList.addEventListener("click", (event) => {
    // event.target is the element that was actually clicked
    if (event.target.classList.contains("delete-btn")) {
        event.target.closest(".task-item").remove();
    }
});
```

This listener catches clicks on **every** delete button inside `#task-list` -- including buttons on elements that will be added in the future. The parent was there from the start, so it always has its listener.

### Why It Works: Event Bubbling

When you click a button inside a list, the click event does not just fire on the button. It **bubbles up** through every ancestor element:

```
  Event Bubbling:

  ┌────────────────────────────────────────────────┐
  │ #task-list (parent)               ③ CAUGHT!    │
  │  ┌──────────────────────────────────────────┐  │
  │  │ .task-item                     ②         │  │
  │  │  ┌────────────────────────────────────┐  │  │
  │  │  │ .delete-btn                ①      │  │  │
  │  │  │   USER CLICKS HERE               │  │  │
  │  │  └────────────────────────────────────┘  │  │
  │  └──────────────────────────────────────────┘  │
  └────────────────────────────────────────────────┘

  Click fires: ① button → ② .task-item → ③ #task-list
```

The listener on `#task-list` catches the event at step 3. Then `event.target` tells you which element was actually clicked (the button at step 1).

### event.target vs event.currentTarget

These two properties answer different questions:

| Property | Meaning | Changes? |
|----------|---------|----------|
| `event.target` | The element the user actually clicked | Yes -- depends on what was clicked |
| `event.currentTarget` | The element the listener is attached to | No -- always the parent |

```javascript
taskList.addEventListener("click", (event) => {
    console.log(event.target);        // The specific element clicked (e.g., delete button)
    console.log(event.currentTarget); // Always #task-list (where the listener lives)
});
```

### Checking What Was Clicked

Since the parent catches **all** clicks inside it, you need to check whether the clicked element is one you care about. There are several ways to do this:

```javascript
taskList.addEventListener("click", (event) => {
    // Method 1: Check for a specific class
    if (event.target.classList.contains("delete-btn")) {
        // Handle delete button click
    }

    // Method 2: Use matches() with any CSS selector
    if (event.target.matches(".delete-btn")) {
        // Same result, but supports complex selectors
    }

    // Method 3: Check the tag name
    if (event.target.tagName === "BUTTON") {
        // Any button inside the list was clicked
    }
});
```

{{< callout type="info" >}}
`matches()` is often the best choice because it accepts any CSS selector, just like `querySelector`. You can write `event.target.matches(".card .delete-btn")` for more specific targeting.
{{< /callout >}}

### closest() -- Finding the Right Container

When the user clicks a delete button, you usually want to remove the entire task item, not just the button. But `event.target` is the button, not the task item. Use `closest()` to walk **up** the DOM tree and find the nearest ancestor that matches a selector:

```javascript
taskList.addEventListener("click", (event) => {
    if (event.target.matches(".delete-btn")) {
        // Walk UP from the button to find the .task-item container
        const taskItem = event.target.closest(".task-item");
        taskItem.remove(); // Remove the entire task
    }
});
```

```
  How closest() works:

  User clicks: .delete-btn
  closest(".task-item") walks UP:

  .delete-btn  →  .task-item  ← FOUND! Returns this element
                       ↑
                  #task-list
                       ↑
                     body
```

If `closest()` reaches the top of the document without finding a match, it returns `null`.

### Complete Event Delegation Example

Here is a complete, working example that demonstrates event delegation with add and delete functionality:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Event Delegation Demo</title>
    <style>
        body { font-family: Arial, sans-serif; max-width: 500px; margin: 40px auto; padding: 0 20px; }
        .task-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px;
            margin: 8px 0;
            background: #f8f9fa;
            border-radius: 6px;
            border: 1px solid #dee2e6;
        }
        .delete-btn {
            background: #ef4444;
            color: white;
            border: none;
            padding: 6px 12px;
            border-radius: 4px;
            cursor: pointer;
        }
        .delete-btn:hover { background: #dc2626; }
        .add-section { display: flex; gap: 8px; margin-bottom: 16px; }
        .add-section input { flex: 1; padding: 8px; border: 1px solid #dee2e6; border-radius: 4px; }
        .add-section button { padding: 8px 16px; background: #3b82f6; color: white; border: none; border-radius: 4px; cursor: pointer; }
    </style>
</head>
<body>
    <h1>Task List</h1>

    <div class="add-section">
        <input type="text" id="task-input" placeholder="Enter a new task...">
        <button id="add-btn">Add</button>
    </div>

    <div id="task-list">
        <div class="task-item">
            <span>Buy groceries</span>
            <button class="delete-btn">Delete</button>
        </div>
        <div class="task-item">
            <span>Finish GP1</span>
            <button class="delete-btn">Delete</button>
        </div>
    </div>

    <script>
        const taskList = document.querySelector("#task-list");
        const taskInput = document.querySelector("#task-input");
        const addBtn = document.querySelector("#add-btn");

        // EVENT DELEGATION: One listener handles all delete buttons
        taskList.addEventListener("click", (event) => {
            if (event.target.matches(".delete-btn")) {
                const taskItem = event.target.closest(".task-item");
                taskItem.remove();
            }
        });

        // Add new task
        addBtn.addEventListener("click", () => {
            const text = taskInput.value.trim();
            if (text === "") return;

            // Create new task element
            const newTask = document.createElement("div");
            newTask.className = "task-item";
            newTask.innerHTML = `
                <span>${text}</span>
                <button class="delete-btn">Delete</button>
            `;
            taskList.appendChild(newTask);
            taskInput.value = ""; // Clear input
        });
    </script>
</body>
</html>
```

Notice that the delete button on newly added tasks **works immediately** -- no extra event listener setup needed. The parent's listener handles everything.

### When to Use Event Delegation

| Situation | Use Delegation? | Why |
|-----------|----------------|-----|
| Items added/removed dynamically | **Yes** | New items need event handling without extra setup |
| Many similar items (50+ buttons) | **Yes** | 1 listener is more efficient than 50 |
| A single, static button | No | Direct listener is simpler and clearer |
| A form that never changes | No | Direct listener is fine |
| Interactive lists, carts, comments | **Yes** | These are dynamic by nature |

{{< callout type="important" >}}
**Rule of thumb:** If elements will be created or removed with JavaScript, use event delegation. If the elements are static and few in number, a direct listener is fine.
{{< /callout >}}

---

## DOM Traversal

### Why Traverse the DOM?

In Session 1, you always used `querySelector` to find elements by their CSS selector. That works, but sometimes you already have a reference to one element and need to find a **related** element -- a parent, a child, or a sibling.

For example, when a delete button is clicked inside a card, you need to find the card that contains that button. When a heading is clicked in an accordion, you need to find the answer that comes right after it. When you want to count items in a list, you need to access the list's children.

DOM traversal lets you navigate the tree from any element, without needing IDs or classes.

### Traversal Directions

From any element, you can go up, down, or sideways:

```
                parentElement / closest()
                        ↑
  previousElementSibling ← [current element] → nextElementSibling
                        ↓
          firstElementChild ... children ... lastElementChild
```

### Going Up: parentElement and closest()

**`parentElement`** returns the direct parent:

```javascript
const button = document.querySelector(".delete-btn");
const card = button.parentElement; // The element directly containing the button
```

**`closest(selector)`** walks up the tree and returns the first ancestor matching the selector:

```javascript
const button = document.querySelector(".delete-btn");
const card = button.closest(".card"); // Finds the nearest .card ancestor
```

`closest()` is usually better than `parentElement` because:

- It works regardless of how deeply the button is nested
- It uses CSS selectors, making it flexible
- It returns `null` if no match is found (safe and predictable)

```html
<!-- parentElement only gets the immediate parent -->
<div class="card">
    <div class="card-body">
        <div class="card-actions">
            <button class="delete-btn">Delete</button>
        </div>
    </div>
</div>
```

```javascript
const btn = document.querySelector(".delete-btn");

btn.parentElement;           // <div class="card-actions"> (immediate parent)
btn.parentElement.parentElement; // <div class="card-body"> (two levels up)
btn.closest(".card");        // <div class="card"> (skips straight to .card!)
```

### Going Down: children, firstElementChild, lastElementChild

```javascript
const list = document.querySelector("#task-list");

list.children;              // HTMLCollection of ALL child elements
list.children.length;       // How many children
list.children[0];           // First child (by index)
list.children[2];           // Third child (by index)
list.firstElementChild;     // First child element
list.lastElementChild;      // Last child element
```

{{< callout type="warning" >}}
`children` returns an **HTMLCollection**, not a NodeList. HTMLCollections do not have `forEach` by default. To loop over them, convert to an array first:

```javascript
Array.from(list.children).forEach(child => {
    console.log(child.textContent);
});

// Or use a for...of loop (works directly on HTMLCollections):
for (const child of list.children) {
    console.log(child.textContent);
}
```
{{< /callout >}}

### Going Sideways: nextElementSibling and previousElementSibling

```javascript
const current = document.querySelector(".active");

current.nextElementSibling;      // The element right after this one
current.previousElementSibling;  // The element right before this one
```

If there is no next or previous sibling, the property returns `null`.

### Why "Element" Matters in the Property Names

You may notice that these properties all have "Element" in the name: `parentElement`, `firstElementChild`, `nextElementSibling`. There are also versions without "Element": `parentNode`, `firstChild`, `nextSibling`. The difference is important:

| Property | Returns |
|----------|---------|
| `parentElement` | Only element nodes (HTML tags) |
| `parentNode` | Any node (including text, comments) |
| `firstElementChild` | First child that is an HTML element |
| `firstChild` | First child node (could be whitespace text) |
| `nextElementSibling` | Next sibling that is an HTML element |
| `nextSibling` | Next sibling node (could be whitespace text) |

**Always use the "Element" versions.** The non-Element versions include invisible text nodes (whitespace between tags) that will confuse you:

```html
<ul>
    <li>Item 1</li>
    <li>Item 2</li>
</ul>
```

```javascript
const ul = document.querySelector("ul");

ul.firstChild;          // A text node (the whitespace/newline before <li>)
ul.firstElementChild;   // <li>Item 1</li> (what you actually want!)
```

### Quick Reference Table

| Direction | Property | Returns | If Not Found |
|-----------|----------|---------|--------------|
| Up (parent) | `parentElement` | Single element | `null` |
| Up (ancestor) | `closest(selector)` | Single element | `null` |
| Down (first child) | `firstElementChild` | Single element | `null` |
| Down (last child) | `lastElementChild` | Single element | `null` |
| Down (all children) | `children` | HTMLCollection | Empty collection |
| Sideways (next) | `nextElementSibling` | Single element | `null` |
| Sideways (previous) | `previousElementSibling` | Single element | `null` |

### Practical Traversal Examples

**Accordion: Click a heading to toggle the answer below it:**

```javascript
const headings = document.querySelectorAll(".faq-question");

headings.forEach(heading => {
    heading.addEventListener("click", () => {
        // The answer is the NEXT element after the heading
        const answer = heading.nextElementSibling;
        answer.classList.toggle("hidden");
    });
});
```

**Delete button removes the entire card:**

```javascript
deleteBtn.addEventListener("click", (event) => {
    // Walk UP to find the card container
    const card = event.target.closest(".card");
    card.remove();
});
```

**Count items in a list:**

```javascript
const list = document.querySelector("#task-list");
const count = list.children.length;
console.log(`There are ${count} tasks`);
```

**Highlight the next sibling:**

```javascript
const current = document.querySelector(".active");
const next = current.nextElementSibling;
if (next) {
    current.classList.remove("active");
    next.classList.add("active");
}
```

### Combining Traversal with Event Delegation

Traversal and delegation work together constantly. Delegation tells you *what* was clicked. Traversal finds the *related* element you want to modify:

```javascript
const taskList = document.querySelector("#task-list");

taskList.addEventListener("click", (event) => {
    // Delegation: check what was clicked
    if (event.target.matches(".complete-btn")) {
        // Traversal: find the parent task item
        const taskItem = event.target.closest(".task-item");

        // Traversal: find the text span inside the task
        const textSpan = taskItem.querySelector("span");

        // Modify: toggle completed state
        textSpan.classList.toggle("completed");
    }
});
```

---

## Data Attributes

### What Are Data Attributes?

**Data attributes** let you store custom information directly on HTML elements. Any attribute that starts with `data-` is a data attribute, and you can name it anything you want.

```html
<div class="task-item"
     data-id="42"
     data-priority="high"
     data-completed="false"
     data-assigned-to="alice">
    Buy groceries
</div>
```

This element now carries metadata: its ID is 42, its priority is "high," it is not completed, and it is assigned to Alice. This data is invisible to the user but accessible to your JavaScript.

### Why Use Data Attributes?

Without data attributes, you would need to store this information in JavaScript variables, arrays, or objects -- and then figure out which variable corresponds to which DOM element. Data attributes keep the data right where it belongs: on the element itself.

**Common uses:**

| Use Case | Attribute | Example |
|----------|-----------|---------|
| Unique identifier | `data-id` | `data-id="42"` |
| Category or type | `data-category` | `data-category="electronics"` |
| State tracking | `data-completed` | `data-completed="true"` |
| Configuration | `data-max-items` | `data-max-items="10"` |
| Content linking | `data-tab` | `data-tab="settings"` |

### Reading Data Attributes with dataset

In JavaScript, every element has a `dataset` property that gives you access to all its `data-*` attributes:

```html
<div class="task-item" data-id="42" data-priority="high" data-completed="false">
    Buy groceries
</div>
```

```javascript
const task = document.querySelector(".task-item");

// Read data attributes
task.dataset.id;          // "42"
task.dataset.priority;    // "high"
task.dataset.completed;   // "false"
```

{{< callout type="warning" >}}
**Data attribute values are always strings.** Even if you write `data-id="42"`, `dataset.id` returns the string `"42"`, not the number `42`. And `data-completed="false"` returns the string `"false"`, not the boolean `false`.

```javascript
task.dataset.id === 42;      // false (string vs number)
task.dataset.id === "42";    // true

task.dataset.completed === false;    // false (string vs boolean)
task.dataset.completed === "false";  // true
```

Convert when needed:
```javascript
const id = Number(task.dataset.id);            // 42 (number)
const isDone = task.dataset.completed === "true"; // false (boolean)
```
{{< /callout >}}

### Naming Convention: Kebab-case to camelCase

HTML data attributes use **kebab-case** (hyphens). In JavaScript, they become **camelCase** (no hyphens):

| HTML Attribute | JavaScript Access |
|---------------|-------------------|
| `data-id` | `dataset.id` |
| `data-priority` | `dataset.priority` |
| `data-assigned-to` | `dataset.assignedTo` |
| `data-max-items` | `dataset.maxItems` |
| `data-is-completed` | `dataset.isCompleted` |

The rule: remove `data-`, drop all hyphens, and capitalize the letter after each hyphen.

### Setting Data Attributes

You can also **set** data attributes from JavaScript:

```javascript
const task = document.querySelector(".task-item");

// Set a data attribute
task.dataset.completed = "true";
// HTML now has: data-completed="true"

// Set a new data attribute that didn't exist before
task.dataset.lastModified = "2026-02-20";
// HTML now has: data-last-modified="2026-02-20"
```

### Practical Example: Using Data Attributes with Delegation

Data attributes shine when combined with event delegation. The parent listener catches the click, and data attributes tell you **which** item was clicked:

```javascript
const taskList = document.querySelector("#task-list");

taskList.addEventListener("click", (event) => {
    if (event.target.matches(".delete-btn")) {
        // Traverse up to the task item
        const taskItem = event.target.closest(".task-item");

        // Read the data attribute to identify the task
        const taskId = taskItem.dataset.id;
        console.log(`Deleting task with ID: ${taskId}`);

        // Remove from the page
        taskItem.remove();
    }
});
```

### Practical Example: Filter by Category

Data attributes are perfect for filtering content:

```html
<div class="filter-buttons">
    <button class="filter-btn" data-filter="all">All</button>
    <button class="filter-btn" data-filter="electronics">Electronics</button>
    <button class="filter-btn" data-filter="clothing">Clothing</button>
</div>

<div class="product-list">
    <div class="product" data-category="electronics">Laptop</div>
    <div class="product" data-category="clothing">T-Shirt</div>
    <div class="product" data-category="electronics">Phone</div>
    <div class="product" data-category="clothing">Jeans</div>
</div>
```

```javascript
const filterButtons = document.querySelector(".filter-buttons");
const products = document.querySelectorAll(".product");

filterButtons.addEventListener("click", (event) => {
    if (!event.target.matches(".filter-btn")) return;

    const filter = event.target.dataset.filter;

    products.forEach(product => {
        if (filter === "all" || product.dataset.category === filter) {
            product.style.display = "block";
        } else {
            product.style.display = "none";
        }
    });
});
```

### Practical Example: Tab Switching with Data Attributes

The tab pattern from Session 1's student notes uses `data-tab` to connect buttons to content panels:

```html
<div class="tabs">
    <button class="tab-btn active" data-tab="overview">Overview</button>
    <button class="tab-btn" data-tab="features">Features</button>
    <button class="tab-btn" data-tab="reviews">Reviews</button>
</div>

<div class="tab-panel active" id="overview">Overview content...</div>
<div class="tab-panel" id="features">Features content...</div>
<div class="tab-panel" id="reviews">Reviews content...</div>
```

```javascript
const tabContainer = document.querySelector(".tabs");

tabContainer.addEventListener("click", (event) => {
    if (!event.target.matches(".tab-btn")) return;

    // Read which tab to show from the data attribute
    const targetId = event.target.dataset.tab;

    // Deactivate all tabs and panels
    document.querySelectorAll(".tab-btn").forEach(btn => btn.classList.remove("active"));
    document.querySelectorAll(".tab-panel").forEach(panel => panel.classList.remove("active"));

    // Activate the clicked tab and its panel
    event.target.classList.add("active");
    document.querySelector(`#${targetId}`).classList.add("active");
});
```

{{< callout type="info" >}}
Data attributes are the "bridge" between your HTML and JavaScript. They let you store information in the HTML that your JavaScript can read and act on, without polluting your JS with hard-coded element relationships.
{{< /callout >}}

---

## Template Literals for Dynamic HTML

### The Problem: Building HTML Strings

When you create elements dynamically, you need to build HTML strings that include data. Regular string concatenation gets messy fast:

```javascript
// Messy string concatenation
const html = '<div class="task-item" data-id="' + id + '" data-priority="' + priority + '">' +
    '<span>' + text + '</span>' +
    '<button class="delete-btn">Delete</button>' +
    '</div>';
```

This is hard to read, hard to maintain, and easy to break with a missing quote or plus sign.

### The Solution: Template Literals

**Template literals** use backticks (`` ` ``) instead of quotes, and embed expressions with `${}`:

```javascript
// Clean, readable template literal
const html = `
    <div class="task-item" data-id="${id}" data-priority="${priority}">
        <span>${text}</span>
        <button class="delete-btn">Delete</button>
    </div>
`;
```

This is the same HTML, but readable and maintainable. The `${}` placeholders are replaced with the actual values of the variables.

### Template Literal Rules

1. **Use backticks**, not single or double quotes: `` ` `` not `'` or `"`
2. **Embed any expression** with `${}`: variables, calculations, function calls, ternaries
3. **Multi-line strings** work naturally -- no need for `\n` or string concatenation
4. **Regular text** outside `${}` is treated as literal characters

```javascript
const name = "Alice";
const score = 95;

// Variables
`Hello, ${name}!`                    // "Hello, Alice!"

// Expressions
`Score: ${score}/100`                // "Score: 95/100"
`Grade: ${score >= 90 ? "A" : "B"}` // "Grade: A"

// Function calls
`Name: ${name.toUpperCase()}`        // "Name: ALICE"

// Multi-line (preserves line breaks)
`Line 1
Line 2
Line 3`
```

### Creating HTML Generator Functions

A common pattern is to create a function that returns an HTML template:

```javascript
function createTaskHTML(id, text, priority) {
    return `
        <div class="task-item" data-id="${id}" data-priority="${priority}">
            <span>${text}</span>
            <button class="delete-btn">Delete</button>
        </div>
    `;
}

// Use it to generate HTML for different tasks
const task1 = createTaskHTML(1, "Buy groceries", "high");
const task2 = createTaskHTML(2, "Finish GP1", "medium");
const task3 = createTaskHTML(3, "Study for quiz", "low");
```

### insertAdjacentHTML -- Adding HTML to the Page

`insertAdjacentHTML()` lets you insert an HTML string at a specific position relative to an element. It is the best way to add dynamically generated HTML to the page.

```javascript
const taskList = document.querySelector("#task-list");

// Add a new task at the END of the list
taskList.insertAdjacentHTML("beforeend",
    createTaskHTML(1, "Buy groceries", "high")
);
```

### insertAdjacentHTML Positions

There are four positions you can insert at:

```
<!-- "beforebegin" — BEFORE the element itself -->
<div id="task-list">
    <!-- "afterbegin" — INSIDE, before the first child -->
    <div class="task-item">Existing task 1</div>
    <div class="task-item">Existing task 2</div>
    <!-- "beforeend" — INSIDE, after the last child (MOST COMMON) -->
</div>
<!-- "afterend" — AFTER the element itself -->
```

| Position | Where It Goes | Common Use |
|----------|--------------|------------|
| `"beforeend"` | Inside, at the end | Adding items to a list (most common) |
| `"afterbegin"` | Inside, at the start | Adding items to the top of a list |
| `"beforebegin"` | Before the element | Adding a sibling before the element |
| `"afterend"` | After the element | Adding a sibling after the element |

{{< callout type="info" >}}
**`"beforeend"` is the position you will use 90% of the time.** It adds new content at the end of a container, like appending a new task to the bottom of a list.
{{< /callout >}}

### insertAdjacentHTML vs innerHTML

You might wonder: why not just use `innerHTML`? There is an important difference:

```javascript
// innerHTML REPLACES all content -- destroys existing elements and event state
taskList.innerHTML += createTaskHTML(1, "New task", "high");
// All existing event listeners on children are LOST

// insertAdjacentHTML ADDS content without touching existing elements
taskList.insertAdjacentHTML("beforeend", createTaskHTML(1, "New task", "high"));
// Existing elements and their listeners are preserved
```

{{< callout type="error" >}}
**Never use `innerHTML +=` to add content.** It destroys and recreates ALL child elements, breaking event listeners and losing any state. Always use `insertAdjacentHTML` for adding content.
{{< /callout >}}

### Complete Example: Dynamic Card Creation

```html
<div class="controls">
    <input type="text" id="card-title" placeholder="Card title">
    <input type="text" id="card-desc" placeholder="Description">
    <select id="card-priority">
        <option value="low">Low</option>
        <option value="medium">Medium</option>
        <option value="high">High</option>
    </select>
    <button id="add-card-btn">Add Card</button>
</div>

<div id="card-list"></div>
```

```javascript
let nextId = 1;

function createCardHTML(id, title, description, priority) {
    return `
        <div class="card" data-id="${id}" data-priority="${priority}">
            <h3>${title}</h3>
            <p>${description}</p>
            <span class="badge badge-${priority}">${priority}</span>
            <button class="delete-btn">Delete</button>
        </div>
    `;
}

const cardList = document.querySelector("#card-list");
const addBtn = document.querySelector("#add-card-btn");

// Add new card
addBtn.addEventListener("click", () => {
    const title = document.querySelector("#card-title").value.trim();
    const desc = document.querySelector("#card-desc").value.trim();
    const priority = document.querySelector("#card-priority").value;

    if (title === "") return;

    cardList.insertAdjacentHTML("beforeend",
        createCardHTML(nextId++, title, desc, priority)
    );

    // Clear inputs
    document.querySelector("#card-title").value = "";
    document.querySelector("#card-desc").value = "";
});

// Event delegation for delete buttons
cardList.addEventListener("click", (event) => {
    if (event.target.matches(".delete-btn")) {
        const card = event.target.closest(".card");
        console.log(`Deleting card ${card.dataset.id}`);
        card.remove();
    }
});
```

Notice how all four patterns work together:
- **Template literals** build the HTML with embedded data
- **Data attributes** store the ID and priority on each card
- **Event delegation** handles delete clicks on dynamic cards
- **DOM traversal** (`closest`) finds the card from the button

---

## localStorage -- Persistent Browser Storage

### The Problem: Everything Disappears on Refresh

Every DOM change you make with JavaScript lives in memory. Add five tasks to a list, style them, mark some as complete -- and then press F5. Everything is gone. The browser reloads the original HTML, and all your JavaScript changes vanish.

This is the "refresh problem," and it makes dynamic features feel temporary and fragile. For your GP1 to feel like a real application, you need a way to **save data that survives page refreshes**.

### The Solution: localStorage

**localStorage** is a built-in browser feature that stores key-value pairs on the user's computer. Data in localStorage persists until the user explicitly clears their browser data -- it survives page refreshes, browser restarts, and even computer restarts.

### Basic Operations

```javascript
// SAVE a value
localStorage.setItem("username", "Alice");

// READ a value
const name = localStorage.getItem("username");
console.log(name); // "Alice"

// REMOVE a specific key
localStorage.removeItem("username");

// CLEAR everything (all keys)
localStorage.clear();
```

| Method | Purpose | Example |
|--------|---------|---------|
| `setItem(key, value)` | Save a value | `localStorage.setItem("theme", "dark")` |
| `getItem(key)` | Read a value | `localStorage.getItem("theme")` → `"dark"` |
| `removeItem(key)` | Delete one key | `localStorage.removeItem("theme")` |
| `clear()` | Delete all keys | `localStorage.clear()` |

### See It in DevTools

You can view and edit localStorage values in your browser's DevTools:

{{% steps %}}

### Open DevTools

Right-click anywhere on the page and select "Inspect," or press F12.

### Go to the Application tab

In Chrome, click the "Application" tab at the top. In Firefox, it is called "Storage."

### Find Local Storage

In the left sidebar, expand "Local Storage" and click on your site's URL. You will see all key-value pairs stored for that site.

### Edit values

You can double-click any value to edit it, or right-click to delete entries. This is useful for testing and debugging.

{{% /steps %}}

### localStorage Only Stores Strings

This is the most important thing to understand about localStorage: **every value is stored as a string**. If you try to store an array or object, it gets converted to a useless string:

```javascript
// Storing an array directly -- BROKEN
localStorage.setItem("tasks", [1, 2, 3]);
localStorage.getItem("tasks"); // "1,2,3" (a string, not an array!)

// Storing an object directly -- BROKEN
localStorage.setItem("user", { name: "Alice" });
localStorage.getItem("user"); // "[object Object]" (useless!)
```

### JSON.stringify and JSON.parse

To store arrays and objects, you need to convert them to a **JSON string** before saving, and convert back when loading:

```javascript
// SAVE: Convert array/object to JSON string
const tasks = [
    { id: 1, text: "Buy groceries", completed: false },
    { id: 2, text: "Finish GP1", completed: true }
];
localStorage.setItem("tasks", JSON.stringify(tasks));
// Stored as: '[{"id":1,"text":"Buy groceries","completed":false},...]'

// LOAD: Parse JSON string back to array/object
const saved = JSON.parse(localStorage.getItem("tasks"));
console.log(saved);       // [{id: 1, text: "Buy groceries", ...}, ...]
console.log(saved[0].text); // "Buy groceries"
```

| Operation | Method | Input | Output |
|-----------|--------|-------|--------|
| Save | `JSON.stringify(data)` | Array or Object | JSON string |
| Load | `JSON.parse(string)` | JSON string | Array or Object |

### The Safe Load Pattern

When your app runs for the first time, there is nothing in localStorage. `getItem` returns `null`, and `JSON.parse(null)` returns `null`. If you try to loop over `null` or push to it, your code crashes.

The safe load pattern uses `||` to provide a fallback:

```javascript
// First time: getItem returns null
// JSON.parse(null) returns null
// null || [] evaluates to []
const tasks = JSON.parse(localStorage.getItem("tasks")) || [];

// tasks is now [] (empty array), safe to work with
```

This one-liner handles both cases:
- **Data exists:** `JSON.parse` returns the array, and `|| []` is ignored
- **No data yet:** `JSON.parse` returns `null`, and `|| []` provides an empty array

{{< callout type="important" >}}
**Always use the `|| []` or `|| {}` fallback pattern when loading from localStorage.** This prevents crashes on first run and makes your code robust.
{{< /callout >}}

### The Load-on-Start, Save-on-Change Pattern

The standard pattern for using localStorage in an application follows this structure:

```javascript
// ===== 1. LOAD existing data (or start with empty array) =====
let tasks = JSON.parse(localStorage.getItem("tasks")) || [];

// ===== 2. SAVE function (call after every change) =====
function saveTasks() {
    localStorage.setItem("tasks", JSON.stringify(tasks));
}

// ===== 3. RENDER function (display data on the page) =====
function renderTasks() {
    const taskList = document.querySelector("#task-list");
    taskList.innerHTML = ""; // Clear current display

    tasks.forEach(task => {
        taskList.insertAdjacentHTML("beforeend", `
            <div class="task-item" data-id="${task.id}">
                <span>${task.text}</span>
                <button class="delete-btn">Delete</button>
            </div>
        `);
    });
}

// ===== 4. MODIFY functions (change data, then save and render) =====
function addTask(text) {
    tasks.push({ id: Date.now(), text, completed: false });
    saveTasks();    // Save to localStorage
    renderTasks();  // Update the page
}

function deleteTask(id) {
    tasks = tasks.filter(task => task.id !== id);
    saveTasks();    // Save to localStorage
    renderTasks();  // Update the page
}

// ===== 5. INITIALIZE (render existing data on page load) =====
renderTasks();
```

The flow is:

```
  Page loads → Load from localStorage → Render on page
                                            ↑
  User action → Modify data array → Save → Render
```

Every change follows the same sequence: **modify the data, save it, then re-render the page**. This keeps your data (the array) and your display (the DOM) always in sync.

### Simple Values: Dark Mode Preference

For simple values like a theme preference, you do not need JSON -- just `setItem` and `getItem`:

```javascript
const themeToggle = document.querySelector("#theme-toggle");

// Load saved preference on page start
const savedTheme = localStorage.getItem("theme");
if (savedTheme === "dark") {
    document.body.classList.add("dark-mode");
    themeToggle.textContent = "Light Mode";
}

// Toggle and save on click
themeToggle.addEventListener("click", () => {
    document.body.classList.toggle("dark-mode");

    const isDark = document.body.classList.contains("dark-mode");
    themeToggle.textContent = isDark ? "Light Mode" : "Dark Mode";

    // Save the preference
    localStorage.setItem("theme", isDark ? "dark" : "light");
});
```

### What to Store in localStorage

| Feature | What to Store | Format |
|---------|--------------|--------|
| Dark mode toggle | Theme preference | Simple string: `"dark"` or `"light"` |
| Shopping cart | Array of items | JSON array: `[{id, name, qty, price}, ...]` |
| Task list | Array of tasks | JSON array: `[{id, text, completed}, ...]` |
| Form drafts | Partially filled form | JSON object: `{name: "...", email: "..."}` |
| User preferences | Settings | JSON object: `{fontSize: "large", lang: "en"}` |
| Filter state | Active filter | Simple string: `"electronics"` |

### Limitations and Cautions

{{< callout type="warning" >}}
**Important limitations of localStorage:**

- **~5MB limit per domain.** This is plenty for tasks, preferences, and small data sets. Do not try to store images or large files.
- **Strings only.** Always use `JSON.stringify` / `JSON.parse` for arrays and objects.
- **Not secure.** Any JavaScript on the page can read localStorage. **Never store passwords, tokens, or sensitive data.**
- **Synchronous.** localStorage operations block the main thread. For the small amounts of data in GP1, this is not noticeable. For large data sets, consider IndexedDB (beyond this course).
- **No expiration.** Data stays until the user clears browser data or your code removes it.
- **Per-domain.** Each website has its own localStorage. Your GP1 data will not interfere with other sites.
{{< /callout >}}

---

## Putting It All Together: A Complete Task List

This example combines **all five patterns** into a single, working task list application. Study how the patterns interact:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Task Manager</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: Arial, sans-serif;
            max-width: 550px;
            margin: 40px auto;
            padding: 0 20px;
            background: #f5f5f5;
        }
        h1 { margin-bottom: 20px; }
        .add-form {
            display: flex;
            gap: 8px;
            margin-bottom: 20px;
        }
        .add-form input {
            flex: 1;
            padding: 10px 14px;
            border: 1px solid #ddd;
            border-radius: 6px;
            font-size: 1rem;
        }
        .add-form select {
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 6px;
        }
        .add-form button {
            padding: 10px 20px;
            background: #3b82f6;
            color: white;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-size: 1rem;
        }
        .add-form button:hover { background: #2563eb; }
        .task-item {
            display: flex;
            align-items: center;
            gap: 12px;
            padding: 14px;
            margin-bottom: 8px;
            background: white;
            border-radius: 8px;
            border: 1px solid #e5e7eb;
        }
        .task-item .task-text {
            flex: 1;
        }
        .task-item .task-text.completed {
            text-decoration: line-through;
            color: #9ca3af;
        }
        .task-item .badge {
            font-size: 0.75rem;
            padding: 2px 8px;
            border-radius: 12px;
            text-transform: uppercase;
        }
        .badge-high { background: #fef2f2; color: #ef4444; }
        .badge-medium { background: #fffbeb; color: #f59e0b; }
        .badge-low { background: #f0fdf4; color: #22c55e; }
        .complete-btn, .delete-btn {
            padding: 6px 12px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 0.85rem;
        }
        .complete-btn {
            background: #d1fae5;
            color: #059669;
        }
        .delete-btn {
            background: #fee2e2;
            color: #dc2626;
        }
        .task-count {
            text-align: center;
            color: #6b7280;
            margin-top: 16px;
        }
    </style>
</head>
<body>
    <h1>Task Manager</h1>

    <div class="add-form">
        <input type="text" id="task-input" placeholder="What needs to be done?">
        <select id="priority-select">
            <option value="low">Low</option>
            <option value="medium">Medium</option>
            <option value="high">High</option>
        </select>
        <button id="add-btn">Add</button>
    </div>

    <div id="task-list"></div>
    <p class="task-count" id="task-count"></p>

    <script>
        // ==========================================
        // 1. LOAD data from localStorage (safe pattern)
        // ==========================================
        let tasks = JSON.parse(localStorage.getItem("tasks")) || [];

        // ==========================================
        // 2. SAVE function
        // ==========================================
        function saveTasks() {
            localStorage.setItem("tasks", JSON.stringify(tasks));
        }

        // ==========================================
        // 3. RENDER function (Template Literals + Data Attributes)
        // ==========================================
        function renderTasks() {
            const taskList = document.querySelector("#task-list");
            const taskCount = document.querySelector("#task-count");

            taskList.innerHTML = "";

            tasks.forEach(task => {
                // TEMPLATE LITERAL: Build HTML with embedded data
                // DATA ATTRIBUTES: Store id and priority on the element
                taskList.insertAdjacentHTML("beforeend", `
                    <div class="task-item" data-id="${task.id}">
                        <span class="task-text ${task.completed ? 'completed' : ''}">
                            ${task.text}
                        </span>
                        <span class="badge badge-${task.priority}">
                            ${task.priority}
                        </span>
                        <button class="complete-btn">
                            ${task.completed ? 'Undo' : 'Done'}
                        </button>
                        <button class="delete-btn">Delete</button>
                    </div>
                `);
            });

            // Update count
            const remaining = tasks.filter(t => !t.completed).length;
            taskCount.textContent = `${remaining} task${remaining !== 1 ? 's' : ''} remaining`;
        }

        // ==========================================
        // 4. EVENT DELEGATION for dynamic buttons
        // ==========================================
        const taskList = document.querySelector("#task-list");

        taskList.addEventListener("click", (event) => {
            // DOM TRAVERSAL: Find the task item from any clicked child
            const taskItem = event.target.closest(".task-item");
            if (!taskItem) return;

            // DATA ATTRIBUTE: Get the task's unique ID
            const taskId = Number(taskItem.dataset.id);

            if (event.target.matches(".delete-btn")) {
                // Delete the task
                tasks = tasks.filter(task => task.id !== taskId);
                saveTasks();
                renderTasks();
            }

            if (event.target.matches(".complete-btn")) {
                // Toggle completed state
                const task = tasks.find(task => task.id === taskId);
                if (task) {
                    task.completed = !task.completed;
                    saveTasks();
                    renderTasks();
                }
            }
        });

        // ==========================================
        // 5. ADD new task
        // ==========================================
        const addBtn = document.querySelector("#add-btn");
        const taskInput = document.querySelector("#task-input");

        addBtn.addEventListener("click", () => {
            const text = taskInput.value.trim();
            if (text === "") return;

            tasks.push({
                id: Date.now(),      // Unique ID based on timestamp
                text: text,
                priority: document.querySelector("#priority-select").value,
                completed: false
            });

            saveTasks();
            renderTasks();
            taskInput.value = "";
            taskInput.focus();
        });

        // Allow Enter key to add task
        taskInput.addEventListener("keydown", (event) => {
            if (event.key === "Enter") {
                addBtn.click();
            }
        });

        // ==========================================
        // 6. INITIALIZE: Render existing tasks on page load
        // ==========================================
        renderTasks();
    </script>
</body>
</html>
```

### How the Patterns Work Together

```
  Pattern Interaction Map:

  localStorage ──── loads data ────→ tasks array
       ↑                                 │
       │                                 ↓
   saveTasks()              renderTasks() uses Template Literals
       ↑                         + Data Attributes on each element
       │                                 │
       │                                 ↓
  User clicks button          DOM displays task list
       │                                 │
       ↓                                 ↓
  Event Delegation catches     DOM Traversal finds the
  click on parent              task-item from button
       │                                 │
       ↓                                 ↓
  event.target identifies      dataset.id identifies
  which button was clicked     which task in the array
```

| Pattern | Role in This Example |
|---------|---------------------|
| **Event Delegation** | One listener on `#task-list` handles all Complete and Delete button clicks |
| **DOM Traversal** | `closest(".task-item")` finds the card from any child button |
| **Data Attributes** | `data-id` on each task connects DOM elements to array objects |
| **Template Literals** | `renderTasks()` builds HTML strings with embedded task data |
| **localStorage** | `saveTasks()` and the safe load pattern persist data across refreshes |

---

## GP1 Feature Ideas Using These Patterns

The patterns from this session map directly to Group Project 1 features. Here is how each GP1 feature uses the patterns:

| GP1 Feature | Patterns Used | Difficulty |
|-------------|--------------|------------|
| Dynamic list / shopping cart | Delegation + Templates + localStorage | Medium |
| Accordion / FAQ | Traversal (`nextElementSibling`) + classList | Easy-Medium |
| Dark mode toggle with persistence | localStorage + classList | Easy |
| Content filter (by category) | Data attributes + Delegation | Medium |
| Tab switching | Data attributes + Delegation + classList | Easy-Medium |
| Modal / popup | Delegation + classList | Easy-Medium |
| Form with saved drafts | localStorage + form events | Medium |
| Favorites / wishlist | localStorage + Delegation + Templates | Medium |

### Feature Implementation Quick-Start

{{% details title="Dark Mode Toggle with localStorage" closed="true" %}}

The simplest GP1 feature that uses localStorage. Copy this pattern and adapt it to your site.

```javascript
const themeToggle = document.querySelector("#theme-toggle");

// Load saved theme
const savedTheme = localStorage.getItem("theme");
if (savedTheme === "dark") {
    document.body.classList.add("dark-mode");
    themeToggle.textContent = "Light Mode";
}

// Toggle on click
themeToggle.addEventListener("click", () => {
    document.body.classList.toggle("dark-mode");
    const isDark = document.body.classList.contains("dark-mode");
    themeToggle.textContent = isDark ? "Light Mode" : "Dark Mode";
    localStorage.setItem("theme", isDark ? "dark" : "light");
});
```

**CSS (use CSS custom properties for easy theme switching):**

```css
:root {
    --bg-color: #ffffff;
    --text-color: #333333;
    --card-bg: #f8f9fa;
}

body.dark-mode {
    --bg-color: #1a1a2e;
    --text-color: #e0e0e0;
    --card-bg: #2d2d44;
}

body {
    background-color: var(--bg-color);
    color: var(--text-color);
    transition: background-color 0.3s, color 0.3s;
}

.card {
    background-color: var(--card-bg);
}
```

{{% /details %}}

{{% details title="Shopping Cart with localStorage" closed="true" %}}

Store cart items in localStorage so they persist across page visits.

```javascript
// Load cart
let cart = JSON.parse(localStorage.getItem("cart")) || [];

function saveCart() {
    localStorage.setItem("cart", JSON.stringify(cart));
}

function renderCart() {
    const cartList = document.querySelector("#cart-items");
    const cartTotal = document.querySelector("#cart-total");
    cartList.innerHTML = "";

    let total = 0;
    cart.forEach(item => {
        total += item.price * item.qty;
        cartList.insertAdjacentHTML("beforeend", `
            <div class="cart-item" data-id="${item.id}">
                <span>${item.name} x${item.qty}</span>
                <span>$${(item.price * item.qty).toFixed(2)}</span>
                <button class="remove-btn">Remove</button>
            </div>
        `);
    });

    cartTotal.textContent = `Total: $${total.toFixed(2)}`;
}

// Add to cart (call from product buttons)
function addToCart(id, name, price) {
    const existing = cart.find(item => item.id === id);
    if (existing) {
        existing.qty += 1;
    } else {
        cart.push({ id, name, price, qty: 1 });
    }
    saveCart();
    renderCart();
}

// Event delegation for remove buttons
document.querySelector("#cart-items").addEventListener("click", (event) => {
    if (event.target.matches(".remove-btn")) {
        const itemId = Number(event.target.closest(".cart-item").dataset.id);
        cart = cart.filter(item => item.id !== itemId);
        saveCart();
        renderCart();
    }
});

renderCart();
```

{{% /details %}}

{{% details title="Content Filter with Data Attributes" closed="true" %}}

Filter items by category using data attributes. Great for portfolio galleries or product pages.

```html
<div class="filter-buttons">
    <button class="filter-btn active" data-filter="all">All</button>
    <button class="filter-btn" data-filter="web">Web</button>
    <button class="filter-btn" data-filter="mobile">Mobile</button>
    <button class="filter-btn" data-filter="design">Design</button>
</div>

<div class="project-grid">
    <div class="project-card" data-category="web">Company Website</div>
    <div class="project-card" data-category="mobile">Fitness App</div>
    <div class="project-card" data-category="web">Blog Platform</div>
    <div class="project-card" data-category="design">Brand Identity</div>
</div>
```

```javascript
const filterContainer = document.querySelector(".filter-buttons");
const projects = document.querySelectorAll(".project-card");

filterContainer.addEventListener("click", (event) => {
    if (!event.target.matches(".filter-btn")) return;

    // Update active button
    document.querySelectorAll(".filter-btn").forEach(btn => btn.classList.remove("active"));
    event.target.classList.add("active");

    // Filter projects
    const filter = event.target.dataset.filter;
    projects.forEach(project => {
        if (filter === "all" || project.dataset.category === filter) {
            project.style.display = "block";
        } else {
            project.style.display = "none";
        }
    });
});
```

{{% /details %}}

{{% details title="Accordion / FAQ with DOM Traversal" closed="true" %}}

An accordion uses `nextElementSibling` to find the answer below each question.

```html
<div class="faq-list">
    <div class="faq-item">
        <button class="faq-question">What is your return policy?</button>
        <div class="faq-answer">
            <p>You can return any item within 30 days of purchase.</p>
        </div>
    </div>
    <div class="faq-item">
        <button class="faq-question">Do you offer free shipping?</button>
        <div class="faq-answer">
            <p>Yes, on all orders over $50.</p>
        </div>
    </div>
</div>
```

```css
.faq-answer {
    max-height: 0;
    overflow: hidden;
    transition: max-height 0.3s ease;
}
.faq-item.open .faq-answer {
    max-height: 200px;
}
```

```javascript
const faqList = document.querySelector(".faq-list");

// Event delegation on the FAQ container
faqList.addEventListener("click", (event) => {
    const question = event.target.closest(".faq-question");
    if (!question) return;

    const faqItem = question.closest(".faq-item");

    // Close all other items (only one open at a time)
    document.querySelectorAll(".faq-item").forEach(item => {
        if (item !== faqItem) item.classList.remove("open");
    });

    // Toggle the clicked item
    faqItem.classList.toggle("open");
});
```

{{% /details %}}

---

## Troubleshooting & Common Mistakes

{{% details title="Event delegation: clicks are not being caught" closed="true" %}}

**Symptoms:** You set up a listener on the parent, but clicking child elements does nothing.

**Common causes and fixes:**

1. **Wrong parent selector.** The listener is on the wrong element. Check that your parent selector matches the actual container.

```javascript
// WRONG: listener is on the list, but items are in a wrapper inside
document.querySelector("#task-list").addEventListener("click", ...);
// But your HTML has: <div id="task-list"><div class="wrapper"> items here </div></div>

// FIX: make sure the listener is on the direct or correct ancestor
```

2. **Wrong target check.** The check inside the handler does not match what was clicked.

```javascript
// WRONG: checking for a class that doesn't exist on the element
if (event.target.classList.contains("delete")) { ... }
// But the HTML has class="delete-btn"

// FIX: match the exact class name
if (event.target.classList.contains("delete-btn")) { ... }
```

3. **Clicking a child of the target.** If the button contains an icon or span, `event.target` might be the icon, not the button.

```html
<button class="delete-btn"><span class="icon">X</span></button>
```

```javascript
// WRONG: event.target might be the <span>, not the <button>
if (event.target.classList.contains("delete-btn")) { ... }

// FIX: use closest() to find the button from any child
const btn = event.target.closest(".delete-btn");
if (btn) { ... }
```

{{% /details %}}

{{% details title="closest() returns null" closed="true" %}}

**Symptoms:** `event.target.closest('.some-class')` returns `null`, and the next line crashes with a `null` error.

**Common causes:**

1. **Typo in the selector.** Double-check the class name (CSS selectors are case-sensitive).
2. **The clicked element is not inside the expected container.** The user clicked somewhere outside the element you are looking for.

**Fix:** Always check for `null` before using the result:

```javascript
const taskItem = event.target.closest(".task-item");
if (!taskItem) return; // Exit early if not found
// Now safe to use taskItem
```

{{% /details %}}

{{% details title="localStorage data is not saving" closed="true" %}}

**Symptoms:** Data appears to save, but after refreshing the page, it is gone.

**Common causes:**

1. **Forgot JSON.stringify.** You stored an object/array without converting it.

```javascript
// WRONG: stores "[object Object]"
localStorage.setItem("tasks", tasks);

// FIX: convert to JSON string first
localStorage.setItem("tasks", JSON.stringify(tasks));
```

2. **Forgot to call the save function.** You modified the data but did not save it.

```javascript
tasks.push(newTask);
// WRONG: forgot to save!

// FIX: always save after modifying
tasks.push(newTask);
saveTasks();
```

3. **Saving the wrong variable.** You saved a stale reference instead of the current data.

{{% /details %}}

{{% details title="localStorage data is corrupted or causes errors" closed="true" %}}

**Symptoms:** `JSON.parse` throws a `SyntaxError`, or the loaded data is not what you expected.

**Fix:** Clear localStorage and start fresh:

```javascript
// In the browser console:
localStorage.removeItem("tasks");

// Or clear everything:
localStorage.clear();
```

**Prevention:** Always use the safe load pattern:

```javascript
let tasks = [];
try {
    tasks = JSON.parse(localStorage.getItem("tasks")) || [];
} catch (error) {
    console.error("Failed to load tasks:", error);
    tasks = [];
}
```

{{% /details %}}

{{% details title="insertAdjacentHTML is not adding content" closed="true" %}}

**Symptoms:** You call `insertAdjacentHTML` but nothing appears on the page.

**Common causes:**

1. **Wrong position string.** Check spelling: `"beforeend"` (no capital letters, no spaces).

```javascript
// WRONG: typos or wrong case
taskList.insertAdjacentHTML("BeforeEnd", html);   // Wrong case
taskList.insertAdjacentHTML("before-end", html);   // Wrong format

// FIX: exact lowercase string
taskList.insertAdjacentHTML("beforeend", html);
```

2. **Empty HTML string.** Your template function is returning an empty string or `undefined`.

```javascript
// Check what your function returns
const html = createTaskHTML(id, text, priority);
console.log(html); // If this is empty, the function has a bug
```

3. **Wrong parent element.** You are inserting into an element that is not visible or does not exist.

{{% /details %}}

{{% details title="Template literal is not embedding values" closed="true" %}}

**Symptoms:** The page shows `${variable}` as literal text instead of the variable's value.

**Cause:** You used regular quotes instead of backticks.

```javascript
// WRONG: single quotes -- ${} is treated as literal text
const html = '<div>${name}</div>';

// FIX: backticks -- ${} is evaluated
const html = `<div>${name}</div>`;
```

The backtick key is usually in the top-left corner of the keyboard, below Escape and to the left of the `1` key.

{{% /details %}}

{{% details title="Data attribute values are not what I expect" closed="true" %}}

**Symptoms:** Comparing `dataset.id` to a number fails, or `dataset.completed` does not work in a conditional.

**Cause:** Data attribute values are always strings.

```javascript
// WRONG: comparing string to number
if (task.dataset.id === 42) { ... }    // Always false

// FIX: convert to number
if (Number(task.dataset.id) === 42) { ... }  // Works

// WRONG: treating string as boolean
if (task.dataset.completed) { ... }    // "false" is truthy!

// FIX: compare to the string "true"
if (task.dataset.completed === "true") { ... }
```

{{% /details %}}

{{% details title="innerHTML += breaks event listeners" closed="true" %}}

**Symptoms:** After adding new content with `innerHTML +=`, previously working event listeners stop firing.

**Cause:** `innerHTML +=` destroys and recreates ALL child elements. Event listeners are attached to the old elements, which no longer exist.

```javascript
// WRONG: destroys all existing children and their listeners
taskList.innerHTML += newTaskHTML;

// FIX: use insertAdjacentHTML to ADD without destroying
taskList.insertAdjacentHTML("beforeend", newTaskHTML);
```

{{% /details %}}

---

## Key Takeaways

{{< callout emoji="🎯" >}}
- **Event delegation** lets you handle events on elements that do not exist yet. Listen on a parent element, use `event.target` to identify what was clicked, and `closest()` to find the containing element.

- **DOM traversal** navigates from one element to related elements: `parentElement` and `closest()` go up, `children` and `firstElementChild` go down, `nextElementSibling` and `previousElementSibling` go sideways. Always use the "Element" versions to skip text nodes.

- **Data attributes** (`data-*`) store custom metadata on HTML elements. Read them with `element.dataset.propertyName`. Values are always strings -- convert with `Number()` or compare with `=== "true"` when needed.

- **Template literals** (backticks) let you build HTML strings with embedded expressions using `${}`. Use `insertAdjacentHTML("beforeend", html)` to add them to the page without destroying existing elements.

- **localStorage** persists data across page refreshes. Use `JSON.stringify()` to save arrays/objects and `JSON.parse()` to load them. Always use the `|| []` fallback pattern for safe loading.

- **The five patterns work together.** A typical dynamic feature uses template literals to create HTML with data attributes, event delegation to handle clicks on those dynamic elements, DOM traversal to find related elements, and localStorage to persist the data.

- **For GP1:** Start with your easiest feature, get it working, then add the next one. Dark mode with localStorage is the simplest feature that uses persistence. A dynamic list (tasks, cart) combines all five patterns.
{{< /callout >}}

---

## Additional Resources

### Official Documentation

{{< cards >}}
  {{< card link="https://javascript.info/event-delegation" title="javascript.info: Event Delegation" subtitle="In-depth tutorial on delegation with examples" icon="book-open" >}}
  {{< card link="https://developer.mozilla.org/en-US/docs/Web/API/Element/closest" title="MDN: Element.closest()" subtitle="Official documentation for the closest() method" icon="document-text" >}}
  {{< card link="https://developer.mozilla.org/en-US/docs/Learn/HTML/Howto/Use_data_attributes" title="MDN: Using Data Attributes" subtitle="Guide to data-* attributes in HTML and JavaScript" icon="document-text" >}}
  {{< card link="https://javascript.info/localstorage" title="javascript.info: localStorage" subtitle="Complete guide to browser storage" icon="book-open" >}}
{{< /cards >}}

### Template Literals and Dynamic HTML

{{< cards >}}
  {{< card link="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals" title="MDN: Template Literals" subtitle="Official guide to backtick strings and ${} expressions" icon="document-text" >}}
  {{< card link="https://developer.mozilla.org/en-US/docs/Web/API/Element/insertAdjacentHTML" title="MDN: insertAdjacentHTML" subtitle="Adding HTML strings to the DOM safely" icon="document-text" >}}
{{< /cards >}}

### DOM Traversal

{{< cards >}}
  {{< card link="https://javascript.info/dom-navigation" title="javascript.info: DOM Navigation" subtitle="Walking the DOM tree with parent, child, and sibling properties" icon="book-open" >}}
  {{< card link="https://developer.mozilla.org/en-US/docs/Web/API/Element/parentElement" title="MDN: parentElement" subtitle="Reference for parent, child, and sibling properties" icon="document-text" >}}
{{< /cards >}}

### Practice and Reference

{{< cards >}}
  {{< card link="https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage" title="MDN: Window.localStorage" subtitle="Complete localStorage API reference" icon="document-text" >}}
  {{< card link="https://javascript.info/bubbling-and-capturing" title="javascript.info: Bubbling and Capturing" subtitle="Deep dive into event propagation" icon="book-open" >}}
{{< /cards >}}

---

**Questions?** Reach out anytime:

- **Email:** dmandloi@neiu.edu
- **Office Hours:**
  - Tuesdays, 10:00 AM -- 12:00 PM (In Person, CBT 102)
  - Fridays, 11:30 AM -- 12:30 PM (Virtual)
  - Book: https://calendar.app.google/nbJhMZwosh4r4i1a8
