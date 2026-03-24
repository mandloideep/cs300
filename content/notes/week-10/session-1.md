---
title: "Week 10 Session 1: Props, State & Interactive Components"
weight: 1
toc: true
---

## Introduction

In Week 8, you built your first React components -- ProfileCard, Button, Header -- and composed them into a complete app. But every ProfileCard showed the same person and every Button said the same text. The components were usable but not flexible. And nothing was interactive -- clicking a button did nothing visible.

Today we fix both problems. **Props** make components reusable by passing different data to each instance. **State** makes components interactive by letting them manage data that changes. Together with **callback props**, these form the core communication pattern in React: data flows down, actions flow up.

### What You'll Learn

- What props are and how they relate to function arguments
- How to pass and destructure props
- Default values for safety
- The children prop for wrapper components
- `useState` for managing changeable data
- Callback props for child-to-parent communication
- The "data down, actions up" pattern

---

## Part 1: Props Fundamentals

### The Week 8 Problem

```jsx
function ProfileCard() {
  const name = "Jane Smith";  // hardcoded!
  const role = "Web Developer";
  return (
    <div className="profile-card">
      <h2>{name}</h2>
      <p>{role}</p>
    </div>
  );
}

// Three identical cards
<ProfileCard />
<ProfileCard />
<ProfileCard />
```

### The Props Solution

```jsx
function ProfileCard(props) {
  return (
    <div className="profile-card">
      <h2>{props.name}</h2>
      <p>{props.role}</p>
    </div>
  );
}

// Three DIFFERENT cards
<ProfileCard name="Jane Smith" role="Developer" />
<ProfileCard name="Bob Jones" role="Designer" />
<ProfileCard name="Alice Chen" role="Student" />
```

{{< callout type="info" >}}
**Components are functions. Props are arguments.** The syntax is different (attributes instead of parentheses), but the concept is identical to passing arguments to a regular JavaScript function. You can pass as many props as you want, but if a component needs 5-6+, it might be doing too much -- consider breaking it into smaller components.
{{< /callout >}}

### Destructuring Props

```jsx
// Standard React pattern -- destructure in the parameter
function ProfileCard({ name, role, bio }) {
  return (
    <div className="profile-card">
      <h2>{name}</h2>
      <p>{role}</p>
      <p>{bio}</p>
    </div>
  );
}
```

### Default Values

```jsx
function ProfileCard({
  name,
  role = "Team Member",
  bio = "No bio provided"
}) {
  return (
    <div>
      <h2>{name}</h2>
      <p>{role}</p>
      <p>{bio}</p>
    </div>
  );
}

<ProfileCard name="Jane" />
// role = "Team Member", bio = "No bio provided"
```

### Data Types as Props

| Type | Syntax | Notes |
|------|--------|-------|
| String | `name="Jane"` | Quotes only |
| Number | `age={25}` | Curly braces |
| Boolean | `isActive={true}` | Curly braces |
| Array | `skills={["React", "CSS"]}` | Curly braces |
| Object | `address={{ city: "Chicago" }}` | Double curly braces |

{{< callout type="warning" >}}
**Props are read-only.** A component must never modify its own props. Props come from the parent -- the child just displays them. If you need data that changes, you need **state** (covered below).
{{< /callout >}}

### Props + .map() = Dynamic Lists

```jsx
const teamMembers = [
  { id: 1, name: "Jane Smith", role: "Developer" },
  { id: 2, name: "Bob Jones", role: "Designer" },
  { id: 3, name: "Alice Chen", role: "Student" },
];

function App() {
  return (
    <div>
      {teamMembers.map(member => (
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

One definition. Three different cards. Add a fourth person to the array and a fourth card appears automatically.

---

## Part 2: Children Prop & Composition

### The Children Prop

Regular props are attributes. Children is whatever goes between the tags:

```jsx
<Card title="About Me">
  <p>This content becomes children!</p>
</Card>

function Card({ title, children }) {
  return (
    <div className="card">
      <h3>{title}</h3>
      <div className="card-body">
        {children}
      </div>
    </div>
  );
}
```

{{< callout type="info" >}}
**Picture frame analogy:** The Card is the frame -- border, shadow, padding. You can put any picture (content) inside. The frame doesn't care what the picture is. Children can be text, HTML elements, other React components, or any combination.
{{< /callout >}}

The same wrapper component can hold completely different content:

```jsx
<Card title="Skills">
  <ul>
    <li>React</li>
    <li>CSS</li>
  </ul>
</Card>

<Card title="Quote">
  <blockquote>
    "Code is like humor. When you have to explain it, it's bad."
  </blockquote>
</Card>
```

Same Card component. One wraps a list, the other wraps a quote. Card doesn't know or care what's inside.

### Wrapper Components

```jsx
function Section({ title, children }) {
  return (
    <section>
      <h2>{title}</h2>
      <div>{children}</div>
    </section>
  );
}

function PageLayout({ children }) {
  return (
    <div className="page">
      <Header />
      <main>{children}</main>
      <Footer />
    </div>
  );
}
```

Build these once, reuse across your entire app.

{{% details title="Going deeper: Composition over Inheritance" %}}
React's approach: build from pieces, not hierarchies. Instead of creating a `FancyCard` that extends `Card`, you compose:

```jsx
// Composition -- ProfileCard uses Card
function ProfileCard({ name, role }) {
  return (
    <Card>
      <h3>{name}</h3>
      <p>{role}</p>
    </Card>
  );
}
```

ProfileCard doesn't inherit from Card. It just uses Card as a wrapper -- passing its content as children. This is the React way.
{{% /details %}}

---

## Part 3: Managing State with useState

### Why State?

Props are read-only -- they come from the parent and can't be changed. But real UIs need data that changes:

- A like count that goes up when clicked
- A form input that updates as you type
- A menu that opens and closes
- A list that filters based on a search

**State** is data that a component owns and can change. When state changes, React automatically re-renders the component.

### The useState Hook

{{< callout type="warning" >}}
**Don't forget the import!** You must add `import { useState } from 'react';` at the top of any file that uses `useState`. Missing this import is one of the most common errors.
{{< /callout >}}

```jsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        +1
      </button>
    </div>
  );
}
```

{{< callout type="info" >}}
**How useState works:**
- `useState(0)` creates a state variable with initial value `0`
- It returns an array: `[currentValue, updaterFunction]`
- We destructure it: `const [count, setCount] = useState(0)`
- Calling `setCount(newValue)` updates the value AND re-renders the component
{{< /callout >}}

### Breaking Down the Syntax

```jsx
const [count, setCount] = useState(0);
//      ^         ^                ^
//   current   updater        initial
//   value     function       value
```

This is **array destructuring** -- the same concept as object destructuring but with position instead of name. `useState` returns an array with two items, and we give them names.

{{% details title="What does array destructuring look like without the shorthand?" %}}
```javascript
// Without destructuring
const stateArray = useState(0);
const count = stateArray[0];      // current value
const setCount = stateArray[1];    // updater function

// With destructuring (same thing, cleaner)
const [count, setCount] = useState(0);
```

You can name the variables anything, but the convention is `[thing, setThing]`.
{{% /details %}}

{{% details title="Why const if the value changes?" %}}
`const [count, setCount] = useState(0)` uses `const`, but the value seems to change when we call `setCount`. Here's what's actually happening:

Within a single render, `count` never changes -- it's truly constant. When you call `setCount(1)`, React doesn't change `count` from 0 to 1. Instead, React **re-renders the component** -- it calls your function again from scratch. This time, `useState(0)` returns `1` (the updated value) instead of `0`. It's a brand new `const` variable with the new value.

Think of it like this: each render is a snapshot. In render #1, `count = 0`. In render #2, `count = 1`. Each snapshot has its own constant value.
{{% /details %}}

### What Happens When You Call the Setter

```jsx
// User clicks the button
setCount(count + 1);

// React does two things:
// 1. Updates the value: count is now 1
// 2. Re-renders the component with the new value
```

The component function runs again. This time `useState(0)` returns `1` (the updated value) instead of `0`. The JSX renders with the new count.

{{< callout type="important" >}}
**The re-render mental model:** When state changes, React calls your component function again from top to bottom. Every line of code in the function runs again. But `useState` is smart -- instead of returning the initial value, it returns the updated value. This is how the UI stays in sync with your data.
{{< /callout >}}

### Multiple State Variables

Each component can have as many state variables as it needs:

```jsx
function ProfileEditor() {
  const [name, setName] = useState("");
  const [bio, setBio] = useState("");
  const [likes, setLikes] = useState(0);

  return (
    <div>
      <p>{name} - {likes} likes</p>
      <button onClick={() => setLikes(likes + 1)}>
        Like
      </button>
    </div>
  );
}
```

Each state variable is completely independent -- updating one doesn't affect the others.

### Rules of Hooks

{{< callout type="warning" >}}
**Two rules you must follow:**

1. **Only call `useState` at the top level** of your component -- not inside `if` statements, loops, or nested functions
2. **Only call `useState` inside React components** (or custom hooks)

These rules ensure React can correctly track your state between re-renders.
{{< /callout >}}

```jsx
// NEVER do this
function Counter() {
  if (someCondition) {
    const [count, setCount] = useState(0); // WRONG
  }
  // ...
}

// ALWAYS do this
function Counter() {
  const [count, setCount] = useState(0); // top level
  // ...
}
```

### Props vs State

| | Props | State |
|--|-------|-------|
| **Who owns it?** | Parent component | This component |
| **Can it change?** | No (read-only) | Yes (via setter function) |
| **How is it set?** | By the parent as attributes | By `useState` inside the component |
| **When it changes** | Parent re-renders child | Component re-renders itself |

{{< callout type="info" >}}
**Think of it this way:** Props are like arguments you receive. State is like variables you create. You can't change your arguments, but you can change your own variables.
{{< /callout >}}

---

## Part 4: Callback Props -- Data Down, Actions Up

### The Communication Problem

Data flows DOWN via props. But what happens when a child component needs to notify the parent?

For example: a LikeButton inside a ProfileCard needs to tell the App component "the user clicked like!" so the App can update a total count.

### The Solution: Pass a Function as a Prop

```jsx
function App() {
  const [totalLikes, setTotalLikes] = useState(0);

  function handleLike() {
    setTotalLikes(totalLikes + 1);
  }

  return (
    <div>
      <p>Total Likes: {totalLikes}</p>
      <LikeButton onLike={handleLike} />
    </div>
  );
}

function LikeButton({ onLike }) {
  return (
    <button onClick={onLike}>
      Like
    </button>
  );
}
```

{{% steps %}}

### App creates state

`const [totalLikes, setTotalLikes] = useState(0)` -- App owns the data.

### App creates a handler function

`handleLike` calls `setTotalLikes` to update the state.

### App passes the function as a prop

`<LikeButton onLike={handleLike} />` -- the function goes DOWN as a prop.

### Child calls the function

When the button is clicked, `onLike()` runs -- which is actually `handleLike` from App.

### State updates and React re-renders

`setTotalLikes` updates the count. React re-renders App with the new value.

{{% /steps %}}

### The Pattern: Data Down, Actions Up

```
        App (owns state)
       /         \
  data ↓           ↓ function (as prop)
       \         /
      LikeButton
          ↑
     calls onLike()
```

| Direction | What Flows | How |
|-----------|-----------|-----|
| **Down** | Data (values) | Parent passes props |
| **Up** | Actions (events) | Child calls parent's function |

{{< callout type="info" >}}
**This is the core React pattern.** The parent owns the state and passes data down. The child triggers changes by calling callback functions. You'll use this pattern in every React app you build.
{{< /callout >}}

### Why Does the Parent Own the State?

If the LikeButton owned the like count, the parent (App) couldn't see it -- data flows DOWN, not up. By keeping state in the parent, the parent can:

- Display the total count
- Pass the count to other components
- Decide how to update it

{{% details title="What's the naming convention for callback props?" %}}
The convention is `onSomething` for the prop name and `handleSomething` for the function:

```jsx
// In the parent
function handleLike() { ... }
function handleDelete() { ... }
function handleSearch(query) { ... }

// Passing to the child
<Button onLike={handleLike} />
<Item onDelete={handleDelete} />
<SearchBar onSearch={handleSearch} />
```

The `on` prefix signals "this is a callback." The `handle` prefix signals "this handles the event."
{{% /details %}}

---

## Conditional Rendering with &&

In the examples above (and in many React apps), you'll see a pattern like `{condition && <element>}`. This is **conditional rendering** -- showing or hiding parts of the UI based on a condition.

```jsx
// Only show the button if onFavorite was passed as a prop
{onFavorite && (
  <button onClick={() => onFavorite(name)}>
    Favorite
  </button>
)}

// Only show the list if there are favorites
{favorites.length > 0 && (
  <p>Favorites: {favorites.join(', ')}</p>
)}

// Only show the title if one was provided
{title && <h3>{title}</h3>}
```

**How it works:** In JavaScript, `&&` returns the second value if the first is truthy, or the first value if it's falsy. JSX ignores `false`, `null`, and `undefined`, so when the condition is false, nothing renders.

{{< callout type="info" >}}
**This is a very common React pattern.** You'll see `{condition && <JSX>}` everywhere in React code. It's the simplest way to conditionally show or hide elements without needing a full `if` statement.
{{< /callout >}}

---

## Troubleshooting

{{% details title="Props showing as undefined" %}}
Check for misspelled names, missing curly braces on non-strings, or props not passed at all. Add default values for safety.
{{% /details %}}

{{% details title="Children prop is undefined" %}}
Use opening and closing tags, not self-closing: `<Card>content</Card>` not `<Card />`.
{{% /details %}}

{{% details title="I'm getting 'props is not defined'" %}}
If you're using destructuring, you don't have a `props` variable:

```jsx
// Can't use props.name with destructuring
function ProfileCard({ name }) {
  return <h2>{props.name}</h2>;  // Error!
}

// Use the destructured variable directly
function ProfileCard({ name }) {
  return <h2>{name}</h2>;  // Correct!
}
```
{{% /details %}}

{{% details title="State not updating when I click" %}}
**Common causes:**

1. **Forgot `onClick`:** Make sure the button has `onClick={() => setCount(count + 1)}`
2. **Calling the function immediately:** Use `onClick={() => setCount(count + 1)}` not `onClick={setCount(count + 1)}` -- the arrow function prevents immediate execution
3. **Mutating state directly:** Don't do `count++`. Use `setCount(count + 1)`.

```jsx
// WRONG -- calls setCount immediately on render
<button onClick={setCount(count + 1)}>

// RIGHT -- calls setCount when clicked
<button onClick={() => setCount(count + 1)}>
```
{{% /details %}}

{{% details title="Callback prop not working" %}}
Make sure:

1. The parent is passing the function: `<Child onAction={handleAction} />`
2. The child is calling it: `onClick={onAction}` or `onClick={() => onAction(data)}`
3. The function names match between parent and child
{{% /details %}}

{{% details title="All cards update when I click one" %}}
Each component needs its OWN `useState` call. If you have a `likes` state in the parent and pass it to all cards, they'll share it. Instead, put `useState` inside each card component so each has independent state.
{{% /details %}}

{{% details title="Getting 'React Hook useState is called conditionally'" %}}
You have a `useState` call inside an `if` statement, loop, or after a `return`. Move it to the top of your component, before any conditions.

```jsx
// WRONG
function MyComponent({ show }) {
  if (!show) return null;
  const [count, setCount] = useState(0); // Error!
}

// RIGHT
function MyComponent({ show }) {
  const [count, setCount] = useState(0); // Top level
  if (!show) return null;
  // ... rest of component
}
```
{{% /details %}}

---

## Putting It All Together: Interactive Card System

Here's a complete example that connects every concept -- props, children, useState, callback props, conditional rendering, and `.map()` -- in a realistic multi-component app.

### The Component Tree

```
App (favorites state)
├── CardGrid (columns=4) ← wrapper using children
│   ├── InfoCard → Card (Team Members: 3)
│   └── InfoCard → Card (Favorites: 0 → updates!)
├── CardGrid (columns=3) ← wrapper using children
│   ├── ProfileCard (likes state) → Card
│   ├── ProfileCard (likes state) → Card
│   └── ProfileCard (likes state) → Card
└── Favorites list (conditional rendering)
```

### ProfileCard: Local State + Callback Props

This component demonstrates local state (`likes`), composition (wraps content in `<Card>`), callback props (`onFavorite`), and conditional rendering with `&&`:

```jsx
import { useState } from 'react';
import Card from './Card';

function ProfileCard({ name, role = "Team Member", bio = "No bio provided", onFavorite }) {
  const [likes, setLikes] = useState(0);

  return (
    <Card>
      <h3>{name}</h3>
      <p>{role}</p>
      <p>{bio}</p>
      <div>
        <button onClick={() => setLikes(likes + 1)}>
          {likes} Likes
        </button>
        {onFavorite && (
          <button onClick={() => onFavorite(name)}>
            Favorite
          </button>
        )}
      </div>
    </Card>
  );
}
```

{{< callout type="info" >}}
**Two types of interaction here:**
- **Likes** use local state -- each ProfileCard tracks its own count independently. Clicking Like on Jane's card doesn't affect Bob's card.
- **Favorite** uses a callback prop -- ProfileCard doesn't know what happens when you click it. It just calls `onFavorite(name)` and lets the parent decide.
{{< /callout >}}

### App: Lifted State + Callbacks + .map()

The App component owns the `favorites` state and passes a callback down to each ProfileCard:

```jsx
import { useState } from 'react';
import ProfileCard from './ProfileCard';
import CardGrid from './CardGrid';

const teamMembers = [
  { id: 1, name: "Jane Smith", role: "Lead Developer", bio: "Loves building with React" },
  { id: 2, name: "Bob Jones", role: "UX Designer", bio: "Makes everything look great" },
  { id: 3, name: "Alice Chen", role: "Full Stack Dev", bio: "JavaScript enthusiast" },
];

function App() {
  const [favorites, setFavorites] = useState([]);

  function handleFavorite(name) {
    if (favorites.includes(name)) {
      setFavorites(favorites.filter(fav => fav !== name));
    } else {
      setFavorites([...favorites, name]);
    }
  }

  return (
    <div>
      <h1>Dashboard</h1>

      <CardGrid columns={3}>
        {teamMembers.map(member => (
          <ProfileCard
            key={member.id}
            name={member.name}
            role={member.role}
            bio={member.bio}
            onFavorite={handleFavorite}
          />
        ))}
      </CardGrid>

      {favorites.length > 0 && (
        <p>Favorites: {favorites.join(', ')}</p>
      )}
    </div>
  );
}
```

### Data Flow

```
App
├── favorites (state) ─────────────────────────┐
├── handleFavorite (function) ──────────┐       │
│                                       │       │
├── InfoCard ← value={favorites.length} ┘       │
│                                               │
└── ProfileCard ← onFavorite={handleFavorite} ──┘
    ├── likes (local state)
    └── onClick → onFavorite(name) → App updates favorites
```

{{< callout type="info" >}}
**Key observations:**
- Each **ProfileCard** has independent `likes` state (local state -- only that card is affected)
- **`favorites`** lives in App and flows down through props, back up through callbacks (lifted state)
- **Card** and **CardGrid** are pure wrapper components using `children` -- they don't know their content
- This is the **"data down, actions up"** pattern in a real project
{{< /callout >}}

---

## Key Takeaways

{{< callout emoji="🎯" >}}
1. **Props = function arguments** -- pass data from parent to child
2. **Destructure** for clean syntax: `{ name, role }`
3. **Defaults** for safety: `role = "Team Member"`
4. **Props are read-only** -- display them, never modify them
5. **Children prop** = flexible content between tags
6. **useState** = data the component owns and can change (don't forget `import { useState } from 'react'`)
7. **Calling the setter** re-renders the component -- the entire function runs again with the new value
8. **Rules of hooks** -- call `useState` at the top level only, inside components only
9. **Callback props** = pass functions down, child calls them up
9. **Data down, actions up** = the core React communication pattern
10. **Conditional rendering** with `&&` -- `{condition && <JSX>}` shows/hides elements
11. **Local vs lifted state** -- local state stays in one component; lifted state lives in a parent and flows via props/callbacks
{{< /callout >}}

---

## Additional Resources

{{< cards >}}

{{< card
  title="React: Passing Props"
  link="https://react.dev/learn/passing-props-to-a-component"
  subtitle="The essential React props tutorial"
>}}

{{< card
  title="React: State - A Component's Memory"
  link="https://react.dev/learn/state-a-components-memory"
  subtitle="Official guide to useState"
>}}

{{< card
  title="React: Responding to Events"
  link="https://react.dev/learn/responding-to-events"
  subtitle="Event handlers and callback props"
>}}

{{< card
  title="React: Thinking in React"
  link="https://react.dev/learn/thinking-in-react"
  subtitle="The 5-step methodology for designing React UIs"
>}}

{{< /cards >}}
