---
title: "Introduction to React"
weight: 1
toc: true
---

## Introduction

So far, you've built interactive web pages by manually selecting DOM elements, attaching event listeners, and updating content with `innerHTML` or `textContent`. This works — but as your apps grow, it becomes harder to manage. What if you need the same card layout on five pages? What if changing one piece of data requires updating three different elements? What if your `script.js` file grows to 500 lines of `querySelector` calls?

**React** solves these problems. It's a JavaScript library that lets you build user interfaces from small, reusable pieces called **components**. Instead of imperatively telling the browser *how* to update the page, you describe *what* the page should look like, and React handles the rest.

### What You'll Learn

- Why React exists and what problems it solves
- How to set up a React project with Vite
- How to write JSX — HTML-like syntax inside JavaScript
- How to create and compose React components
- How React projects are structured

---

## Why React?

### The Problem with Vanilla JavaScript

Consider building a profile card in vanilla JavaScript:

```javascript
// Vanilla JS — building a card by hand
function createCard(name, role) {
  const card = document.createElement("div");
  card.classList.add("card");

  const h2 = document.createElement("h2");
  h2.textContent = name;

  const p = document.createElement("p");
  p.textContent = role;

  card.appendChild(h2);
  card.appendChild(p);

  return card;
}

// Use it
const container = document.querySelector("#cards");
container.appendChild(createCard("Alice", "Developer"));
container.appendChild(createCard("Bob", "Designer"));
```

This works, but notice the problems:

- **Verbose:** 12 lines just to create a simple card
- **Hard to read:** The structure of the card is buried in `createElement` and `appendChild` calls
- **No reusability:** If you want this card on another page, you copy-paste the entire function
- **Difficult to maintain:** Change the card structure and you need to update every `createElement` call

### The React Solution

Here's the same card in React:

```jsx
function Card({ name, role }) {
  return (
    <div className="card">
      <h2>{name}</h2>
      <p>{role}</p>
    </div>
  );
}

// Use it — looks just like HTML!
<Card name="Alice" role="Developer" />
<Card name="Bob" role="Designer" />
```

The React version is:

- **Readable:** The structure looks like HTML
- **Reusable:** Use `<Card />` anywhere, as many times as you want
- **Maintainable:** Change the card in one place, it updates everywhere

### What is React?

- A **JavaScript library** for building user interfaces, created by Meta (Facebook) in 2013
- The **most popular** front-end library — used by Meta, Netflix, Airbnb, Discord, and thousands more
- Built on a simple idea: **components** — small, self-contained pieces of UI that you compose together

{{< callout type="info" >}}
React doesn't replace what you've learned. JavaScript, DOM concepts, events, and fetch are all still there underneath. React organizes them into a cleaner, more scalable pattern.
{{< /callout >}}

### Components = Building Blocks

Think of components like Lego blocks. Each component handles one piece of the UI:

```
Your App
├── Header          ← Navigation, logo
├── Main
│   ├── SearchBar   ← Input field, search button
│   ├── Card        ← Single result
│   ├── Card        ← Another result
│   └── Card        ← Another result
└── Footer          ← Copyright, links
```

You build small components and compose them into bigger ones. The `App` component contains everything, just like your `<body>` in HTML contains all your page content.

---

## Setting Up React with Vite

### What is Vite?

**Vite** (French for "fast") is a modern build tool that:

- Creates a ready-to-use React project in seconds
- Compiles JSX into regular JavaScript that browsers understand
- Provides **hot module replacement** — your browser updates instantly when you save a file
- Replaces the older Create React App (CRA), which is no longer recommended

### Creating Your First Project

{{% steps %}}

### Open your terminal

Open a terminal in VS Code (`` Ctrl+` `` or `Terminal → New Terminal`).

### Create the project

```bash
npm create vite@latest my-first-react -- --template react
```

This downloads the Vite project creator and sets up a React project in a folder called `my-first-react`.

### Navigate into the project folder

```bash
cd my-first-react
```

### Install dependencies

```bash
npm install
```

This downloads React and other packages into the `node_modules/` folder.

### Start the development server

```bash
npm run dev
```

Open the URL shown in the terminal (usually `http://localhost:5173`) in your browser. You should see the default Vite + React welcome page.

{{% /steps %}}

{{< callout type="warning" >}}
**Node.js required!** You need Node.js installed to use Vite. Run `node -v` in your terminal to check. If you don't have it, download it from [nodejs.org](https://nodejs.org/).
{{< /callout >}}

### Project Structure

After setup, your project looks like this:

{{< filetree/container >}}
  {{< filetree/folder name="my-first-react" >}}
    {{< filetree/folder name="node_modules" >}}
      {{< filetree/file name="(dependencies — don't edit)" >}}
    {{< /filetree/folder >}}
    {{< filetree/folder name="public" >}}
      {{< filetree/file name="vite.svg" >}}
    {{< /filetree/folder >}}
    {{< filetree/folder name="src" >}}
      {{< filetree/file name="App.jsx" >}}
      {{< filetree/file name="App.css" >}}
      {{< filetree/file name="main.jsx" >}}
      {{< filetree/file name="index.css" >}}
    {{< /filetree/folder >}}
    {{< filetree/file name="index.html" >}}
    {{< filetree/file name="package.json" >}}
    {{< filetree/file name="vite.config.js" >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}}

**Key files:**

| File | Purpose |
|------|---------|
| `src/App.jsx` | Your main component — **start here** |
| `src/main.jsx` | Entry point — renders `App` into the page |
| `src/index.css` | Global styles |
| `index.html` | Root HTML file with `<div id="root">` |
| `package.json` | Project configuration and dependencies |

### How It All Connects

```
index.html          →  Has <div id="root"></div>
    ↓
src/main.jsx        →  Finds #root and renders <App /> into it
    ↓
src/App.jsx         →  Your main component (this is where you work)
```

Here's what `main.jsx` looks like:

```jsx
import ReactDOM from 'react-dom/client'
import App from './App.jsx'
import './index.css'

ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```

You rarely need to edit `main.jsx`. Almost all your work happens in `App.jsx` and the components you create.

---

## Understanding JSX

### What is JSX?

**JSX** (JavaScript XML) is a syntax extension that lets you write HTML-like code inside JavaScript. It's not HTML — it's a special syntax that Vite compiles into JavaScript function calls.

```jsx
// This JSX:
<h1>Hello, World!</h1>

// Gets compiled to this JavaScript:
React.createElement('h1', null, 'Hello, World!')
```

You never need to write `React.createElement` yourself — JSX handles it for you.

### JSX Expressions with Curly Braces

Use curly braces `{}` to embed any JavaScript **expression** inside JSX:

```jsx
function App() {
  const name = "CS300";
  const year = 2026;

  return (
    <div>
      <h1>Welcome to {name}</h1>
      <p>Year: {year}</p>
      <p>Sum: {2 + 2}</p>
      <p>Uppercase: {name.toUpperCase()}</p>
      <p>Random: {Math.floor(Math.random() * 100)}</p>
    </div>
  );
}
```

{{< callout type="info" >}}
**Mental model:** If you're comfortable with template literals (`` `Hello ${name}` ``), JSX is the same idea. The syntax changes from `${}` to `{}`, but the concept is identical — embedding JavaScript inside markup.
{{< /callout >}}

### JSX vs HTML — Key Differences

JSX looks like HTML but has some important differences:

| HTML | JSX | Why |
|------|-----|-----|
| `class="card"` | `className="card"` | `class` is a reserved keyword in JavaScript |
| `for="email"` | `htmlFor="email"` | `for` is a reserved keyword in JavaScript |
| `<br>` | `<br />` | All tags must be explicitly closed |
| `<img src="...">` | `<img src="..." />` | Self-closing tags need the `/` |
| `style="color: red"` | `style={{ color: 'red' }}` | Styles are JavaScript objects |
| `onclick="..."` | `onClick={...}` | Event handlers use camelCase |

The two most common ones to remember: **`className`** instead of `class`, and **close all tags**.

### The One Root Rule

A component must return **one** root element. You can't return two sibling elements:

{{< tabs items="Error,Fix with div,Fix with Fragment" >}}
  {{< tab >}}
```jsx
// ❌ ERROR — two root elements
function App() {
  return (
    <h1>Title</h1>
    <p>Content</p>
  );
}
```
  {{< /tab >}}
  {{< tab >}}
```jsx
// ✅ Wrap in a div
function App() {
  return (
    <div>
      <h1>Title</h1>
      <p>Content</p>
    </div>
  );
}
```
  {{< /tab >}}
  {{< tab >}}
```jsx
// ✅ Use a Fragment (no extra DOM node)
function App() {
  return (
    <>
      <h1>Title</h1>
      <p>Content</p>
    </>
  );
}
```
  {{< /tab >}}
{{< /tabs >}}

**Fragments** (`<>...</>`) let you group elements without adding an extra `<div>` to the DOM. Use them when you don't need a wrapper element for styling.

---

## Creating Components

### What is a Component?

A React component is a **JavaScript function** that returns JSX. Three rules:

1. **PascalCase name** — `Greeting`, not `greeting` (React treats lowercase as HTML elements)
2. **Returns JSX** — the function's return value is what appears on the page
3. **Exported** — so other files can import and use it

### Your First Component

```jsx
// Greeting.jsx
function Greeting() {
  return <h2>Hello, welcome to our site!</h2>;
}

export default Greeting;
```

### Using a Component

Import it in another file and use it like an HTML tag:

```jsx
// App.jsx
import Greeting from './Greeting';

function App() {
  return (
    <div>
      <Greeting />
      <Greeting />
      <Greeting />
    </div>
  );
}

export default App;
```

Three `<Greeting />` tags = three greetings on the page. Write the component once, use it as many times as you want.

{{< callout type="important" >}}
**PascalCase is required.** React uses the casing to distinguish between your components and regular HTML elements. `<Greeting />` renders your component. `<greeting />` would look for an HTML element called `greeting` (which doesn't exist).
{{< /callout >}}

### Component with Variables

Components can contain variables and logic, just like any JavaScript function:

```jsx
// ProfileCard.jsx
function ProfileCard() {
  const name = "Jane Smith";
  const role = "Web Developer";
  const bio = "I love building things for the web!";
  const yearStarted = 2020;
  const experience = 2026 - yearStarted;

  return (
    <div className="profile-card">
      <h2>{name}</h2>
      <p className="role">{role}</p>
      <p>{bio}</p>
      <p>{experience} years of experience</p>
    </div>
  );
}

export default ProfileCard;
```

---

## Composing Components

The real power of React is **composition** — building complex UIs from simple components.

### Building an App from Components

```jsx
// Header.jsx
function Header() {
  return (
    <header>
      <h1>My React App</h1>
      <nav>
        <a href="#home">Home</a>
        <a href="#about">About</a>
        <a href="#contact">Contact</a>
      </nav>
    </header>
  );
}

export default Header;
```

```jsx
// Footer.jsx
function Footer() {
  return (
    <footer>
      <p>&copy; 2026 CS300 Class</p>
    </footer>
  );
}

export default Footer;
```

```jsx
// App.jsx — composing everything together
import Header from './Header';
import Footer from './Footer';
import ProfileCard from './ProfileCard';

function App() {
  return (
    <div>
      <Header />
      <main>
        <h2>Team Members</h2>
        <ProfileCard />
        <ProfileCard />
        <ProfileCard />
      </main>
      <Footer />
    </div>
  );
}

export default App;
```

### The Component Tree

Every React app forms a tree of components:

```
App
├── Header
│   └── nav (HTML)
├── main (HTML)
│   ├── ProfileCard
│   ├── ProfileCard
│   └── ProfileCard
└── Footer
```

`App` is the **root component** — it contains everything. Each component manages its own piece of the UI.

### Benefits of Composition

- **Reusability:** Use `<ProfileCard />` on any page
- **Isolation:** Changing `Header` doesn't affect `Footer`
- **Readability:** `App.jsx` reads like a table of contents
- **Teamwork:** Different developers can work on different components

---

## Common Patterns

### Cleaning Up the Default Vite Project

When you create a new Vite project, it comes with demo content. Here's how to start fresh:

{{% steps %}}

### Clear App.jsx

Replace the default content with a simple component:

```jsx
function App() {
  return (
    <div>
      <h1>Hello React!</h1>
    </div>
  );
}

export default App;
```

### Clear App.css

Delete all the default styles or replace with your own.

### Clear index.css

Keep only basic resets if desired:

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: system-ui, sans-serif;
  line-height: 1.6;
  padding: 20px;
}
```

### Delete unused files

Remove `src/assets/react.svg` and any other demo files you don't need.

{{% /steps %}}

### Creating a New Component

Follow this pattern every time you create a component:

```jsx
// 1. Create a new file with PascalCase name: ComponentName.jsx

// 2. Write the function
function ComponentName() {
  return (
    <div>
      {/* Your JSX here */}
    </div>
  );
}

// 3. Export it
export default ComponentName;

// 4. Import it where you need it:
// import ComponentName from './ComponentName';
```

### Adding CSS to a Component

Create a CSS file with the same name and import it:

```css
/* ProfileCard.css */
.profile-card {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 20px;
  max-width: 300px;
}

.profile-card h2 {
  margin-bottom: 8px;
  color: #333;
}

.profile-card .role {
  color: #666;
  font-style: italic;
}
```

```jsx
// ProfileCard.jsx
import './ProfileCard.css';

function ProfileCard() {
  return (
    <div className="profile-card">
      <h2>Jane Smith</h2>
      <p className="role">Web Developer</p>
      <p>I love building things for the web!</p>
    </div>
  );
}

export default ProfileCard;
```

---

## Troubleshooting & Common Mistakes

{{% details title="I get 'React is not defined' or JSX errors" %}}
Make sure your file has the `.jsx` extension (not `.js`). Vite uses the extension to know it should process JSX. If you're using an older setup, you may need to `import React from 'react'` at the top.
{{% /details %}}

{{% details title="My component doesn't show up" %}}
Check these things in order:

1. **Did you export it?** The component file needs `export default ComponentName;`
2. **Did you import it?** The file using it needs `import ComponentName from './ComponentName';`
3. **Did you use it in JSX?** Add `<ComponentName />` in your return statement
4. **Is it PascalCase?** `<greeting />` won't work — use `<Greeting />`
5. **Is the file path correct?** Check for typos in the import path
{{% /details %}}

{{% details title="I see 'Adjacent JSX elements must be wrapped in an enclosing tag'" %}}
Your component is returning multiple root elements. Wrap them in a `<div>` or a Fragment `<>...</>`:

```jsx
// ❌ Error
return (
  <h1>Title</h1>
  <p>Content</p>
)

// ✅ Fix
return (
  <>
    <h1>Title</h1>
    <p>Content</p>
  </>
)
```
{{% /details %}}

{{% details title="I used 'class' instead of 'className' and got a warning" %}}
In JSX, HTML's `class` attribute becomes `className` because `class` is a reserved word in JavaScript. Same for `for` → `htmlFor`.

```jsx
// ❌ Warning
<div class="card">

// ✅ Correct
<div className="card">
```
{{% /details %}}

{{% details title="npm run dev shows an error or nothing loads" %}}
Common fixes:

1. Make sure you ran `npm install` first
2. Check that you're in the project folder (`cd my-first-react`)
3. Make sure Node.js is installed (`node -v` should show a version)
4. Try deleting `node_modules/` and running `npm install` again
5. Check that port 5173 isn't already in use by another dev server
{{% /details %}}

{{% details title="My changes don't show up in the browser" %}}
- Make sure you **saved the file** (Ctrl+S / Cmd+S)
- Check the terminal — if there's a compilation error, fix it and save again
- Try a hard refresh in the browser (Ctrl+Shift+R / Cmd+Shift+R)
- Make sure `npm run dev` is still running in your terminal
{{% /details %}}

---

## Key Takeaways

{{< callout emoji="🎯" >}}
- **React** is a JavaScript library for building UIs from reusable **components**
- **Components** are functions that return JSX — PascalCase names, one root element, exported
- **Vite** sets up your project: `npm create vite@latest` → `npm install` → `npm run dev`
- **JSX** looks like HTML but lives in JavaScript — use `{}` for expressions, `className` instead of `class`
- **Composition** = building big components from small ones, like Lego blocks
- React doesn't replace JavaScript — it **organizes** the DOM manipulation, events, and data you already know
{{< /callout >}}

---

## Additional Resources

### Official Documentation

{{< cards >}}
  {{< card link="https://react.dev/learn" title="React: Quick Start" subtitle="Official getting started guide" >}}
  {{< card link="https://react.dev/learn/describing-the-ui" title="React: Describing the UI" subtitle="Components, JSX, and rendering" >}}
  {{< card link="https://vitejs.dev/guide/" title="Vite: Getting Started" subtitle="Vite setup and configuration" >}}
{{< /cards >}}

### Tutorials

{{< cards >}}
  {{< card link="https://react.dev/learn/your-first-component" title="Your First Component" subtitle="Step-by-step component tutorial" >}}
  {{< card link="https://react.dev/learn/writing-markup-with-jsx" title="Writing Markup with JSX" subtitle="JSX syntax and rules" >}}
  {{< card link="https://react.dev/learn/javascript-in-jsx-with-curly-braces" title="JavaScript in JSX" subtitle="Using expressions in JSX" >}}
{{< /cards >}}
