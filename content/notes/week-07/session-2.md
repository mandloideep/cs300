---
title: "Application Architecture Patterns: From Vanilla JS to React"
weight: 3
toc: true
---

## Introduction

You know how to fetch data from APIs, handle loading and error states, and render results to the page. But how do you organize a **whole application** — one with multiple pages, user interactions, and persistent data?

This guide teaches you six architectural patterns that work for any application you'll ever build. Each pattern has a direct equivalent in React, so when you start React next week, you'll already understand the concepts — you'll just be learning new syntax.

We'll apply all six patterns by building **CourseHub**, a multi-view book browser that searches the Open Library API, bookmarks favorites, and persists data across page refreshes.

{{< callout type="important" >}}
These aren't "vanilla JS patterns" — they're **application patterns**. React, Vue, Angular, and every modern framework uses the same six concepts. Learn them once, use them everywhere.
{{< /callout >}}

---

## API Foundations

Before we look at the architecture patterns, here are four essential concepts for working with APIs professionally.

### Every API is Different

APIs return data in different shapes. Never assume the structure — always explore first.

```javascript
// Three common response shapes:

// 1. Simple object
{ "name": "Alice", "email": "alice@example.com" }

// 2. Array of objects
[{ "id": 1, "title": "Post 1" }, { "id": 2, "title": "Post 2" }]

// 3. Wrapped response (most common)
{ "status": "ok", "docs": [{ "title": "JavaScript" }] }
```

**The exploration strategy:**

```javascript
const data = await response.json();
console.log(data);              // What shape is it?
console.log(Object.keys(data)); // What properties exist?
```

{{< callout type="info" >}}
**Rule:** Never guess the structure — `console.log` first, then access the data you need.
{{< /callout >}}

### URL Construction & encodeURIComponent

Query parameters tell the API what you want. Build URLs with template literals:

```javascript
const query = "javascript";
const url = `https://openlibrary.org/search.json?q=${query}&limit=12`;
//                                                └ search ┘  └ limit ┘
```

**Problem:** User input can break URLs. The `&` in "mac & cheese" would start a new parameter:

```javascript
// BAD:  ...?q=mac & cheese   ← & starts a new parameter!
// GOOD: ...?q=mac%20%26%20cheese  ← URL-safe

const url = `https://api.com/search?q=${encodeURIComponent(query)}`;
```

{{< callout type="warning" >}}
**Rule:** Always wrap user input with `encodeURIComponent()` when building URLs.
{{< /callout >}}

### The UI State Machine

Every search feature needs five states. Skipping any one creates a bad user experience.

```
              ┌──────────────┐
              │   initial    │  "Search for books"
              └──────┬───────┘
                     │ (submit)
              ┌──────▼───────┐
              │   loading    │  "Searching..."
              └──────┬───────┘
                     │ (response)
         ┌───────────┼───────────┐
   ┌─────▼─────┐ ┌──▼───┐ ┌────▼─────┐
   │  success  │ │ empty │ │  error   │
   │ show data │ │ "No   │ │ "Failed  │
   │           │ │results"│ │ to load" │
   └───────────┘ └───────┘ └──────────┘
```

- **Initial** — before the user does anything ("Search for books to get started!")
- **Loading** — waiting for the API response ("Searching...")
- **Success** — data received, render results
- **Empty** — API returned no results ("No books found")
- **Error** — something went wrong ("Search failed")

{{< callout type="info" >}}
Every search feature on the internet follows this pattern. We implement all five states in the CourseHub `renderBrowseView()` function.
{{< /callout >}}

### FormData API

The modern way to read form inputs in JavaScript:

```javascript
form.addEventListener("submit", (e) => {
  e.preventDefault();                                    // Stop page reload
  const query = new FormData(e.target).get("query").trim(); // Read input by name
  if (!query) return;                                    // Don't fetch on empty
  searchBooks(query);
});
```

Key steps:
- `e.preventDefault()` — stop the page from reloading on form submit
- `new FormData(e.target)` — reads all named inputs from the form
- `.get("query")` — gets the value of the input with `name="query"`
- `.trim()` — removes extra whitespace
- Empty check — don't waste an API call on empty input

{{< callout type="info" >}}
For simple forms (1-2 inputs), `input.value` also works fine. `FormData` scales better for complex forms with many fields.
{{< /callout >}}

---

## JavaScript Techniques You'll Need

CourseHub uses several JavaScript techniques that may be new to you. This section is a **reference** — skim it now, then come back when you encounter these in the build guide below.

### Array Methods

These methods let you work with arrays without writing `for` loops. Each one takes a **callback function** — a small function that runs on each item.

#### `.map()` — Transform Every Item

Creates a **new array** by applying a function to every element. The original array is unchanged.

```javascript
const numbers = [1, 2, 3];
const doubled = numbers.map(n => n * 2);
// doubled = [2, 4, 6]
// numbers is still [1, 2, 3]

// In CourseHub: convert books → HTML strings
const cards = books.map(book => createBookCard(book));
// cards = ["<div class='card'>...</div>", "<div class='card'>...</div>", ...]
```

#### `.join()` — Combine Array Into a String

Takes an array of strings and joins them into one, with an optional separator between each.

```javascript
const words = ["hello", "world"];
words.join(" ");   // "hello world"
words.join(", ");  // "hello, world"
words.join("");    // "helloworld"

// In CourseHub: combine card HTML strings into one block
const html = cards.join('');
// "<div class='card'>...</div><div class='card'>...</div>"
```

{{< callout type="info" >}}
**Common pattern:** `.map().join('')` — transform an array of data into an array of HTML strings, then combine them into one HTML string. You'll see this everywhere in CourseHub.
{{< /callout >}}

#### `.filter()` — Keep Only Matching Items

Creates a **new array** with only the items where the callback returns `true`.

```javascript
const numbers = [1, 2, 3, 4, 5];
const evens = numbers.filter(n => n % 2 === 0);
// evens = [2, 4]

// In CourseHub: remove a bookmark
const updated = bookmarks.filter(b => b.key !== bookToRemove.key);
// Keeps every bookmark EXCEPT the one we want to remove
```

#### `.find()` — Get the First Match

Returns the **first item** where the callback returns `true`, or `undefined` if none match.

```javascript
const users = [{ id: 1, name: "Alice" }, { id: 2, name: "Bob" }];
const bob = users.find(u => u.name === "Bob");
// bob = { id: 2, name: "Bob" }

// In CourseHub: find a book by its key
const book = allBooks.find(b => b.key === clickedKey);
```

#### `.some()` — Check If Any Match

Returns `true` if **at least one** item matches, `false` otherwise. Useful for checking membership.

```javascript
const numbers = [1, 2, 3];
numbers.some(n => n > 2);   // true (3 is > 2)
numbers.some(n => n > 10);  // false (none are > 10)

// In CourseHub: check if a book is already bookmarked
const isBookmarked = bookmarks.some(b => b.key === book.key);
// true if this book is in the bookmarks array
```

### Object Techniques

#### `Object.assign()` — Merge Objects

Copies properties from one object into another. Properties that already exist get **overwritten**; other properties are left alone.

```javascript
const target = { a: 1, b: 2, c: 3 };
Object.assign(target, { b: 99, d: 4 });
// target is now { a: 1, b: 99, c: 3, d: 4 }
// b was updated, d was added, a and c are unchanged
```

```javascript
// In CourseHub: setState merges updates into state
function setState(updates) {
  Object.assign(state, updates);  // Only changes what you pass
  render();
}

setState({ isLoading: true });
// Updates isLoading without erasing currentView, searchResults, etc.
```

#### Spread Operator `[...]` — Copy and Combine Arrays

The `...` (spread) operator "unpacks" an array into individual items. It creates a **new array** rather than modifying the original.

```javascript
const a = [1, 2];
const b = [3, 4];
const combined = [...a, ...b];     // [1, 2, 3, 4]
const withExtra = [...a, 99];      // [1, 2, 99]
// a is still [1, 2] — unchanged
```

```javascript
// In CourseHub: search across both arrays
const allBooks = [...state.searchResults, ...state.bookmarks];

// Add a bookmark (create a new array with the book added)
const updated = [...state.bookmarks, newBook];

// Remove a bookmark (filter creates a new array)
const updated = state.bookmarks.filter(b => b.key !== keyToRemove);
```

{{< callout type="warning" >}}
**Why new arrays?** We never modify `state.bookmarks` directly — we create a new array and pass it to `setState()`. This is called **immutability** and it's a core concept in React. Modifying state directly can cause bugs because `render()` might not notice the change.
{{< /callout >}}

### Safe Data Access

#### Optional Chaining `?.` — Access Without Crashing

Normally, accessing a property on `undefined` crashes your code. Optional chaining (`?.`) returns `undefined` instead of crashing.

```javascript
const book = { title: "JavaScript" };
// book.author_name is undefined

book.author_name[0];    // ERROR: Cannot read properties of undefined
book.author_name?.[0];  // undefined (no crash)
```

```javascript
// In CourseHub: some books don't have author data
book.author_name?.[0] || 'Unknown Author'
//   ↑ if author_name is undefined, returns undefined
//                       ↑ then || picks the fallback
```

#### `||` for Default Values

The `||` operator returns the **right side** if the left side is "falsy" (undefined, null, empty string, 0, false).

```javascript
undefined || 'default'     // 'default'
null || 'default'          // 'default'
'' || 'default'            // 'default'
'hello' || 'default'       // 'hello' (already truthy)

// In CourseHub: show nothing if year is missing
book.first_publish_year || ''
```

#### Ternary Operator `? :` — Inline If/Else

A compact way to choose between two values based on a condition. Format: `condition ? valueIfTrue : valueIfFalse`

```javascript
const age = 20;
const status = age >= 18 ? 'adult' : 'minor';
// status = 'adult'

// In CourseHub: change button text and style based on bookmark status
class="btn ${isBookmarked ? 'bookmarked' : ''}"
// If bookmarked → class="btn bookmarked"
// If not        → class="btn"

${isBookmarked ? 'Bookmarked' : 'Bookmark'}
// If bookmarked → shows "Bookmarked"
// If not        → shows "Bookmark"
```

### Browser APIs

#### `localStorage` — Save Data Across Page Refreshes

The browser provides a simple key-value store that survives page refreshes and browser restarts. It only stores **strings**, so you need `JSON.stringify()` and `JSON.parse()` for objects and arrays.

```javascript
// Save (convert to string first)
const bookmarks = [{ key: "/works/OL123", title: "JavaScript" }];
localStorage.setItem('my-bookmarks', JSON.stringify(bookmarks));

// Load (convert back to object/array)
const saved = JSON.parse(localStorage.getItem('my-bookmarks'));
// saved = [{ key: "/works/OL123", title: "JavaScript" }]

// Handle missing data (first visit — nothing saved yet)
const bookmarks = JSON.parse(localStorage.getItem('my-bookmarks')) || [];
// If nothing is saved, getItem returns null
// JSON.parse(null) returns null
// null || [] gives us an empty array
```

#### HTML `data-*` Attributes — Attach Data to Elements

Custom attributes starting with `data-` let you store information on HTML elements. JavaScript reads them via `element.dataset`.

```html
<button data-action="bookmark" data-key="/works/OL123">
  Bookmark
</button>
```

```javascript
// In an event handler:
e.target.dataset.action  // "bookmark"
e.target.dataset.key     // "/works/OL123"

// data-action → dataset.action (the "data-" prefix is removed)
// data-key    → dataset.key
```

#### `classList.toggle()` — Add or Remove a CSS Class

Adds a class if a condition is true, removes it if false.

```javascript
link.classList.toggle('active', href === '#browse');
// If href is '#browse' → adds 'active' class
// If href is anything else → removes 'active' class
```

{{< callout type="info" >}}
You don't need to memorize all of these. Use this section as a **reference** while following the build guide below — come back whenever you see an unfamiliar technique.
{{< /callout >}}

---

## The Six Patterns

Here's a quick overview before we dive into each one:

| # | Pattern | What It Does | React Equivalent |
|---|---------|-------------|-----------------|
| 1 | **Component** | Functions that create pieces of UI | React components |
| 2 | **State** | One object holds all application data | `useState` |
| 3 | **Props** | Pass data into component functions | Component props |
| 4 | **Events** | Handle user interactions via delegation | `onClick`, `onSubmit` |
| 5 | **Routing** | URL hash controls which view is shown | React Router |
| 6 | **Structure** | Code organized by responsibility | Separate component files |

---

## Pattern 1: Component Functions

A **component** is a function that takes data and returns HTML. Instead of writing HTML directly, you create reusable functions that generate it.

### Why Components?

Without components, you'd duplicate HTML every time you need a similar element:

```javascript
// Without components — repetitive and hard to maintain
resultsDiv.innerHTML = `
  <div class="card"><h3>${books[0].title}</h3><p>${books[0].author}</p></div>
  <div class="card"><h3>${books[1].title}</h3><p>${books[1].author}</p></div>
  <div class="card"><h3>${books[2].title}</h3><p>${books[2].author}</p></div>
`;
```

With components, you write the template once and reuse it:

```javascript
// With components — write once, use for any book
function createBookCard(book) {
  return `
    <div class="card">
      <img src="${book.coverUrl}" alt="${book.title}" />
      <h3>${book.title}</h3>
      <p>${book.author}</p>
    </div>
  `;
}

// Generate cards for any number of books
resultsDiv.innerHTML = books.map(book => createBookCard(book)).join('');
```

### The React Connection

{{< tabs items="Vanilla JS,React (Preview)" >}}
  {{< tab >}}
  ```javascript
  function createBookCard(book) {
    return `
      <div class="card">
        <h3>${book.title}</h3>
        <p>${book.author}</p>
      </div>
    `;
  }
  ```
  {{< /tab >}}
  {{< tab >}}
  ```jsx
  function BookCard({ book }) {
    return (
      <div className="card">
        <h3>{book.title}</h3>
        <p>{book.author}</p>
      </div>
    );
  }
  ```
  {{< /tab >}}
{{< /tabs >}}

Same concept: function takes data, returns UI. Template literals become JSX. `${}` becomes `{}`.

---

## Pattern 2: Central State

**State** is a single object that holds ALL the data your application needs. When state changes, the UI re-renders to reflect the new data.

### Why Central State?

Without it, data lives in scattered variables and the UI gets out of sync:

```javascript
// Scattered state — hard to track, easy to forget to update UI
let currentPage = 'home';
let results = [];
let loading = false;
// Did I update the UI after changing loading? Did I forget?
```

With central state, there's one source of truth and one update mechanism:

```javascript
// Central state — organized and predictable
const state = {
  currentView: 'browse',
  searchResults: [],
  bookmarks: [],
  isLoading: false,
  error: null,
  searchQuery: ''
};

function setState(updates) {
  Object.assign(state, updates);  // Merge updates into state
  render();                        // Re-draw the page
}
```

### How It Works

```
setState({ isLoading: true })
    ↓
Object.assign merges { isLoading: true } into state
    ↓
render() runs
    ↓
UI shows "Searching..." (because state.isLoading is true)
```

{{< callout type="info" >}}
`Object.assign(state, updates)` **merges** — it only changes the properties you pass. `setState({ isLoading: true })` updates `isLoading` without erasing `currentView`, `searchResults`, etc.
{{< /callout >}}

### The React Connection

{{< tabs items="Vanilla JS,React (Preview)" >}}
  {{< tab >}}
  ```javascript
  const state = { isLoading: false, results: [] };

  function setState(updates) {
    Object.assign(state, updates);
    render(); // Must call manually
  }

  setState({ isLoading: true });
  ```
  {{< /tab >}}
  {{< tab >}}
  ```jsx
  const [isLoading, setIsLoading] = useState(false);
  const [results, setResults] = useState([]);

  setIsLoading(true); // React calls render automatically
  ```
  {{< /tab >}}
{{< /tabs >}}

React splits state into individual pieces and handles re-rendering automatically. The concept is the same — state changes trigger UI updates.

---

## Pattern 3: Props (Function Parameters)

**Props** are the data you pass into component functions. The same component renders differently depending on what props it receives.

```javascript
function createBookCard(book, isBookmarked) {
  return `
    <div class="card">
      <h3>${book.title}</h3>
      <button class="btn ${isBookmarked ? 'bookmarked' : ''}">
        ${isBookmarked ? 'Bookmarked' : 'Bookmark'}
      </button>
    </div>
  `;
}

// Same function, different props → different output
createBookCard(jsBook, false);   // Shows "Bookmark" button (blue)
createBookCard(jsBook, true);    // Shows "Bookmarked" button (green)
```

### The React Connection

{{< tabs items="Vanilla JS,React (Preview)" >}}
  {{< tab >}}
  ```javascript
  createBookCard(myBook, true);
  createBookCard(otherBook, false);
  ```
  {{< /tab >}}
  {{< tab >}}
  ```jsx
  <BookCard book={myBook} isBookmarked={true} />
  <BookCard book={otherBook} isBookmarked={false} />
  ```
  {{< /tab >}}
{{< /tabs >}}

In React, props look like HTML attributes. In vanilla JS, they're just function parameters. Same data flow.

---

## Pattern 4: Event Handling (Delegation)

**Event delegation** means placing one event listener on a parent container instead of one on every child element. This is essential when elements are created dynamically by JavaScript.

### Why Delegation?

```javascript
// Problem: book cards don't exist when the page loads!
// They're created later by render() after a search.
// This WON'T work:
document.querySelector('.card').addEventListener('click', ...);
// Error: .card doesn't exist yet
```

```javascript
// Solution: listen on #app, which always exists
document.querySelector('#app').addEventListener('click', (e) => {
  // Check what was actually clicked using data attributes
  if (e.target.dataset.action === 'bookmark') {
    const bookKey = e.target.dataset.key;
    handleBookmark(bookKey);
  }
});
```

### How Data Attributes Work

The HTML created by your component includes `data-*` attributes that identify the element:

```html
<button data-action="bookmark" data-key="/works/OL123">
  Bookmark
</button>
```

The event handler reads these with `e.target.dataset`:

```javascript
e.target.dataset.action  // "bookmark"
e.target.dataset.key     // "/works/OL123"
```

### The React Connection

{{< tabs items="Vanilla JS,React (Preview)" >}}
  {{< tab >}}
  ```javascript
  // Event delegation on container
  app.addEventListener('click', (e) => {
    if (e.target.dataset.action === 'bookmark') {
      handleBookmark(e.target.dataset.key);
    }
  });
  ```
  {{< /tab >}}
  {{< tab >}}
  ```jsx
  // Direct handler on element (React manages lifecycle)
  <button onClick={() => handleBookmark(book.key)}>
    Bookmark
  </button>
  ```
  {{< /tab >}}
{{< /tabs >}}

React eliminates the need for delegation by managing element lifecycle automatically. But the concept — "user action triggers a handler that updates state" — is identical.

---

## Pattern 5: Hash-Based Routing

**Routing** makes the URL control which view is displayed. Hash-based routing uses the `#` portion of the URL (`#browse`, `#bookmarks`, `#about`) to switch views without reloading the page.

### How It Works

```javascript
// HTML — nav links with hash hrefs
<a href="#browse">Browse</a>
<a href="#bookmarks">Bookmarks</a>
<a href="#about">About</a>

// JavaScript — listen for hash changes
window.addEventListener('hashchange', () => {
  const view = location.hash.slice(1) || 'browse';
  // location.hash = "#bookmarks" → view = "bookmarks"
  setState({ currentView: view });
});
```

```
User clicks "Bookmarks"
    ↓
URL changes to myapp.com/#bookmarks
    ↓
hashchange event fires
    ↓
setState({ currentView: 'bookmarks' })
    ↓
render() picks renderBookmarksView()
    ↓
Bookmarks page appears — no page reload!
```

The browser's back and forward buttons work automatically because it tracks hash changes in history.

{{< callout type="info" >}}
This is called a **Single-Page Application (SPA)**. One HTML file, JavaScript swaps the content. React apps are almost always SPAs.
{{< /callout >}}

### The React Connection

{{< tabs items="Vanilla JS,React (Preview)" >}}
  {{< tab >}}
  ```javascript
  window.addEventListener('hashchange', () => {
    const view = location.hash.slice(1) || 'browse';
    setState({ currentView: view });
  });
  ```
  {{< /tab >}}
  {{< tab >}}
  ```jsx
  <Route path="/browse" element={<Browse />} />
  <Route path="/bookmarks" element={<Bookmarks />} />
  <Route path="/about" element={<About />} />
  ```
  {{< /tab >}}
{{< /tabs >}}

React Router handles the URL matching and view switching with declarative components.

---

## Pattern 6: Project Structure

Even in a single JavaScript file, organize your code by **responsibility** — not by when you wrote it.

```javascript
// app.js — organized in sections

// === STATE ===
const state = { ... };
function setState(updates) { ... }

// === COMPONENTS ===
function createBookCard(book) { ... }
function createSearchBar() { ... }

// === VIEWS ===
function renderBrowseView() { ... }
function renderBookmarksView() { ... }
function renderAboutView() { ... }

// === RENDER ===
function render() { ... }

// === API ===
async function searchBooks(query) { ... }

// === EVENT HANDLERS ===
document.querySelector('#app').addEventListener(...)

// === ROUTER ===
window.addEventListener('hashchange', ...)

// === INITIALIZE ===
render();
```

In React, each section becomes its own file:

{{< filetree/container >}}
  {{< filetree/folder name="src" >}}
    {{< filetree/file name="App.jsx" >}}
    {{< filetree/folder name="components" >}}
      {{< filetree/file name="BookCard.jsx" >}}
      {{< filetree/file name="SearchBar.jsx" >}}
    {{< /filetree/folder >}}
    {{< filetree/folder name="pages" >}}
      {{< filetree/file name="Browse.jsx" >}}
      {{< filetree/file name="Bookmarks.jsx" >}}
      {{< filetree/file name="About.jsx" >}}
    {{< /filetree/folder >}}
    {{< filetree/file name="api.js" >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}}

Same organization, separate files.

---

## Building CourseHub Step by Step

Now let's put all six patterns together by building **CourseHub** — a multi-view book browser that searches the Open Library API, bookmarks favorites, and persists data across page refreshes.

Each step adds one piece. **Test after every step** — if something breaks, you'll know exactly where.

{{< filetree/container >}}
  {{< filetree/folder name="coursehub" >}}
    {{< filetree/file name="index.html" >}}
    {{< filetree/file name="style.css" >}}
    {{< filetree/file name="app.js" >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}}

---

### Step 1: HTML Skeleton

Create `index.html` with the minimal structure every SPA needs: a nav bar, a container for views, and a script tag.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>CourseHub</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <nav id="main-nav">
    <h1>CourseHub</h1>
    <div class="nav-links">
      <a href="#browse" class="nav-link active">Browse</a>
      <a href="#bookmarks" class="nav-link">Bookmarks</a>
      <a href="#about" class="nav-link">About</a>
    </div>
  </nav>

  <main id="app"></main>

  <script src="app.js"></script>
</body>
</html>
```

What to notice:
- **Hash links** (`#browse`, `#bookmarks`, `#about`) — these change the URL without reloading the page. They'll drive our router.
- **`<main id="app">`** is empty — JavaScript will render all content here. This is exactly how React apps work: one root element, everything rendered by JS.
- **Three files** — HTML skeleton, CSS styles, JavaScript logic. That's the entire project.

**Test:** Open in browser. You'll see the nav with three links and an empty page below. Click the links — the URL changes but nothing else happens yet.

---

### Step 2: CSS

Create `style.css`. This is a lot of CSS, so paste it in rather than typing it. But notice how it's **organized by responsibility** — the same principle we'll use for JavaScript.

```css
/* === VARIABLES === */
:root {
  --color-primary: #2563eb;
  --color-primary-dark: #1d4ed8;
  --color-bg: #f8fafc;
  --color-surface: #ffffff;
  --color-text: #1e293b;
  --color-text-light: #64748b;
  --color-border: #e2e8f0;
  --color-success: #10b981;
  --color-error: #ef4444;
  --radius: 12px;
  --shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
}

/* === RESET & BASE === */
* { margin: 0; padding: 0; box-sizing: border-box; }

body {
  font-family: system-ui, -apple-system, sans-serif;
  background: var(--color-bg);
  color: var(--color-text);
  min-height: 100vh;
}

/* === NAVIGATION === */
nav {
  background: var(--color-surface);
  padding: 16px 24px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid var(--color-border);
  position: sticky;
  top: 0;
  z-index: 10;
}

nav h1 { font-size: 1.25rem; color: var(--color-primary); }
.nav-links { display: flex; gap: 8px; }

.nav-link {
  text-decoration: none;
  padding: 8px 16px;
  border-radius: 8px;
  color: var(--color-text-light);
  font-weight: 500;
  transition: background 0.2s, color 0.2s;
}

.nav-link:hover { background: var(--color-bg); }
.nav-link.active { background: var(--color-primary); color: white; }

/* === MAIN CONTAINER === */
main {
  max-width: 1000px;
  margin: 24px auto;
  padding: 0 24px;
}

/* === SEARCH BAR === */
.search-bar {
  display: flex;
  gap: 8px;
  margin-bottom: 24px;
}

.search-bar input {
  flex: 1;
  padding: 12px 16px;
  font-size: 1rem;
  border: 2px solid var(--color-border);
  border-radius: var(--radius);
  outline: none;
  transition: border-color 0.2s;
}

.search-bar input:focus { border-color: var(--color-primary); }

.search-bar button {
  padding: 12px 24px;
  font-size: 1rem;
  background: var(--color-primary);
  color: white;
  border: none;
  border-radius: var(--radius);
  cursor: pointer;
  font-weight: 500;
}

.search-bar button:hover { background: var(--color-primary-dark); }

/* === CARD GRID === */
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: 20px;
}

/* === BOOK CARD === */
.card {
  background: var(--color-surface);
  border-radius: var(--radius);
  overflow: hidden;
  box-shadow: var(--shadow);
  transition: transform 0.2s;
}

.card:hover { transform: translateY(-4px); }

.card img {
  width: 100%;
  height: 280px;
  object-fit: cover;
  background: var(--color-bg);
}

.card-body { padding: 12px 16px; }
.card-body h3 { font-size: 1rem; margin-bottom: 4px; line-height: 1.3; }
.card-body .author { color: var(--color-text-light); font-size: 0.85rem; }
.card-body .year { color: var(--color-text-light); font-size: 0.8rem; }

/* === BOOKMARK BUTTON === */
.btn {
  display: inline-block;
  margin-top: 8px;
  padding: 6px 12px;
  font-size: 0.85rem;
  border: 2px solid var(--color-primary);
  border-radius: 8px;
  background: transparent;
  color: var(--color-primary);
  cursor: pointer;
  transition: background 0.2s, color 0.2s;
}

.btn:hover { background: var(--color-primary); color: white; }
.btn.bookmarked {
  background: var(--color-success);
  border-color: var(--color-success);
  color: white;
}

/* === STATE MESSAGES === */
.loading, .error, .empty, .initial {
  text-align: center;
  padding: 60px 20px;
  font-size: 1.1rem;
  color: var(--color-text-light);
}

.error { color: var(--color-error); }

/* === ABOUT PAGE === */
.about { max-width: 600px; line-height: 1.8; }
.about h2 { margin-bottom: 12px; }
.about h3 { margin: 20px 0 8px; }
.about ul { margin-left: 20px; }
.about li { margin-bottom: 4px; }
```

Key CSS patterns to notice:
- **CSS Variables** at the top — change one value, it updates everywhere
- **Section labels** — navigation, search, cards, states, about — just like we'll organize JavaScript
- **Component classes** — `.card`, `.search-bar`, `.btn` are self-contained styles
- **State classes** — `.loading`, `.error`, `.empty`, `.initial` — one for each UI state
- **`repeat(auto-fill, minmax(220px, 1fr))`** — a responsive grid that wraps automatically, no media queries needed

**Test:** Refresh the browser. The nav should now be styled with a blue "Browse" link.

---

### Step 3: State + Render Scaffold

Create `app.js`. This is the most important step — we establish the **state → render** cycle that drives the entire application.

```javascript
// ============================================
// STATE — One object holds ALL application data
// ============================================
const state = {
  currentView: 'browse',
  searchResults: [],
  bookmarks: JSON.parse(localStorage.getItem('coursehub-bookmarks')) || [],
  isLoading: false,
  error: null,
  searchQuery: ''
};

function setState(updates) {
  Object.assign(state, updates);
  render();
}

// ============================================
// RENDER — Reads state, updates the page
// ============================================
function render() {
  const app = document.querySelector('#app');
  app.innerHTML = `<p>Current view: ${state.currentView}</p>`;
}

// ============================================
// INITIALIZE
// ============================================
render();
```

{{< callout type="info" >}}
**New techniques in this step:**
- **`Object.assign(state, updates)`** merges the updates into state without erasing other properties — see [Object Techniques](#object-techniques) above
- **`JSON.parse(localStorage.getItem(...))`** loads saved bookmarks from the browser — see [Browser APIs](#browser-apis) above
- **`|| []`** provides an empty array as a fallback if nothing is saved yet — see [Safe Data Access](#safe-data-access) above
{{< /callout >}}

How the state cycle works:

```
setState({ isLoading: true })
    ↓
Object.assign merges { isLoading: true } into state
    ↓
render() runs
    ↓
UI shows updated content
```

`setState({ isLoading: true })` only changes `isLoading` — it doesn't erase `currentView`, `searchResults`, or any other property.

**Test:** Refresh the browser. You should see "Current view: browse". Open the console and type `setState({ currentView: 'bookmarks' })` — the text changes. That's the entire pattern: change state, UI updates.

---

### Step 4: Router

Add a hash-change listener so the nav links actually switch views. Add this code **above the INITIALIZE section**:

```javascript
// ============================================
// ROUTER — Hash changes drive view switching
// ============================================
window.addEventListener('hashchange', () => {
  const view = location.hash.slice(1) || 'browse';
  setState({ currentView: view });
});
```

Also update the INITIALIZE section to read the hash on first load:

```javascript
// ============================================
// INITIALIZE
// ============================================
const initialView = location.hash.slice(1) || 'browse';
setState({ currentView: initialView });
```

How it works:
- `hashchange` fires whenever the `#` part of the URL changes
- `location.hash` returns `"#browse"` — `.slice(1)` removes the `#` to get `"browse"`
- We call `setState`, which triggers `render` — the cycle continues
- The `|| 'browse'` fallback handles the case where there's no hash in the URL
- On first load, `hashchange` hasn't fired yet, so we read the hash manually

**Test:** Click the nav links — the text changes between "Current view: browse", "bookmarks", "about". Try the browser's back/forward buttons — they work too!

---

### Step 5: Components + Views

Now we replace the simple render with real view functions. Each view is a function that returns HTML — this is the **Component pattern**.

Add the **COMPONENTS** and **VIEWS** sections above the render function, then update `render()`:

```javascript
// ============================================
// COMPONENTS — Functions that return HTML
// ============================================
function createSearchBar() {
  return `
    <form id="search-form" class="search-bar">
      <input type="text" name="query"
        placeholder="Search for books... (e.g., javascript, design)"
        value="${state.searchQuery}" />
      <button type="submit">Search</button>
    </form>
  `;
}

// ============================================
// VIEWS — Composed from components
// ============================================
function renderBrowseView() {
  let content = createSearchBar();

  if (state.isLoading) {
    content += '<p class="loading">Searching...</p>';
  } else if (state.error) {
    content += `<p class="error">${state.error}</p>`;
  } else if (state.searchResults.length > 0) {
    content += '<div class="card-grid">Cards go here</div>';
  } else if (state.searchQuery) {
    content += `<p class="empty">No books found for "${state.searchQuery}"</p>`;
  } else {
    content += '<p class="initial">Search for books to get started!</p>';
  }

  return content;
}

function renderBookmarksView() {
  if (state.bookmarks.length === 0) {
    return '<p class="empty">No bookmarks yet. Browse and save some books!</p>';
  }
  return `<h2>Your Bookmarks (${state.bookmarks.length})</h2>
    <div class="card-grid">Bookmark cards go here</div>`;
}

function renderAboutView() {
  return `
    <div class="about">
      <h2>About CourseHub</h2>
      <p>A demo app built with vanilla JavaScript to demonstrate
         application architecture patterns.</p>
      <h3>Patterns Used</h3>
      <ul>
        <li>Component Functions</li>
        <li>Central State Management</li>
        <li>Props (Function Parameters)</li>
        <li>Event Delegation</li>
        <li>Hash-Based Routing</li>
        <li>Organized Project Structure</li>
      </ul>
      <p><strong>All of these patterns transfer directly to React!</strong></p>
    </div>
  `;
}
```

Now **replace** the simple `render()` function with the real one:

```javascript
// ============================================
// RENDER — Reads state, updates the page
// ============================================
function render() {
  const app = document.querySelector('#app');

  const views = {
    browse: renderBrowseView,
    bookmarks: renderBookmarksView,
    about: renderAboutView
  };

  const viewFn = views[state.currentView] || renderBrowseView;
  app.innerHTML = viewFn();

  // Update active nav link
  document.querySelectorAll('.nav-link').forEach(link => {
    const href = link.getAttribute('href');
    link.classList.toggle('active', href === `#${state.currentView}`);
  });
}
```

{{< callout type="info" >}}
**New technique:** `classList.toggle('active', condition)` adds the `'active'` class when the condition is `true` and removes it when `false` — see [Browser APIs](#browser-apis) above.
{{< /callout >}}

Notice how `renderBrowseView()` handles **all five states** from the UI State Machine:
1. `state.isLoading` → "Searching..."
2. `state.error` → error message
3. `state.searchResults.length > 0` → show cards (placeholder for now)
4. `state.searchQuery` (but no results) → "No books found"
5. Default → "Search for books to get started!"

The `render()` function uses a **views object** that maps route names to view functions — this is the same idea as React Router's route configuration.

**Test:** Refresh the browser. You should see the search bar + "Search for books to get started!" Click "Bookmarks" — "No bookmarks yet." Click "About" — the patterns list. Click "Browse" — back to search.

---

### Step 6: API Search Function

Now we connect to a real API. Add the **API** section above the ROUTER:

```javascript
// ============================================
// API — Fetch data and update state
// ============================================
async function searchBooks(query) {
  setState({ isLoading: true, error: null, searchQuery: query });

  try {
    const url = `https://openlibrary.org/search.json?q=${encodeURIComponent(query)}&limit=12`;
    const response = await fetch(url);

    if (!response.ok) {
      throw new Error(`Search failed (${response.status})`);
    }

    const data = await response.json();
    setState({ searchResults: data.docs || [], isLoading: false });
  } catch (error) {
    setState({ error: error.message, isLoading: false, searchResults: [] });
  }
}
```

This function uses everything from the API Foundations section:
- **`encodeURIComponent(query)`** — makes user input URL-safe (what if they search "c++ programming"? The `+` would break the URL)
- **`async/await` + `try/catch`** — handles the async API call and any errors
- **`data.docs || []`** — Open Library wraps results in a `docs` property; `|| []` provides a fallback if it's missing
- **State machine transitions** — sets `isLoading: true` at the start, then updates to either success (`searchResults: data.docs`) or error (`error: error.message`)

We can't test this yet — we need an event handler to call `searchBooks`. That's next.

---

### Step 7: Search Event Handler

Add the **EVENT HANDLERS** section above the ROUTER. This uses **event delegation** — we attach the listener to `#app` (which always exists) instead of the form (which is created dynamically by `render()`):

```javascript
// ============================================
// EVENT HANDLERS — Delegated on #app
// ============================================
document.querySelector('#app').addEventListener('submit', async (e) => {
  if (e.target.id === 'search-form') {
    e.preventDefault();
    const query = new FormData(e.target).get('query').trim();
    if (!query) return;
    searchBooks(query);
  }
});
```

{{< callout type="info" >}}
**Why event delegation?** The search form doesn't exist when the page first loads — it's created by `renderBrowseView()`. If we tried `document.querySelector('#search-form').addEventListener(...)`, it would crash because the form doesn't exist yet. Instead, we listen on `#app` which is always in the HTML, and check `e.target.id` to see what was submitted.
{{< /callout >}}

How FormData works here:
- `e.preventDefault()` — stops the form from reloading the page
- `new FormData(e.target)` — reads all named inputs from the form
- `.get('query')` — gets the value of the input with `name="query"`
- `.trim()` — removes leading/trailing whitespace
- `if (!query) return` — don't call the API on empty input

**Test:** Type "javascript" in the search bar and press Enter. You should see "Searching..." briefly, then "Cards go here" (we'll make real cards next). Try "xyznotarealbook" — you should see "No books found."

---

### Step 8: Book Card Component

Now let's create real book cards. This step uses many JavaScript techniques at once.

Add `createBookCard` to the **COMPONENTS** section (above `createSearchBar`):

```javascript
function createBookCard(book, isBookmarked) {
  const coverId = book.cover_i;
  const coverUrl = coverId
    ? `https://covers.openlibrary.org/b/id/${coverId}-M.jpg`
    : 'https://placehold.co/200x300/e2e8f0/64748b?text=No+Cover';

  return `
    <div class="card" data-key="${book.key}">
      <img src="${coverUrl}" alt="${book.title}" />
      <div class="card-body">
        <h3>${book.title}</h3>
        <p class="author">${book.author_name?.[0] || 'Unknown Author'}</p>
        <p class="year">${book.first_publish_year || ''}</p>
        <button data-action="bookmark" data-key="${book.key}"
          class="btn ${isBookmarked ? 'bookmarked' : ''}">
          ${isBookmarked ? 'Bookmarked' : 'Bookmark'}
        </button>
      </div>
    </div>
  `;
}
```

{{< callout type="info" >}}
**Techniques used in this component:**
- **Ternary `? :`** — chooses the cover URL (real cover vs placeholder), button text, and CSS class — see [Safe Data Access](#safe-data-access)
- **Optional chaining `?.`** — `book.author_name?.[0]` safely accesses the first author even if `author_name` is undefined — see [Safe Data Access](#safe-data-access)
- **`||` for defaults** — `|| 'Unknown Author'` provides a fallback when data is missing — see [Safe Data Access](#safe-data-access)
- **`data-*` attributes** — `data-action="bookmark"` and `data-key="${book.key}"` identify the button for event handling — see [Browser APIs](#browser-apis)
{{< /callout >}}

The function takes two **props** (function parameters): `book` (the data) and `isBookmarked` (a boolean that controls the button's appearance). Same component, different props, different output.

Now **update `renderBrowseView()`** — replace the `'Cards go here'` placeholder:

```javascript
  } else if (state.searchResults.length > 0) {
    const cards = state.searchResults.map(book => {
      const isBookmarked = state.bookmarks.some(b => b.key === book.key);
      return createBookCard(book, isBookmarked);
    }).join('');
    content += `<div class="card-grid">${cards}</div>`;
  }
```

Here's what this code does, step by step:

```
state.searchResults = [book1, book2, book3, ...]
        │
        ▼  .map(book => ...)
[createBookCard(book1), createBookCard(book2), createBookCard(book3), ...]
        │
        ▼  result of .map()
["<div class='card'>...</div>", "<div class='card'>...</div>", ...]
        │
        ▼  .join('')
"<div class='card'>...</div><div class='card'>...</div>..."
```

- **`.map()`** transforms each book object into an HTML string by calling `createBookCard()`
- **`.some()`** checks if each book is already bookmarked
- **`.join('')`** combines all the HTML strings into one big string

**Test:** Search "javascript" — you should see real book cards with cover images, titles, and authors! Books without covers get a placeholder image.

---

### Step 9: Bookmark Handler

Add the bookmark click handler to the **EVENT HANDLERS** section (after the submit handler):

```javascript
document.querySelector('#app').addEventListener('click', (e) => {
  if (e.target.dataset.action === 'bookmark') {
    const key = e.target.dataset.key;
    const book = [...state.searchResults, ...state.bookmarks].find(b => b.key === key);
    if (!book) return;

    const exists = state.bookmarks.some(b => b.key === key);
    const updatedBookmarks = exists
      ? state.bookmarks.filter(b => b.key !== key)
      : [...state.bookmarks, book];

    localStorage.setItem('coursehub-bookmarks', JSON.stringify(updatedBookmarks));
    setState({ bookmarks: updatedBookmarks });
  }
});
```

This is the most technique-dense code in the app. Let's break it down line by line:

```javascript
// 1. Event delegation: check if a bookmark button was clicked
if (e.target.dataset.action === 'bookmark')

// 2. Read the book's key from the data attribute
const key = e.target.dataset.key;

// 3. Find the full book object across both arrays
const book = [...state.searchResults, ...state.bookmarks].find(b => b.key === key);
//           └─── spread: combine into one array ───┘     └─ find: get the match ─┘

// 4. Check if this book is already bookmarked
const exists = state.bookmarks.some(b => b.key === key);

// 5. Toggle: remove if exists, add if not
const updatedBookmarks = exists
  ? state.bookmarks.filter(b => b.key !== key)   // Remove: keep all EXCEPT this one
  : [...state.bookmarks, book];                   // Add: copy existing + append new

// 6. Save to localStorage so bookmarks survive page refresh
localStorage.setItem('coursehub-bookmarks', JSON.stringify(updatedBookmarks));

// 7. Update state → triggers render → UI updates
setState({ bookmarks: updatedBookmarks });
```

{{< callout type="warning" >}}
**Key concept: immutable updates.** We never do `state.bookmarks.push(book)` — that would modify the array in place. Instead, we create a **new array** with `[...state.bookmarks, book]` or `.filter()`. This is how React works: always create new data, never mutate existing data.
{{< /callout >}}

Now **update `renderBookmarksView()`** to show real cards instead of the placeholder:

```javascript
function renderBookmarksView() {
  if (state.bookmarks.length === 0) {
    return '<p class="empty">No bookmarks yet. Browse and save some books!</p>';
  }
  const cards = state.bookmarks.map(book => createBookCard(book, true)).join('');
  return `<h2>Your Bookmarks (${state.bookmarks.length})</h2>
    <div class="card-grid">${cards}</div>`;
}
```

Notice: we pass `true` as `isBookmarked` for every card in the bookmarks view — they're all bookmarked by definition.

**Test:**
1. Search "javascript" → click "Bookmark" on a few books → buttons turn green ("Bookmarked")
2. Click "Bookmarks" in the nav → your saved books appear
3. **Refresh the page** → bookmarks are still there! (localStorage)
4. Click a green "Bookmarked" button → it unbookmarks (removed from the list)

---

### Step 10: Final Check

Your `app.js` should now have these sections in order:

```
STATE           → state object + setState()
COMPONENTS      → createBookCard() + createSearchBar()
VIEWS           → renderBrowseView() + renderBookmarksView() + renderAboutView()
RENDER          → render() function
API             → searchBooks()
EVENT HANDLERS  → submit handler + click handler
ROUTER          → hashchange listener
INITIALIZE      → read hash + first render
```

Run through this checklist to make sure everything works:

| Action | Expected Result |
|--------|----------------|
| Page loads | Nav + search bar + "Search for books to get started!" |
| Search "javascript" | Loading → book cards with covers and titles |
| Search "xyznotarealbook" | Loading → "No books found for xyznotarealbook" |
| Network offline + search | Loading → error message |
| Click "Bookmark" on a card | Button changes to green "Bookmarked" |
| Navigate to Bookmarks | Saved books appear |
| Refresh the page | Bookmarks still there (localStorage) |
| Click "Bookmarked" to remove | Book removed from bookmarks |
| Navigate to About | About page with patterns list |
| Browser back/forward | Views switch correctly |
| Click nav links | Active link highlights, view switches |

Every state is handled. No surprises for the user.

---

## Complete Reference Code

If you followed the steps above, your code should match this. Use it to compare or as a starting point if you got stuck.

{{% details title="index.html (click to expand)" closed="true" %}}
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>CourseHub</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <nav id="main-nav">
    <h1>CourseHub</h1>
    <div class="nav-links">
      <a href="#browse" class="nav-link active">Browse</a>
      <a href="#bookmarks" class="nav-link">Bookmarks</a>
      <a href="#about" class="nav-link">About</a>
    </div>
  </nav>

  <main id="app"></main>

  <script src="app.js"></script>
</body>
</html>
```
{{% /details %}}

{{% details title="style.css (click to expand)" closed="true" %}}
```css
/* === VARIABLES === */
:root {
  --color-primary: #2563eb;
  --color-primary-dark: #1d4ed8;
  --color-bg: #f8fafc;
  --color-surface: #ffffff;
  --color-text: #1e293b;
  --color-text-light: #64748b;
  --color-border: #e2e8f0;
  --color-success: #10b981;
  --color-error: #ef4444;
  --radius: 12px;
  --shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
}

/* === RESET & BASE === */
* { margin: 0; padding: 0; box-sizing: border-box; }

body {
  font-family: system-ui, -apple-system, sans-serif;
  background: var(--color-bg);
  color: var(--color-text);
  min-height: 100vh;
}

/* === NAVIGATION === */
nav {
  background: var(--color-surface);
  padding: 16px 24px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid var(--color-border);
  position: sticky;
  top: 0;
  z-index: 10;
}

nav h1 { font-size: 1.25rem; color: var(--color-primary); }
.nav-links { display: flex; gap: 8px; }

.nav-link {
  text-decoration: none;
  padding: 8px 16px;
  border-radius: 8px;
  color: var(--color-text-light);
  font-weight: 500;
  transition: background 0.2s, color 0.2s;
}

.nav-link:hover { background: var(--color-bg); }
.nav-link.active { background: var(--color-primary); color: white; }

/* === MAIN CONTAINER === */
main {
  max-width: 1000px;
  margin: 24px auto;
  padding: 0 24px;
}

/* === SEARCH BAR === */
.search-bar {
  display: flex;
  gap: 8px;
  margin-bottom: 24px;
}

.search-bar input {
  flex: 1;
  padding: 12px 16px;
  font-size: 1rem;
  border: 2px solid var(--color-border);
  border-radius: var(--radius);
  outline: none;
  transition: border-color 0.2s;
}

.search-bar input:focus { border-color: var(--color-primary); }

.search-bar button {
  padding: 12px 24px;
  font-size: 1rem;
  background: var(--color-primary);
  color: white;
  border: none;
  border-radius: var(--radius);
  cursor: pointer;
  font-weight: 500;
}

.search-bar button:hover { background: var(--color-primary-dark); }

/* === CARD GRID === */
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: 20px;
}

/* === BOOK CARD === */
.card {
  background: var(--color-surface);
  border-radius: var(--radius);
  overflow: hidden;
  box-shadow: var(--shadow);
  transition: transform 0.2s;
}

.card:hover { transform: translateY(-4px); }

.card img {
  width: 100%;
  height: 280px;
  object-fit: cover;
  background: var(--color-bg);
}

.card-body { padding: 12px 16px; }
.card-body h3 { font-size: 1rem; margin-bottom: 4px; line-height: 1.3; }
.card-body .author { color: var(--color-text-light); font-size: 0.85rem; }
.card-body .year { color: var(--color-text-light); font-size: 0.8rem; }

/* === BOOKMARK BUTTON === */
.btn {
  display: inline-block;
  margin-top: 8px;
  padding: 6px 12px;
  font-size: 0.85rem;
  border: 2px solid var(--color-primary);
  border-radius: 8px;
  background: transparent;
  color: var(--color-primary);
  cursor: pointer;
  transition: background 0.2s, color 0.2s;
}

.btn:hover { background: var(--color-primary); color: white; }
.btn.bookmarked {
  background: var(--color-success);
  border-color: var(--color-success);
  color: white;
}

/* === STATE MESSAGES === */
.loading, .error, .empty, .initial {
  text-align: center;
  padding: 60px 20px;
  font-size: 1.1rem;
  color: var(--color-text-light);
}

.error { color: var(--color-error); }

/* === ABOUT PAGE === */
.about { max-width: 600px; line-height: 1.8; }
.about h2 { margin-bottom: 12px; }
.about h3 { margin: 20px 0 8px; }
.about ul { margin-left: 20px; }
.about li { margin-bottom: 4px; }
```
{{% /details %}}

{{% details title="app.js (click to expand)" closed="true" %}}
```javascript
// ============================================
// STATE — One object holds ALL application data
// ============================================
const state = {
  currentView: 'browse',
  searchResults: [],
  bookmarks: JSON.parse(localStorage.getItem('coursehub-bookmarks')) || [],
  isLoading: false,
  error: null,
  searchQuery: ''
};

function setState(updates) {
  Object.assign(state, updates);
  render();
}

// ============================================
// COMPONENTS — Functions that return HTML
// ============================================
function createBookCard(book, isBookmarked) {
  const coverId = book.cover_i;
  const coverUrl = coverId
    ? `https://covers.openlibrary.org/b/id/${coverId}-M.jpg`
    : 'https://placehold.co/200x300/e2e8f0/64748b?text=No+Cover';

  return `
    <div class="card" data-key="${book.key}">
      <img src="${coverUrl}" alt="${book.title}" />
      <div class="card-body">
        <h3>${book.title}</h3>
        <p class="author">${book.author_name?.[0] || 'Unknown Author'}</p>
        <p class="year">${book.first_publish_year || ''}</p>
        <button data-action="bookmark" data-key="${book.key}"
          class="btn ${isBookmarked ? 'bookmarked' : ''}">
          ${isBookmarked ? 'Bookmarked' : 'Bookmark'}
        </button>
      </div>
    </div>
  `;
}

function createSearchBar() {
  return `
    <form id="search-form" class="search-bar">
      <input type="text" name="query"
        placeholder="Search for books... (e.g., javascript, design)"
        value="${state.searchQuery}" />
      <button type="submit">Search</button>
    </form>
  `;
}

// ============================================
// VIEWS — Composed from components
// ============================================
function renderBrowseView() {
  let content = createSearchBar();

  if (state.isLoading) {
    content += '<p class="loading">Searching...</p>';
  } else if (state.error) {
    content += `<p class="error">${state.error}</p>`;
  } else if (state.searchResults.length > 0) {
    const cards = state.searchResults.map(book => {
      const isBookmarked = state.bookmarks.some(b => b.key === book.key);
      return createBookCard(book, isBookmarked);
    }).join('');
    content += `<div class="card-grid">${cards}</div>`;
  } else if (state.searchQuery) {
    content += `<p class="empty">No books found for "${state.searchQuery}"</p>`;
  } else {
    content += '<p class="initial">Search for books to get started!</p>';
  }

  return content;
}

function renderBookmarksView() {
  if (state.bookmarks.length === 0) {
    return '<p class="empty">No bookmarks yet. Browse and save some books!</p>';
  }
  const cards = state.bookmarks.map(book => createBookCard(book, true)).join('');
  return `<h2>Your Bookmarks (${state.bookmarks.length})</h2>
    <div class="card-grid">${cards}</div>`;
}

function renderAboutView() {
  return `
    <div class="about">
      <h2>About CourseHub</h2>
      <p>A demo app built with vanilla JavaScript to demonstrate
         application architecture patterns.</p>
      <h3>Patterns Used</h3>
      <ul>
        <li>Component Functions</li>
        <li>Central State Management</li>
        <li>Props (Function Parameters)</li>
        <li>Event Delegation</li>
        <li>Hash-Based Routing</li>
        <li>Organized Project Structure</li>
      </ul>
      <p><strong>All of these patterns transfer directly to React!</strong></p>
    </div>
  `;
}

// ============================================
// RENDER — Reads state, updates the page
// ============================================
function render() {
  const app = document.querySelector('#app');

  const views = {
    browse: renderBrowseView,
    bookmarks: renderBookmarksView,
    about: renderAboutView
  };

  const viewFn = views[state.currentView] || renderBrowseView;
  app.innerHTML = viewFn();

  // Update active nav link
  document.querySelectorAll('.nav-link').forEach(link => {
    const href = link.getAttribute('href');
    link.classList.toggle('active', href === `#${state.currentView}`);
  });
}

// ============================================
// API — Fetch data and update state
// ============================================
async function searchBooks(query) {
  setState({ isLoading: true, error: null, searchQuery: query });

  try {
    const url = `https://openlibrary.org/search.json?q=${encodeURIComponent(query)}&limit=12`;
    const response = await fetch(url);

    if (!response.ok) {
      throw new Error(`Search failed (${response.status})`);
    }

    const data = await response.json();
    setState({ searchResults: data.docs || [], isLoading: false });
  } catch (error) {
    setState({ error: error.message, isLoading: false, searchResults: [] });
  }
}

// ============================================
// EVENT HANDLERS — Delegated on #app
// ============================================
document.querySelector('#app').addEventListener('submit', async (e) => {
  if (e.target.id === 'search-form') {
    e.preventDefault();
    const query = new FormData(e.target).get('query').trim();
    if (!query) return;
    searchBooks(query);
  }
});

document.querySelector('#app').addEventListener('click', (e) => {
  if (e.target.dataset.action === 'bookmark') {
    const key = e.target.dataset.key;
    const book = [...state.searchResults, ...state.bookmarks].find(b => b.key === key);
    if (!book) return;

    const exists = state.bookmarks.some(b => b.key === key);
    const updatedBookmarks = exists
      ? state.bookmarks.filter(b => b.key !== key)
      : [...state.bookmarks, book];

    localStorage.setItem('coursehub-bookmarks', JSON.stringify(updatedBookmarks));
    setState({ bookmarks: updatedBookmarks });
  }
});

// ============================================
// ROUTER — Hash changes drive view switching
// ============================================
window.addEventListener('hashchange', () => {
  const view = location.hash.slice(1) || 'browse';
  setState({ currentView: view });
});

// ============================================
// INITIALIZE
// ============================================
const initialView = location.hash.slice(1) || 'browse';
setState({ currentView: initialView });
```
{{% /details %}}

---

## The React Bridge

This table maps everything you built today to what you'll see in React:

| Vanilla JS (Today) | React (Next Week) | Concept |
|---|---|---|
| `function createCard(book) { return \`...\` }` | `function Card({ book }) { return <div>...</div> }` | **Component** |
| `state` object + `setState()` + `render()` | `useState()` + automatic re-render | **State** |
| `createCard(book, true)` | `<Card book={book} saved={true} />` | **Props** |
| Template literals `` \`<h3>${title}</h3>\` `` | JSX `<h3>{title}</h3>` | **Templating** |
| Event delegation on `#app` | `onClick={handler}` on elements | **Events** |
| `hashchange` listener | React Router `<Route>` | **Routing** |
| `localStorage.setItem(...)` | `useEffect(() => {...}, [])` | **Side Effects** |
| Organized code sections | Separate `.jsx` files | **Structure** |

{{< callout type="info" >}}
React doesn't add new concepts — it **automates the plumbing**. You already understand components, state, props, events, and routing. React just provides a cleaner API for them.
{{< /callout >}}

---

## Troubleshooting & Common Mistakes

{{% details title="setState doesn't update the UI" closed="true" %}}
Make sure `setState()` calls `render()`. If you update state directly (`state.isLoading = true`), the UI won't change because `render()` never runs.

```javascript
// Wrong — UI won't update
state.isLoading = true;

// Right — triggers render
setState({ isLoading: true });
```
{{% /details %}}

{{% details title="Event listeners don't work on dynamic elements" closed="true" %}}
Elements created by `render()` don't exist when the page loads. Use **event delegation** — attach the listener to `#app` (which always exists) and check `e.target` to identify what was clicked.

```javascript
// Wrong — .card doesn't exist yet
document.querySelector('.card').addEventListener('click', ...);

// Right — #app always exists
document.querySelector('#app').addEventListener('click', (e) => {
  if (e.target.dataset.action === 'bookmark') { ... }
});
```
{{% /details %}}

{{% details title="Search crashes with 'Cannot read properties of undefined'" closed="true" %}}
API responses may have missing fields. Use **optional chaining** and **fallback values**:

```javascript
// Might crash if author_name is undefined
book.author_name[0]

// Safe — returns 'Unknown Author' if author_name is missing
book.author_name?.[0] || 'Unknown Author'
```
{{% /details %}}

{{% details title="Bookmarks disappear on refresh" closed="true" %}}
Make sure you're saving to `localStorage` whenever bookmarks change AND loading from `localStorage` when the app initializes:

```javascript
// Save — in the bookmark handler
localStorage.setItem('coursehub-bookmarks', JSON.stringify(updatedBookmarks));

// Load — in the initial state
bookmarks: JSON.parse(localStorage.getItem('coursehub-bookmarks')) || []
```
{{% /details %}}

{{% details title="Hash routing doesn't work on first load" closed="true" %}}
The `hashchange` event only fires when the hash **changes**. On first load, read the hash directly:

```javascript
// Handle first load
const initialView = location.hash.slice(1) || 'browse';
setState({ currentView: initialView });

// Handle subsequent navigation
window.addEventListener('hashchange', () => { ... });
```
{{% /details %}}

---

## Key Takeaways

{{< callout emoji="🎯" >}}
**Six patterns for any application:**
1. **Component** — functions that return HTML; reusable UI pieces
2. **State** — one data object + `setState()` + `render()` = predictable UI
3. **Props** — function parameters that customize components
4. **Events** — delegation on containers for dynamic content
5. **Routing** — URL hash drives view switching (SPA pattern)
6. **Structure** — organize code by responsibility, not by when you wrote it

**The React connection:** Every pattern maps directly to a React concept. Learn them in vanilla JS → recognize them instantly in React.
{{< /callout >}}

---

## Additional Resources

### Official Documentation

{{< cards >}}
  {{< card link="https://developer.mozilla.org/en-US/docs/Web/API/Window/hashchange_event" title="MDN: hashchange Event" subtitle="Hash-based routing reference" icon="book-open" >}}
  {{< card link="https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API" title="MDN: Web Storage API" subtitle="localStorage and sessionStorage" icon="book-open" >}}
  {{< card link="https://developer.mozilla.org/en-US/docs/Web/API/FormData" title="MDN: FormData API" subtitle="Processing form data in JavaScript" icon="book-open" >}}
{{< /cards >}}

### API Documentation

{{< cards >}}
  {{< card link="https://openlibrary.org/developers/api" title="Open Library API" subtitle="Free book search API (no key required)" icon="code" >}}
{{< /cards >}}

### Further Learning

{{< cards >}}
  {{< card link="https://javascript.info/event-delegation" title="javascript.info: Event Delegation" subtitle="In-depth event delegation guide" icon="academic-cap" >}}
  {{< card link="https://developer.mozilla.org/en-US/docs/Glossary/SPA" title="MDN: Single-Page Application" subtitle="What SPAs are and how they work" icon="academic-cap" >}}
{{< /cards >}}

---

## Questions?

- **Email:** dmandloi@neiu.edu
- **Office Hours:**
  - Tuesdays, 10:00 AM – 12:00 PM (In Person) — Room: CBT 102
  - Fridays, 11:30 AM – 12:30 PM (Virtual)
- **Book an appointment:** https://calendar.app.google/nbJhMZwosh4r4i1a8
