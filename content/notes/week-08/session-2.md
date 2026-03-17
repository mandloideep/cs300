---
title: "JSX Deep Dive: Lists, Conditional Rendering, Events & Props"
weight: 3
toc: true
---

## Introduction

In the previous session, you set up your first React project with Vite, learned JSX basics, and created simple components. This session goes deeper — covering the patterns you will use every day when writing React.

This session is split into two parts. Part 1 covers the JSX-HTML differences, rendering lists from data, Fragments, inline styles, and component organization. Part 2 covers conditional rendering (showing and hiding content), event handling, and a first look at props.

### What You'll Learn

**Part 1**
- All the differences between JSX and HTML
- How to render lists from arrays using `.map()` and how it compares to vanilla JS
- Why React needs the `key` prop and how to use it correctly
- When to use Fragments and why extra `<div>` wrappers cause real problems
- When and how to use inline styles vs CSS files
- How to organize components into files and folders with proper imports/exports

**Part 2**
- Three patterns for conditional rendering: `&&`, ternary (`? :`), and early return
- How to render nothing with `null`
- How to apply conditional CSS classes
- Event handling — responding to clicks and input with `onClick` and `onChange`
- Props preview — passing data to components to make them reusable

---

## JSX vs HTML — Complete Reference

JSX looks like HTML, but it is actually JavaScript. Because JSX is compiled into JavaScript function calls, it must follow JavaScript's rules — not HTML's rules. Words like `class` and `for` are reserved keywords in JavaScript, so JSX uses alternatives.

| HTML | JSX | Reason |
|------|-----|--------|
| `class="card"` | `className="card"` | `class` is a reserved word in JavaScript |
| `for="email"` | `htmlFor="email"` | `for` is a reserved word in JavaScript |
| `<br>` | `<br />` | All tags must be explicitly closed |
| `<img src="...">` | `<img src="..." />` | Self-closing tags need the `/` |
| `<input type="text">` | `<input type="text" />` | Same — must self-close |
| `style="color: red"` | `style={{ color: 'red' }}` | Styles are JavaScript objects |
| `onclick="..."` | `onClick={...}` | Event handlers use camelCase |
| `tabindex="0"` | `tabIndex={0}` | Attributes use camelCase |

{{< callout type="info" >}}
**The pattern:** If an HTML attribute is a reserved word in JavaScript (`class`, `for`), JSX uses an alternative name. If an attribute is multi-word (`onclick`, `tabindex`), JSX uses camelCase (`onClick`, `tabIndex`). Once you see the pattern, it becomes second nature.
{{< /callout >}}

---

## Rendering Lists with .map()

Almost every web application displays lists of data: a feed of posts, search results, a shopping cart, a list of contacts. In vanilla JavaScript, you built these lists by creating HTML strings and injecting them with `innerHTML`. React takes a different approach: transform arrays of data into arrays of JSX elements using `.map()`, and let React handle all the DOM updates.

### Vanilla JS vs React — Side by Side

You already know `.map()` from Weeks 5 and 6. In React, the concept is identical — only the output format changes.

{{< tabs items="Vanilla JS (Week 6),React (Today)" >}}
  {{< tab >}}
```javascript
// Vanilla JS — build a string, inject into DOM
const users = ["Alice", "Bob", "Carol"];
const html = users.map(user => `<li>${user}</li>`).join("");
document.querySelector("#list").innerHTML = html;
```

You build an HTML string with template literals, join the array, and set `innerHTML`. You are responsible for finding the DOM element and updating it yourself.
  {{< /tab >}}
  {{< tab >}}
```jsx
// React — return JSX directly, React handles the DOM
function UserList() {
  const users = ["Alice", "Bob", "Carol"];

  return (
    <ul>
      {users.map((user) => (
        <li key={user}>{user}</li>
      ))}
    </ul>
  );
}
```

`.map()` returns JSX elements instead of strings. No `.join("")`, no `innerHTML`, no `document.querySelector`. React renders them into the DOM for you.
  {{< /tab >}}
{{< /tabs >}}

### Rendering Arrays of Objects

Most real data comes as arrays of objects — from APIs, databases, or local state. Map over them the same way:

```jsx
function TeamList() {
  const team = [
    { id: 1, name: "Alice", role: "Developer" },
    { id: 2, name: "Bob", role: "Designer" },
    { id: 3, name: "Carol", role: "Project Manager" },
  ];

  return (
    <div>
      <h2>Our Team</h2>
      {team.map((member) => (
        <div key={member.id} className="team-member">
          <h3>{member.name}</h3>
          <p>{member.role}</p>
        </div>
      ))}
    </div>
  );
}
```

{{< callout type="important" >}}
Notice the `key` prop on each mapped element. React **requires** a unique key for every item in a list. See the next section for why.
{{< /callout >}}

### Destructuring Inside .map()

When your objects have many properties, destructure them directly in the `.map()` callback to keep the JSX clean:

```jsx
{team.map(({ id, name, role }) => (
  <div key={id} className="team-member">
    <h3>{name}</h3>
    <p>{role}</p>
  </div>
))}
```

---

## The key Prop

When you render a list with `.map()`, React needs a way to identify each item so it can efficiently update the DOM when the list changes. Without keys, React cannot tell which items were added, removed, or moved — it has to destroy and rebuild the entire list on every change.

Think of it like a classroom roster. If the teacher only knows students by their seat number (index) and two students swap seats, the teacher thinks they are different people. If the teacher knows students by their student ID, swapping seats causes no confusion. Keys are student IDs for your list items.

### Rules for Keys

```jsx
// GOOD — unique, stable identifier from your data
{users.map((user) => (
  <li key={user.id}>{user.name}</li>
))}

// OK — unique string value (when items don't have IDs)
{fruits.map((fruit) => (
  <li key={fruit}>{fruit}</li>
))}

// BAD — array index as key (shifts when items change)
{users.map((user, index) => (
  <li key={index}>{user.name}</li>
))}
```

### What Happens Without Stable Keys

Imagine you have a list of three items and you remove the middle one:

**With stable keys (using IDs):**

```
Before:   [Alice (key=1)] [Bob (key=2)] [Carol (key=3)]
After:    [Alice (key=1)]               [Carol (key=3)]

React sees: key=2 is gone → remove that one element. Done.
```

**With index keys:**

```
Before:   [Alice (key=0)] [Bob (key=1)] [Carol (key=2)]
After:    [Alice (key=0)] [Carol (key=1)]

React sees: key=0 same. key=1 changed from Bob to Carol → update it.
            key=2 is gone → remove it.
React updated TWO elements instead of ONE.
```

With index keys, React does more work than necessary, and in complex cases (like form inputs inside list items) it causes visible bugs where the wrong data appears in the wrong row.

### Key Rules Summary

| Rule | Example |
|------|---------|
| Keys must be **unique** among siblings | No two items in the same list should share a key |
| Keys should be **stable** | Use IDs from your data, not random values |
| Keys go on the **outermost element** in the map | The first JSX element inside `.map()` |
| Keys are **not passed** as a prop | You cannot access `props.key` inside the component |

{{< callout type="warning" >}}
If you forget keys, React will show a console warning: *"Each child in a list should have a unique 'key' prop."* Always check your browser console for this warning when working with lists.
{{< /callout >}}

---

## Fragments

React components must return a single root element. Without Fragments, you would be forced to wrap everything in a `<div>`, even when that extra `<div>` causes real problems.

Fragments let you group multiple elements together without adding any extra nodes to the DOM. They are invisible wrappers that satisfy React's single-root requirement while keeping your HTML clean.

```jsx
// Extra div (sometimes unwanted)
return (
  <div>
    <h1>Title</h1>
    <p>Content</p>
  </div>
);

// Fragment — no extra DOM node
return (
  <>
    <h1>Title</h1>
    <p>Content</p>
  </>
);
```

### When Extra `<div>` Wrappers Break Your Layout

Extra `<div>` elements are not just clutter — they can break CSS layouts. This is especially common with Flexbox and Grid, where the parent-child relationship between elements matters.

{{< tabs items="Broken Layout (div),Fixed Layout (Fragment)" >}}
  {{< tab >}}
```jsx
// Parent uses Flexbox — expects direct children
function Navigation() {
  return (
    <nav style={{ display: 'flex', gap: '16px' }}>
      <Logo />
      <NavLinks />
      <UserMenu />
    </nav>
  );
}

// NavLinks wraps items in a div — breaks the flex layout!
function NavLinks() {
  return (
    <div>  {/* This div becomes a single flex child */}
      <a href="/home">Home</a>
      <a href="/about">About</a>
      <a href="/contact">Contact</a>
    </div>
  );
}
```

The extra `<div>` becomes a single flex child, so the three links are no longer direct flex children of the `<nav>`.
  {{< /tab >}}
  {{< tab >}}
```jsx
// Fragment — links become direct flex children
function NavLinks() {
  return (
    <>
      <a href="/home">Home</a>
      <a href="/about">About</a>
      <a href="/contact">Contact</a>
    </>
  );
}
```

With a Fragment, the `<a>` elements are injected directly into the `<nav>` as flex children. The layout works as intended.
  {{< /tab >}}
{{< /tabs >}}

### Fragments with Keys

If you need a `key` on a Fragment (inside `.map()`), use the explicit `Fragment` import. The short syntax `<>...</>` does not support the `key` prop.

```jsx
import { Fragment } from 'react';

{items.map((item) => (
  <Fragment key={item.id}>
    <dt>{item.term}</dt>
    <dd>{item.definition}</dd>
  </Fragment>
))}
```

{{< callout type="info" >}}
**When to use which syntax:**
- `<>...</>` — most of the time, when you just need to group elements
- `<Fragment key={...}>` — only when you need a key (inside `.map()`)
{{< /callout >}}

---

## Inline Styles in JSX

In HTML, the `style` attribute is a string: `style="color: red; font-size: 18px"`. In JSX, everything inside `{}` is JavaScript, so the `style` attribute takes a **JavaScript object** instead. CSS property names must be written in camelCase, and values are usually strings.

Most of the time, use CSS files for styling. Inline styles are best reserved for **dynamic values** — styles that change based on data.

```jsx
// HTML
<div style="background-color: #f0f0f0; padding: 16px;">

// JSX
<div style={{ backgroundColor: '#f0f0f0', padding: '16px' }}>
```

### Why Double Curly Braces?

- The **outer** `{}` is the JSX expression delimiter
- The **inner** `{}` is a JavaScript object literal

### Style Property Names

CSS properties become camelCase in JSX:

| CSS | JSX Style Object |
|-----|-----------------|
| `background-color` | `backgroundColor` |
| `font-size` | `fontSize` |
| `border-radius` | `borderRadius` |
| `margin-top` | `marginTop` |
| `z-index` | `zIndex` |

### Dynamic Styles — When Inline Styles Shine

Inline styles are most useful when the style values depend on data:

```jsx
function ProgressBar({ percentage }) {
  return (
    <div style={{ width: '100%', backgroundColor: '#e5e7eb', borderRadius: '4px' }}>
      <div style={{
        width: `${percentage}%`,
        backgroundColor: percentage > 75 ? 'green' : percentage > 50 ? 'yellow' : 'red',
        height: '20px',
        borderRadius: '4px',
        transition: 'width 0.3s ease',
      }}>
        {percentage}%
      </div>
    </div>
  );
}
```

This would be difficult with a CSS file alone because `width` and `backgroundColor` are computed from the `percentage` prop.

{{< callout type="info" >}}
**When to use inline styles vs CSS files:** Use CSS files for most styling. Inline styles are useful for dynamic values that change based on data (like a progress bar width or a user-chosen color).
{{< /callout >}}

---

## Component Organization

As your project grows, you need a clear structure for your components.

### One Component Per File

Each component gets its own `.jsx` file, named to match the component:

```
src/
├── components/
│   ├── Header.jsx       → exports Header
│   ├── Footer.jsx       → exports Footer
│   ├── ProfileCard.jsx  → exports ProfileCard
│   └── Button.jsx       → exports Button
├── App.jsx
├── main.jsx
└── index.css
```

### Component + CSS Together

As your project grows, group each component with its CSS:

{{< filetree/container >}}
  {{< filetree/folder name="src" >}}
    {{< filetree/folder name="components" >}}
      {{< filetree/folder name="Card" >}}
        {{< filetree/file name="Card.jsx" >}}
        {{< filetree/file name="Card.css" >}}
      {{< /filetree/folder >}}
      {{< filetree/folder name="Button" >}}
        {{< filetree/file name="Button.jsx" >}}
        {{< filetree/file name="Button.css" >}}
      {{< /filetree/folder >}}
      {{< filetree/folder name="Header" >}}
        {{< filetree/file name="Header.jsx" >}}
        {{< filetree/file name="Header.css" >}}
      {{< /filetree/folder >}}
    {{< /filetree/folder >}}
    {{< filetree/file name="App.jsx" >}}
    {{< filetree/file name="App.css" >}}
    {{< filetree/file name="main.jsx" >}}
    {{< filetree/file name="index.css" >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}}

This pattern is called **co-location** — keeping related files together.

### When to Create a New Component

1. **Is it reused?** The same UI appears in multiple places → extract it
2. **Is it complex?** The JSX is longer than ~30 lines → extract it
3. **Does it have a clear job?** "This is the navigation bar" → extract it

If the answer to any of these is yes, create a new component.

### The Full Import/Export Flow

```jsx
// src/components/Header.jsx — define and export the component
import './Header.css';

function Header() {
  return (
    <header className="header">
      <h1>My App</h1>
      <nav>
        <a href="/">Home</a>
        <a href="/about">About</a>
      </nav>
    </header>
  );
}

export default Header;
```

```jsx
// src/App.jsx — import and use the component
import Header from './components/Header';

function App() {
  return (
    <div>
      <Header />
      <main>
        <p>Welcome to the app!</p>
      </main>
    </div>
  );
}

export default App;
```

### Exports: Default vs Named

{{< tabs items="Default Export,Named Export" >}}
  {{< tab >}}
```jsx
// Button.jsx — one component per file (most common)
function Button() {
  return <button className="btn">Click</button>;
}
export default Button;

// Importing (no curly braces for default exports):
import Button from './Button';
```

Use default exports for components. Each file exports one thing.
  {{< /tab >}}
  {{< tab >}}
```jsx
// Buttons.jsx — multiple related components
export function PrimaryButton() {
  return <button className="btn btn-primary">Primary</button>;
}
export function SecondaryButton() {
  return <button className="btn btn-secondary">Secondary</button>;
}

// Importing (must use exact names with curly braces):
import { PrimaryButton, SecondaryButton } from './Buttons';
```

Use named exports for multiple small, related items in one file.
  {{< /tab >}}
{{< /tabs >}}

---

## Conditional Rendering

React doesn't have special template syntax for conditions (like `v-if` in Vue). Instead, you use regular JavaScript. The key shift is from **imperative** (Vanilla JS) to **declarative** (React):

```javascript
// Vanilla JS approach (Week 6) — imperative
if (isLoggedIn) {
  welcomeMessage.classList.remove("hidden");
} else {
  welcomeMessage.classList.add("hidden");
}
```

```jsx
// React approach — declarative
function Header({ isLoggedIn }) {
  return (
    <header>
      {isLoggedIn ? (
        <p className="welcome">Welcome back!</p>
      ) : (
        <button>Log In</button>
      )}
    </header>
  );
}
```

No `classList.add`, no `classList.remove`, no tracking hidden states.

### Pattern 1: && Operator (Show or Hide)

Use `&&` when you want to **show something or show nothing**:

```jsx
function Dashboard({ messageCount }) {
  return (
    <div>
      <h1>Dashboard</h1>
      {messageCount > 0 && (
        <p className="notification">
          You have {messageCount} new messages!
        </p>
      )}
    </div>
  );
}
```

**How it works:** If the left side is truthy, React renders the right side. If falsy, nothing renders.

{{< callout type="warning" >}}
**Watch out for zero!** `{count && <p>Messages</p>}` will render the number `0` on screen when count is 0, because `0` is falsy but still a renderable value in React. Always use an explicit comparison: `{count > 0 && <p>Messages</p>}`.
{{< /callout >}}

### Pattern 2: Ternary Operator (Either/Or)

Use the ternary when you want to **choose between two things**:

```jsx
function LoginButton({ isLoggedIn }) {
  return (
    <div>
      {isLoggedIn ? (
        <button>Log Out</button>
      ) : (
        <button>Log In</button>
      )}
    </div>
  );
}
```

**How it works:** `condition ? (show this) : (show that)` — the `?` means "then" and `:` means "else."

### Pattern 3: Early Return (Edge Cases)

Use early return when you need to **handle edge cases before the main UI**:

```jsx
function UserProfile({ user }) {
  if (!user) {
    return <p>Loading user data...</p>;
  }

  if (user.error) {
    return <p className="error">Could not load profile.</p>;
  }

  return (
    <div className="profile">
      <h2>{user.name}</h2>
      <p>{user.email}</p>
      <p>{user.bio}</p>
    </div>
  );
}
```

**How it works:** Check for edge cases at the top of the component. If one matches, return immediately. The main UI at the bottom only runs when everything is normal.

{{< callout type="important" >}}
**The order of early returns matters.** Check the most critical conditions first (loading, errors) and work down to less critical ones (empty data). This prevents runtime errors like "Cannot read properties of undefined."
{{< /callout >}}

### Rendering Nothing with null

Sometimes a component should explicitly render nothing. Returning `null` tells React to skip this component entirely:

```jsx
function MaybeAlert({ message }) {
  if (!message) {
    return null; // Explicitly render nothing
  }

  return <div className="alert">{message}</div>;
}
```

{{< callout type="info" >}}
**When to use `null` vs `&&`:** Use `return null` when the *entire component* should render nothing. Use `&&` when you want to show or hide *one piece* within a larger component.
{{< /callout >}}

### When to Use Each Pattern

| Pattern | Use When | Real Example |
|---------|----------|-------------|
| `&&` | Show or hide **one thing** | Badge, notification, tooltip, "New!" label |
| `? :` | Choose between **two options** | Login/logout button, light/dark theme, expand/collapse |
| Early return | Handle **edge cases** before normal UI | Loading state, error state, empty data |
| `return null` | The **entire component** should render nothing | Conditional alert, optional tooltip |

### Decision Flowchart

```
Need to conditionally render content?
│
├── Show or hide ONE thing?
│   └── Use && operator
│       Example: {isAdmin && <DeleteButton />}
│
├── Choose between TWO things?
│   └── Use ternary ? :
│       Example: {isLoggedIn ? <Logout /> : <Login />}
│
├── Handle edge cases before main UI?
│   └── Use early return
│       Example: if (isLoading) return <Spinner />;
│
└── Entire component should render nothing?
    └── return null
        Example: if (!message) return null;
```

---

## Conditional CSS Classes

You often need to change an element's **styling** based on a condition. Do this by conditionally setting the `className` prop.

```jsx
// Toggle between active and inactive styles
<button className={`btn ${isActive ? "btn-active" : "btn-inactive"}`}>
  Click Me
</button>

// Add an optional class (empty string when false)
<div className={`card ${isHighlighted ? "card-highlighted" : ""}`}>
  Card Content
</div>

// Dynamic class based on data value
function StatusBadge({ status }) {
  return (
    <span className={`badge badge-${status}`}>
      {status}
    </span>
  );
}
```

{{< callout type="warning" >}}
**Avoid using `&&` directly in className.** Writing `className={\`card ${isHighlighted && "card-highlighted"}\`}` will insert `false` into the class string when the condition is falsy, producing `class="card false"`. Always use a ternary with an empty string as the fallback.
{{< /callout >}}

---

## Event Handling in React

### Events vs Vanilla JS

You already know DOM events from Week 5-6. In React, events are props you pass directly to JSX elements — no `querySelector`, no `addEventListener`:

| Vanilla JS | React JSX |
|------------|-----------|
| `element.addEventListener('click', handler)` | `onClick={handler}` |
| `element.addEventListener('input', handler)` | `onChange={handler}` |
| `element.addEventListener('submit', handler)` | `onSubmit={handler}` |

**Rule:** All React event names are camelCase (`onClick`, `onChange`, `onSubmit`).

### The Handler Pattern

Define the handler above the `return`, then pass it as a prop:

```jsx
function LikeButton() {
  function handleClick() {
    console.log("Liked!");
  }

  return <button onClick={handleClick}>Like ♡</button>;
}
```

**Naming convention:** `handle` + what it handles: `handleClick`, `handleSubmit`, `handleChange`.

### The #1 Mistake — Parentheses

This is the most common beginner error:

```jsx
// WRONG — calls the function immediately when the component renders!
<button onClick={handleClick()}>Like</button>

// RIGHT — passes the function to be called when clicked
<button onClick={handleClick}>Like</button>
```

Think of it as: handing someone your phone number vs calling them right now.

### Inline Arrow Functions

```jsx
// Inline arrow — fine for short logic
<button onClick={() => console.log("clicked!")}>Click me</button>

// Use an arrow function when passing arguments
<button onClick={() => handleDelete(item.id)}>Delete</button>

// Named handler — better for complex logic
function handleDelete() {
  console.log("Deleting...");
  // more logic here
}
<button onClick={handleDelete}>Delete</button>
```

### The Event Object

React passes an event object to every handler — same as vanilla JS:

```jsx
function SearchInput() {
  function handleChange(event) {
    console.log(event.target.value); // logs what the user typed
  }

  return <input onChange={handleChange} placeholder="Search..." />;
}
```

**Common uses:**
- `event.target.value` → the input's current text
- `event.preventDefault()` → stop a form from refreshing the page
- `event.target` → the element that fired the event

```jsx
function ContactForm() {
  function handleSubmit(event) {
    event.preventDefault(); // stop page reload
    console.log("Form submitted!");
  }

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" placeholder="Your name" />
      <button type="submit">Submit</button>
    </form>
  );
}
```

{{< callout type="info" >}}
**Note:** Right now, events let you *see* user actions in the console. To make the UI actually update in response (e.g., show what the user typed), you need `useState` — coming in Week 11. Getting familiar with event syntax now is essential preparation.
{{< /callout >}}

---

## Props Preview

### The Problem with Hardcoded Components

Every component you have built so far shows the same data:

```jsx
function ProfileCard() {
  return (
    <div>
      <h2>Alice</h2>
      <p>Developer</p>
    </div>
  );
}
```

This component always shows Alice. To show Bob, you would need to copy-paste the whole component. That does not scale — imagine 50 profile cards.

### Props — Function Arguments for Components

Props let you pass different data to the same component:

```jsx
// Usage — pass data like HTML attributes
<ProfileCard name="Alice" role="Developer" />
<ProfileCard name="Bob" role="Designer" />
<ProfileCard name="Carol" role="PM" />

// Definition — receive data via destructuring
function ProfileCard({ name, role }) {
  return (
    <div>
      <h2>{name}</h2>
      <p>{role}</p>
    </div>
  );
}
```

**One component definition → unlimited reuse with different data.**

### Props with .map()

The full pattern for data-driven UIs combines `.map()` with props:

```jsx
function TeamList() {
  const team = [
    { id: 1, name: "Alice", role: "Developer" },
    { id: 2, name: "Bob", role: "Designer" },
    { id: 3, name: "Carol", role: "PM" },
  ];

  return (
    <div>
      {team.map((member) => (
        <ProfileCard
          key={member.id}
          name={member.name}
          role={member.role}
        />
      ))}
    </div>
  );
}
```

This is the complete data-driven pattern: an array of objects + `.map()` + props = a list of components each showing different data.

{{< callout type="important" >}}
**Week 10 Deep Dive:** Props are a major topic. This is just a preview. In Week 10 (after Spring Break), you will cover: default prop values, prop validation with PropTypes, the `children` prop, passing functions as props, and common patterns for component communication.
{{< /callout >}}

---

## Common Patterns

### Rendering a Filtered List

Combine `.filter()` and `.map()` to show only matching items:

```jsx
function ActiveUsers() {
  const users = [
    { id: 1, name: "Alice", active: true },
    { id: 2, name: "Bob", active: false },
    { id: 3, name: "Carol", active: true },
  ];

  return (
    <ul>
      {users
        .filter((user) => user.active)
        .map((user) => (
          <li key={user.id}>{user.name}</li>
        ))}
    </ul>
  );
}
```

### Empty State Pattern

Always handle the case when there is no data to show:

```jsx
function SearchResults({ results }) {
  if (results.length === 0) {
    return <p className="empty">No results found. Try a different search.</p>;
  }

  return (
    <div>
      {results.map((result) => (
        <div key={result.id}>
          <h3>{result.title}</h3>
          <p>{result.description}</p>
        </div>
      ))}
    </div>
  );
}
```

### Putting It All Together

Here is a complete component that uses every pattern from this session:

```jsx
function ProductCard({ product }) {
  // Early return for missing data
  if (!product) {
    return <p>Product not found</p>;
  }

  return (
    <div className="product-card">
      <h3>{product.name}</h3>
      <p>${product.price}</p>

      {/* && for optional badge */}
      {product.onSale && <span className="sale-badge">SALE</span>}

      {/* Ternary for stock status */}
      {product.inStock ? (
        <button>Add to Cart</button>
      ) : (
        <p className="out-of-stock">Out of Stock</p>
      )}

      {/* Ingredients list with .map() */}
      <ul>
        {product.tags.map((tag) => (
          <li key={tag}>{tag}</li>
        ))}
      </ul>
    </div>
  );
}
```

---

## Troubleshooting & Common Mistakes

{{% details title="I see 'Each child in a list should have a unique key prop'" %}}
Every element returned by `.map()` needs a `key` prop. Use a unique identifier from your data:

```jsx
// Missing key
{items.map(item => <li>{item.name}</li>)}

// With key
{items.map(item => <li key={item.id}>{item.name}</li>)}
```

If your data does not have IDs, use a unique string value. Avoid using array index as a key.
{{% /details %}}

{{% details title="My && condition renders '0' on the screen" %}}
When the left side of `&&` is `0` (not `false`), React renders the `0`. Use an explicit comparison:

```jsx
// Renders "0" when count is 0
{count && <p>{count} messages</p>}

// Renders nothing when count is 0
{count > 0 && <p>{count} messages</p>}
```
{{% /details %}}

{{% details title="My inline styles aren't working" %}}
JSX styles use camelCase and are JavaScript objects, not strings.

```jsx
// Wrong — string style like HTML
<div style="background-color: red">

// Wrong — single curly braces (syntax error)
<div style={ backgroundColor: 'red' }>

// Correct — double curly braces, camelCase
<div style={{ backgroundColor: 'red' }}>
```
{{% /details %}}

{{% details title="My click handler fires when the page loads, not when I click" %}}
You accidentally called the function with `()`. Remove the parentheses:

```jsx
// Problem — handleClick() is called immediately on render
<button onClick={handleClick()}>Click</button>

// Fix — pass the reference, no parentheses
<button onClick={handleClick}>Click</button>
```

If you need to pass an argument, use an arrow function wrapper:

```jsx
<button onClick={() => handleDelete(item.id)}>Delete</button>
```
{{% /details %}}

{{% details title="My component doesn't re-render when I change data" %}}
If you are changing a variable inside the component and expecting the UI to update, that will not work yet. You need **state** (coming in Week 11). For now, components render once with their initial data.

```jsx
// This won't update the UI
function Counter() {
  let count = 0;
  // Changing count here doesn't trigger a re-render
  return <p>{count}</p>;
}

// We'll learn useState in Week 11 to solve this!
```
{{% /details %}}

{{% details title="I get 'Cannot find module' when importing a component" %}}
Check these things:
1. The file path is correct (case-sensitive on Mac/Linux)
2. The file extension is included if needed: `import Card from './Card'` or `'./Card.jsx'`
3. The component file has `export default ComponentName` at the bottom
4. You are importing from the right directory (e.g., `'./components/Card'`)
{{% /details %}}

{{% details title="My conditional className includes 'false' in the output" %}}
This happens when you use `&&` inside a template literal for class names. Use a ternary with an empty string fallback instead:

```jsx
// Problem: class="card false"
<div className={`card ${isHighlighted && "card-highlighted"}`}>

// Solution: class="card " (harmless extra space)
<div className={`card ${isHighlighted ? "card-highlighted" : ""}`}>
```
{{% /details %}}

{{% details title="My onChange isn't logging anything" %}}
Make sure you are passing the handler correctly (no `()`) and the `event` parameter is named in the function:

```jsx
// Problem — missing event parameter
function handleChange() {
  console.log(event.target.value); // ReferenceError!
}

// Fix — accept event as a parameter
function handleChange(event) {
  console.log(event.target.value); // Works correctly
}
```
{{% /details %}}

---

## Key Takeaways

{{< callout emoji="🎯" >}}
- **JSX differences:** `className` not `class`, close all tags, camelCase attributes, styles are objects
- **Render lists** with `.map()` — always provide a unique `key` prop (use IDs from your data, not array index)
- **Keys** help React track which items changed — think of them as student IDs for your list items
- **Fragments** (`<>...</>`) wrap elements without adding extra DOM nodes — use them to avoid breaking Flexbox/Grid layouts
- **Inline styles** use double curly braces and camelCase property names — best for dynamic values
- **Organize components:** one per file, PascalCase names, group with CSS in folders, use `export default`
- **Three conditional patterns:** `&&` to show/hide one thing, `? :` to choose between two things, early return for edge cases
- **`return null`** renders nothing from the entire component; `&&` skips one element within a component
- **Conditional classes:** use ternary inside template literals, never `&&` in className
- **Event handling:** `onClick={handler}` — pass the function reference, never call it with `()`
- **Props preview:** components accept data via destructured parameters — `function Card({ title, date })`; full deep dive in Week 10
{{< /callout >}}

---

## Additional Resources

### Official Documentation

{{< cards >}}
  {{< card link="https://react.dev/learn/rendering-lists" title="React: Rendering Lists" subtitle="Official guide to .map(), keys, and list patterns" >}}
  {{< card link="https://react.dev/learn/conditional-rendering" title="React: Conditional Rendering" subtitle="Official guide to all conditional patterns" >}}
  {{< card link="https://react.dev/learn/writing-markup-with-jsx" title="React: Writing Markup with JSX" subtitle="JSX syntax rules and differences from HTML" >}}
  {{< card link="https://react.dev/learn/responding-to-events" title="React: Responding to Events" subtitle="Complete guide to event handling in React" >}}
  {{< card link="https://react.dev/learn/importing-and-exporting-components" title="React: Importing and Exporting Components" subtitle="How to organize components across files" >}}
  {{< card link="https://react.dev/learn/passing-props-to-a-component" title="React: Passing Props to a Component" subtitle="How props work — preview of Week 10" >}}
{{< /cards >}}
