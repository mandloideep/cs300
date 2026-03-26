---
title: "Week 10 Session 2: Composition, Lifecycle, Hooks & Thinking in React"
weight: 2
toc: true
---

## Introduction

Session 1 gave you the complete React communication toolkit: props for data down, useState for changeable data, and callback props for actions up. This session adds the remaining pieces: **Git workflow for React projects**, **composition patterns**, the **component lifecycle**, **useEffect** (side effects), **useRef** (DOM access), and the **Thinking in React** methodology.

### What You'll Learn

- How to initialize a Git repo with a Vite React project and push to GitHub
- Composition patterns (named slots, specialization, container/presentational)
- The React component lifecycle (mount, update, unmount)
- useEffect for side effects with dependency arrays
- useRef for DOM access and persistent values
- The 5-step Thinking in React methodology
- React Developer Tools for debugging

---

## Part 1: Git + Vite React Workflow

### The Problem

Up until now, your Git workflow has been: create a repo on GitHub with a README, clone it, and start working. But `npm create vite` needs an **empty folder** — or it creates its own. These two workflows conflict.

```bash
# Old workflow (HTML/CSS projects):
# 1. Create repo on GitHub WITH README ✅
# 2. Clone it → folder has README
# 3. Start coding

# React problem:
# npm create vite@latest my-app -- --template react
# ❌ Error: directory is not empty (has README)
```

### The Solution: Vite-First Workflow

**Create the Vite project first, initialize Git inside it, then push to an empty GitHub repo.**

```bash
# Step 1: Create the Vite project
npm create vite@latest my-react-app -- --template react

# Step 2: Enter the project and install dependencies
cd my-react-app
npm install

# Step 3: Vite already created a .gitignore for you!
# (includes node_modules/, dist/, .env, etc.)

# Step 4: Initialize Git
git init

# Step 5: First commit
git add .
git commit -m "Initial commit: Vite React setup"

# Step 6: Create an EMPTY repo on GitHub
# ⚠️ Do NOT check "Add a README" or "Add .gitignore"

# Step 7: Connect to GitHub
git remote add origin git@github.com:your-username/my-react-app.git

# Step 8: Push
git push -u origin main
```

{{< callout type="warning" >}}
**The GitHub repo MUST be empty.** When creating the repo on GitHub, do NOT check "Add a README file" or "Add .gitignore". Vite already provides a `.gitignore`. If both repos have files, you'll get merge conflicts on the first push.
{{< /callout >}}

### Key Details

- **Vite auto-generates `.gitignore`**: Already excludes `node_modules/`, `dist/`, `.env` — you don't need to create one
- **The `-u` flag**: `git push -u origin main` sets up tracking so future pushes just need `git push`
- **Workflow summary**: Vite first → Git second → GitHub last

### Team Workflow (GP2)

For group projects, **one team member** does the full setup above. Everyone else:

```bash
git clone git@github.com:team/project.git
cd project
npm install    # Installs node_modules locally (not in repo)
npm run dev    # Start development server
```

{{< callout type="info" >}}
**Why `npm install` after cloning?** The `node_modules/` folder is in `.gitignore` — it's NOT in the repository. Each person installs their own copy of the dependencies locally. This keeps the repo small (node_modules can be hundreds of megabytes).
{{< /callout >}}

---

## Part 1: Composition Patterns

### JSX as Named Props (Slots)

The `children` prop gives you one slot. Named props give you **multiple slots**:

```jsx
function Panel({ header, footer, children }) {
  return (
    <div className="panel">
      <div className="panel-header">{header}</div>
      <div className="panel-body">{children}</div>
      <div className="panel-footer">{footer}</div>
    </div>
  );
}

// Three separate content areas!
<Panel
  header={<h2>User Settings</h2>}
  footer={<button>Save Changes</button>}
>
  <p>Update your profile information below.</p>
  <form>...</form>
</Panel>
```

{{< callout type="info" >}}
**When to use named slots:** When your wrapper component has clearly distinct content areas (header/body/footer, sidebar/main). For simple wrappers with one content area, `children` alone is sufficient.
{{< /callout >}}

### Specialization

Create purpose-built versions of generic components:

```jsx
function Button({ variant = "primary", children, onClick }) {
  return (
    <button className={`btn btn-${variant}`} onClick={onClick}>
      {children}
    </button>
  );
}

// Specialized version — preset props
function DangerButton({ children, onClick }) {
  return <Button variant="danger" onClick={onClick}>{children}</Button>;
}
```

**The key insight:** DangerButton isn't a new component — it's the same Button with specific props preset. This is composition, not inheritance.

### Container vs Presentational

A common pattern for separating concerns:

```jsx
// Container — owns state and logic
function ProductCatalog() {
  const [search, setSearch] = useState("");
  const filtered = products.filter(p =>
    p.name.toLowerCase().includes(search.toLowerCase())
  );
  return <ProductList products={filtered} />;
}

// Presentational — pure display
function ProductList({ products }) {
  return (
    <div className="grid">
      {products.map(p => <ProductCard key={p.id} {...p} />)}
    </div>
  );
}
```

{{< callout type="info" >}}
**Container = logic/state. Presentational = display only.** This is a guideline, not a strict rule. In smaller apps, components often do both. The key idea: separating "what to show" from "how to show it" makes components more reusable.
{{< /callout >}}

---

## Part 2: Component Lifecycle

Every React component goes through three phases:

```
    Mount              Update              Unmount
 (first render)    (state/props change)   (removed)

  ┌──────────┐      ┌──────────┐       ┌──────────┐
  │  Render   │ ──→  │ Re-render │ ──→   │  Cleanup  │
  │  first    │      │  with new │       │           │
  │  time     │      │  data     │       │           │
  └──────────┘      └──────────┘       └──────────┘
       ↓                  ↓
   useEffect()        useEffect()
   runs here          runs here (if deps changed)
```

| Phase | What Happens | Example |
|-------|-------------|---------|
| **Mount** | Component appears on screen for the first time | Page loads, user navigates to this route |
| **Update** | Props or state change, component re-renders | User types in a search field, clicks a filter |
| **Unmount** | Component is removed from the screen | User navigates to a different page |

This matters because `useEffect` lets you hook into these phases — run code when a component mounts, when specific values update, or clean up when it unmounts.

### Seeing the Lifecycle in Action

Open your browser console and watch the logs as you interact with this component:

```jsx
import { useState, useEffect } from 'react';

function LifecycleDemo() {
  const [count, setCount] = useState(0);

  // Runs ONCE when the component mounts
  useEffect(() => {
    console.log("1. MOUNT: Component appeared on screen");

    return () => {
      console.log("3. UNMOUNT: Component is being removed");
    };
  }, []);

  // Runs every time count changes
  useEffect(() => {
    console.log(`2. UPDATE: count changed to ${count}`);
  }, [count]);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>+1</button>
    </div>
  );
}
```

**What you'll see in the console:**

```
// Page loads (component mounts):
1. MOUNT: Component appeared on screen
2. UPDATE: count changed to 0

// Click the +1 button:
2. UPDATE: count changed to 1

// Click again:
2. UPDATE: count changed to 2

// Navigate away (component unmounts):
3. UNMOUNT: Component is being removed
```

**To see unmounting**, wrap it with a toggle so you can add/remove the component:

```jsx
function App() {
  const [show, setShow] = useState(true);

  return (
    <div>
      <button onClick={() => setShow(!show)}>
        {show ? "Hide" : "Show"} LifecycleDemo
      </button>
      {show && <LifecycleDemo />}
    </div>
  );
}
```

Click "Hide" and watch the unmount log appear. Click "Show" and the mount log fires again. This is the component lifecycle — mount, update, unmount — and `useEffect` is how you hook into each phase.

---

## Part 2: useEffect Deep Dive

### Why useEffect Exists

Components should be **pure functions**: given the same props and state, they return the same JSX. But sometimes you need to reach outside the component:

- Updating `document.title`
- Logging to the console
- Saving data to `localStorage`
- Fetching data from an API (Week 11)
- Setting up event listeners

These are **side effects** — they don't belong in the render logic. `useEffect` is React's dedicated mechanism for running side effects.

### useState vs useEffect

These two hooks solve completely different problems:

| | **useState** | **useEffect** |
|--|-------------|--------------|
| **Purpose** | Remember data between renders | Run code after rendering |
| **Triggers re-render?** | Yes — calling the setter re-renders | No — effects run after render |
| **When it runs** | You call the setter (e.g., on click) | Automatically, after render |
| **Think of it as** | "Remember this" | "After rendering, do this" |

**State drives what you see. Effects synchronize with the outside world.**

### Mental Model: Effects Run AFTER Render

```
1. Component function runs
2. JSX is returned
3. React updates the DOM
4. Browser paints the screen
5. ──→ useEffect fires HERE ←──
```

On **mount**, the component renders for the first time, then useEffect fires. On **update**, the component re-renders with new state, then useEffect fires again — but only if its dependencies changed.

### Basic Syntax

```jsx
import { useState, useEffect } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `Count: ${count}`;
  }, [count]);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>+1</button>
    </div>
  );
}
```

{{< callout type="info" >}}
**How useEffect works:**
1. Your component renders
2. React updates the DOM
3. useEffect runs the function you provided
4. When a dependency changes, React runs the effect again
{{< /callout >}}

### The Dependency Array

The second argument to `useEffect` controls **when** the effect runs:

```jsx
// Runs ONCE — when the component first appears (mount)
useEffect(() => {
  console.log("Component mounted!");
}, []);

// Runs when `count` changes (mount + updates)
useEffect(() => {
  document.title = `Count: ${count}`;
}, [count]);

// Runs on EVERY render (rarely what you want)
useEffect(() => {
  console.log("Rendered!");
});
```

| Dependency Array | When It Runs | Lifecycle Phase | Use Case |
|-----------------|-------------|-----------------|----------|
| `[]` (empty) | Once, on mount | Mount only | Setup, initial data load |
| `[count]` | When `count` changes | Mount + specific updates | React to state changes |
| No array | Every render | Mount + all updates | Rarely useful |

{{< callout type="warning" >}}
**Always include the dependency array.** Omitting it means the effect runs on every render, which can cause performance issues or infinite loops. When in doubt, start with `[]` and add dependencies as needed.
{{< /callout >}}

### Practical Examples

**Debugging with console.log:**

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log(`Count changed to: ${count}`);
  }, [count]);

  return <button onClick={() => setCount(count + 1)}>Count: {count}</button>;
}
```

Drop a useEffect with console.log to watch when and how state changes. Open the browser console and you'll see every update.

**localStorage Preview (Week 11 taste):**

```jsx
function Counter() {
  const [count, setCount] = useState(() => {
    return Number(localStorage.getItem('count')) || 0;
  });

  useEffect(() => {
    localStorage.setItem('count', count);
  }, [count]);

  return <button onClick={() => setCount(count + 1)}>Count: {count}</button>;
}
```

Every time count changes, we save it to localStorage. When the component mounts, we read the saved value. Refresh the page — the count persists! We'll go deeper into this pattern next week.

**Multiple useEffects:**

```jsx
function App() {
  const [count, setCount] = useState(0);
  const [name, setName] = useState("React");

  // Effect 1: Update document title (depends on count and name)
  useEffect(() => {
    document.title = `${name} - Count: ${count}`;
  }, [count, name]);

  // Effect 2: Log on mount only
  useEffect(() => {
    console.log("App mounted!");
  }, []);

  // ...
}
```

Each `useEffect` handles one concern. A dependency array can have multiple values — the effect runs when ANY of them change.

### Cleanup Functions: Tidying Up After Effects

If your effect sets something up, the **cleanup function** tears it down. Return a function from inside useEffect:

#### The Pattern

```jsx
useEffect(() => {
  // Setup: runs when the effect fires
  const connection = createConnection();

  return () => {
    // Cleanup: runs before the next effect OR when unmounting
    connection.close();
  };
}, [dependency]);
```

#### When Cleanup Runs

```
Mount:    setup runs
Update:   cleanup of PREVIOUS effect → new setup runs
Unmount:  cleanup runs one final time
```

This means React always cleans up the old effect before running the new one. It's like washing dishes before cooking a new meal.

#### Example: Window Resize Listener

```jsx
import { useState, useEffect } from 'react';

function WindowWidth() {
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    // Setup: add the listener
    function handleResize() {
      setWidth(window.innerWidth);
    }
    window.addEventListener('resize', handleResize);

    // Cleanup: remove the listener
    return () => {
      window.removeEventListener('resize', handleResize);
    };
  }, []); // Empty deps = setup once on mount, cleanup on unmount

  return <p>Window width: {width}px</p>;
}
```

**What happens without cleanup?** Every time the component re-renders, you'd add another `resize` listener. After 10 re-renders, you'd have 10 listeners all firing at once. Cleanup prevents this leak.

#### Example: setTimeout with Cleanup

```jsx
function DelayedMessage({ message, delay }) {
  const [show, setShow] = useState(false);

  useEffect(() => {
    const timerId = setTimeout(() => {
      setShow(true);
    }, delay);

    return () => clearTimeout(timerId); // Cancel if component unmounts early
  }, [message, delay]);

  return show ? <p>{message}</p> : <p>Waiting...</p>;
}
```

If the component unmounts before the timer fires, the cleanup cancels it — no error, no state update on an unmounted component.

#### Quick Reference: Common Cleanup Patterns

| Setup | Cleanup |
|-------|---------|
| `addEventListener` | `removeEventListener` |
| `setTimeout` | `clearTimeout` |
| `setInterval` | `clearInterval` |
| Open connection | Close connection |

{{< callout type="info" >}}
**Week 11 preview:** We'll explore more cleanup patterns next week (API call cancellation, localStorage sync, keyboard listeners). For now, remember the rule: **if you set something up, return a function to tear it down.**
{{< /callout >}}

{{< callout type="info" >}}
**Coming in Week 13 — Fetching Data:**

```jsx
useEffect(() => {
  fetch('https://api.example.com/products')
    .then(res => res.json())
    .then(data => setProducts(data));
}, []); // Fetch once on mount
```

Same pattern — run a side effect after mount. Week 13 covers error handling, loading states, and cleanup for fetch requests.
{{< /callout >}}

### When NOT to Use useEffect

{{< tabs items="Derived Values,Event Responses" >}}

{{< tab >}}
```jsx
// ❌ WRONG — computing derived state in useEffect
const [search, setSearch] = useState("");
const [filtered, setFiltered] = useState(allProducts);

useEffect(() => {
  setFiltered(products.filter(p =>
    p.name.toLowerCase().includes(search.toLowerCase())
  ));
}, [search]);

// ✅ RIGHT — compute during render
const [search, setSearch] = useState("");
const filtered = products.filter(p =>
  p.name.toLowerCase().includes(search.toLowerCase())
);
```

**If you can calculate it from existing state, compute it during render.** Don't store the result in state and don't use useEffect to update it.
{{< /tab >}}

{{< tab >}}
```jsx
// ❌ WRONG — responding to events in useEffect
useEffect(() => {
  if (submitted) {
    alert("Form submitted!");
  }
}, [submitted]);

// ✅ RIGHT — handle in the event handler
function handleSubmit() {
  setSubmitted(true);
  alert("Form submitted!");
}
```

**If something happens in response to a user action, handle it in the event handler.** useEffect is for synchronizing with external systems, not for reacting to events.
{{< /tab >}}

{{< /tabs >}}

### Rules Reminder

Same rules as `useState`:

1. **Only call `useEffect` at the top level** — not inside `if` statements, loops, or nested functions
2. **Only call `useEffect` inside React components** (or custom hooks)

---

## Part 2: useRef

### What Is useRef?

`useRef` creates a mutable reference that persists across renders — but unlike `useState`, changing it does **NOT** cause a re-render.

```jsx
import { useRef } from 'react';

const myRef = useRef(initialValue);
// Access: myRef.current
// Update: myRef.current = newValue (no re-render!)
```

| | **useState** | **useRef** |
|--|-------------|-----------|
| **Triggers re-render?** | Yes | **No** |
| **When to use** | Data that affects what's displayed | DOM access, values that don't affect display |
| **Access** | `value` | `ref.current` |

### Use Case 1: DOM Access

The most common use of `useRef` — accessing DOM elements directly:

```jsx
import { useRef } from 'react';

function SearchBar() {
  const inputRef = useRef(null);

  function handleClick() {
    inputRef.current.focus();  // Direct DOM access!
  }

  return (
    <div>
      <input ref={inputRef} type="text" placeholder="Search..." />
      <button onClick={handleClick}>Focus Search</button>
    </div>
  );
}
```

Pass a ref to a JSX element's `ref` attribute, and `ref.current` gives you the actual DOM element. Common uses:

- **Focusing an input** (`inputRef.current.focus()`)
- **Scrolling to an element** (`elementRef.current.scrollIntoView()`)
- **Reading dimensions** (`elementRef.current.getBoundingClientRect()`)

### Use Case 2: Persisting Values

Store values that should persist across renders but **don't** need to trigger a re-render:

```jsx
function Timer() {
  const [seconds, setSeconds] = useState(0);
  const renderCount = useRef(0);

  useEffect(() => {
    renderCount.current += 1;
    console.log(`Render #${renderCount.current}`);
  });

  return <p>Seconds: {seconds}</p>;
}
```

Common uses for persistent refs:

- **Render counters** (debugging)
- **Timer IDs** (for `clearInterval`) — see example below
- **Previous values** (comparing old vs new)

**Storing Timer IDs:**

```jsx
import { useState, useRef } from 'react';

function Stopwatch() {
  const [seconds, setSeconds] = useState(0);
  const [running, setRunning] = useState(false);
  const intervalRef = useRef(null);  // Store the timer ID

  function start() {
    setRunning(true);
    intervalRef.current = setInterval(() => {
      setSeconds(prev => prev + 1);
    }, 1000);
  }

  function stop() {
    clearInterval(intervalRef.current);  // Use the stored ID to stop
    intervalRef.current = null;
    setRunning(false);
  }

  return (
    <div>
      <p>{seconds}s</p>
      <button onClick={running ? stop : start}>
        {running ? "Stop" : "Start"}
      </button>
    </div>
  );
}
```

**Why useRef and not useState for the interval ID?** We never display the ID — we just need to remember it so we can call `clearInterval` later. Changing a ref doesn't re-render, which is exactly what we want. If we used `useState`, every time we stored the ID, React would re-render unnecessarily.

### useEffect + useRef: Working Together

The most powerful pattern combines both hooks: **useEffect** handles setup/cleanup, **useRef** stores values that shouldn't trigger re-renders.

**Improved Stopwatch with Proper Cleanup:**

```jsx
import { useState, useEffect, useRef } from 'react';

function Stopwatch() {
  const [seconds, setSeconds] = useState(0);
  const [running, setRunning] = useState(false);
  const intervalRef = useRef(null);

  useEffect(() => {
    if (running) {
      // Setup: start the interval
      intervalRef.current = setInterval(() => {
        setSeconds(prev => prev + 1);
      }, 1000);
    }

    // Cleanup: clear the interval
    return () => {
      if (intervalRef.current) {
        clearInterval(intervalRef.current);
      }
    };
  }, [running]); // Re-runs when running changes

  return (
    <div>
      <p>{seconds}s</p>
      <button onClick={() => setRunning(r => !r)}>
        {running ? "Stop" : "Start"}
      </button>
    </div>
  );
}
```

**Breaking it down:**
- **useEffect** handles the lifecycle: when `running` becomes `true`, start the interval. When it becomes `false` (or the component unmounts), the cleanup clears it.
- **useRef** stores the interval ID — we never display it, and changing it shouldn't re-render.
- **Functional updater** (`prev => prev + 1`) avoids the stale closure problem — we don't need `seconds` in the dependency array.

{{< callout type="info" >}}
**This pattern — store an ID in a ref, set it up in useEffect, clear it in cleanup — is extremely common in React.** You'll use it for timers, animations, WebSocket connections, and more. Week 11 builds directly on this.
{{< /callout >}}

**Another Example: Auto-scrolling to Bottom**

```jsx
function ChatMessages({ messages }) {
  const bottomRef = useRef(null);

  useEffect(() => {
    // Scroll to the bottom whenever messages change
    bottomRef.current?.scrollIntoView({ behavior: 'smooth' });
  }, [messages]); // Runs when messages array changes

  return (
    <div className="chat">
      {messages.map((msg, i) => (
        <p key={i}>{msg}</p>
      ))}
      <div ref={bottomRef} /> {/* Invisible anchor at bottom */}
    </div>
  );
}
```

Here, useRef targets a DOM element and useEffect triggers the scroll whenever the `messages` prop changes. No cleanup needed — scrolling is a one-time action, not an ongoing subscription.

### Rules

Same hooks rules — call `useRef` at the top level, only in components (or custom hooks).

---

## Part 2: Thinking in React

This is the **official methodology** from the React documentation for designing React applications. Now that you know props, useState, useEffect, useRef, and callback props, you can apply all five steps.

{{% steps %}}

### Step 1: Break the UI into Components

Draw boxes around every piece that feels like an independent unit:

```
┌──────────────────────────────────────────┐
│ App                                       │
│ ┌──────────────────────────────────────┐ │
│ │ SearchBar                             │ │
│ └──────────────────────────────────────┘ │
│ ┌──────────────────────────────────────┐ │
│ │ FilterBar  [All] [Electronics] [...]  │ │
│ └──────────────────────────────────────┘ │
│ ┌──────────────────────────────────────┐ │
│ │ ProductList                           │ │
│ │ ┌──────────┐ ┌──────────┐ ┌────────┐ │ │
│ │ │ProductCard│ │ProductCard│ │Product │ │ │
│ │ │          │ │          │ │  Card  │ │ │
│ │ └──────────┘ └──────────┘ └────────┘ │ │
│ └──────────────────────────────────────┘ │
└──────────────────────────────────────────┘
```

Each component has **one responsibility**: SearchBar searches, FilterBar filters, ProductCard displays one product.

### Step 2: Build a Static Version

All components with hardcoded data — no interactivity:

```jsx
function ProductCard({ name, price, category }) {
  return (
    <div className="card">
      <h3>{name}</h3>
      <span>{category}</span>
      <p>${price}</p>
    </div>
  );
}
```

Focus on structure and props. Get the UI on screen. Adding behavior is easier once the structure is solid.

### Step 3: Find the Minimal State

Ask: "What data changes over time?"

| Data | Changes? | State? |
|------|----------|--------|
| Product list | No | No — constant |
| Categories | No | No — constant |
| Search text | Yes (user types) | **Yes** |
| Selected category | Yes (user clicks) | **Yes** |
| Filtered products | Yes, but... | **No** — computed from search + category |

Don't put computed values in state. Store the inputs (`searchTerm`, `activeCategory`), compute the output (`filteredProducts`).

### Step 4: Determine Where State Lives

State lives in the **nearest common ancestor** of all components that need it:

```
App ← state lives here (searchTerm, activeCategory)
├── SearchBar ← needs searchTerm, triggers onSearch
├── FilterBar ← needs activeCategory, triggers onFilterChange
└── ProductGrid ← needs filteredProducts (computed from state)
```

Since all three components need data from the same state, that state lives in `App`.

### Step 5: Add Interactivity

Wire callback props to state setters:

```jsx
function App() {
  const [searchTerm, setSearchTerm] = useState("");
  const [activeCategory, setActiveCategory] = useState("All");

  const filteredProducts = products
    .filter(p => activeCategory === "All" || p.category === activeCategory)
    .filter(p => p.name.toLowerCase().includes(searchTerm.toLowerCase()));

  return (
    <div>
      <SearchBar searchTerm={searchTerm} onSearch={setSearchTerm} />
      <FilterBar
        categories={categories}
        activeCategory={activeCategory}
        onFilterChange={setActiveCategory}
      />
      <div className="product-grid">
        {filteredProducts.map(p => (
          <ProductCard key={p.id} {...p} />
        ))}
      </div>
    </div>
  );
}
```

State in App → data down via props → actions up via callbacks → state updates → React re-renders.

{{% /steps %}}

---

## React Developer Tools

**React Developer Tools** is a Chrome extension that lets you inspect React components in the browser.

### How to Install

1. Search "React Developer Tools" in the Chrome Web Store
2. Click "Add to Chrome"
3. Open any React app → open Chrome DevTools → look for the **Components** tab

### What You Can Do

- **See the component tree** — every component and how they're nested
- **Inspect props and state** — click any component to see its current values
- **Edit state live** — change a value in DevTools and watch the UI update
- **Debug data flow** — trace where props come from and how state flows down

{{< callout type="info" >}}
**Install React DevTools before next week.** You'll use it constantly for debugging GP2, Assignment 4, and the Final Project.
{{< /callout >}}

---

## Connecting It All Together

Here's how everything from Week 10 works together:

| Concept | Role in an App |
|---------|---------------|
| **Props** | Pass data from parent to child |
| **Destructuring & defaults** | Clean component signatures |
| **Children prop** | Wrapper/layout components |
| **useState** | Data the component owns and can change |
| **Callback props** | Child-to-parent communication |
| **useEffect** | Side effects (document title, logging, later: API calls) |
| **useRef** | DOM access and persistent values |
| **Composition** | Building complex UIs from simple pieces |
| **Container/Presentational** | Separating logic from display |
| **Thinking in React** | Methodology for designing apps |
| **React DevTools** | Debugging components, props, and state |

---

## Real-World Example: All Three Hooks Together

The examples above teach each hook in isolation. In real apps, you'll use `useState`, `useEffect`, and `useRef` together in the same component. Here's a practical example that shows **why** each hook exists and **when** you'd reach for it.

### Search Bar with Auto-Focus and Debounced Document Title

Imagine a search bar that:
1. **Auto-focuses** the input when the page loads (useRef)
2. **Tracks** what the user types (useState)
3. **Updates the page title** after the user stops typing for 500ms (useEffect + useRef)

```jsx
import { useState, useEffect, useRef } from 'react';

function SearchPage() {
  // useState: track the search text and filtered results
  const [query, setQuery] = useState("");
  const [results, setResults] = useState([]);

  // useRef: access the input DOM element for focusing
  const inputRef = useRef(null);

  // useRef: store the debounce timer ID (no re-render needed)
  const timerRef = useRef(null);

  // Data to search through
  const allItems = [
    "React Hooks Guide",
    "CSS Grid Layout",
    "JavaScript Promises",
    "HTML Semantic Elements",
    "Responsive Design Patterns",
    "Git Branch Strategies",
  ];

  // useEffect: auto-focus the search input on mount
  useEffect(() => {
    inputRef.current.focus();
  }, []);

  // useEffect: update document title after user stops typing (debounce)
  useEffect(() => {
    // Clear any existing timer
    clearTimeout(timerRef.current);

    // Set a new timer — only updates title after 500ms of no typing
    timerRef.current = setTimeout(() => {
      if (query) {
        document.title = `Search: ${query}`;
      } else {
        document.title = "Search Page";
      }
    }, 500);

    // Cleanup: cancel the timer if query changes again before 500ms
    return () => clearTimeout(timerRef.current);
  }, [query]);

  // Derived state: filter results during render (NOT in useEffect)
  const filtered = query
    ? allItems.filter(item =>
        item.toLowerCase().includes(query.toLowerCase())
      )
    : allItems;

  return (
    <div>
      <h1>Search</h1>
      <input
        ref={inputRef}
        type="text"
        placeholder="Search articles..."
        value={query}
        onChange={(e) => setQuery(e.target.value)}
      />
      <p>{filtered.length} result{filtered.length !== 1 ? "s" : ""}</p>
      <ul>
        {filtered.map((item, i) => (
          <li key={i}>{item}</li>
        ))}
      </ul>
    </div>
  );
}

export default SearchPage;
```

### Why Each Hook Is Used

| Hook | What It Does Here | Why This Hook? |
|------|-------------------|----------------|
| `useState(query)` | Stores search text, triggers re-render on every keystroke | The UI depends on this value — it needs to re-render |
| `useState(results)` | Could store fetched results (simplified here) | Display data that changes over time |
| `useRef(inputRef)` | Points to the `<input>` DOM element | Need direct DOM access for `.focus()` — no re-render needed |
| `useRef(timerRef)` | Stores the `setTimeout` ID between renders | Need to remember the timer to cancel it — no re-render needed |
| `useEffect([], mount)` | Focuses the input once on mount | Side effect (DOM manipulation) that runs after render |
| `useEffect([query])` | Debounces the document title update | Side effect (document.title) that syncs with external world |
| Derived `filtered` | Filters the list during render | **NOT** a side effect — computed from existing state, no useEffect needed |

### What "Debounce" Means

Debouncing waits until the user **stops** doing something before acting. Without debounce, the title would update on every single keystroke. With debounce:

```
User types: "R" → timer starts (500ms)
User types: "Re" → old timer canceled, new timer starts
User types: "Rea" → old timer canceled, new timer starts
User types: "Reac" → old timer canceled, new timer starts
User types: "React" → old timer canceled, new timer starts
... 500ms passes with no typing ...
→ Title updates to "Search: React"
```

This is the same pattern you'll use in Week 13 to avoid making an API call on every keystroke.

{{< callout type="info" >}}
**Notice what is NOT in a useEffect:** The `filtered` list is computed during render, not inside useEffect. Remember the rule: if you can calculate it from existing state, compute it during render.
{{< /callout >}}

---

## Troubleshooting & Common Mistakes

{{% details title="useEffect runs too many times" %}}
Check your dependency array:

```jsx
// ❌ Missing dependency array — runs every render
useEffect(() => {
  document.title = `Count: ${count}`;
});

// ✅ With dependency — runs only when count changes
useEffect(() => {
  document.title = `Count: ${count}`;
}, [count]);
```
{{% /details %}}

{{% details title="useEffect infinite loop" %}}
This happens when you set state inside useEffect with that state in the dependency array:

```jsx
// ❌ Infinite loop!
useEffect(() => {
  setCount(count + 1);  // Updates count → triggers effect → updates count → ...
}, [count]);

// ✅ If you need to increment, use functional update
useEffect(() => {
  // Only if you truly need this pattern (rare)
  setCount(prev => prev + 1);
}, []); // Run once on mount
```
{{% /details %}}

{{% details title="useEffect uses old/stale values" %}}
This happens when you use a variable inside useEffect but forget to include it in the dependency array:

```jsx
function Timer() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const id = setInterval(() => {
      // ❌ This always logs 0!
      console.log(`Count is: ${count}`);
    }, 1000);
    return () => clearInterval(id);
  }, []); // Problem: count is used but not in the dependency array

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>+1</button>
    </div>
  );
}
```

**Why this happens:** The effect ran once on mount and captured `count` as `0`. Because `count` isn't in the dependency array, the effect never re-runs — it's stuck with the old value forever. This is called a **stale closure**.

**Fix Option 1:** Add the variable to the dependency array:

```jsx
useEffect(() => {
  const id = setInterval(() => {
    console.log(`Count is: ${count}`); // ✅ Now always current
  }, 1000);
  return () => clearInterval(id);
}, [count]); // Effect re-runs when count changes
```

**Fix Option 2:** Use a functional updater (when you only need to update state):

```jsx
useEffect(() => {
  const id = setInterval(() => {
    setCount(prev => prev + 1); // ✅ Always has the latest value
  }, 1000);
  return () => clearInterval(id);
}, []); // Safe: functional updater doesn't need count in deps
```

**Rule of thumb:** If your effect reads a variable, that variable belongs in the dependency array. React's linter will warn you — pay attention to those warnings.
{{% /details %}}

{{% details title="useRef.current is null" %}}
The ref isn't attached yet. Make sure:

```jsx
// ❌ Trying to access ref before render
const inputRef = useRef(null);
inputRef.current.focus(); // ❌ null on first render!

// ✅ Access ref in an event handler or useEffect
const inputRef = useRef(null);

useEffect(() => {
  inputRef.current.focus(); // ✅ Runs after render
}, []);

// Or in an event handler:
function handleClick() {
  inputRef.current.focus(); // ✅ Runs after user action
}
```
{{% /details %}}

{{% details title="My specialized component doesn't pass all props" %}}
Forward the props the base component needs:

```jsx
// ❌ Forgot to pass onClick
function DangerButton({ children }) {
  return <Button variant="danger">{children}</Button>;
}

// ✅ Pass all relevant props
function DangerButton({ children, onClick }) {
  return <Button variant="danger" onClick={onClick}>{children}</Button>;
}
```
{{% /details %}}

{{% details title="Git push rejected after Vite setup" %}}
If you accidentally created a GitHub repo with a README:

```bash
# Option 1: Start fresh — delete the GitHub repo and create an empty one

# Option 2: Force push (only if the repo is brand new with just a README)
git push -u origin main --force
```

The safest option is to delete and recreate the GitHub repo as empty.
{{% /details %}}

---

## Key Takeaways

{{< callout emoji="🎯" >}}
**Session 2 essentials:**

1. **Vite-first Git workflow** — create Vite project → git init → push to empty GitHub repo
2. **Named slots** = pass JSX as any prop for multiple content areas
3. **Specialization** = preset props via composition, not inheritance
4. **Container/Presentational** = separate logic from display
5. **Component lifecycle** = mount → update → unmount
6. **useEffect** = side effects AFTER render, controlled by dependency array
7. **Cleanup functions** = return a function from useEffect to tear down listeners/timers (prevents memory leaks)
8. **useRef** = DOM access + persistent values without re-rendering
9. **useEffect + useRef** = store IDs in refs, set up in effects, clean up in returns
10. **Thinking in React** = 5 steps: components → static → state → location → interactivity
11. **Don't put computed values in state** — store inputs, compute outputs
12. **React DevTools** = inspect components, props, and state live
{{< /callout >}}

---

## Additional Resources

{{< cards >}}

{{< card
  title="React: Thinking in React"
  link="https://react.dev/learn/thinking-in-react"
  subtitle="The official 5-step methodology with interactive examples"
>}}

{{< card
  title="React: Synchronizing with Effects"
  link="https://react.dev/learn/synchronizing-with-effects"
  subtitle="Official guide to useEffect"
>}}

{{< card
  title="React: Referencing Values with Refs"
  link="https://react.dev/learn/referencing-values-with-refs"
  subtitle="Official guide to useRef"
>}}

{{< card
  title="React: You Might Not Need an Effect"
  link="https://react.dev/learn/you-might-not-need-an-effect"
  subtitle="Official guide on when NOT to use useEffect"
>}}

{{< /cards >}}

---

*Next week (Week 11): Advanced useEffect patterns (API calls, localStorage, cleanup), lifting state up, controlled forms, and Assignment 4.*
