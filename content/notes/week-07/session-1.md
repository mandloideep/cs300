---
title: "Async JavaScript & Fetch API"
weight: 1
toc: true
---

## Introduction

So far, all the data in your JavaScript programs has been hardcoded — typed directly into your HTML or JavaScript files. Real web applications get their data from external sources: weather services, social media platforms, databases, and other servers. The **Fetch API** lets your JavaScript request data from the internet, and **asynchronous programming** ensures your page stays responsive while waiting for that data to arrive.

### What You'll Learn

- Why JavaScript needs asynchronous code
- How Promises represent future values
- How `async/await` makes async code readable
- How to fetch data from APIs and display it on a page

---

## Synchronous vs Asynchronous

### Synchronous Code

By default, JavaScript runs **synchronously** — one line at a time, in order, waiting for each line to finish before moving to the next.

```javascript
console.log("Step 1");
console.log("Step 2");
console.log("Step 3");
// Output: Step 1, Step 2, Step 3 (in order)
```

This works fine when every operation is fast. But what if one step takes several seconds — like fetching data from a server?

```javascript
console.log("Start");
const data = fetchFromServer(); // Imagine this takes 3 seconds...
console.log("Done");            // This line WAITS 3 seconds to run
// The entire page is FROZEN for 3 seconds. No scrolling, clicking, or typing.
```

### Asynchronous Code

Asynchronous code starts a task, **moves on immediately**, and comes back when the task finishes.

```javascript
console.log("1. Start");

setTimeout(() => {
  console.log("2. This runs after 2 seconds");
}, 2000);

console.log("3. This runs immediately!");

// Output: "1. Start", "3. This runs immediately!", "2. This runs after 2 seconds"
```

Notice that `"3"` prints before `"2"`. JavaScript didn't wait for the timer — it scheduled it and kept going.

{{< callout type="info" >}}
**Restaurant Analogy:** Think of a restaurant. The waiter (JavaScript) takes your order and sends it to the kitchen (the server/API). The waiter doesn't stand at the kitchen door waiting — they serve other tables. When the food is ready, the kitchen signals the waiter to deliver it. That's async JavaScript.
{{< /callout >}}

### Key Differences

| | Synchronous | Asynchronous |
|---|-------------|--------------|
| **Execution** | Line by line, in order | Start task, continue, come back |
| **Blocking** | Yes — page freezes | No — page stays responsive |
| **Use cases** | Math, string operations | API calls, timers, file reads |

---

## Callbacks

A **callback** is a function you pass to another function, to be called when the async task completes.

```javascript
function fetchUser(id, callback) {
  setTimeout(() => {
    const user = { id: id, name: "Alice" };
    callback(user); // Call the function when data is "ready"
  }, 1000);
}

fetchUser(1, (user) => {
  console.log(user.name); // "Alice" — runs after 1 second
});
```

You've already used callbacks extensively:

```javascript
// addEventListener takes a callback
button.addEventListener("click", () => {
  console.log("Clicked!");
});

// setTimeout takes a callback
setTimeout(() => {
  console.log("Time's up!");
}, 3000);

// forEach takes a callback
[1, 2, 3].forEach((num) => {
  console.log(num);
});
```

### The Problem: Callback Hell

When you chain multiple async operations with callbacks, the code nests deeper and deeper:

```javascript
getUser(1, (user) => {
  getPosts(user.id, (posts) => {
    getComments(posts[0].id, (comments) => {
      getAuthor(comments[0].authorId, (author) => {
        console.log(author.name);
        // Four levels deep — hard to read and debug!
      });
    });
  });
});
```

This "pyramid of doom" is why **Promises** were invented.

---

## Promises

A **Promise** is an object that represents a value that will be available in the future. Think of it like an order receipt at a restaurant — you don't have the food yet, but you have a guarantee it's coming.

### Promise States

```
                 ┌─── fulfilled (resolved with a value)
                 │
[pending] ───────┤
                 │
                 └─── rejected (failed with an error)
```

- **Pending:** The operation is still in progress
- **Fulfilled:** The operation completed successfully (has a value)
- **Rejected:** The operation failed (has an error)

### Using Promises with .then() and .catch()

```javascript
fetch("https://api.example.com/data")
  .then((response) => response.json())   // When fetch succeeds, parse JSON
  .then((data) => console.log(data))      // When parsing succeeds, log data
  .catch((error) => console.error(error)); // If ANYTHING fails, handle error
```

Key points:
- `.then()` runs when the Promise **succeeds** — it receives the resolved value
- `.catch()` runs when the Promise **fails** — it receives the error
- You can chain multiple `.then()` calls — each receives the return value of the previous one
- One `.catch()` at the end handles errors from ANY step in the chain

### Promises vs Callbacks

```javascript
// Callback hell (nested)
getUser(1, (user) => {
  getPosts(user.id, (posts) => {
    getComments(posts[0].id, (comments) => {
      console.log(comments);
    });
  });
});

// Promise chain (flat)
getUser(1)
  .then((user) => getPosts(user.id))
  .then((posts) => getComments(posts[0].id))
  .then((comments) => console.log(comments))
  .catch((error) => console.error(error));
```

---

## async/await

`async/await` is modern syntax that makes Promises even cleaner. It lets you write async code that **looks like** synchronous code.

### Basic Syntax

```javascript
// With .then() chain
function loadData() {
  return fetch("https://api.example.com/users")
    .then((response) => response.json())
    .then((users) => console.log(users));
}

// With async/await — same behavior, cleaner syntax
async function loadData() {
  const response = await fetch("https://api.example.com/users");
  const users = await response.json();
  console.log(users);
}
```

### Rules

1. **`async`** goes before the function keyword — it marks the function as asynchronous
2. **`await`** goes before any Promise — it pauses that function until the Promise resolves
3. `await` can only be used inside an `async` function

### Error Handling with try/catch

```javascript
async function loadData() {
  try {
    const response = await fetch("https://api.example.com/users");
    const users = await response.json();
    console.log(users);
  } catch (error) {
    console.error("Failed to load:", error.message);
  }
}
```

- `try` — attempt this code
- `catch` — if anything in `try` throws an error, run this instead
- This replaces `.catch()` from Promise chains

### Comparison

| Feature | Callbacks | Promises (.then) | async/await |
|---------|-----------|-------------------|-------------|
| Readability | Poor (nesting) | Good (flat chain) | Best (looks sync) |
| Error handling | Manual per callback | `.catch()` at end | `try/catch` block |
| When added | Always existed | ES6 (2015) | ES2017 |

{{< callout type="important" >}}
`async/await` is **not** a different mechanism from Promises — it's cleaner syntax for the same thing. Under the hood, `await` works with Promises. For this course, use `async/await` as your primary approach.
{{< /callout >}}

---

## The Fetch API

`fetch()` is a built-in browser function for making HTTP requests. No installation or imports needed.

### Your First Fetch

```javascript
async function getUser() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users/1");
  const user = await response.json();
  console.log(user);
}

getUser();
```

### The Two-Step Process

```
fetch(url)  →  Response object  →  response.json()  →  JavaScript data
               (HTTP info)         (parse body)         (your object!)
```

**Step 1:** `fetch(url)` sends a request and returns a **Response** object containing HTTP status, headers, etc.

**Step 2:** `response.json()` reads the response body and parses it as JSON, returning the actual data.

```javascript
const response = await fetch(url);  // Step 1: get the Response
const data = await response.json(); // Step 2: parse the body as JSON
```

{{< callout type="info" >}}
**Why two steps?** `fetch()` resolves as soon as the HTTP headers arrive. The body (your data) might still be downloading. `.json()` waits for the complete body and parses it. This is efficient — you could check the status before reading a potentially large response.
{{< /callout >}}

### The Complete Fetch Pattern

This is the pattern you'll use again and again:

```javascript
async function fetchData(url) {
  try {
    const response = await fetch(url);

    if (!response.ok) {
      throw new Error(`HTTP error! Status: ${response.status}`);
    }

    const data = await response.json();
    return data;
  } catch (error) {
    console.error("Fetch failed:", error.message);
  }
}
```

### Checking response.ok

`fetch()` only throws an error on **network failures** (no internet, DNS error). A 404 or 500 response does NOT throw — it's still a "successful" fetch.

```javascript
// BAD — this won't catch a 404!
try {
  const response = await fetch("/nonexistent-url");
  const data = await response.json(); // Might fail here with bad JSON
} catch (error) {
  // Only catches network errors, not 404s
}

// GOOD — explicitly check response.ok
const response = await fetch("/nonexistent-url");
if (!response.ok) {
  throw new Error(`HTTP error! Status: ${response.status}`);
}
const data = await response.json();
```

---

## Working with JSON

**JSON** (JavaScript Object Notation) is the standard data format for APIs.

```json
{
  "id": 1,
  "name": "Alice Johnson",
  "email": "alice@example.com",
  "address": {
    "city": "Chicago",
    "state": "IL"
  },
  "hobbies": ["coding", "hiking"]
}
```

### Accessing JSON Data

```javascript
const response = await fetch("https://jsonplaceholder.typicode.com/users/1");
const user = await response.json();

// Access properties
console.log(user.name);           // "Leanne Graham"
console.log(user.email);          // "Sincere@april.biz"

// Access nested properties
console.log(user.address.city);   // "Gwenborough"

// Access array items
console.log(user.company.name);   // "Romaguera-Crona"
```

### Exploring Unknown JSON

When working with a new API, always log the response first:

```javascript
const response = await fetch(url);
const data = await response.json();

console.log(data);               // See the full structure
console.log(typeof data);        // "object" or check if array
console.log(Object.keys(data));  // See top-level property names

// If it's an array:
console.log(data.length);        // How many items?
console.log(data[0]);            // What does one item look like?
```

---

## Rendering API Data to the DOM

This is where Weeks 6 and 7 come together. The DOM manipulation you learned last week is used to display the data you fetch.

### Single Item

```javascript
async function showUser() {
  const response = await fetch(
    "https://jsonplaceholder.typicode.com/users/1"
  );
  const user = await response.json();

  document.querySelector("#name").textContent = user.name;
  document.querySelector("#email").textContent = user.email;
  document.querySelector("#city").textContent = user.address.city;
}

showUser();
```

### List of Items

```javascript
async function showUsers() {
  const response = await fetch(
    "https://jsonplaceholder.typicode.com/users"
  );
  const users = await response.json();

  const list = document.querySelector("#user-list");
  list.innerHTML = users
    .map(
      (user) => `
      <li>
        <strong>${user.name}</strong> — ${user.email}
        <br />
        <small>${user.address.city}</small>
      </li>
    `
    )
    .join("");
}

showUsers();
```

The pattern: **fetch → parse → `.map()` to create HTML → set `innerHTML`**

---

## Error Handling Best Practices

### Network Errors vs HTTP Errors

```javascript
async function safeFetch(url) {
  try {
    const response = await fetch(url);

    // Check for HTTP errors (404, 500, etc.)
    if (!response.ok) {
      throw new Error(`Server error: ${response.status}`);
    }

    return await response.json();
  } catch (error) {
    if (error.message.includes("Server error")) {
      console.error("The server had a problem:", error.message);
    } else {
      console.error("Network error — check your internet:", error.message);
    }
  }
}
```

### User-Friendly Error Messages

```javascript
async function searchCity(city) {
  try {
    const response = await fetch(
      `https://wttr.in/${encodeURIComponent(city)}?format=j1`
    );

    if (!response.ok) throw new Error("City not found");

    const data = await response.json();
    displayWeather(data);
  } catch (error) {
    // Show the user a helpful message, not a technical error
    document.querySelector("#result").innerHTML = `
      <p class="error">
        Could not find weather for "${city}". Please check the spelling and try again.
      </p>
    `;
  }
}
```

---

## Practical Example: Weather Lookup App

Here's a complete example that ties everything together:

```html
<h1>Weather Lookup</h1>
<form id="weather-form">
  <input type="text" id="city-input" placeholder="Enter a city..." required />
  <button type="submit">Get Weather</button>
</form>
<div id="result"></div>
```

```javascript
const form = document.querySelector("#weather-form");
const input = document.querySelector("#city-input");
const result = document.querySelector("#result");

form.addEventListener("submit", async (event) => {
  event.preventDefault();
  const city = input.value.trim();
  if (!city) return;

  // Loading state
  result.innerHTML = "<p>Loading weather data...</p>";

  try {
    const response = await fetch(
      `https://wttr.in/${encodeURIComponent(city)}?format=j1`
    );

    if (!response.ok) throw new Error("City not found");

    const data = await response.json();
    const current = data.current_condition[0];

    // Success state
    result.innerHTML = `
      <h2>Weather in ${city}</h2>
      <p>Temperature: ${current.temp_F}°F / ${current.temp_C}°C</p>
      <p>Condition: ${current.weatherDesc[0].value}</p>
      <p>Humidity: ${current.humidity}%</p>
      <p>Wind: ${current.windspeedMiles} mph</p>
    `;
  } catch (error) {
    // Error state
    result.innerHTML = `<p class="error">Error: ${error.message}</p>`;
  }
});
```

**Key patterns in this example:**

1. `event.preventDefault()` — stop the form from reloading the page (Week 6)
2. `async` on the event handler — event handlers can be async
3. `encodeURIComponent(city)` — make user input URL-safe
4. Two-step fetch: `fetch()` → `response.json()`
5. `response.ok` check — handle HTTP errors
6. Template literals — build HTML from data (Week 5)
7. `try/catch` — handle errors gracefully

---

## Debugging API Calls

### Use the Network Tab

1. Open DevTools (F12 or right-click → Inspect)
2. Go to the **Network** tab
3. Filter by **Fetch/XHR** to see only API calls
4. Click a request to see:
   - **Headers:** URL, HTTP method, status code
   - **Response:** The raw JSON data
   - **Preview:** Formatted view of the JSON

### Common Debugging Steps

```javascript
async function debugFetch(url) {
  console.log("Fetching:", url);           // Log the URL

  const response = await fetch(url);
  console.log("Status:", response.status); // Log the status
  console.log("OK?", response.ok);        // Log if successful

  const data = await response.json();
  console.log("Data:", data);              // Log the parsed data
  console.log("Type:", typeof data);       // Object or Array?
  console.log("Keys:", Object.keys(data)); // Top-level properties

  return data;
}
```

{{% details title="My fetch returns undefined" %}}
Make sure you're using `await` before `fetch()` and `response.json()`. Without `await`, you get a Promise object instead of the actual data.

```javascript
// Wrong — missing await
const response = fetch(url);       // This is a Promise, not a Response!
const data = response.json();      // This won't work

// Correct
const response = await fetch(url); // Now it's a Response
const data = await response.json(); // Now it's your data
```
{{% /details %}}

{{% details title="I get a CORS error" %}}
CORS (Cross-Origin Resource Sharing) is a browser security feature. Some APIs don't allow requests from browsers. Try:
- Using a different API that supports CORS
- The APIs used in this course (JSONPlaceholder, wttr.in, TheMealDB) all support CORS
- You cannot fix this from your JavaScript — the API server must allow it
{{% /details %}}

{{% details title="My data looks like [object Object]" %}}
You're trying to display an object directly. Use `JSON.stringify()` for debugging, or access specific properties:

```javascript
// Wrong
result.textContent = data; // Shows [object Object]

// Debugging
result.textContent = JSON.stringify(data, null, 2);

// Correct
result.textContent = data.name; // Access the specific property you want
```
{{% /details %}}

{{% details title="fetch doesn't catch 404 errors" %}}
`fetch()` only rejects on network errors. You must check `response.ok`:

```javascript
const response = await fetch(url);
if (!response.ok) {
  throw new Error(`HTTP error: ${response.status}`);
}
```
{{% /details %}}

---

## Key Takeaways

{{< callout emoji="🎯" >}}
- **Asynchronous code** lets JavaScript start tasks and continue without waiting — keeping the page responsive
- **Promises** represent future values — use `.then()/.catch()` or `async/await`
- **async/await** is the modern standard — use it everywhere in this course
- **fetch()** is a two-step process: get the Response, then parse the body with `.json()`
- **Always check `response.ok`** — fetch doesn't throw on 404/500 errors
- **The pattern:** fetch → check ok → parse json → update DOM → catch errors
{{< /callout >}}

---

## Additional Resources

### Official Documentation

{{< cards >}}
{{< card link="https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch" title="MDN: Using Fetch" >}}
{{< card link="https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous" title="MDN: Asynchronous JavaScript" >}}
{{< card link="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function" title="MDN: async function" >}}
{{< /cards >}}

### Tutorials

{{< cards >}}
{{< card link="https://javascript.info/async" title="javascript.info: Async" >}}
{{< card link="https://javascript.info/fetch" title="javascript.info: Fetch" >}}
{{< /cards >}}

### Practice APIs (Free, No Key Required)

{{< cards >}}
{{< card link="https://jsonplaceholder.typicode.com/" title="JSONPlaceholder" >}}
{{< card link="https://wttr.in/:help" title="wttr.in Weather API" >}}
{{< card link="https://www.themealdb.com/api.php" title="TheMealDB" >}}
{{< card link="https://pokeapi.co/" title="PokeAPI" >}}
{{< /cards >}}
