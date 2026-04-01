---
title: "React Hooks & Patterns Lab Guide"
weight: 1
toc: true
---

{{< callout type="info" >}}
**How to use this guide:** Follow every step in order. Copy the code snippets exactly. After each section, run `npm run dev` and open your browser console (F12 → Console) to see what's happening. Then try the "Try This" challenges to deepen your understanding.

**Prerequisites:** Node.js installed (v18+), a GitHub account, a code editor (VS Code recommended).
{{< /callout >}}

## Part 0: Project Setup

### What is React?

React is a JavaScript library for building user interfaces. Instead of manually finding DOM elements and changing them (the way you'd do with vanilla JS or jQuery), you **describe what the UI should look like** for any given state, and React figures out what to change in the DOM for you.

Think of it this way:
- **Vanilla JS (imperative):** "Find the paragraph element. Change its text to 5. Change its color to red."
- **React (declarative):** "The paragraph shows `count`. When `count` is above 5, the color is red." You change `count`, and React updates everything automatically.

### What is Vite?

Vite is a build tool that gives you a fast development server with hot module replacement (when you save a file, the browser updates instantly). It handles JSX compilation, module bundling, and more.

{{% steps %}}

### Step 1: Create a GitHub Repository

1. Go to [github.com](https://github.com) and click **New Repository**
2. Name it `cs300-react-demos` (or any name you like)
3. **Check "Add a README file"** — this initializes the repo
4. Click **Create repository**
5. Copy the HTTPS clone URL

### Step 2: Clone and Scaffold with Vite

Open your terminal and navigate to where you want the project:

```bash
# Clone your repo
git clone https://github.com/YOUR-USERNAME/cs300-react-demos.git

# Go to the PARENT folder (not inside the repo)
cd ..

# Scaffold Vite inside your existing repo folder
# The latest Vite allows you to use an existing folder name
npm create vite@latest cs300-react-demos -- --template react

# When prompted:
#   - Select framework: React
#   - Select variant: JavaScript
#   - If asked about existing files: allow overwrite

# Now enter the project
cd cs300-react-demos

# Install dependencies
npm install

# Start the dev server to verify it works
npm run dev
```

You should see a URL like `http://localhost:5173`. Open it — you'll see the default Vite + React page.

### Step 3: Clean Up the Default Files

Stop the dev server (Ctrl+C), then clean up:

```bash
# Remove files we don't need
rm src/App.css
rm -rf src/assets
```

### Step 4: Replace `src/main.jsx`

This is the entry point. It mounts your App component into the HTML page.

**File: `src/main.jsx`**
```jsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import './index.css'
import App from './App.jsx'

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <App />
  </StrictMode>,
)
```

{{< callout type="info" >}}
**What is StrictMode?** It's a development-only wrapper that helps find bugs. It runs your effects twice on mount to make sure cleanup works. You'll see double console logs in development — this is normal and expected. It does NOT affect production.
{{< /callout >}}

### Step 5: Replace `src/App.jsx` (Starter Version)

For now, start with a minimal App. We'll add tabs later.

**File: `src/App.jsx`**
```jsx
function App() {
  return (
    <div>
      <h1>CS300 React Demos</h1>
      <p>We'll add demos here step by step.</p>
    </div>
  );
}

export default App;
```

### Step 6: Replace `src/index.css`

Replace the entire contents of `src/index.css` with this CSS. It provides all the styling we'll need throughout the project.

**File: `src/index.css`**
```css
:root {
  --primary-color: #3498db;
  --secondary-color: #2ecc71;
  --font-family: "Arial, sans-serif";
  --background-color: #f5f5f5;
  --text-color: #333;
}

body {
  margin: 0;
  padding: 50px;
  font-family: var(--font-family);
  background-color: var(--background-color);
  color: var(--text-color);
}

h1 {
  color: var(--primary-color);
}

.btn {
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}

.btn-primary {
  background-color: var(--primary-color);
  color: #fff;
}

.btn-secondary {
  background-color: var(--secondary-color);
  color: #fff;
}

.btn-danger {
  background-color: #e74c3c;
  color: #fff;
}

/* ===== Demo Navigation ===== */
.demo-nav {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  padding: 16px 0;
  margin-bottom: 24px;
  border-bottom: 2px solid #ddd;
}

.demo-nav .btn {
  font-size: 14px;
  padding: 8px 16px;
}

.demo-nav .btn-active {
  background-color: var(--primary-color);
  color: #fff;
}

/* ===== Demo Sections ===== */
.demo-section {
  padding: 16px 0;
  max-width: 800px;
}

.demo-section h2 {
  color: var(--primary-color);
  margin-bottom: 8px;
}

.demo-subsection {
  border-left: 4px solid var(--primary-color);
  padding-left: 16px;
  margin-bottom: 32px;
}

.demo-subsection h3 {
  color: var(--text-color);
  margin-top: 0;
}

.demo-note {
  font-style: italic;
  color: #888;
  font-size: 14px;
  margin: 8px 0;
}

.demo-practical {
  background-color: #eef6ff;
  border: 1px solid var(--primary-color);
  border-radius: 8px;
  padding: 16px 20px;
  margin-top: 32px;
}

.demo-practical h3 {
  margin-top: 0;
  color: var(--primary-color);
}

.demo-practical ul {
  margin: 0;
  padding-left: 20px;
}

.demo-practical li {
  margin-bottom: 4px;
}

/* ===== Traffic Light ===== */
.traffic-light {
  display: flex;
  flex-direction: column;
  gap: 8px;
  width: 60px;
  background: #333;
  padding: 12px;
  border-radius: 8px;
  align-items: center;
  margin: 12px 0;
}

.light-circle {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  transition: opacity 0.3s;
}

/* ===== Side by Side ===== */
.side-by-side {
  display: flex;
  gap: 24px;
  flex-wrap: wrap;
}

.side-by-side > div {
  flex: 1;
  min-width: 300px;
}

/* ===== Shell / Composition ===== */
.shell {
  border: 2px solid var(--shell-bg, #ccc);
  border-radius: 8px;
  margin: 12px 0;
  overflow: hidden;
}

.shell-title {
  background-color: var(--shell-bg, #ccc);
  color: var(--shell-text, #fff);
  padding: 8px 16px;
  font-weight: bold;
  font-size: 14px;
}

.shell-content {
  padding: 16px;
  background-color: #fff;
}

.card {
  background: #fff;
  border: 1px solid #ddd;
  border-radius: 6px;
  padding: 16px;
  margin: 8px 0;
  box-shadow: 0 1px 3px rgba(0,0,0,0.08);
}

/* ===== Form Demo ===== */
.form-group {
  margin-bottom: 16px;
}

.form-group label {
  display: block;
  margin-bottom: 4px;
  font-weight: bold;
}

.form-group input[type="text"],
.form-group input[type="email"],
.form-group select {
  padding: 8px 12px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 16px;
  width: 100%;
  max-width: 300px;
  box-sizing: border-box;
}

.form-error {
  color: #e74c3c;
  font-size: 14px;
  margin-top: 4px;
}

/* ===== Section Stepper ===== */
.stepper-nav {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 4px;
}

.stepper-tabs {
  display: flex;
  gap: 4px;
  flex-wrap: wrap;
  flex: 1;
}

.stepper-tab {
  padding: 6px 12px;
  border: 1px solid #ddd;
  border-radius: 4px;
  background: #fff;
  cursor: pointer;
  font-size: 13px;
  color: var(--text-color);
  transition: all 0.2s;
}

.stepper-tab:hover {
  border-color: var(--primary-color);
}

.stepper-tab-active {
  background-color: var(--primary-color);
  color: #fff;
  border-color: var(--primary-color);
  font-weight: bold;
}

.stepper-tab-done {
  background-color: #e8f4e8;
  border-color: var(--secondary-color);
  color: var(--secondary-color);
}

.stepper-progress {
  height: 4px;
  background: #eee;
  border-radius: 2px;
  margin-bottom: 20px;
  overflow: hidden;
}

.stepper-progress-bar {
  height: 100%;
  background: var(--primary-color);
  border-radius: 2px;
  transition: width 0.3s;
}

.stepper-content {
  min-height: 200px;
}
```

### Step 7: Verify and Commit

```bash
npm run dev
# Open http://localhost:5173 — you should see "CS300 React Demos"
# Stop the server (Ctrl+C)
```

{{< callout type="important" >}}
**Checkpoint: Commit your work**
{{< /callout >}}

```bash
git add -A
git commit -m "Initial Vite React scaffold with project CSS"
git push
```

{{% /steps %}}

---

## Part 1: React vs Vanilla JS — Why React?

### The Core Idea

In vanilla JavaScript, you write **imperative** code: step-by-step instructions to manipulate the DOM.

```js
// Vanilla JS: "change this, then change that, then change the other thing"
document.getElementById("count").textContent = count;
document.getElementById("count").style.color = count > 5 ? "red" : "black";
```

In React, you write **declarative** code: you describe what the UI should look like for any given state, and React handles the DOM updates.

```jsx
// React: "here's what the UI looks like when count is this value"
<span style={{ color: count > 5 ? "red" : "black" }}>{count}</span>
```

Why does this matter? With 5 elements on a page, imperative is fine. With 500 elements, forms, lists, modals, and real-time data — imperative code becomes a tangled mess. React lets you think about **what** the UI should be, not **how** to get there.

### What is State?

**State** is data that changes over time and affects what the user sees. Examples:
- A counter value (0, 1, 2, 3...)
- Whether a sidebar is open or closed (true/false)
- The text in a search box ("hel", "hell", "hello")
- Which tab is active ("home", "settings", "profile")

When state changes, React re-renders the component (re-runs the function) to produce new UI that matches the new state. You never manually update the DOM — you update state, and React updates the DOM for you.

### Create the SectionStepper Component

This is a reusable navigation component that every demo will use. It shows one section at a time with Previous/Next buttons.

**File: `src/SectionStepper.jsx`** (create this new file)
```jsx
import { useState } from "react";

// ============================================================
// SectionStepper — Show one section at a time
// ============================================================
// Each demo page uses this to step through sections A, B, C, D.
// Only the active section is MOUNTED — previous sections are unmounted.
// This keeps the console clean and lets students focus on one concept.
//
// Usage:
//   <SectionStepper sections={[
//     { label: "A. The Broken Counter", content: <BrokenCounter /> },
//     { label: "B. The Working Counter", content: <WorkingCounter /> },
//   ]} />

export default function SectionStepper({ sections }) {
  const [step, setStep] = useState(0);

  const current = sections[step];

  return (
    <div>
      {/* Step indicator and navigation */}
      <div className="stepper-nav">
        <button
          className="btn btn-secondary"
          onClick={() => setStep(prev => prev - 1)}
          disabled={step === 0}
        >
          ← Previous
        </button>

        <div className="stepper-tabs">
          {sections.map((section, i) => (
            <button
              key={i}
              className={`stepper-tab ${i === step ? "stepper-tab-active" : ""} ${i < step ? "stepper-tab-done" : ""}`}
              onClick={() => {
                console.log(`SECTION: switching to "${section.label}" (previous section will unmount)`);
                setStep(i);
              }}
            >
              {section.label}
            </button>
          ))}
        </div>

        <button
          className="btn btn-primary"
          onClick={() => setStep(prev => prev + 1)}
          disabled={step === sections.length - 1}
        >
          Next →
        </button>
      </div>

      {/* Progress bar */}
      <div className="stepper-progress">
        <div
          className="stepper-progress-bar"
          style={{ width: `${((step + 1) / sections.length) * 100}%` }}
        />
      </div>

      {/* Only the active section is mounted.
          When you switch, the old section unmounts (cleanup runs)
          and the new section mounts (effects run fresh).
          Watch the console! */}
      <div className="stepper-content">
        {current.content}
      </div>
    </div>
  );
}
```

### Create the React vs JS Demo Files

Create the folder `src/reactVsJs/` and add these files:

**File: `src/reactVsJs/ImperativeVsDeclarative.jsx`**
```jsx
import { useState, useRef } from "react";

// ============================================================
// Sections A & B: Imperative vs Declarative (Side by Side)
// ============================================================
// These two components do the SAME thing — increment a counter
// and change its color when it goes above 5.
// The difference is HOW they do it:
//   A (Imperative): manually tells the DOM what to change
//   B (Declarative): describes what the UI should look like
// Open the browser console to follow along!

// --- Section A: The Imperative (Vanilla JS) Way ---
function ImperativeWay() {
  // Using refs to get raw DOM elements — this is how jQuery/vanilla JS works
  const countRef = useRef(null);
  let count = 0;

  function handleClick() {
    count += 1;
    // We manually tell the DOM EXACTLY what to change:
    countRef.current.textContent = count;
    countRef.current.style.color = count > 5 ? "red" : "black";
    countRef.current.style.fontWeight = "bold";
    console.log("IMPERATIVE: I manually set textContent to", count, "and color to", count > 5 ? "red" : "black");
    // This is imperative: step 1, step 2, step 3...
    // YOU are responsible for every single DOM change.
  }

  return (
    <div className="demo-subsection">
      <h3>A. The Imperative Way (Vanilla JS Thinking)</h3>
      <p className="demo-note">
        This works, but YOU have to manually update every part of the DOM.
        Imagine doing this for 100 elements on a page!
      </p>
      <p>Count: <span ref={countRef}>0</span></p>
      <button className="btn btn-secondary" onClick={handleClick}>
        Increment (imperative)
      </button>
      {/* Problems with this approach:
          - You must track every DOM element manually
          - Easy to forget to update something
          - Hard to keep UI and data in sync
          - Gets unmanageable with complex UIs */}
    </div>
  );
}

// --- Section B: The Declarative (React) Way ---
function DeclarativeWay() {
  const [count, setCount] = useState(0);

  // React calls this entire function on every state change.
  // We just DESCRIBE what the UI should look like:
  console.log("DECLARATIVE: describing UI for count =", count);

  return (
    <div className="demo-subsection">
      <h3>B. The Declarative Way (React Thinking)</h3>
      <p className="demo-note">
        Same result, but we never touch the DOM directly.
        We describe WHAT the UI should be. React figures out HOW to update it.
      </p>
      {/* Notice: we don't say "change the color to red."
          We say: "the color IS red when count > 5."
          React handles the actual DOM changes. */}
      <p>
        Count:{" "}
        <span style={{ color: count > 5 ? "red" : "black", fontWeight: "bold" }}>
          {count}
        </span>
      </p>
      <button className="btn btn-primary" onClick={() => setCount(prev => prev + 1)}>
        Increment (declarative)
      </button>
    </div>
  );
}

// Combined: shown side by side
export default function ImperativeVsDeclarative() {
  return (
    <div className="side-by-side">
      <ImperativeWay />
      <DeclarativeWay />
    </div>
  );
}
```

**File: `src/reactVsJs/TrafficLight.jsx`**
```jsx
import { useState } from "react";

// ============================================================
// Section C: State Drives UI — Traffic Light
// ============================================================
// The core React idea: you change STATE, and React updates the UI.
// You never say "turn off the red light, turn on the green light."
// You say "the light is green" and React figures out the DOM changes.
// Open the browser console to follow along!

export default function TrafficLight() {
  const [light, setLight] = useState("red");

  console.log("TRAFFIC LIGHT: state is", JSON.stringify(light), "→ React re-renders → UI updates automatically");

  // We never say "turn off the red, turn on the green."
  // We say "the light is green" and React updates everything.
  return (
    <div className="demo-subsection">
      <h3>C. State Drives UI — Traffic Light</h3>
      <p className="demo-note">
        Click a button. The state changes to a color name.
        The JSX says "if state is red, show red at full opacity."
        React handles the rest.
      </p>

      <div className="traffic-light">
        <div
          className="light-circle"
          style={{
            backgroundColor: "#e74c3c",
            opacity: light === "red" ? 1 : 0.2,
          }}
        />
        <div
          className="light-circle"
          style={{
            backgroundColor: "#f1c40f",
            opacity: light === "yellow" ? 1 : 0.2,
          }}
        />
        <div
          className="light-circle"
          style={{
            backgroundColor: "#2ecc71",
            opacity: light === "green" ? 1 : 0.2,
          }}
        />
      </div>

      <div style={{ display: "flex", gap: 8 }}>
        <button className="btn btn-danger" onClick={() => setLight("red")}>Red</button>
        <button className="btn btn-secondary" onClick={() => setLight("yellow")} style={{ backgroundColor: "#f1c40f", color: "#333" }}>Yellow</button>
        <button className="btn btn-primary" onClick={() => setLight("green")} style={{ backgroundColor: "#2ecc71" }}>Green</button>
      </div>

      <p>Current state: <strong>{light}</strong></p>
      <p className="demo-note">
        The JSX is like a template: "given this state, here's what the UI looks like."
        You change state → React re-renders → UI updates.
        You never manually toggle DOM elements on/off.
      </p>
    </div>
  );
}
```

**File: `src/reactVsJs/ReRenderingDemo.jsx`**
```jsx
import { useState } from "react";

// ============================================================
// Section D: Re-Rendering is Okay!
// ============================================================
// Every time state changes, React re-calls the entire component function.
// This sounds expensive, but it's actually fast!
// React compares old and new output and only touches what changed in the real DOM.
// Open the browser console to follow along!

export default function ReRenderingDemo() {
  const [count, setCount] = useState(0);
  const [text, setText] = useState("");

  // This runs every time the component renders
  console.log("RE-RENDER: This entire function just ran. count =", count, ", text =", JSON.stringify(text));

  return (
    <div className="demo-subsection">
      <h3>D. Re-Rendering is Okay!</h3>
      <p className="demo-note">
        Every click or keystroke re-runs this entire function.
        That sounds expensive, but it's actually fast!
        React compares old and new output and only touches what changed in the real DOM.
      </p>
      {console.log("RE-RENDER: computing JSX (this is cheap!)")}

      <div style={{ marginBottom: 8 }}>
        <button className="btn btn-primary" onClick={() => setCount(prev => prev + 1)} style={{ marginRight: 8 }}>
          Count: {count}
        </button>
        <input
          type="text"
          value={text}
          onChange={(e) => setText(e.target.value)}
          placeholder="Type to trigger re-renders..."
          style={{ padding: "4px 8px" }}
        />
      </div>

      <p className="demo-note" style={{ marginTop: 12 }}>
        <strong>How React works under the hood:</strong><br />
        1. State changes (setCount/setText called)<br />
        2. React re-calls this function → gets new JSX<br />
        3. React compares new JSX with old JSX (this is called "diffing")<br />
        4. React only updates the DOM elements that actually changed<br />
        5. Browser re-paints only the changed pixels<br />
        <br />
        This is why React is fast even though the whole function re-runs!
      </p>
    </div>
  );
}
```

**File: `src/reactVsJs/index.jsx`**
```jsx
import SectionStepper from "../SectionStepper";
import ImperativeVsDeclarative from "./ImperativeVsDeclarative";
import TrafficLight from "./TrafficLight";
import ReRenderingDemo from "./ReRenderingDemo";

// ============================================================
// React vs JavaScript — Understanding the React Mental Model
// ============================================================
// This file shows the fundamental difference between how
// vanilla JavaScript and React approach building UIs.
// Each section mounts one at a time so the console stays clean.

const sections = [
  { label: "A+B. Imperative vs Declarative", content: <ImperativeVsDeclarative /> },
  { label: "C. Traffic Light", content: <TrafficLight /> },
  { label: "D. Re-Rendering", content: <ReRenderingDemo /> },
];

export default function ReactVsJs() {
  return (
    <div className="demo-section">
      <h2>React vs JavaScript — A New Way to Think About UI</h2>
      <p className="demo-note">
        Open your browser console (F12 → Console) to see what happens behind the scenes.
        Use the section buttons below to step through each concept one at a time.
      </p>
      <SectionStepper sections={sections} />
    </div>
  );
}
```

### Update App.jsx to Show the First Demo

**File: `src/App.jsx`** (replace the contents)
```jsx
import { useState } from "react";
import ReactVsJs from "./reactVsJs";

const TABS = [
  { id: "reactVsJs", label: "React vs JS" },
];

function App() {
  const [activeDemo, setActiveDemo] = useState("reactVsJs");

  return (
    <div>
      <h1>CS300 React Demos</h1>
      <nav className="demo-nav">
        {TABS.map((tab) => (
          <button
            key={tab.id}
            className={`btn ${activeDemo === tab.id ? "btn-active" : "btn-secondary"}`}
            onClick={() => {
              console.log("NAV: switching to", tab.id, "(previous component will unmount)");
              setActiveDemo(tab.id);
            }}
          >
            {tab.label}
          </button>
        ))}
      </nav>

      {activeDemo === "reactVsJs" && <ReactVsJs />}
    </div>
  );
}

export default App;
```

### Verify and Commit

```bash
npm run dev
# Open http://localhost:5173
# Open the browser console (F12 → Console)
# Click through the sections A+B, C, D
# Watch the console logs!
```


{{< callout type="important" >}}
**Checkpoint: Commit your work**
{{< /callout >}}

```bash
git add -A
git commit -m "Add React vs JS demo — imperative vs declarative"
git push
```

{{% details title="Try This (Experiments)" %}}


1. **In the Traffic Light:** Add a fourth light (e.g., a flashing blue for "walk"). You'll need to add another `<div className="light-circle">` and another button.
2. **In the Imperative counter:** Try to make it change the background color too. Notice how many manual DOM operations you need.
3. **In the Declarative counter:** Add a background color change. Notice how you just add one more style property — React handles the rest.
4. **In the Re-Rendering demo:** Count how many console logs appear per keystroke. This proves the entire function re-runs each time.


{{% /details %}}

---

## Part 2: useState — Making React Aware of Changes

### Why Can't We Use Regular Variables?

{{< callout type="warning" >}}
When React calls your component function, **local variables are re-created from scratch every time**. A `let count = 0` resets to 0 on every render. Even if you increment it inside a click handler, React doesn't know the variable changed — so it never re-renders.
{{< /callout >}}

`useState` solves this:
1. React **remembers** the value between renders
2. When you call the setter function, React **re-renders** the component with the new value

```jsx
const [count, setCount] = useState(0);
// count = the current value
// setCount = function to update it and trigger a re-render
// 0 = the initial value (only used on first render)
```

### Create the useState Demo Files

Create the folder `src/useStateDemo/` and add these files:

**File: `src/useStateDemo/BrokenCounter.jsx`**
```jsx
// ============================================================
// Section A: The Broken Counter
// ============================================================
// This component uses a plain variable instead of useState.
// Watch the console vs the screen — the variable changes, but the UI does not!
// This is the "aha" moment: React only re-renders when STATE changes.
// Open the browser console to follow along!

export default function BrokenCounter() {
  let count = 0; // This resets to 0 every time React calls this function!

  function handleClick() {
    count = count + 1;
    // The variable DID change — check the console:
    console.log("BROKEN COUNTER: variable is now", count);
    // But React doesn't know about it. No re-render happens.
    // The screen stays frozen at 0.
  }

  return (
    <div className="demo-subsection">
      <h3>A. The Broken Counter (plain variable)</h3>
      <p className="demo-note">
        Click the button and watch the console vs the screen. The variable changes, but the UI does not!
      </p>
      <p>Count on screen: <strong>{count}</strong></p>
      <button className="btn btn-secondary" onClick={handleClick}>
        Increment (broken)
      </button>
      {/* WHY is it broken?
          React only re-renders when STATE changes.
          A plain variable changing is invisible to React.
          Even if the variable changes, React never re-calls this function. */}
    </div>
  );
}
```

**File: `src/useStateDemo/WorkingCounter.jsx`**
```jsx
import { useState } from "react";

// ============================================================
// Section B: The Working Counter
// ============================================================
// Same counter as Section A, but now using useState.
// React knows about changes and re-renders the component!
// Open the browser console to follow along!

export default function WorkingCounter() {
  // useState returns [currentValue, setterFunction]
  // React will re-call this entire function whenever setCount is called.
  const [count, setCount] = useState(0);

  // This log runs every time the component renders (function is called):
  console.log("WORKING COUNTER: rendering with count =", count);

  function handleClick() {
    // setCount tells React: "Hey, state changed! Re-render me."
    setCount(count + 1);
    console.log("WORKING COUNTER: called setCount. count is STILL", count, "(until next render)");
    // Notice: count doesn't change immediately! It's a snapshot of THIS render.
  }

  return (
    <div className="demo-subsection">
      <h3>B. The Working Counter (useState)</h3>
      <p className="demo-note">
        Now the UI updates! Check the console — notice the component re-renders each time.
      </p>
      <p>Count on screen: <strong>{count}</strong></p>
      <button className="btn btn-primary" onClick={handleClick}>
        Increment (works!)
      </button>
    </div>
  );
}
```

**File: `src/useStateDemo/StaleStateTrap.jsx`**
```jsx
import { useState } from "react";

// ============================================================
// Section C: The Stale State Trap
// ============================================================
// Why you sometimes need the updater function form: setCount(prev => prev + 1)
// The "direct" way sees a STALE snapshot of state.
// The "updater" way always reads the LATEST pending value.
// Open the browser console to follow along!

export default function StaleStateTrap() {
  const [count, setCount] = useState(0);

  console.log("STALE STATE: rendering with count =", count);

  function addThreeDirect() {
    // These all see the SAME snapshot of count (e.g., 0)
    // So this is like saying: setCount(0+1), setCount(0+1), setCount(0+1)
    // Result: count goes to 1, not 3!
    setCount(count + 1);
    setCount(count + 1);
    setCount(count + 1);
    console.log("STALE STATE: called setCount(count+1) three times. count was", count);
  }

  function addThreeUpdater() {
    // The updater function always receives the LATEST pending value
    // prev starts at current, then each call builds on the last
    setCount(prev => prev + 1); // prev=0 → 1
    setCount(prev => prev + 1); // prev=1 → 2
    setCount(prev => prev + 1); // prev=2 → 3
    console.log("STALE STATE: called setCount(prev => prev+1) three times");
  }

  return (
    <div className="demo-subsection">
      <h3>C. The Stale State Trap</h3>
      <p className="demo-note">
        Click each button and compare. The first only adds 1 (stale closure). The second adds 3 (updater function).
      </p>
      <p>Count: <strong>{count}</strong></p>
      <button className="btn btn-danger" onClick={addThreeDirect} style={{ marginRight: 8 }}>
        Add 3 (direct — broken)
      </button>
      <button className="btn btn-primary" onClick={addThreeUpdater}>
        Add 3 (updater — correct)
      </button>
      <br />
      <button className="btn btn-secondary" onClick={() => setCount(0)} style={{ marginTop: 8 }}>
        Reset
      </button>
    </div>
  );
}
```

**File: `src/useStateDemo/MultipleStates.jsx`**
```jsx
import { useState } from "react";

// ============================================================
// Section D: Multiple State Variables
// ============================================================
// Each useState call is independent. Changing one doesn't affect the other.
// React remembers them by the ORDER they are called.
// This is why hooks must not be called inside conditions or loops.
// Open the browser console to follow along!

export default function MultipleStates() {
  const [name, setName] = useState("");
  const [age, setAge] = useState(0);

  console.log("MULTIPLE STATES: rendering with name =", JSON.stringify(name), "age =", age);

  return (
    <div className="demo-subsection">
      <h3>D. Multiple State Variables</h3>
      <p className="demo-note">
        Change one input. Watch the console — the component re-renders, but the OTHER state stays the same.
      </p>
      <div style={{ marginBottom: 8 }}>
        <label>Name: </label>
        <input
          type="text"
          value={name}
          onChange={(e) => {
            console.log("MULTIPLE STATES: name changing to", JSON.stringify(e.target.value));
            setName(e.target.value);
          }}
          style={{ padding: "4px 8px", marginLeft: 8 }}
        />
      </div>
      <div style={{ marginBottom: 8 }}>
        <label>Age: </label>
        <button className="btn btn-secondary" onClick={() => {
          console.log("MULTIPLE STATES: age incrementing");
          setAge(prev => prev + 1);
        }}>
          {age} (click to increment)
        </button>
      </div>
      <p>Current values — name: "<strong>{name}</strong>", age: <strong>{age}</strong></p>
      {/* Each useState call tracks ONE piece of state.
          React remembers them by the ORDER they are called.
          This is why hooks must not be called inside conditions or loops. */}
    </div>
  );
}
```

**File: `src/useStateDemo/index.jsx`**
```jsx
import SectionStepper from "../SectionStepper";
import BrokenCounter from "./BrokenCounter";
import WorkingCounter from "./WorkingCounter";
import StaleStateTrap from "./StaleStateTrap";
import MultipleStates from "./MultipleStates";

// ============================================================
// useState Demo — Understanding React State
// ============================================================
// This file teaches WHY React state exists, HOW it works,
// and common pitfalls students encounter.
// Each section mounts one at a time so the console stays clean.

const PRACTICAL = (
  <div className="demo-practical">
    <h3>When do you use useState in real apps?</h3>
    <ul>
      <li><strong>Form inputs</strong> — tracking what the user types (name, email, password)</li>
      <li><strong>Toggles</strong> — dark mode on/off, sidebar open/closed, modal visible/hidden</li>
      <li><strong>Counters</strong> — items in a shopping cart, notification badges, pagination</li>
      <li><strong>Loading/error states</strong> — showing a spinner while data loads, showing error messages</li>
      <li><strong>Selected items</strong> — which tab is active, which list item is highlighted</li>
      <li><strong>Any data that, when it changes, should update what the user sees</strong></li>
    </ul>
    <p className="demo-note">
      Rule of thumb: if the UI should change when a value changes, put it in state.
      If not (like a timer ID or a cache), use useRef instead.
    </p>
  </div>
);

const sections = [
  { label: "A. Broken Counter", content: <BrokenCounter /> },
  { label: "B. Working Counter", content: <WorkingCounter /> },
  { label: "C. Stale State Trap", content: <StaleStateTrap /> },
  { label: "D. Multiple States", content: <MultipleStates /> },
  { label: "Practical Use Cases", content: PRACTICAL },
];

export default function UseStateDemo() {
  return (
    <div className="demo-section">
      <h2>useState — Making React Aware of Changes</h2>
      <p className="demo-note">
        Open your browser console (F12 → Console) to see what happens behind the scenes.
        Use the section buttons below to step through each concept one at a time.
      </p>
      <SectionStepper sections={sections} />
    </div>
  );
}
```

### Update App.jsx — Add the useState Tab

**File: `src/App.jsx`** (replace the contents)
```jsx
import { useState } from "react";
import ReactVsJs from "./reactVsJs";
import UseStateDemo from "./useStateDemo";

const TABS = [
  { id: "reactVsJs", label: "React vs JS" },
  { id: "useState", label: "useState" },
];

function App() {
  const [activeDemo, setActiveDemo] = useState("reactVsJs");

  return (
    <div>
      <h1>CS300 React Demos</h1>
      <nav className="demo-nav">
        {TABS.map((tab) => (
          <button
            key={tab.id}
            className={`btn ${activeDemo === tab.id ? "btn-active" : "btn-secondary"}`}
            onClick={() => {
              console.log("NAV: switching to", tab.id, "(previous component will unmount)");
              setActiveDemo(tab.id);
            }}
          >
            {tab.label}
          </button>
        ))}
      </nav>

      {activeDemo === "reactVsJs" && <ReactVsJs />}
      {activeDemo === "useState" && <UseStateDemo />}
    </div>
  );
}

export default App;
```

### Commit


{{< callout type="important" >}}
**Checkpoint: Commit your work**
{{< /callout >}}

```bash
git add -A
git commit -m "Add useState demo — broken counter, working counter, stale state, multiple states"
git push
```

{{% details title="Try This (Experiments)" %}}


1. **In BrokenCounter:** Add a `console.log` at the TOP of the function. Click the button. Does the log appear? (No — React never re-calls the function because no state changed.)
2. **In WorkingCounter:** After `setCount(count + 1)`, immediately `console.log(count)`. Notice it still shows the OLD value — state updates are batched and applied on the next render.
3. **In StaleStateTrap:** Change `addThreeDirect` to call `setCount` five times. Does it add 5 or 1?
4. **Add a third state** in MultipleStates — maybe a `[color, setColor]` with a dropdown. Verify that changing one state doesn't reset the others.


{{% /details %}}

---

## Part 3: useEffect — Side Effects Outside Rendering

### What is a Side Effect?

Your component function should be **pure** — given the same state/props, it should return the same JSX. But sometimes you need to do things that "reach outside" the component:

- Fetching data from an API
- Starting a timer
- Listening to window resize events
- Updating the document title
- Writing to localStorage

These are **side effects**. `useEffect` is where you put them. Effects run **after** React has updated the DOM and the browser has painted — so they don't block the UI.

```jsx
useEffect(() => {
  // This code runs AFTER the screen updates
  console.log("effect!");

  return () => {
    // This cleanup code runs before the effect re-runs or on unmount
    console.log("cleanup!");
  };
}, [dependency]); // Controls WHEN the effect runs
```

**The dependency array:**
- `useEffect(fn)` — no array → runs after **every** render
- `useEffect(fn, [])` — empty array → runs **once** on mount
- `useEffect(fn, [x])` — runs when `x` changes

### Create the useEffect Demo Files

Create the folder `src/useEffectDemo/` and add these files:

**File: `src/useEffectDemo/RenderVsEffect.jsx`**
```jsx
import { useState, useEffect } from "react";

// ============================================================
// Section A: Render vs Effect Timing
// ============================================================
// Shows that the component function body runs FIRST (during rendering),
// and useEffect runs AFTER the browser paints the screen.
// Open the browser console to follow along!

export default function RenderVsEffect() {
  const [count, setCount] = useState(0);

  // 1️⃣ This runs DURING rendering (the function body)
  console.log("A. RENDER: component function is running, count =", count);

  // 2️⃣ This runs AFTER the browser paints the screen
  useEffect(() => {
    console.log("A. EFFECT: this runs AFTER the screen updated, count =", count);
  });

  return (
    <div className="demo-subsection">
      <h3>A. Render vs Effect Timing</h3>
      <p className="demo-note">
        Click the button and watch the console. "RENDER" logs first, then "EFFECT" logs after.
        The component function runs → React updates the DOM → browser paints → useEffect runs.
      </p>
      {console.log("A. JSX: inside the return statement, count =", count)}
      <p>Count: <strong>{count}</strong></p>
      <button className="btn btn-primary" onClick={() => setCount(prev => prev + 1)}>
        Increment (watch console order)
      </button>
    </div>
  );
}
```

**File: `src/useEffectDemo/DependencyArray.jsx`**
```jsx
import { useState, useEffect } from "react";

// ============================================================
// Section B: The Dependency Array
// ============================================================
// The second argument to useEffect controls WHEN the effect runs:
//   - No array    → runs after EVERY render
//   - Empty []    → runs ONCE on mount
//   - [x]         → runs when x changes
// Open the browser console to follow along!

export default function DependencyArray() {
  const [searchTerm, setSearchTerm] = useState("");
  const [clickCount, setClickCount] = useState(0);

  // No dependency array → runs after EVERY render
  useEffect(() => {
    console.log("B. EFFECT (no deps): runs after EVERY render");
  });

  // Empty array → runs ONCE when component first mounts
  useEffect(() => {
    console.log("B. EFFECT ([]): runs ONCE on mount");
  }, []);

  // [searchTerm] → runs when searchTerm changes
  useEffect(() => {
    console.log("B. EFFECT ([searchTerm]): searchTerm changed to", JSON.stringify(searchTerm));
  }, [searchTerm]);

  // [clickCount] → runs when clickCount changes
  useEffect(() => {
    console.log("B. EFFECT ([clickCount]): clickCount changed to", clickCount);
  }, [clickCount]);

  return (
    <div className="demo-subsection">
      <h3>B. The Dependency Array</h3>
      <p className="demo-note">
        Type in the input and click the button. Watch which effects fire in the console.
      </p>
      <div style={{ marginBottom: 8 }}>
        <label>Search: </label>
        <input
          type="text"
          value={searchTerm}
          onChange={(e) => setSearchTerm(e.target.value)}
          placeholder="Type here..."
          style={{ padding: "4px 8px", marginLeft: 8 }}
        />
      </div>
      <button className="btn btn-secondary" onClick={() => setClickCount(prev => prev + 1)}>
        Click count: {clickCount}
      </button>
      <div className="demo-note" style={{ marginTop: 12 }}>
        <strong>Summary:</strong><br />
        • <code>useEffect(fn)</code> — no array → runs after every render<br />
        • <code>useEffect(fn, [])</code> — empty array → runs once on mount<br />
        • <code>useEffect(fn, [x])</code> — runs when x changes
      </div>
    </div>
  );
}
```

**File: `src/useEffectDemo/CleanupDemo.jsx`**
```jsx
import { useState, useEffect } from "react";

// ============================================================
// Section C: Cleanup Functions
// ============================================================
// When a useEffect returns a function, that function runs:
//   1. When the component unmounts
//   2. Before the effect re-runs (if deps changed)
// This is how you prevent memory leaks from timers, subscriptions, etc.
// Open the browser console to follow along!
// NOTE: In development, React StrictMode runs effects twice. This is normal!

// A sub-component that runs a setInterval and cleans it up
function TickingClock() {
  const [ticks, setTicks] = useState(0);

  useEffect(() => {
    console.log("C. CLEANUP: ⏱️ starting interval (clock mounted)");

    const id = setInterval(() => {
      setTicks(prev => prev + 1);
      console.log("C. CLEANUP: ⏱️ tick");
    }, 1000);

    // This cleanup function runs:
    // 1. When the component unmounts
    // 2. Before the effect re-runs (if deps changed)
    return () => {
      console.log("C. CLEANUP: 🛑 clearing interval (clock unmounting)");
      clearInterval(id);
    };
  }, []); // Empty deps = mount once, cleanup on unmount

  return <p>Clock has ticked <strong>{ticks}</strong> times</p>;
}

export default function CleanupDemo() {
  const [showClock, setShowClock] = useState(false);

  return (
    <div className="demo-subsection">
      <h3>C. Cleanup Functions</h3>
      <p className="demo-note">
        Toggle the clock on/off. Watch the console — "starting interval" on mount, "clearing interval" on unmount.
        Cleanup prevents memory leaks!
      </p>
      <button
        className="btn btn-primary"
        onClick={() => {
          console.log("C. CLEANUP: toggling clock", showClock ? "OFF" : "ON");
          setShowClock(prev => !prev);
        }}
      >
        {showClock ? "Hide Clock (unmount)" : "Show Clock (mount)"}
      </button>
      {showClock && <TickingClock />}
      {/* When showClock becomes false, React unmounts TickingClock.
          The cleanup function inside its useEffect runs, clearing the interval.
          Without cleanup, the interval would keep running forever! */}
    </div>
  );
}
```

**File: `src/useEffectDemo/DebouncedSearch.jsx`**
```jsx
import { useState, useEffect } from "react";

// ============================================================
// Section D: Debounced Search (Practical Pattern)
// ============================================================
// A common real-world pattern: wait for the user to STOP typing
// before performing an expensive operation (like an API call).
// useEffect + cleanup makes this elegant.
// Open the browser console to follow along!

export default function DebouncedSearch() {
  const [query, setQuery] = useState("");
  const [debouncedQuery, setDebouncedQuery] = useState("");

  useEffect(() => {
    // Set a timer to "search" after 500ms of no typing
    console.log("D. DEBOUNCE: setting 500ms timer for", JSON.stringify(query));

    const timerId = setTimeout(() => {
      console.log("D. DEBOUNCE: ✅ Timer fired! Searching for:", JSON.stringify(query));
      setDebouncedQuery(query);
    }, 500);

    // Cleanup: if the user types again before 500ms, cancel the old timer
    return () => {
      console.log("D. DEBOUNCE: ❌ Cancelled timer (user typed again)");
      clearTimeout(timerId);
    };
    // This effect runs every time query changes.
    // The cleanup cancels the PREVIOUS timer before starting a new one.
    // Result: only the LAST keystroke (after 500ms pause) triggers the "search."
  }, [query]);

  return (
    <div className="demo-subsection">
      <h3>D. Debounced Search (Practical Pattern)</h3>
      <p className="demo-note">
        Type quickly, then stop. Only the final value gets "searched" after 500ms. Watch the console!
      </p>
      <input
        type="text"
        value={query}
        onChange={(e) => setQuery(e.target.value)}
        placeholder="Type a search query..."
        style={{ padding: "8px 12px", fontSize: 16, width: "100%", maxWidth: 300, boxSizing: "border-box" }}
      />
      <p>You typed: <strong>{query}</strong></p>
      <p>Debounced (searched) value: <strong>{debouncedQuery || "(waiting...)"}</strong></p>
    </div>
  );
}
```

**File: `src/useEffectDemo/index.jsx`**
```jsx
import SectionStepper from "../SectionStepper";
import RenderVsEffect from "./RenderVsEffect";
import DependencyArray from "./DependencyArray";
import CleanupDemo from "./CleanupDemo";
import DebouncedSearch from "./DebouncedSearch";

// ============================================================
// useEffect Demo — Running Code Outside the Render Cycle
// ============================================================
// useEffect lets you run "side effects" AFTER React has updated the screen.
// Side effects = anything that reaches outside the component:
//   - fetching data, setting timers, subscribing to events, logging, etc.
// Each section mounts one at a time so the console stays clean.

const PRACTICAL = (
  <div className="demo-practical">
    <h3>When do you use useEffect in real apps?</h3>
    <ul>
      <li><strong>Fetching data from an API</strong> — load user data when a profile page mounts</li>
      <li><strong>Subscribing to events</strong> — listen for window resize, WebSocket messages, keyboard shortcuts</li>
      <li><strong>Syncing with localStorage</strong> — save user preferences whenever they change</li>
      <li><strong>Updating the document title</strong> — show notification count in the browser tab</li>
      <li><strong>Setting up timers</strong> — intervals, timeouts, debouncing, polling</li>
      <li><strong>Analytics/logging</strong> — track page views or user interactions</li>
      <li><strong>Cleanup on unmount</strong> — unsubscribe, clear timers, cancel network requests</li>
    </ul>
    <p className="demo-note">
      Rule of thumb: if code needs to "reach outside" the component (browser APIs, network, DOM) → useEffect.
      If it's just computing a value from props/state → do it in the function body, no effect needed.
    </p>
  </div>
);

const sections = [
  { label: "A. Render vs Effect", content: <RenderVsEffect /> },
  { label: "B. Dependency Array", content: <DependencyArray /> },
  { label: "C. Cleanup", content: <CleanupDemo /> },
  { label: "D. Debounced Search", content: <DebouncedSearch /> },
  { label: "Practical Use Cases", content: PRACTICAL },
];

export default function UseEffectDemo() {
  return (
    <div className="demo-section">
      <h2>useEffect — Side Effects Outside the Render Cycle</h2>
      <p className="demo-note">
        Open your browser console (F12 → Console) to see what happens behind the scenes.
        Use the section buttons below to step through each concept one at a time.
      </p>
      <SectionStepper sections={sections} />
    </div>
  );
}
```

### Update App.jsx — Add the useEffect Tab

Add this import at the top of `src/App.jsx`:
```jsx
import UseEffectDemo from "./useEffectDemo";
```

Add to the TABS array:
```jsx
{ id: "useEffect", label: "useEffect" },
```

Add this line in the return after the useState conditional render:
```jsx
{activeDemo === "useEffect" && <UseEffectDemo />}
```

### Commit


{{< callout type="important" >}}
**Checkpoint: Commit your work**
{{< /callout >}}

```bash
git add -A
git commit -m "Add useEffect demo — render vs effect, deps array, cleanup, debounce"
git push
```

{{% details title="Try This (Experiments)" %}}


1. **In CleanupDemo:** Comment out the `return () => { clearInterval(id); }` cleanup. Toggle the clock on/off several times. Open the console — you'll see "tick" logs multiplying because old intervals never get cleared. This is a **memory leak**!
2. **In DebouncedSearch:** Change the debounce delay from 500ms to 2000ms. Type quickly and observe the longer wait.
3. **In DependencyArray:** Add a third state variable and a corresponding effect with `[newVar]`. Verify it only fires when that specific variable changes.
4. **Remove the dependency array** entirely from one effect (just `useEffect(() => { ... })`). Notice how it runs after EVERY render.


{{% /details %}}

---

## Part 4: useRef — Persistent Values Without Re-Renders

### What is useRef?

`useRef` gives you a "box" (`.current`) that:
1. **Persists** across renders (doesn't reset like a `let` variable)
2. **Does NOT trigger a re-render** when changed (unlike `useState`)

Two main use cases:
- **Storing values** that shouldn't trigger re-renders (timer IDs, previous values)
- **Accessing DOM elements** directly (focus an input, measure size)

```jsx
const myRef = useRef(initialValue);
// myRef.current = the stored value
// Changing myRef.current does NOT cause a re-render
```

### When to Use What?

| | useState | useRef | let variable |
|---|---|---|---|
| Persists across renders? | Yes | Yes | **No** (resets each render) |
| Triggers re-render on change? | **Yes** | No | No |
| Use for... | UI-visible data | Hidden values, DOM access | Temporary calculations |

### Create the useRef Demo Files

Create the folder `src/useRefDemo/` and add these files:

**File: `src/useRefDemo/RefVsState.jsx`**
```jsx
import { useState, useRef } from "react";

// ============================================================
// Section A: useRef vs useState
// ============================================================
// Changing a ref does NOT update the screen. Changing state DOES.
// This is the key difference between useRef and useState.
// Open the browser console to follow along!

export default function RefVsState() {
  const [stateCount, setStateCount] = useState(0);
  const refCount = useRef(0);

  // This increments every render — it's a render counter!
  refCount.current += 1;

  console.log("REF VS STATE: rendering. stateCount =", stateCount, ", refCount.current =", refCount.current);

  function handleRefClick() {
    refCount.current += 100;
    // The ref changed, but React doesn't know or care — no re-render!
    console.log("REF VS STATE: ref is now", refCount.current, "(screen won't update)");
  }

  function handleStateClick() {
    // This triggers a re-render, which also increments refCount by 1
    setStateCount(prev => prev + 1);
    console.log("REF VS STATE: state changing (will re-render, and refCount will increment by 1)");
  }

  return (
    <div className="demo-subsection">
      <h3>A. useRef vs useState</h3>
      <p className="demo-note">
        Click "Change Ref" — the console updates but the screen does NOT.
        Click "Change State" — the screen updates AND the ref render count goes up.
      </p>
      <p>State count (on screen): <strong>{stateCount}</strong></p>
      <p>Ref count (on screen): <strong>{refCount.current}</strong></p>
      <p className="demo-note">
        The ref value on screen only updates when something ELSE causes a re-render.
      </p>
      <button className="btn btn-danger" onClick={handleRefClick} style={{ marginRight: 8 }}>
        Change Ref (+100, no re-render)
      </button>
      <button className="btn btn-primary" onClick={handleStateClick}>
        Change State (+1, causes re-render)
      </button>
    </div>
  );
}
```

**File: `src/useRefDemo/DomAccess.jsx`**
```jsx
import { useRef } from "react";

// ============================================================
// Section B: Accessing DOM Elements
// ============================================================
// useRef can hold a reference to an actual DOM element.
// ref.current points to the actual DOM node after mount.
// This is React's escape hatch to the real DOM.
// Open the browser console to follow along!

export default function DomAccess() {
  const inputRef = useRef(null);
  // inputRef.current will point to the <input> DOM node after mount

  function handleFocus() {
    // Directly calling a method on the DOM element
    inputRef.current.focus();
    console.log("DOM ACCESS: focused the input via inputRef.current.focus()");
  }

  function handleLogValue() {
    // Reading the raw DOM value — bypassing React's state
    console.log("DOM ACCESS: input's DOM value is:", JSON.stringify(inputRef.current.value));
  }

  return (
    <div className="demo-subsection">
      <h3>B. Accessing DOM Elements</h3>
      <p className="demo-note">
        ref.current points to the actual DOM node. This is React's escape hatch to the real DOM.
      </p>
      <input
        ref={inputRef}
        type="text"
        placeholder="Type something here..."
        style={{ padding: "8px 12px", fontSize: 16, marginBottom: 8, display: "block" }}
      />
      <button className="btn btn-primary" onClick={handleFocus} style={{ marginRight: 8 }}>
        Focus the Input
      </button>
      <button className="btn btn-secondary" onClick={handleLogValue}>
        Log Input Value (check console)
      </button>
    </div>
  );
}
```

**File: `src/useRefDemo/PreviousValue.jsx`**
```jsx
import { useState, useRef, useEffect } from "react";

// ============================================================
// Section C: Storing a Previous Value
// ============================================================
// Refs persist across renders, so they're perfect for
// "remembering" the last value without causing extra re-renders.
// Open the browser console to follow along!

export default function PreviousValue() {
  const [count, setCount] = useState(0);
  const prevCountRef = useRef(0);

  // After each render, save the current count as the "previous" for next time
  useEffect(() => {
    console.log("PREVIOUS VALUE: saving", count, "as previous (was", prevCountRef.current, ")");
    prevCountRef.current = count;
  }, [count]);

  return (
    <div className="demo-subsection">
      <h3>C. Storing a Previous Value</h3>
      <p className="demo-note">
        The ref remembers the last value without causing extra re-renders.
      </p>
      <p>Current count: <strong>{count}</strong></p>
      <p>Previous count: <strong>{prevCountRef.current}</strong></p>
      <button className="btn btn-primary" onClick={() => setCount(prev => prev + 1)} style={{ marginRight: 8 }}>
        Increment
      </button>
      <button className="btn btn-secondary" onClick={() => setCount(0)}>
        Reset
      </button>
      {/* How it works:
          1. count changes → re-render → screen shows new count and OLD prevCountRef
          2. After render, useEffect runs → saves count into prevCountRef
          3. Next render will show the updated prevCountRef */}
    </div>
  );
}
```

**File: `src/useRefDemo/Stopwatch.jsx`**
```jsx
import { useState, useRef, useEffect } from "react";

// ============================================================
// Section D: Stopwatch (timer ID in a ref)
// ============================================================
// Timer IDs should be stored in refs because:
// - They need to persist across renders (so we can clear them later)
// - Changing them should NOT cause a re-render
// Open the browser console to follow along!

export default function Stopwatch() {
  const [seconds, setSeconds] = useState(0);
  const [isRunning, setIsRunning] = useState(false);
  const timerRef = useRef(null);

  function start() {
    if (timerRef.current !== null) return; // Already running
    console.log("STOPWATCH: starting interval");
    setIsRunning(true);
    timerRef.current = setInterval(() => {
      setSeconds(prev => prev + 1);
    }, 1000);
    // We store the interval ID in a ref, not state,
    // because changing it shouldn't re-render the component.
  }

  function stop() {
    console.log("STOPWATCH: stopping interval, timer ID was", timerRef.current);
    clearInterval(timerRef.current);
    timerRef.current = null;
    setIsRunning(false);
  }

  function reset() {
    stop();
    setSeconds(0);
    console.log("STOPWATCH: reset to 0");
  }

  // Cleanup on unmount — if user switches tabs while stopwatch is running
  useEffect(() => {
    return () => {
      if (timerRef.current !== null) {
        console.log("STOPWATCH: cleanup — clearing interval on unmount");
        clearInterval(timerRef.current);
      }
    };
  }, []);

  return (
    <div className="demo-subsection">
      <h3>D. Stopwatch (timer ID in a ref)</h3>
      <p className="demo-note">
        The interval ID is stored in a ref, not state. We need it to persist (to clear later) but changing it shouldn't re-render.
      </p>
      <p style={{ fontSize: 32, fontWeight: "bold", fontFamily: "monospace" }}>
        {seconds}s
      </p>
      <button className="btn btn-primary" onClick={start} disabled={isRunning} style={{ marginRight: 8 }}>
        Start
      </button>
      <button className="btn btn-danger" onClick={stop} disabled={!isRunning} style={{ marginRight: 8 }}>
        Stop
      </button>
      <button className="btn btn-secondary" onClick={reset}>
        Reset
      </button>
    </div>
  );
}
```

**File: `src/useRefDemo/index.jsx`**
```jsx
import SectionStepper from "../SectionStepper";
import RefVsState from "./RefVsState";
import DomAccess from "./DomAccess";
import PreviousValue from "./PreviousValue";
import Stopwatch from "./Stopwatch";

// ============================================================
// useRef Demo — Values That Persist Without Re-Rendering
// ============================================================
// useRef gives you a "box" (.current) that persists across renders
// but DOES NOT trigger a re-render when changed.
// Each section mounts one at a time so the console stays clean.

const PRACTICAL = (
  <div className="demo-practical">
    <h3>When do you use useRef in real apps?</h3>
    <ul>
      <li><strong>Auto-focusing inputs</strong> — focus a search bar when a page loads</li>
      <li><strong>Storing timer/interval IDs</strong> — need to clear them later without re-rendering</li>
      <li><strong>Tracking previous values</strong> — for animations or comparison logic</li>
      <li><strong>Measuring DOM elements</strong> — getting element width/height for layout calculations</li>
      <li><strong>Integrating non-React libraries</strong> — a chart library that needs a DOM node to render into</li>
      <li><strong>Storing any value that should NOT trigger a re-render when it changes</strong></li>
    </ul>
    <p className="demo-note">
      Rule of thumb: if changing a value should update the screen → useState.
      If it should NOT update the screen → useRef.
    </p>
  </div>
);

const sections = [
  { label: "A. Ref vs State", content: <RefVsState /> },
  { label: "B. DOM Access", content: <DomAccess /> },
  { label: "C. Previous Value", content: <PreviousValue /> },
  { label: "D. Stopwatch", content: <Stopwatch /> },
  { label: "Practical Use Cases", content: PRACTICAL },
];

export default function UseRefDemo() {
  return (
    <div className="demo-section">
      <h2>useRef — Persistent Values Without Re-Renders</h2>
      <p className="demo-note">
        Open your browser console (F12 → Console) to see what happens behind the scenes.
        Use the section buttons below to step through each concept one at a time.
      </p>
      <SectionStepper sections={sections} />
    </div>
  );
}
```

### Update App.jsx — Add the useRef Tab

Add this import at the top of `src/App.jsx`:
```jsx
import UseRefDemo from "./useRefDemo";
```

Add to the TABS array:
```jsx
{ id: "useRef", label: "useRef" },
```

Add this line in the return:
```jsx
{activeDemo === "useRef" && <UseRefDemo />}
```

### Commit


{{< callout type="important" >}}
**Checkpoint: Commit your work**
{{< /callout >}}

```bash
git add -A
git commit -m "Add useRef demo — ref vs state, DOM access, previous value, stopwatch"
git push
```

{{% details title="Try This (Experiments)" %}}


1. **In RefVsState:** Click "Change Ref" 5 times, then click "Change State" once. What value does the ref show on screen now? (It should show all the accumulated changes.)
2. **In DomAccess:** Add a second input and second ref. Make a button that focuses the second input instead.
3. **In Stopwatch:** Try replacing `useRef(null)` with `useState(null)` for the timer ID. What happens? (Hint: every time you store a new interval ID, it triggers an unnecessary re-render.)


{{% /details %}}

---

## Part 5: Composition — Building UIs from Small Pieces

### What is Composition?

Composition means building complex UIs by combining small, reusable components — like LEGO blocks. Each component does one thing well, and you snap them together.

The key concept is the **children** prop: whatever you put between a component's opening and closing tags becomes `children`.

```jsx
<Shell title="My Wrapper">
  <p>This paragraph becomes the children prop!</p>
</Shell>
```

### Create the Composition Demo Files

Create the folder `src/compositionDemo/` and add these files:

**File: `src/compositionDemo/Shell.jsx`**
```jsx
// ============================================================
// Shell Component — Reusable wrapper with colored title bar
// ============================================================
// Used across multiple composition demo sections.
// Takes backgroundColor, textColor, title, and children props.
// Uses CSS custom properties for theming.

export default function Shell({ backgroundColor, textColor, title, children }) {
  console.log("SHELL: rendering", JSON.stringify(title), "with bg =", backgroundColor);

  return (
    <div
      className="shell"
      style={{
        "--shell-bg": backgroundColor || "#3498db",
        "--shell-text": textColor || "#fff",
      }}
    >
      <div className="shell-title">{title}</div>
      <div className="shell-content">
        {children}
        {/* children = whatever you put BETWEEN <Shell> and </Shell>.
            This is how composition works — the parent decides what goes inside. */}
      </div>
    </div>
  );
}
```

**File: `src/compositionDemo/Card.jsx`**
```jsx
// ============================================================
// Card Component — Simple card that can go inside a Shell
// ============================================================
// Used across multiple composition demo sections.

export default function Card({ title, children }) {
  console.log("CARD: rendering", JSON.stringify(title));
  return (
    <div className="card">
      <strong>{title}</strong>
      <div>{children}</div>
    </div>
  );
}
```

**File: `src/compositionDemo/ShellPatternDemo.jsx`**
```jsx
import Shell from "./Shell";

// ============================================================
// Section A: The Shell Pattern
// ============================================================
// A Shell is a reusable wrapper. You pass it a title and background color,
// and put anything inside it using children.
// Open the browser console to follow along!

export default function ShellPatternDemo() {
  return (
    <div className="demo-subsection">
      <h3>A. The Shell Pattern</h3>
      <p className="demo-note">
        A Shell is a reusable wrapper. You pass it a title and background color,
        and put anything inside it using children. Look at how the blue shell
        wraps the content below.
      </p>

      <Shell backgroundColor="#3498db" title="My First Shell">
        <p>This content is INSIDE the shell.</p>
        <p>The shell provides the colored title bar and border.</p>
        <p>Whatever we put here becomes the shell's <code>children</code> prop.</p>
      </Shell>

      <Shell backgroundColor="#e74c3c" title="A Red Shell">
        <p>Same component, different color. The Shell doesn't care what's inside!</p>
      </Shell>
    </div>
  );
}
```

**File: `src/compositionDemo/NestingDemo.jsx`**
```jsx
import Shell from "./Shell";
import Card from "./Card";

// ============================================================
// Section B: Nesting Components (Composition)
// ============================================================
// Components inside components inside components.
// Each Shell has a different color so you can SEE the nesting.
// Open the browser console to see the rendering order.

export default function NestingDemo() {
  return (
    <div className="demo-subsection">
      <h3>B. Nesting Components (Composition)</h3>
      <p className="demo-note">
        Components inside components inside components.
        Each Shell has a different color so you can SEE the nesting.
        Check the console to see the rendering order.
      </p>

      <Shell backgroundColor="#3498db" title="Outer Shell (Blue)">
        <p>I'm in the outer (blue) shell.</p>

        <Shell backgroundColor="#2ecc71" title="Inner Shell (Green)">
          <p>I'm in the inner (green) shell, which is INSIDE the blue shell.</p>

          <Card title="A Card Inside the Green Shell">
            <p>This card is the deepest level of nesting.</p>
          </Card>
        </Shell>

        <Card title="Another Card (in the Blue Shell)">
          <p>This card is inside the blue shell but outside the green one.</p>
        </Card>
      </Shell>

      {/* This is composition:
          - Shell doesn't know about Card
          - Card doesn't know about Shell
          - They work together because they both accept children
          - YOU decide how to combine them */}
    </div>
  );
}
```

**File: `src/compositionDemo/PropsFlowDemo.jsx`**
```jsx
import { useState } from "react";
import Shell from "./Shell";

// ============================================================
// Section C: Props Flow Down (One-Way Data Flow)
// ============================================================
// The parent owns the theme state. It passes it down as a prop.
// Data flows DOWN: Parent → Shell → Card → Text.
// Children can NOT change the parent's state directly.
// Open the browser console to follow along!

function ThemedText({ theme, children }) {
  console.log("THEMED TEXT: received theme =", JSON.stringify(theme));
  return (
    <p style={{
      color: theme === "dark" ? "#ecf0f1" : "#2c3e50",
      fontWeight: "bold",
    }}>
      {children}
    </p>
  );
}

function ThemedCard({ theme, title, children }) {
  console.log("THEMED CARD: received theme =", JSON.stringify(theme));
  return (
    <div className="card" style={{
      backgroundColor: theme === "dark" ? "#34495e" : "#fff",
      borderColor: theme === "dark" ? "#2c3e50" : "#ddd",
    }}>
      <ThemedText theme={theme}>{title}</ThemedText>
      <div style={{ color: theme === "dark" ? "#bdc3c7" : "#333" }}>
        {children}
      </div>
    </div>
  );
}

export default function PropsFlowDemo() {
  const [theme, setTheme] = useState("light");

  console.log("PROPS FLOW: parent rendering with theme =", JSON.stringify(theme));

  return (
    <div className="demo-subsection">
      <h3>C. Props Flow Down (One-Way Data Flow)</h3>
      <p className="demo-note">
        The parent owns the theme state. It passes it down as a prop.
        Toggle the theme and watch the console — every level re-renders with the new value.
      </p>

      <button
        className="btn btn-primary"
        onClick={() => setTheme(prev => prev === "light" ? "dark" : "light")}
        style={{ marginBottom: 12 }}
      >
        Toggle Theme (current: {theme})
      </button>

      <Shell
        backgroundColor={theme === "dark" ? "#2c3e50" : "#3498db"}
        textColor="#fff"
        title={`Shell — theme: ${theme}`}
      >
        <ThemedCard theme={theme} title="Themed Card">
          <p>This text color comes from the theme prop, which started at the TOP.</p>
          <p>Data flows DOWN: Parent → Shell → Card → Text</p>
        </ThemedCard>
      </Shell>

      <p className="demo-note">
        <strong>One-way data flow:</strong> Parent → Child → Grandchild.
        Children can NOT change the parent's state directly.
        If a child needs to communicate up, it uses a callback function (next section!).
      </p>
    </div>
  );
}
```

**File: `src/compositionDemo/LiftingStateDemo.jsx`**
```jsx
import { useState } from "react";

// ============================================================
// Section D: Lifting State Up
// ============================================================
// When two sibling components need to share data,
// move (lift) the state to their nearest common parent.
// The parent passes the state down as props and a callback to update it.
// Open the browser console to follow along!

// Two independent counters (each has its own state)
function IndependentCounter({ label }) {
  const [count, setCount] = useState(0);
  console.log("INDEPENDENT COUNTER:", label, "rendering with count =", count);
  return (
    <div className="card" style={{ display: "inline-block", marginRight: 12 }}>
      <strong>{label}:</strong> {count}
      <br />
      <button className="btn btn-primary" onClick={() => setCount(prev => prev + 1)} style={{ marginTop: 4 }}>
        +1
      </button>
    </div>
  );
}

// A counter that receives its value and setter from the parent (lifted state)
function SharedCounter({ label, count, onIncrement }) {
  console.log("SHARED COUNTER:", label, "rendering with count =", count);
  return (
    <div className="card" style={{ display: "inline-block", marginRight: 12 }}>
      <strong>{label}:</strong> {count}
      <br />
      <button className="btn btn-primary" onClick={onIncrement} style={{ marginTop: 4 }}>
        +1
      </button>
    </div>
  );
}

// Temperature converter — classic lifting state example
function TemperatureConverter() {
  // The shared state lives in the PARENT
  const [celsius, setCelsius] = useState(0);

  // Derived values — computed from state, not stored separately
  const fahrenheit = (celsius * 9) / 5 + 32;

  console.log("TEMPERATURE: celsius =", celsius, "fahrenheit =", fahrenheit.toFixed(1));

  return (
    <div style={{ marginTop: 16 }}>
      <strong>Temperature Converter (shared state):</strong>
      <div style={{ marginTop: 8 }}>
        <label>
          Celsius:{" "}
          <input
            type="number"
            value={celsius}
            onChange={(e) => setCelsius(Number(e.target.value))}
            style={{ padding: "4px 8px", width: 80 }}
          />
        </label>
        <span style={{ margin: "0 12px" }}>=</span>
        <label>
          Fahrenheit:{" "}
          <input
            type="number"
            value={fahrenheit.toFixed(1)}
            onChange={(e) => setCelsius(((Number(e.target.value) - 32) * 5) / 9)}
            style={{ padding: "4px 8px", width: 80 }}
          />
        </label>
      </div>
      <p className="demo-note">
        Both inputs share the same state (celsius). Changing either one updates the other.
        The parent owns the "source of truth."
      </p>
    </div>
  );
}

export default function LiftingStateDemo() {
  const [sharedCount, setSharedCount] = useState(0);

  console.log("LIFTING STATE: parent rendering, sharedCount =", sharedCount);

  return (
    <div className="demo-subsection">
      <h3>D. Lifting State Up</h3>

      <p className="demo-note">
        <strong>Before lifting:</strong> each counter has its own state (they don't sync).
      </p>
      <div style={{ marginBottom: 16 }}>
        <IndependentCounter label="Counter A" />
        <IndependentCounter label="Counter B" />
      </div>

      <p className="demo-note">
        <strong>After lifting:</strong> the parent owns the state and passes it down.
        Both counters show the same value!
      </p>
      <div style={{ marginBottom: 16 }}>
        <SharedCounter
          label="Counter A"
          count={sharedCount}
          onIncrement={() => setSharedCount(prev => prev + 1)}
        />
        <SharedCounter
          label="Counter B"
          count={sharedCount}
          onIncrement={() => setSharedCount(prev => prev + 1)}
        />
      </div>

      <TemperatureConverter />
    </div>
  );
}
```

**File: `src/compositionDemo/index.jsx`**
```jsx
import SectionStepper from "../SectionStepper";
import ShellPatternDemo from "./ShellPatternDemo";
import NestingDemo from "./NestingDemo";
import PropsFlowDemo from "./PropsFlowDemo";
import LiftingStateDemo from "./LiftingStateDemo";

// ============================================================
// Composition Demo — Shells, Layouts, Props, and Lifting State
// ============================================================
// React is all about COMPOSING small pieces into bigger pieces.
// Components are like LEGO blocks — each one does one thing,
// and you snap them together to build complex UIs.
// Each section mounts one at a time so the console stays clean.

const PRACTICAL = (
  <div className="demo-practical">
    <h3>When do you use composition in real apps?</h3>
    <ul>
      <li><strong>Dashboard layouts</strong> — a Shell for the sidebar, a Shell for the main content area</li>
      <li><strong>Theme providers</strong> — pass dark/light theme down through the component tree</li>
      <li><strong>Modal/dialog wrappers</strong> — a Shell that adds an overlay and a close button</li>
      <li><strong>Form field containers</strong> — a wrapper that adds a label, error message, and styling</li>
      <li><strong>Page layouts</strong> — header + sidebar + content + footer, all composed together</li>
      <li><strong>Lifting state</strong> — whenever two siblings need to share data, move the state to their parent</li>
    </ul>
    <p className="demo-note">
      Rule of thumb: if two components need to stay in sync, lift their shared state to
      the nearest common parent and pass it down as props.
    </p>
  </div>
);

const sections = [
  { label: "A. Shell Pattern", content: <ShellPatternDemo /> },
  { label: "B. Nesting", content: <NestingDemo /> },
  { label: "C. Props Flow Down", content: <PropsFlowDemo /> },
  { label: "D. Lifting State", content: <LiftingStateDemo /> },
  { label: "Practical Use Cases", content: PRACTICAL },
];

export default function CompositionDemo() {
  return (
    <div className="demo-section">
      <h2>Composition — Building UIs from Small Pieces</h2>
      <p className="demo-note">
        Open your browser console (F12 → Console) to see what happens behind the scenes.
        Use the section buttons below to step through each concept one at a time.
      </p>
      <SectionStepper sections={sections} />
    </div>
  );
}
```

### Update App.jsx — Add the Composition Tab

Add this import at the top of `src/App.jsx`:
```jsx
import CompositionDemo from "./compositionDemo";
```

Add to the TABS array:
```jsx
{ id: "composition", label: "Composition" },
```

Add this line in the return:
```jsx
{activeDemo === "composition" && <CompositionDemo />}
```

### Commit


{{< callout type="important" >}}
**Checkpoint: Commit your work**
{{< /callout >}}

```bash
git add -A
git commit -m "Add composition demo — shell, nesting, props flow, lifting state"
git push
```

{{% details title="Try This (Experiments)" %}}


1. **In ShellPatternDemo:** Add a third Shell with a purple background. Put a list inside it.
2. **In NestingDemo:** Add a fourth level of nesting — a Card inside a Card inside a Shell.
3. **In PropsFlowDemo:** Add a third level — make `ThemedText` pass the theme to yet another component.
4. **In LiftingStateDemo:** Add a "Reset" button in the parent that sets `sharedCount` back to 0. Both counters should reset.


{{% /details %}}

---

## Part 6: Forms — Controlled Inputs in React

### Controlled vs Uncontrolled Inputs

{{< callout type="info" >}}
In vanilla HTML, form inputs manage their own state internally. In React, we make inputs **controlled** — React state is the single source of truth. The pattern: store the value in state, set the input's `value` prop to state, and update state on change. This means React always knows what the form data is — no need for `document.getElementById` or `FormData`.
{{< /callout >}}

The controlled input pattern:
1. Store the value in state: `const [name, setName] = useState("")`
2. Set the input's `value` prop to state: `value={name}`
3. Update state on change: `onChange={(e) => setName(e.target.value)}`

### Create the Form Demo Files

Create the folder `src/formDemo/` and add these files:

**File: `src/formDemo/ControlledTextInput.jsx`**
```jsx
import { useState } from "react";

// ============================================================
// Section A: Controlled Text Input
// ============================================================
// React owns the value. The input shows whatever state says.
// onChange updates state → re-render → input shows new value.
// Open the browser console to follow along!

export default function ControlledTextInput() {
  const [name, setName] = useState("");

  console.log("CONTROLLED INPUT: rendering, name =", JSON.stringify(name));

  return (
    <div className="demo-subsection">
      <h3>A. Controlled Text Input</h3>
      <p className="demo-note">
        React owns the value. The input shows whatever state says.
        onChange updates state → re-render → input shows new value.
        Watch the console on every keystroke!
      </p>

      <div className="form-group">
        <label>Your name:</label>
        <input
          type="text"
          value={name}
          onChange={(e) => {
            console.log("CONTROLLED INPUT: onChange fired, new value =", JSON.stringify(e.target.value));
            setName(e.target.value);
          }}
          placeholder="Type your name..."
        />
      </div>

      <p>Hello, <strong>{name || "..."}</strong>!</p>

      {/* The cycle:
          1. User types "A"
          2. onChange fires → setName("A")
          3. React re-renders → name is now "A"
          4. Input's value={name} shows "A"

          This is a "controlled" input — React controls what's displayed.
          If you remove onChange, the input becomes READ-ONLY (try it!). */}
    </div>
  );
}
```

**File: `src/formDemo/CheckboxAndSelect.jsx`**
```jsx
import { useState } from "react";

// ============================================================
// Section B: Checkbox and Select
// ============================================================
// Same pattern as text inputs: state drives the value, onChange updates state.
// For checkboxes, it's `checked` instead of `value`.
// Open the browser console to follow along!

export default function CheckboxAndSelect() {
  const [agreed, setAgreed] = useState(false);
  const [color, setColor] = useState("red");

  console.log("CHECKBOX/SELECT: rendering, agreed =", agreed, ", color =", JSON.stringify(color));

  return (
    <div className="demo-subsection">
      <h3>B. Checkbox and Select</h3>
      <p className="demo-note">
        Same pattern as text inputs: state drives the value, onChange updates state.
        For checkboxes, it's <code>checked</code> instead of <code>value</code>.
      </p>

      <div className="form-group">
        <label>
          <input
            type="checkbox"
            checked={agreed}
            onChange={(e) => {
              console.log("CHECKBOX: changed to", e.target.checked);
              setAgreed(e.target.checked);
            }}
          />
          {" "}I agree to the terms
        </label>
        <p>Agreed: <strong>{agreed ? "Yes ✓" : "No ✗"}</strong></p>
      </div>

      <div className="form-group">
        <label>Favorite color:</label>
        <select
          value={color}
          onChange={(e) => {
            console.log("SELECT: changed to", JSON.stringify(e.target.value));
            setColor(e.target.value);
          }}
        >
          <option value="red">Red</option>
          <option value="green">Green</option>
          <option value="blue">Blue</option>
          <option value="purple">Purple</option>
        </select>
        <p>
          Selected:{" "}
          <strong style={{ color: color }}>{color}</strong>
        </p>
      </div>
    </div>
  );
}
```

**File: `src/formDemo/ValidationDemo.jsx`**
```jsx
import { useState, useEffect } from "react";

// ============================================================
// Section C: Validation with useEffect
// ============================================================
// useEffect watches the email state.
// Whenever email changes, we run validation as a side effect.
// Open the browser console to follow along!
// NOTE: In development, React StrictMode runs effects twice. This is normal!

export default function ValidationDemo() {
  const [email, setEmail] = useState("");
  const [emailError, setEmailError] = useState("");

  // useEffect watches the email state.
  // Whenever email changes, we run validation as a side effect.
  useEffect(() => {
    if (email === "") {
      setEmailError("");
      console.log("VALIDATION EFFECT: email is empty, no error");
    } else if (!email.includes("@")) {
      setEmailError("Email must contain @");
      console.log("VALIDATION EFFECT: email =", JSON.stringify(email), "→ INVALID (no @)");
    } else if (!email.includes(".")) {
      setEmailError("Email must contain a domain (e.g., .com)");
      console.log("VALIDATION EFFECT: email =", JSON.stringify(email), "→ INVALID (no domain)");
    } else {
      setEmailError("");
      console.log("VALIDATION EFFECT: email =", JSON.stringify(email), "→ VALID ✓");
    }
  }, [email]); // Only re-run when email changes

  return (
    <div className="demo-subsection">
      <h3>C. Validation with useEffect</h3>
      <p className="demo-note">
        useEffect watches the email state. When it changes, validation runs as a side effect.
        Watch the console to see "VALIDATION EFFECT" fire on each change.
      </p>

      <div className="form-group">
        <label>Email:</label>
        <input
          type="email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
          placeholder="Enter your email..."
          style={{
            borderColor: emailError ? "#e74c3c" : email ? "#2ecc71" : "#ccc",
          }}
        />
        {emailError && <p className="form-error">{emailError}</p>}
        {!emailError && email && (
          <p style={{ color: "#2ecc71", fontSize: 14, marginTop: 4 }}>Looks good!</p>
        )}
      </div>

      {/* Why useEffect for validation?
          - Validation is a "side effect" of the email changing
          - We could also validate in the onChange handler, but useEffect
            makes it explicit: "whenever email changes, run this"
          - This pattern scales well: you can add more validation effects
            without cluttering the onChange handler */}
    </div>
  );
}
```

**File: `src/formDemo/FormSubmitDemo.jsx`**
```jsx
import { useState } from "react";

// ============================================================
// Section D: Form Submission
// ============================================================
// In React, "submitting" a form is just reading state.
// No document.getElementById, no FormData, no DOM traversal.
// The state IS the form data.
// Open the browser console to follow along!

export default function FormSubmitDemo() {
  const [formData, setFormData] = useState({
    username: "",
    email: "",
    role: "student",
    subscribe: false,
  });

  const [submitted, setSubmitted] = useState(false);

  console.log("FORM SUBMIT: rendering, formData =", JSON.stringify(formData));

  // A helper to update one field at a time
  function updateField(field, value) {
    console.log("FORM SUBMIT: updating", field, "to", JSON.stringify(value));
    setFormData(prev => ({ ...prev, [field]: value }));
    setSubmitted(false);
  }

  function handleSubmit(e) {
    e.preventDefault(); // Don't reload the page!
    // In React, "submitting" a form is just reading state:
    console.log("FORM SUBMIT: ============================");
    console.log("FORM SUBMIT: Form submitted with data:", formData);
    console.log("FORM SUBMIT: ============================");
    // No document.getElementById, no FormData, no DOM traversal.
    // The state IS the form data.
    setSubmitted(true);
  }

  return (
    <div className="demo-subsection">
      <h3>D. Form Submission</h3>
      <p className="demo-note">
        In React, form submission = reading state. No need for getElementById or FormData.
        Fill out the form and submit — check the console for the collected data.
      </p>

      <form onSubmit={handleSubmit}>
        <div className="form-group">
          <label>Username:</label>
          <input
            type="text"
            value={formData.username}
            onChange={(e) => updateField("username", e.target.value)}
            placeholder="Your username"
          />
        </div>

        <div className="form-group">
          <label>Email:</label>
          <input
            type="email"
            value={formData.email}
            onChange={(e) => updateField("email", e.target.value)}
            placeholder="your@email.com"
          />
        </div>

        <div className="form-group">
          <label>Role:</label>
          <select
            value={formData.role}
            onChange={(e) => updateField("role", e.target.value)}
          >
            <option value="student">Student</option>
            <option value="ta">Teaching Assistant</option>
            <option value="instructor">Instructor</option>
          </select>
        </div>

        <div className="form-group">
          <label>
            <input
              type="checkbox"
              checked={formData.subscribe}
              onChange={(e) => updateField("subscribe", e.target.checked)}
            />
            {" "}Subscribe to newsletter
          </label>
        </div>

        <button type="submit" className="btn btn-primary">
          Submit (check console)
        </button>
      </form>

      {submitted && (
        <div className="card" style={{ marginTop: 16, backgroundColor: "#eef6ff" }}>
          <strong>Submitted data (also in console):</strong>
          <pre style={{ fontSize: 14, margin: "8px 0 0" }}>
            {JSON.stringify(formData, null, 2)}
          </pre>
        </div>
      )}
    </div>
  );
}
```

**File: `src/formDemo/index.jsx`**
```jsx
import SectionStepper from "../SectionStepper";
import ControlledTextInput from "./ControlledTextInput";
import CheckboxAndSelect from "./CheckboxAndSelect";
import ValidationDemo from "./ValidationDemo";
import FormSubmitDemo from "./FormSubmitDemo";

// ============================================================
// Form Demo — Controlled Inputs and Form Handling in React
// ============================================================
// In React, form inputs are "controlled" — React state is the
// single source of truth for the input's value.
// Each section mounts one at a time so the console stays clean.

const PRACTICAL = (
  <div className="demo-practical">
    <h3>When do you use controlled forms in real apps?</h3>
    <ul>
      <li><strong>Login/signup forms</strong> — collect username, password, validate as they type</li>
      <li><strong>Search bars</strong> — filter results as the user types (debounce with useEffect)</li>
      <li><strong>Settings pages</strong> — toggle preferences, choose themes, update profiles</li>
      <li><strong>Multi-step wizards</strong> — collect data across steps, submit at the end</li>
      <li><strong>Any user input</strong> — React needs to "own" the value to keep UI and state in sync</li>
    </ul>
    <p className="demo-note">
      Rule of thumb: if React needs to know about or react to an input's value, make it controlled
      (value + onChange). The state is always the "source of truth."
    </p>
  </div>
);

const sections = [
  { label: "A. Text Input", content: <ControlledTextInput /> },
  { label: "B. Checkbox & Select", content: <CheckboxAndSelect /> },
  { label: "C. Validation", content: <ValidationDemo /> },
  { label: "D. Form Submit", content: <FormSubmitDemo /> },
  { label: "Practical Use Cases", content: PRACTICAL },
];

export default function FormDemo() {
  return (
    <div className="demo-section">
      <h2>Forms — Controlled Inputs in React</h2>
      <p className="demo-note">
        Open your browser console (F12 → Console) to see what happens behind the scenes.
        Use the section buttons below to step through each concept one at a time.
      </p>
      <SectionStepper sections={sections} />
    </div>
  );
}
```

### Update App.jsx — Add the Forms Tab

Add this import at the top of `src/App.jsx`:
```jsx
import FormDemo from "./formDemo";
```

Add to the TABS array:
```jsx
{ id: "forms", label: "Forms" },
```

Add this line in the return:
```jsx
{activeDemo === "forms" && <FormDemo />}
```

### Commit


{{< callout type="important" >}}
**Checkpoint: Commit your work**
{{< /callout >}}

```bash
git add -A
git commit -m "Add forms demo — controlled inputs, checkbox, validation, form submission"
git push
```

{{% details title="Try This (Experiments)" %}}


1. **In ControlledTextInput:** Remove the `onChange` handler. Try typing — the input is now read-only! This proves React controls the input.
2. **In ValidationDemo:** Add a third validation rule — check that the email is at least 5 characters long.
3. **In FormSubmitDemo:** Add a "phone number" field to the form. Remember to add it to the initial `formData` state object and create an input for it.
4. **In CheckboxAndSelect:** Add a radio button group for "Size" (S, M, L). Radio buttons work like text inputs: `checked={size === "M"}` and `onChange`.


{{% /details %}}

---

## Part 7: Lifecycle — When React Does What

### The Component Lifecycle

Every React component goes through three phases:

1. **Mount** — component appears on screen for the first time
2. **Update (re-render)** — component re-runs because state or props changed
3. **Unmount** — component is removed from the screen

Understanding this helps you know:
- When to fetch data (mount)
- When to clean up timers (unmount)
- Why effects run when they do (deps changed)
- Why cleanup runs before re-running an effect

### Create the Lifecycle Demo Files

Create the folder `src/lifecycle/` and add these files:

**File: `src/lifecycle/LifecycleTimeline.jsx`**
```jsx
import { useState, useEffect, useRef } from "react";

// ============================================================
// Section A: The Full Lifecycle Timeline
// ============================================================
// This component logs every phase of the React lifecycle:
//   1. Function body (render phase)
//   2. Mount effect
//   3. Update effect
//   4. Cleanup before next update
//   5. Unmount cleanup
// Open the browser console and watch the numbered logs!
// NOTE: In development, React StrictMode runs effects twice. This is normal!

export default function LifecycleTimeline() {
  const [count, setCount] = useState(0);
  const renderCountRef = useRef(0);

  renderCountRef.current += 1;

  // This runs in the FUNCTION BODY — during rendering
  console.log(`TIMELINE [render #${renderCountRef.current}]: 1️⃣ Function body runs (computing JSX). count = ${count}`);

  // Mount effect — runs ONCE after the first render
  useEffect(() => {
    console.log("TIMELINE: 2️⃣ MOUNT effect — component just appeared on screen");
    console.log("TIMELINE:    This is where you'd fetch initial data or set up subscriptions.");

    return () => {
      console.log("TIMELINE: 5️⃣ UNMOUNT cleanup — component is being removed from screen");
      console.log("TIMELINE:    This is where you'd cancel subscriptions or clear timers.");
    };
  }, []);

  // Update effect — runs when count changes
  useEffect(() => {
    console.log(`TIMELINE: 3️⃣ UPDATE effect — count changed to ${count}`);
    console.log("TIMELINE:    This is where you'd react to specific state changes.");

    return () => {
      console.log(`TIMELINE: 4️⃣ CLEANUP before next update — cleaning up for count = ${count}`);
      console.log("TIMELINE:    The OLD effect cleans up before the NEW effect runs.");
    };
  }, [count]);

  return (
    <div className="demo-subsection">
      <h3>A. The Full Lifecycle Timeline</h3>
      <p className="demo-note">
        Click the button and watch the console. The logs are numbered in the order they execute.
        Then switch to another tab to see the UNMOUNT log.
      </p>
      {console.log(`TIMELINE [render #${renderCountRef.current}]: 1b️⃣ Inside JSX return (still rendering)`)}
      <p>Count: <strong>{count}</strong> (render #{renderCountRef.current})</p>
      <button className="btn btn-primary" onClick={() => setCount(prev => prev + 1)}>
        Increment (triggers re-render)
      </button>

      <div className="demo-note" style={{ marginTop: 12 }}>
        <strong>Execution order on first render (mount):</strong><br />
        1️⃣ Function body runs → computes JSX<br />
        1b️⃣ JSX return executes (including inline console.logs)<br />
        2️⃣ React updates the DOM → browser paints<br />
        2️⃣ Mount effect runs (empty deps [])<br />
        3️⃣ Update effect runs ([count] changed from nothing to 0)<br />
        <br />
        <strong>On each subsequent click:</strong><br />
        1️⃣ Function body runs again with new count<br />
        4️⃣ Old update effect cleans up<br />
        3️⃣ New update effect runs with new count<br />
        <br />
        <strong>When component unmounts (switch tabs):</strong><br />
        4️⃣ Update effect cleanup runs<br />
        5️⃣ Mount effect cleanup runs
      </div>
    </div>
  );
}
```

**File: `src/lifecycle/ParentChildLifecycle.jsx`**
```jsx
import { useState, useEffect } from "react";

// ============================================================
// Section B: Parent-Child Render Order
// ============================================================
// Shows that parent renders BEFORE child, but effects run AFTER all children.
// Render phase (top-down): Parent body → Child A body → Child B body
// Effect phase (bottom-up): Child A effect → Child B effect → Parent effect
// Open the browser console to follow along!

function Child({ name, value }) {
  console.log(`PARENT-CHILD: 📦 ${name} function body runs (value = ${value})`);

  useEffect(() => {
    console.log(`PARENT-CHILD: ✅ ${name} effect runs (value = ${value})`);
    return () => {
      console.log(`PARENT-CHILD: 🧹 ${name} cleanup (value = ${value})`);
    };
  }, [name, value]);

  return (
    <div className="card" style={{ margin: "4px 0" }}>
      {console.log(`PARENT-CHILD: 📦 ${name} JSX rendering`)}
      <strong>{name}</strong>: value = {value}
    </div>
  );
}

export default function ParentChildLifecycle() {
  const [parentCount, setParentCount] = useState(0);

  console.log("PARENT-CHILD: 📦 Parent function body runs (parentCount =", parentCount, ")");

  useEffect(() => {
    console.log("PARENT-CHILD: ✅ Parent effect runs");
    return () => {
      console.log("PARENT-CHILD: 🧹 Parent cleanup");
    };
  }, [parentCount]);

  return (
    <div className="demo-subsection">
      <h3>B. Parent-Child Render Order</h3>
      <p className="demo-note">
        Click the button and watch the console. The render order is:
        Parent body → Child A body → Child B body → Child A effect → Child B effect → Parent effect.
        React renders top-down but runs effects bottom-up!
      </p>
      {console.log("PARENT-CHILD: 📦 Parent JSX rendering")}

      <Child name="Child A" value={parentCount} />
      <Child name="Child B" value={parentCount * 10} />

      <button
        className="btn btn-primary"
        onClick={() => setParentCount(prev => prev + 1)}
        style={{ marginTop: 8 }}
      >
        Update Parent (count: {parentCount})
      </button>

      <div className="demo-note" style={{ marginTop: 12 }}>
        <strong>Render phase (top-down):</strong> Parent body → Child A body → Child B body<br />
        <strong>Effect phase (bottom-up):</strong> Child A effect → Child B effect → Parent effect<br />
        <br />
        React finishes ALL rendering first, then runs effects from deepest child up to parent.
      </div>
    </div>
  );
}
```

**File: `src/lifecycle/ConditionalMounting.jsx`**
```jsx
import { useState, useEffect } from "react";

// ============================================================
// Section C: Conditional Rendering = Real Mount/Unmount
// ============================================================
// Shows that conditional rendering causes real mount/unmount cycles.
// This is NOT just hiding with CSS — React actually creates and destroys the component.
// Open the browser console to follow along!

function Notification({ type }) {
  useEffect(() => {
    console.log(`CONDITIONAL: 🟢 ${type} notification MOUNTED`);
    return () => {
      console.log(`CONDITIONAL: 🔴 ${type} notification UNMOUNTED`);
    };
  }, [type]);

  const colors = {
    success: { bg: "#d4edda", border: "#28a745", text: "Operation successful!" },
    warning: { bg: "#fff3cd", border: "#ffc107", text: "Please check your input." },
    error: { bg: "#f8d7da", border: "#dc3545", text: "Something went wrong!" },
  };

  const style = colors[type] || colors.success;

  return (
    <div style={{
      padding: "12px 16px",
      backgroundColor: style.bg,
      border: `2px solid ${style.border}`,
      borderRadius: 6,
      margin: "8px 0",
    }}>
      <strong>{type.toUpperCase()}:</strong> {style.text}
    </div>
  );
}

export default function ConditionalMounting() {
  const [showSuccess, setShowSuccess] = useState(false);
  const [showWarning, setShowWarning] = useState(false);
  const [showError, setShowError] = useState(false);

  return (
    <div className="demo-subsection">
      <h3>C. Conditional Rendering = Real Mount/Unmount</h3>
      <p className="demo-note">
        Toggle each notification. Watch the console — each toggle is a real mount or unmount.
        This is NOT just hiding with CSS. React actually creates and destroys the component.
      </p>

      <div style={{ display: "flex", gap: 8, marginBottom: 12 }}>
        <button
          className={`btn ${showSuccess ? "btn-primary" : "btn-secondary"}`}
          onClick={() => setShowSuccess(prev => !prev)}
        >
          {showSuccess ? "Hide" : "Show"} Success
        </button>
        <button
          className={`btn ${showWarning ? "btn-primary" : "btn-secondary"}`}
          onClick={() => setShowWarning(prev => !prev)}
        >
          {showWarning ? "Hide" : "Show"} Warning
        </button>
        <button
          className={`btn ${showError ? "btn-primary" : "btn-secondary"}`}
          onClick={() => setShowError(prev => !prev)}
        >
          {showError ? "Hide" : "Show"} Error
        </button>
      </div>

      {showSuccess && <Notification type="success" />}
      {showWarning && <Notification type="warning" />}
      {showError && <Notification type="error" />}

      {!showSuccess && !showWarning && !showError && (
        <p className="demo-note">No notifications shown. Toggle one above!</p>
      )}
    </div>
  );
}
```

**File: `src/lifecycle/EffectDepsVisualized.jsx`**
```jsx
import { useState, useEffect } from "react";

// ============================================================
// Section D: Effect Dependencies Visualized
// ============================================================
// A clear visual showing which effects run based on what changed.
// The on-screen log shows you exactly which effects fired and why.
// Open the browser console to follow along!

export default function EffectDepsVisualized() {
  const [name, setName] = useState("");
  const [age, setAge] = useState(0);
  const [effectLog, setEffectLog] = useState([]);

  function addLog(message) {
    const timestamp = new Date().toLocaleTimeString();
    setEffectLog(prev => [`[${timestamp}] ${message}`, ...prev].slice(0, 15));
  }

  // No deps — runs after EVERY render
  useEffect(() => {
    const msg = "🔄 No deps: runs after EVERY render";
    console.log("DEPS VISUALIZED:", msg);
    addLog(msg);
  });

  // Empty deps — runs ONCE on mount
  useEffect(() => {
    const msg = "📌 Empty []: runs ONCE on mount";
    console.log("DEPS VISUALIZED:", msg);
    addLog(msg);
  }, []);

  // [name] — runs when name changes
  useEffect(() => {
    const msg = `📝 [name]: name changed to "${name}"`;
    console.log("DEPS VISUALIZED:", msg);
    addLog(msg);
  }, [name]);

  // [age] — runs when age changes
  useEffect(() => {
    const msg = `🔢 [age]: age changed to ${age}`;
    console.log("DEPS VISUALIZED:", msg);
    addLog(msg);
  }, [age]);

  return (
    <div className="demo-subsection">
      <h3>D. Effect Dependencies Visualized</h3>
      <p className="demo-note">
        Change the name or age. The log below shows which effects fired and why.
        Notice: "No deps" fires on EVERY change, but "[name]" only fires when you type.
      </p>

      <div style={{ display: "flex", gap: 16, marginBottom: 12 }}>
        <div>
          <label>Name: </label>
          <input
            type="text"
            value={name}
            onChange={(e) => setName(e.target.value)}
            placeholder="Type a name..."
            style={{ padding: "4px 8px" }}
          />
        </div>
        <div>
          <label>Age: </label>
          <button className="btn btn-secondary" onClick={() => setAge(prev => prev + 1)}>
            {age} (click +1)
          </button>
        </div>
      </div>

      <div style={{
        backgroundColor: "#1e1e1e",
        color: "#d4d4d4",
        padding: 12,
        borderRadius: 6,
        fontFamily: "monospace",
        fontSize: 13,
        maxHeight: 200,
        overflow: "auto",
      }}>
        <strong style={{ color: "#569cd6" }}>Effect Log (newest first):</strong>
        {effectLog.length === 0 && <p style={{ color: "#666" }}>Interact with the inputs above...</p>}
        {effectLog.map((log, i) => (
          <div key={i} style={{ opacity: 1 - i * 0.06 }}>{log}</div>
        ))}
      </div>
    </div>
  );
}
```

**File: `src/lifecycle/index.jsx`**
```jsx
import SectionStepper from "../SectionStepper";
import LifecycleTimeline from "./LifecycleTimeline";
import ParentChildLifecycle from "./ParentChildLifecycle";
import ConditionalMounting from "./ConditionalMounting";
import EffectDepsVisualized from "./EffectDepsVisualized";

// ============================================================
// Lifecycle Demo — Understanding When React Does What
// ============================================================
// Every React component goes through a lifecycle:
//   1. MOUNT — component appears on screen for the first time
//   2. RE-RENDER — component updates because state or props changed
//   3. UNMOUNT — component is removed from the screen
// Each section mounts one at a time so the console stays clean.

const PRACTICAL = (
  <div className="demo-practical">
    <h3>Why does the lifecycle matter?</h3>
    <ul>
      <li><strong>Mount</strong> — fetch data, set up event listeners, start animations</li>
      <li><strong>Update</strong> — react to state/prop changes, sync external systems</li>
      <li><strong>Unmount</strong> — clean up timers, cancel network requests, remove event listeners</li>
      <li><strong>Parent-child order</strong> — know that children finish rendering before parent effects run</li>
      <li><strong>Conditional rendering</strong> — toggling a component is a real mount/unmount, not just CSS display:none</li>
    </ul>
    <p className="demo-note">
      Understanding the lifecycle helps you debug: "Why did my effect run twice?"
      "Why is my cleanup not firing?" "When does my data fetch happen?"
    </p>
  </div>
);

const sections = [
  { label: "A. Lifecycle Timeline", content: <LifecycleTimeline /> },
  { label: "B. Parent-Child Order", content: <ParentChildLifecycle /> },
  { label: "C. Conditional Mount", content: <ConditionalMounting /> },
  { label: "D. Deps Visualized", content: <EffectDepsVisualized /> },
  { label: "Practical Use Cases", content: PRACTICAL },
];

export default function LifecycleDemo() {
  return (
    <div className="demo-section">
      <h2>Lifecycle — When React Does What</h2>
      <p className="demo-note">
        Open your browser console (F12 → Console) to see the full lifecycle logs.
        Use the section buttons below to step through each concept one at a time.
      </p>
      <SectionStepper sections={sections} />
    </div>
  );
}
```

### Update App.jsx — Add the Lifecycle Tab

Add this import at the top of `src/App.jsx`:
```jsx
import LifecycleDemo from "./lifecycle";
```

Add to the TABS array:
```jsx
{ id: "lifecycle", label: "Lifecycle" },
```

Add this line in the return:
```jsx
{activeDemo === "lifecycle" && <LifecycleDemo />}
```

### Commit


{{< callout type="important" >}}
**Checkpoint: Commit your work**
{{< /callout >}}

```bash
git add -A
git commit -m "Add lifecycle demo — timeline, parent-child order, conditional mounting, deps visualized"
git push
```

{{% details title="Try This (Experiments)" %}}


1. **In LifecycleTimeline:** Click "Increment" twice, then switch to a different tab. Read the console logs from top to bottom — trace the exact execution order.
2. **In ParentChildLifecycle:** Add a `<Child name="Child C" value={parentCount * 100} />`. Verify the render order: Parent → A → B → C → A effect → B effect → C effect → Parent effect.
3. **In ConditionalMounting:** Show all three notifications, then hide them one by one. Count the MOUNTED and UNMOUNTED logs.
4. **In EffectDepsVisualized:** Type in the Name field. Notice that `[age]` does NOT fire — only `[name]` and `No deps` do. Then click Age and see which effects fire.


{{% /details %}}

---

## Part 8: Window & Browser APIs

### The Pattern: Subscribe in useEffect, Cleanup on Unmount

Whenever you connect React to a browser API (resize, mouse, online/offline, timers), the pattern is always the same:

```jsx
useEffect(() => {
  // 1. Subscribe to the event
  window.addEventListener("resize", handler);

  // 2. Return a cleanup function that unsubscribes
  return () => window.removeEventListener("resize", handler);
}, []); // 3. Usually on mount only
```

{{< callout type="info" >}}
The pattern is always: subscribe in useEffect, update state in the handler, unsubscribe in the cleanup function. Once you learn this pattern, you can connect React to anything.
{{< /callout >}}

### Create the Window Demo Files

Create the folder `src/window/` and add these files:

**File: `src/window/WindowWidthLive.jsx`**
```jsx
import { useState, useEffect } from "react";

// ============================================================
// Section A: Live Window Width
// ============================================================
// Listens to the browser's resize event and updates React state.
// This shows the subscribe/cleanup pattern for browser events.
// Open the browser console to follow along!

export default function WindowWidthLive() {
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    console.log("WINDOW WIDTH: 📏 subscribing to resize event");

    function handleResize() {
      const newWidth = window.innerWidth;
      console.log("WINDOW WIDTH: 📏 resized to", newWidth, "px");
      setWidth(newWidth);
    }

    window.addEventListener("resize", handleResize);

    return () => {
      console.log("WINDOW WIDTH: 📏 unsubscribing from resize event");
      window.removeEventListener("resize", handleResize);
    };
  }, []);

  return (
    <div className="demo-subsection">
      <h3>A. Live Window Width</h3>
      <p className="demo-note">
        Resize your browser window. The value below updates in real-time!
        Check the console to see the event listener subscribe/unsubscribe.
      </p>
      <p style={{ fontSize: 28, fontWeight: "bold", fontFamily: "monospace" }}>
        Window width: {width}px
      </p>
      <p>
        Screen size:{" "}
        <strong>
          {width < 600 ? "📱 Mobile" : width < 1024 ? "📟 Tablet" : "🖥️ Desktop"}
        </strong>
      </p>
    </div>
  );
}
```

**File: `src/window/DelayedMessageDemo.jsx`**
```jsx
import { useState, useEffect } from "react";

// ============================================================
// Section B: Delayed Message (Timeout with Cleanup)
// ============================================================
// Shows a message after a delay, with proper cleanup if unmounted early.
// Open the browser console to follow along!
// NOTE: In development, React StrictMode runs effects twice. This is normal!

function DelayedMessage({ message, delay }) {
  const [show, setShow] = useState(false);
  const [timeLeft, setTimeLeft] = useState(Math.ceil(delay / 1000));

  // Reset state when props change, before the effect runs
  const [prevMessage, setPrevMessage] = useState(message);
  const [prevDelay, setPrevDelay] = useState(delay);
  if (message !== prevMessage || delay !== prevDelay) {
    setPrevMessage(message);
    setPrevDelay(delay);
    setShow(false);
    setTimeLeft(Math.ceil(delay / 1000));
  }

  useEffect(() => {
    console.log(`DELAYED MSG: ⏳ starting ${delay}ms timer for "${message}"`);

    const countdownId = setInterval(() => {
      setTimeLeft(prev => {
        if (prev <= 1) {
          clearInterval(countdownId);
          return 0;
        }
        return prev - 1;
      });
    }, 1000);

    const timerId = setTimeout(() => {
      console.log(`DELAYED MSG: ✅ timer fired! Showing "${message}"`);
      setShow(true);
    }, delay);

    return () => {
      console.log(`DELAYED MSG: 🧹 cleaning up timers (component unmounted or props changed)`);
      clearTimeout(timerId);
      clearInterval(countdownId);
    };
  }, [message, delay]);

  return (
    <div>
      {show ? (
        <p style={{ fontSize: 20, color: "#2ecc71", fontWeight: "bold" }}>{message}</p>
      ) : (
        <p style={{ fontSize: 20, color: "#888" }}>
          Waiting... ({timeLeft}s remaining)
        </p>
      )}
    </div>
  );
}

export default function DelayedMessageDemo() {
  const [delay, setDelay] = useState(3000);
  const [message, setMessage] = useState("Hello from the future!");
  const [mounted, setMounted] = useState(true);

  return (
    <div className="demo-subsection">
      <h3>B. Delayed Message (Timeout with Cleanup)</h3>
      <p className="demo-note">
        The message appears after a delay. Change the delay or unmount early to see cleanup in action.
      </p>

      <div style={{ display: "flex", gap: 12, marginBottom: 12, flexWrap: "wrap" }}>
        <div>
          <label>Message: </label>
          <input
            type="text"
            value={message}
            onChange={(e) => setMessage(e.target.value)}
            style={{ padding: "4px 8px" }}
          />
        </div>
        <div>
          <label>Delay: </label>
          <select value={delay} onChange={(e) => setDelay(Number(e.target.value))} style={{ padding: "4px 8px" }}>
            <option value={1000}>1 second</option>
            <option value={3000}>3 seconds</option>
            <option value={5000}>5 seconds</option>
          </select>
        </div>
        <button
          className={`btn ${mounted ? "btn-danger" : "btn-primary"}`}
          onClick={() => setMounted(prev => !prev)}
        >
          {mounted ? "Unmount (cancel early)" : "Mount (start timer)"}
        </button>
      </div>

      {mounted && <DelayedMessage message={message} delay={delay} />}
      {!mounted && <p className="demo-note">Component unmounted — timer was cleaned up. Check the console!</p>}
    </div>
  );
}
```

**File: `src/window/MouseTracker.jsx`**
```jsx
import { useState, useEffect } from "react";

// ============================================================
// Section C: Mouse Position Tracker
// ============================================================
// Tracks the mouse position across the entire document.
// Demonstrates toggling a subscription on/off with useEffect.
// Open the browser console to follow along!

export default function MouseTracker() {
  const [position, setPosition] = useState({ x: 0, y: 0 });
  const [tracking, setTracking] = useState(false);

  useEffect(() => {
    if (!tracking) return;

    console.log("MOUSE TRACKER: 🖱️ subscribing to mousemove");

    function handleMouseMove(e) {
      setPosition({ x: e.clientX, y: e.clientY });
    }

    window.addEventListener("mousemove", handleMouseMove);

    return () => {
      console.log("MOUSE TRACKER: 🖱️ unsubscribing from mousemove");
      window.removeEventListener("mousemove", handleMouseMove);
    };
  }, [tracking]);

  return (
    <div className="demo-subsection">
      <h3>C. Mouse Position Tracker</h3>
      <p className="demo-note">
        Toggle tracking on, then move your mouse. The position updates in real-time.
        Toggle off to unsubscribe — check the console for subscribe/unsubscribe logs.
      </p>

      <button
        className={`btn ${tracking ? "btn-danger" : "btn-primary"}`}
        onClick={() => setTracking(prev => !prev)}
        style={{ marginBottom: 12 }}
      >
        {tracking ? "Stop Tracking" : "Start Tracking"}
      </button>

      <div style={{
        fontFamily: "monospace",
        fontSize: 24,
        fontWeight: "bold",
        padding: 16,
        backgroundColor: tracking ? "#eef6ff" : "#f5f5f5",
        borderRadius: 8,
        border: `2px solid ${tracking ? "#3498db" : "#ddd"}`,
        transition: "all 0.3s",
      }}>
        x: {position.x}, y: {position.y}
      </div>

      {tracking && (
        <p className="demo-note" style={{ marginTop: 8 }}>
          Move your mouse anywhere on the page!
        </p>
      )}
    </div>
  );
}
```

**File: `src/window/OnlineStatus.jsx`**
```jsx
import { useState, useEffect } from "react";

// ============================================================
// Section D: Online/Offline Status
// ============================================================
// Detects if the user's browser goes offline or comes back online.
// To test: open DevTools → Network tab → toggle "Offline" checkbox.
// Open the browser console to follow along!

export default function OnlineStatus() {
  const [isOnline, setIsOnline] = useState(navigator.onLine);
  const [history, setHistory] = useState([]);

  useEffect(() => {
    console.log("ONLINE STATUS: 🌐 subscribing to online/offline events");

    function handleOnline() {
      console.log("ONLINE STATUS: 🟢 back online!");
      setIsOnline(true);
      setHistory(prev => [...prev, `🟢 Online at ${new Date().toLocaleTimeString()}`].slice(-5));
    }

    function handleOffline() {
      console.log("ONLINE STATUS: 🔴 went offline!");
      setIsOnline(false);
      setHistory(prev => [...prev, `🔴 Offline at ${new Date().toLocaleTimeString()}`].slice(-5));
    }

    window.addEventListener("online", handleOnline);
    window.addEventListener("offline", handleOffline);

    return () => {
      console.log("ONLINE STATUS: 🌐 unsubscribing from online/offline events");
      window.removeEventListener("online", handleOnline);
      window.removeEventListener("offline", handleOffline);
    };
  }, []);

  return (
    <div className="demo-subsection">
      <h3>D. Online/Offline Status</h3>
      <p className="demo-note">
        This detects your real network status. To test: open DevTools → Network tab → toggle "Offline" checkbox.
      </p>

      <div style={{
        padding: 16,
        borderRadius: 8,
        backgroundColor: isOnline ? "#d4edda" : "#f8d7da",
        border: `2px solid ${isOnline ? "#28a745" : "#dc3545"}`,
        fontSize: 20,
        fontWeight: "bold",
        marginBottom: 8,
      }}>
        {isOnline ? "🟢 Online" : "🔴 Offline"}
      </div>

      {history.length > 0 && (
        <div style={{ fontFamily: "monospace", fontSize: 13 }}>
          <strong>Status history:</strong>
          {history.map((entry, i) => (
            <div key={i}>{entry}</div>
          ))}
        </div>
      )}
    </div>
  );
}
```

**File: `src/window/DocumentTitleSync.jsx`**
```jsx
import { useState, useEffect, useRef } from "react";

// ============================================================
// Section E: Document Title Sync
// ============================================================
// A simple but common use case: updating the browser tab title.
// Open the browser console to follow along!

export default function DocumentTitleSync() {
  const [notifications, setNotifications] = useState(0);
  const originalTitle = useRef(document.title);

  useEffect(() => {
    const savedTitle = originalTitle.current;

    if (notifications > 0) {
      document.title = `(${notifications}) New notifications`;
      console.log("DOC TITLE: 📋 updated to", document.title);
    } else {
      document.title = savedTitle;
      console.log("DOC TITLE: 📋 reset to original");
    }

    return () => {
      document.title = savedTitle;
    };
  }, [notifications]);

  return (
    <div className="demo-subsection">
      <h3>E. Document Title Sync</h3>
      <p className="demo-note">
        Click to add notifications. Watch your browser tab title change!
        This is a common useEffect pattern for keeping external state in sync.
      </p>

      <p style={{ fontSize: 20, fontWeight: "bold" }}>
        Notifications: {notifications}
      </p>

      <div style={{ display: "flex", gap: 8 }}>
        <button className="btn btn-primary" onClick={() => setNotifications(prev => prev + 1)}>
          Add Notification
        </button>
        <button className="btn btn-secondary" onClick={() => setNotifications(prev => Math.max(0, prev - 1))}>
          Dismiss One
        </button>
        <button className="btn btn-danger" onClick={() => setNotifications(0)}>
          Clear All
        </button>
      </div>
    </div>
  );
}
```

**File: `src/window/index.jsx`**
```jsx
import SectionStepper from "../SectionStepper";
import WindowWidthLive from "./WindowWidthLive";
import DelayedMessageDemo from "./DelayedMessageDemo";
import MouseTracker from "./MouseTracker";
import OnlineStatus from "./OnlineStatus";
import DocumentTitleSync from "./DocumentTitleSync";

// ============================================================
// Window & Browser APIs Demo — React + the Outside World
// ============================================================
// React manages the UI, but sometimes you need to interact with
// browser APIs: window size, timers, scroll position, online status, etc.
// useEffect is the bridge between React and the browser.
// Each section mounts one at a time so the console stays clean.

const PRACTICAL = (
  <div className="demo-practical">
    <h3>When do you connect React to browser APIs?</h3>
    <ul>
      <li><strong>Responsive layouts</strong> — listen to resize, adjust UI based on screen size</li>
      <li><strong>Timers & countdowns</strong> — setTimeout/setInterval with proper cleanup</li>
      <li><strong>User input tracking</strong> — mouse position, scroll depth, keyboard shortcuts</li>
      <li><strong>Network status</strong> — show offline banners, retry failed requests</li>
      <li><strong>Document title</strong> — unread count, page name, notification badges</li>
      <li><strong>LocalStorage sync</strong> — persist user preferences across page reloads</li>
      <li><strong>Geolocation, clipboard, notifications</strong> — any browser API via useEffect</li>
    </ul>
    <p className="demo-note">
      The pattern is always the same: subscribe in useEffect, update state in the handler,
      unsubscribe in the cleanup function. Once you learn this pattern, you can connect React to anything.
    </p>
  </div>
);

const sections = [
  { label: "A. Window Width", content: <WindowWidthLive /> },
  { label: "B. Delayed Message", content: <DelayedMessageDemo /> },
  { label: "C. Mouse Tracker", content: <MouseTracker /> },
  { label: "D. Online Status", content: <OnlineStatus /> },
  { label: "E. Doc Title", content: <DocumentTitleSync /> },
  { label: "Practical Use Cases", content: PRACTICAL },
];

export default function WindowDemo() {
  return (
    <div className="demo-section">
      <h2>Window & Browser APIs — React Meets the Real World</h2>
      <p className="demo-note">
        Open your browser console (F12 → Console) to see subscribe/unsubscribe logs.
        Use the section buttons below to step through each concept one at a time.
      </p>
      <SectionStepper sections={sections} />
    </div>
  );
}
```

### Update App.jsx — Add the Window Tab

Add this import at the top of `src/App.jsx`:
```jsx
import WindowDemo from "./window";
```

Add to the TABS array:
```jsx
{ id: "window", label: "Window" },
```

Add this line in the return:
```jsx
{activeDemo === "window" && <WindowDemo />}
```

### Commit


{{< callout type="important" >}}
**Checkpoint: Commit your work**
{{< /callout >}}

```bash
git add -A
git commit -m "Add window/browser APIs demo — resize, timers, mouse, online status, doc title"
git push
```

{{% details title="Try This (Experiments)" %}}


1. **In WindowWidthLive:** Add a height tracker alongside the width. Use `window.innerHeight`.
2. **In MouseTracker:** Display a small colored dot at the mouse position using absolute positioning.
3. **In OnlineStatus:** Open DevTools → Network → check "Offline". Watch the status change. Uncheck to go back online.
4. **In DocumentTitleSync:** Look at your browser tab — the title changes with the notification count!
5. **Challenge:** Create a `ScrollTracker.jsx` that shows how far down the page the user has scrolled (use `window.scrollY` and `window.addEventListener("scroll", ...)`).


{{% /details %}}

---

## Part 9: Home Page with Reusable Components

### Create Simple Reusable Components

These demonstrate basic React component patterns: props, children, and composition.

**File: `src/button.jsx`**
```jsx
export default function Button({ variant, onClick, children }) {
  return (
    <button
      className={`btn btn-${variant}`}
      onClick={onClick}
    >
      {children}
    </button>
  );
}
```

**File: `src/dangerButton.jsx`**
```jsx
export default function DangerButton({ onClick, children }) {
  return (
    <button
      className="btn btn-danger"
      onClick={onClick}
    >
      {children}
    </button>
  );
}
```

**File: `src/layout.jsx`**
```jsx
export default function Layout({ header, children, footer }) {
  return (
    <div className="panel">
      <div className="panel-header">{header}</div>
      <div className="panel-body">{children}</div>
      <div className="panel-footer">{footer}</div>
    </div>
  );
}
```

### Final App.jsx — All Tabs

Replace `src/App.jsx` with the complete final version:

**File: `src/App.jsx`**
```jsx
import { useState } from "react";
import Button from "./button";
import DangerButton from "./dangerButton";
import Layout from "./layout";
import LifecycleDemo from "./lifecycle";
import WindowDemo from "./window";
import ReactVsJs from "./reactVsJs";
import UseStateDemo from "./useStateDemo";
import UseEffectDemo from "./useEffectDemo";
import UseRefDemo from "./useRefDemo";
import CompositionDemo from "./compositionDemo";
import FormDemo from "./formDemo";

// Navigation tabs in teaching order (left to right)
const TABS = [
  { id: "reactVsJs", label: "React vs JS" },
  { id: "useState", label: "useState" },
  { id: "useEffect", label: "useEffect" },
  { id: "useRef", label: "useRef" },
  { id: "composition", label: "Composition" },
  { id: "forms", label: "Forms" },
  { id: "lifecycle", label: "Lifecycle" },
  { id: "window", label: "Window" },
  { id: "home", label: "Home (Original)" },
];

function App() {
  // Start on React vs JS — the first concept to teach
  const [activeDemo, setActiveDemo] = useState("reactVsJs");

  return (
    <div>
      {/* Navigation bar */}
      <h1>CS300 React Demos</h1>
      <nav className="demo-nav">
        {TABS.map((tab) => (
          <button
            key={tab.id}
            className={`btn ${activeDemo === tab.id ? "btn-active" : "btn-secondary"}`}
            onClick={() => {
              console.log("NAV: switching to", tab.id, "(previous component will unmount)");
              setActiveDemo(tab.id);
            }}
          >
            {tab.label}
          </button>
        ))}
      </nav>

      {/* Conditional rendering — switching tabs unmounts/mounts components.
          Watch the console for cleanup effects! */}
      {activeDemo === "reactVsJs" && <ReactVsJs />}
      {activeDemo === "useState" && <UseStateDemo />}
      {activeDemo === "useEffect" && <UseEffectDemo />}
      {activeDemo === "useRef" && <UseRefDemo />}
      {activeDemo === "composition" && <CompositionDemo />}
      {activeDemo === "forms" && <FormDemo />}
      {activeDemo === "lifecycle" && <LifecycleDemo />}
      {activeDemo === "window" && <WindowDemo />}
      {activeDemo === "home" && (
        <Layout header={<h1>Header</h1>} footer={<h1>Footer</h1>}>
          <p>This is the original home page body.</p>
          <Button variant="primary" onClick={() => alert("Primary button clicked!")}>
            Primary Button
          </Button>
          <Button variant="secondary" onClick={() => alert("Secondary button clicked!")}>
            Secondary Button
          </Button>
          <DangerButton onClick={() => alert("Danger button clicked!")}>
            Danger Button
          </DangerButton>
        </Layout>
      )}
    </div>
  );
}

export default App;
```

### Final Commit


{{< callout type="important" >}}
**Checkpoint: Commit your work**
{{< /callout >}}

```bash
git add -A
git commit -m "Add home page with reusable Button, DangerButton, Layout components"
git push
```

---

## Quick Reference Cheat Sheet

{{< tabs >}}
{{< tab name="useState" >}}

```jsx
const [value, setValue] = useState(initialValue);
// setValue(newValue)            — set directly
// setValue(prev => prev + 1)   — updater function (use when based on previous)
```

{{< /tab >}}
{{< tab name="useEffect" >}}

```jsx
useEffect(() => {
  // runs after render
  return () => { /* cleanup */ };
}, [deps]);
// No array    → every render
// []          → mount only
// [x]         → when x changes
```

{{< /tab >}}
{{< tab name="useRef" >}}

```jsx
const ref = useRef(initialValue);
// ref.current — the stored value (persists, no re-render on change)
// <input ref={ref} /> — access DOM node via ref.current
```

{{< /tab >}}
{{< tab name="Component Pattern" >}}

```jsx
function MyComponent({ prop1, prop2, children }) {
  return <div>{children}</div>;
}
// <MyComponent prop1="a">content here</MyComponent>
```

{{< /tab >}}
{{< tab name="Controlled Input" >}}

```jsx
const [value, setValue] = useState("");
<input value={value} onChange={(e) => setValue(e.target.value)} />
```

{{< /tab >}}
{{< tab name="Event Listener" >}}

```jsx
useEffect(() => {
  const handler = (e) => setState(e.someValue);
  window.addEventListener("eventname", handler);
  return () => window.removeEventListener("eventname", handler);
}, []);
```

{{< /tab >}}
{{< tab name="Rules of Hooks" >}}

1. Only call hooks at the **top level** (not inside loops, conditions, or nested functions)
2. Only call hooks from **React function components** or custom hooks
3. Hooks are identified by **call order** — changing the order breaks everything

{{< /tab >}}
{{< tab name="Mental Models" >}}

- **State** = data that changes over time and affects what the user sees
- **Declarative** = describe WHAT the UI should look like, not HOW to get there
- **Re-rendering** = React re-calls your function and diffs the old/new JSX (this is fast!)
- **One-way data flow** = data flows down (parent → child), events flow up (child → parent via callbacks)
- **Lifting state** = move shared state to the nearest common parent
- **Cleanup** = always clean up timers, subscriptions, and event listeners to prevent memory leaks

{{< /tab >}}
{{< /tabs >}}