---
title: "Assignment 3: JavaScript API Application Guide"
weight: 3
toc: true
---

## Introduction

This assignment asks you to build an **interactive web application that fetches real data from a public API** and displays it dynamically on the page. Instead of hardcoding content into HTML, your app will request live data from the internet and render it using JavaScript -- the same pattern behind every modern web application you use daily.

APIs (Application Programming Interfaces) are how the web communicates. When you search on Google, stream on Netflix, or check the weather on your phone, your device is making API calls behind the scenes. Learning to work with APIs is the bridge between building static pages and building real, dynamic applications.

**What you will learn:**

- How to fetch data from external APIs using `async/await`
- How to manipulate the DOM to display dynamic content
- How to handle errors gracefully and manage different UI states
- How to respond to user interactions with event listeners
- How to structure a JavaScript application with clean, organized code

---

## Before You Start

{{% steps %}}

### Choose Your Project and API

You have 5 options: **Movie/Book Search** (OMDB or Open Library API), **Trivia Quiz** (Open Trivia DB), **Recipe Finder** (TheMealDB), **GitHub Profile Viewer** (GitHub API), or **Custom API** (with my approval). Pick the one that genuinely interests you -- you will be spending significant time with this API, so choose something you find engaging.

### Test Your API in the Browser First

Before writing a single line of JavaScript, open your chosen API URL directly in your browser. For example, paste `https://www.themealdb.com/api/json/v1/1/search.php?s=pasta` into your address bar and hit Enter. You will see raw JSON data. Study the structure: What are the property names? How is the data nested? Where are the values you need (titles, images, descriptions)?

### Review JavaScript Fundamentals

Revisit the Week 5 and Week 6 student notes on JavaScript fundamentals, DOM manipulation, and events. Make sure you are comfortable with functions, template literals, array methods like `forEach` and `map`, and how to select and modify DOM elements.

### Understand Async Concepts

The key concept: **API calls take time**, and your code must wait for the data to arrive before trying to use it. This is fundamentally different from the instant operations you have done so far (like changing a CSS class). Your code needs to say "go fetch this data, and when it comes back, THEN do something with it."

### Set Up Your Repository

Create a public GitHub repository named `cs300-assignment-3`. Create a feature branch to work on (e.g., `feature/recipe-finder`). Set up GitHub Pages deployment from Settings.

{{% /steps %}}

{{< callout type="important" >}}
**Test your API FIRST, before writing any code.** Open the API URL in your browser, look at the JSON response, and understand the data structure. Know exactly which property names contain the data you need. This single step saves hours of confusion later.
{{< /callout >}}

---

## Understanding the Requirements

| Criteria            | Points | What This Means                                            |
| ------------------- | ------ | ---------------------------------------------------------- |
| API Integration     | 12     | Successful fetch with async/await, proper error handling   |
| DOM Manipulation    | 12     | Dynamic element creation, content updates, event listeners |
| User Interactivity  | 10     | At least 2 interaction types that work smoothly            |
| Error Handling & UX | 8      | All 5 UI states handled, user-friendly messages            |
| Code Quality        | 8      | Clean, organized, well-structured JavaScript               |

**Total: 50 points**

### The 5 UI States Requirement

Your app must handle five distinct states. Think of these as five different "screens" your app can show depending on what is happening:

| State   | What the User Sees                 | When It Happens                                    |
| ------- | ---------------------------------- | -------------------------------------------------- |
| Initial | Instructions or default content    | Page first loads, before any search                |
| Loading | Spinner or "Loading..." message    | During the API fetch (data is in transit)          |
| Success | Results displayed as cards or list | API returns data successfully                      |
| Empty   | "No results found" message         | API returns successfully but with no matching data |
| Error   | Friendly error message             | API call fails (network issue, server down, etc.)  |

{{< callout type="warning" >}}
**All 5 UI states are required.** Many students forget the **initial state** (what the page shows before the user does anything) and the **empty state** (what happens when a search returns zero results). These are separate from the error state. Plan for all five from the start.
{{< /callout >}}

---

## Choosing Your API

{{< tabs items="No Key Required,Free Key Required" >}}
{{< tab >}}
These APIs work immediately with no registration or setup:

| API            | URL                        | Description                                   |
| -------------- | -------------------------- | --------------------------------------------- |
| TheMealDB      | themealdb.com/api.php      | Recipe data with images and instructions      |
| Open Trivia DB | opentdb.com/api_config.php | Quiz questions by category and difficulty     |
| PokeAPI        | pokeapi.co                 | Pokemon data with stats and images            |
| Open Library   | openlibrary.org/developers | Book data with covers and author info         |
| REST Countries | restcountries.com          | Country data with flags, populations, regions |
| GitHub API     | api.github.com             | User profiles, repositories, and activity     |

{{< /tab >}}
{{< tab >}}
These APIs require a free API key (quick registration):

| API            | URL                | Notes                                           |
| -------------- | ------------------ | ----------------------------------------------- |
| OMDB           | omdbapi.com        | Movie and TV show data (free key via email)     |
| OpenWeatherMap | openweathermap.org | Weather data by city or coordinates (free tier) |

{{< /tab >}}
{{< /tabs >}}

{{< callout type="info" >}}
**APIs that require no key are simpler to set up and deploy.** If you choose an API with a key, **never commit the key to GitHub**. Anyone can see public repository code, and exposed API keys get abused. Store the key in a separate config file that you add to `.gitignore`, or use an environment variable approach.
{{< /callout >}}

---

## Recommended Approach

{{% steps %}}

### Start with the API

Open the API URL directly in your browser and examine the JSON. Pay attention to the structure: What is the top-level object? Is the data in an array? What are the property names for the title, image, description, and other fields you need? Write these property names down -- you will reference them constantly.

### Build the HTML Skeleton

Create your page structure with empty containers where dynamic content will go. You need a search input, a search button, and a results container at minimum. You also need designated areas for each UI state (loading message, error message, empty state message). These containers start empty or hidden -- JavaScript will fill them later.

### Get Data to the Console First

Write a single fetch call that retrieves data from your API and logs it to the browser console with `console.log`. Do not try to display anything on the page yet. Just verify that you can successfully request data and see it in the console. This isolates the API communication from everything else.

### Render One Piece of Data

Take a single item from your API response and figure out how to display it on the page. If the API returns an array of 10 movies, just display the first one. Get the title, image, and description appearing correctly in the DOM before worrying about loops or multiple cards.

### Connect Fetch to Rendering

Replace any hardcoded test data with the actual API response. Loop through the results and render each item. At this point you should have a working search: type a query, click search, see results appear on the page.

### Add UI States One at a Time

Implement each state separately: **Initial state** first (what the page shows on load), then **loading** (show a message when fetch starts, hide it when fetch completes), then **success** (already done from step 5), then **empty** (check if results array is empty), then **error** (the catch block in your try/catch). Each state is a separate display concern.

### Add Your Second Interaction

You need at least 2 interaction types. Search is your first. For the second, consider: filtering results by category, sorting results by a property, clicking a card to expand details, or adding pagination. Pick the one that makes the most sense for your chosen API.

### Style and Make Responsive

Apply CSS for all five states. The loading spinner, error message, and empty state should all look intentional and polished -- not like afterthoughts. Use Flexbox or Grid for your results layout. Ensure the page works on mobile (no horizontal scrolling, readable text, tappable buttons).

### Test Edge Cases

Try searching with an empty input. Try special characters. Try a search term that returns no results. Disconnect your wifi and try searching (to test error handling). Try rapidly clicking the search button. Make sure your app handles all of these gracefully.

{{% /steps %}}

{{< callout type="important" >}}
**Build incrementally.** Do not try to build everything at once. Get one thing working, commit it, then move to the next. The progression is: fetch data to console, display one item, display all items, add UI states, add second interaction, style everything. Each step builds on the last.
{{< /callout >}}

---

## Key Concepts to Review

### async/await and Fetch (Mental Model)

Think of `fetch` like **ordering food delivery**. You place the order (call fetch with a URL), you wait for it to arrive (await the response), and eventually the food shows up (the data). If something goes wrong -- the restaurant is closed, the address is wrong, the delivery driver gets lost -- you need a plan for handling the problem (the catch block).

The general pattern works like this:

1. `fetch` sends a request to a URL and returns a **Promise** (a placeholder for data that has not arrived yet)
2. `await` pauses your function until the response arrives
3. The response object needs to be **converted from JSON** format into a JavaScript object you can work with
4. `try/catch` wraps the entire operation so that if anything fails at any step, your code does not crash -- it runs the catch block instead

The critical insight is that **two awaits are needed**: one for the network response and one for parsing the JSON body. Forgetting the second await is one of the most common mistakes.

### DOM Manipulation (Conceptual)

DOM manipulation is how you make your page dynamic. Instead of writing all the HTML by hand, JavaScript creates, modifies, and removes elements in real time. There are three main approaches for putting content on the page:

**Approach 1: createElement** -- You create elements one at a time using `document.createElement`, set their properties, and append them to the page. This gives you fine-grained control over each element but requires more lines of code.

**Approach 2: innerHTML with template literals** -- You build an HTML string using backtick template literals and assign it to a container's `innerHTML`. This is concise and easy to read, making it good for rendering cards or lists. However, be cautious: if you insert user-provided text directly into `innerHTML` without sanitizing it, you create a security risk (cross-site scripting).

**Approach 3: insertAdjacentHTML** -- You insert an HTML string at a specific position relative to an element (before it, after it, inside at the start, inside at the end). This is useful when you want to add content without replacing what is already there.

The tradeoff: `createElement` is safer but more verbose; `innerHTML` is concise but requires care with user input. For this assignment, either approach works -- pick the one you find more readable and be consistent.

### Event Listeners (Conceptual)

Your app needs to **respond to user actions**. Events are how the browser tells your JavaScript that something happened. The common events you will use:

- **Click events** on buttons -- the primary way users trigger searches or interact with results
- **Keydown events** -- listening for the Enter key so users can submit a search without clicking the button
- **Change events** on select/dropdown elements -- useful for category filtering or sorting
- **Input events** -- fire every time the user types a character, useful for real-time filtering or search suggestions

The pattern is always the same: select the element, call `addEventListener` with the event name and a function that runs when the event fires. Your function receives an **event object** with details about what happened.

### Error Handling (Conceptual)

Many things can go wrong when your app talks to an external API:

- **Network disconnected** -- the user's wifi drops or the server is unreachable
- **API is down** -- the server returns a 500 error or times out
- **Rate limit exceeded** -- you have made too many requests too quickly
- **Invalid response format** -- the API returns something unexpected
- **Empty results** -- the search term matched nothing (this is not an error, but a state you must handle)

The `try/catch` pattern is your safety net. Everything that might fail goes inside `try`. If any line throws an error, execution jumps immediately to `catch`, where you display a friendly message to the user instead of crashing the app. You should also check `response.ok` after a fetch to catch HTTP errors (like 404 or 500) that fetch does not throw automatically.

---

## Thinking Through UI States

Your app is a state machine. At any given moment, it is in exactly one of five states. User actions and API responses cause transitions between states:

```
                          +-----------+
                          |  Initial  |
                          +-----+-----+
                                |
                       user searches
                                |
                          +-----v-----+
                          |  Loading  |
                          +-----+-----+
                                |
               +----------------+----------------+
               |                |                |
          data received    no results       error occurs
               |                |                |
         +-----v-----+   +-----v-----+   +------v----+
         |  Success   |   |   Empty   |   |   Error   |
         +-----------+    +-----------+   +-----------+
```

From the **Success**, **Empty**, or **Error** states, the user can search again, which sends the app back to **Loading**.

{{< callout type="info" >}}
**Each state should have its own display function.** Think of them as separate "screens" your app can show. When entering a new state, the function should hide all other states and show only the content for the current state. This keeps your logic clean and prevents leftover content from previous states bleeding through.
{{< /callout >}}

---

## Project Structure

{{< filetree/container >}}
{{< filetree/folder name="cs300-assignment-3" >}}
{{< filetree/file name="index.html" >}}
{{< filetree/folder name="css" >}}
{{< filetree/file name="styles.css" >}}
{{< /filetree/folder >}}
{{< filetree/folder name="js" >}}
{{< filetree/file name="app.js" >}}
{{< /filetree/folder >}}
{{< filetree/folder name="images" >}}
{{< filetree/file name="(optional assets)" >}}
{{< /filetree/folder >}}
{{< filetree/file name="README.md" >}}
{{< /filetree/folder >}}
{{< /filetree/container >}}

{{< callout type="info" >}}
Keep your JavaScript in a separate `js/app.js` file, not inline in the HTML. This separation of concerns -- HTML for structure, CSS for presentation, JavaScript for behavior -- is a professional practice that makes your code easier to maintain and debug.
{{< /callout >}}

---

## Video Explanation Guide

{{% steps %}}

### Demo the App

Show your application working: search for something, show the loading state, display results, try a search that returns no results (empty state), and demonstrate error handling. Walk through each of the 5 UI states so the reviewer can see them all.

### Walk Through Your Code

Explain how your fetch function works, how you manipulate the DOM to display results, and how your event listeners connect user actions to app behavior. Point to specific parts of your code as you explain.

### Explain Your Structure and Challenges

How did you organize your JavaScript? Why did you choose the functions you created? What was the hardest part of working with the API, and how did you solve it?

{{% /steps %}}

{{< callout type="info" >}}
Upload your video directly to D2L. Typical length is 4-6 minutes. Demonstrating all 5 UI states clearly is more important than video length.
{{< /callout >}}

---

## Submission Checklist

Before submitting, verify everything:

- [ ] GitHub repo is public and named `cs300-assignment-3`
- [ ] App fetches real data from a public API using `async/await`
- [ ] DOM is updated dynamically (no page reloads)
- [ ] At least 2 user interactions work (search + one more)
- [ ] All 5 UI states are handled (initial, loading, success, empty, error)
- [ ] Error handling with `try/catch`
- [ ] Vanilla JavaScript only (no frameworks or libraries)
- [ ] Page is responsive on mobile
- [ ] README includes your name, API choice, live URL, and screenshots of UI states
- [ ] Pull Request created with description and live URL
- [ ] Video uploaded to D2L

---

## Troubleshooting & Common Mistakes

{{% details title="My fetch is returning undefined" closed="true" %}}
Make sure you are using `await` before `fetch()` AND `await` before `.json()`. Both are asynchronous operations that return Promises, and without `await`, you get the Promise object instead of the actual data. Also verify that your function is marked with the `async` keyword -- you cannot use `await` inside a regular function.
{{% /details %}}

{{% details title="CORS error when calling the API" closed="true" %}}
CORS (Cross-Origin Resource Sharing) errors happen when an API does not allow direct requests from a browser on a different domain. Not all APIs support browser requests. If you see a CORS error, first check the API documentation for a CORS-friendly endpoint. If none exists, try a different API from the recommended list. All APIs listed in this guide support browser requests.
{{% /details %}}

{{% details title="My results container shows [object Object]" closed="true" %}}
You are trying to display a JavaScript object directly as text. Objects cannot be displayed as strings meaningfully. You need to access specific properties within the object. For example, if your data looks like `{ title: "Pasta", image: "url..." }`, you need to use `data.title` and `data.image` -- not just `data`. Use `console.log(data)` to inspect the structure and find the exact property names.
{{% /details %}}

{{% details title="The loading spinner never goes away" closed="true" %}}
Make sure you hide the loading indicator in **both** the success path AND the error path of your `try/catch`. If you only hide loading in the `try` block and an error occurs, execution jumps to `catch` and the loading hide line never runs. Consider placing the loading hide in a `finally` block, which runs after either `try` or `catch` completes.
{{% /details %}}

{{% details title="My app works locally but not on GitHub Pages" closed="true" %}}
Check these common causes:

- **File paths**: All paths must be relative (e.g., `css/styles.css`), not absolute (not `/css/styles.css`). GitHub Pages serves from a subdirectory
- **HTTPS**: Your API URL must use `https://`, not `http://`. GitHub Pages is served over HTTPS and blocks mixed content
- **Case sensitivity**: File names are case-sensitive on GitHub Pages but not on Mac. If your file is `App.js` but your script tag says `app.js`, it will fail on GitHub Pages
- **Missing files**: Run `git status` to make sure all files are committed and pushed
  {{% /details %}}

{{% details title="I am getting rate limited by the API" closed="true" %}}
Some APIs limit how many requests you can make per minute or per day. If you are hitting rate limits during development, add a small delay between requests, or cache results locally so you do not re-fetch the same query repeatedly. Avoid calling the API on every keystroke -- use a button click or Enter key press to trigger searches instead.
{{% /details %}}

---

## Key Takeaways

{{< callout emoji="🎯" >}}

- **Test your API in the browser BEFORE writing any code** -- understand the JSON structure and property names first
- **Build incrementally** -- fetch first, display second, interactions third, styling last
- **All 5 UI states are required**: initial, loading, success, empty, error -- plan for all five from the start
- **async/await with try/catch** is the standard pattern for API calls -- two awaits are needed (one for fetch, one for JSON parsing)
- **Each UI state should be a separate display function** -- this keeps your code organized and prevents states from overlapping
- **DOM manipulation brings your page to life** -- this is the skill that separates static HTML pages from real web applications
- **Vanilla JavaScript only** (no frameworks) -- this forces you to understand the fundamentals that all frameworks are built on
- **Commit often and create a Pull Request** with a description and live URL for submission
  {{< /callout >}}

---

## Additional Resources

### Video Tutorials

{{< cards >}}
{{< card link="https://www.youtube.com/watch?v=Oive66jrwBs" title="Traversy Media: Fetch API Introduction" subtitle="Learn the Fetch API basics" icon="play" >}}
{{< card link="https://www.youtube.com/watch?v=cuEtnrL9-H0" title="Web Dev Simplified: Learn Fetch API in 6 Minutes" subtitle="Quick practical introduction" icon="play" >}}
{{< card link="https://www.youtube.com/playlist?list=PL4cUxeGkcC9jx2TTZk3IGWKSbtugYdrlu" title="The Net Ninja: Async JavaScript Tutorial" subtitle="Complete async JS playlist" icon="play" >}}
{{< /cards >}}

### Official Documentation

{{< cards >}}
{{< card link="https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch" title="MDN: Fetch API" subtitle="Official Fetch API guide and reference" icon="book-open" >}}
{{< card link="https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Promises" title="MDN: async/await" subtitle="Understanding Promises and async patterns" icon="book-open" >}}
{{< card link="https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction" title="MDN: DOM Manipulation" subtitle="Introduction to the Document Object Model" icon="book-open" >}}
{{< /cards >}}

### Tools

{{< cards >}}
{{< card link="https://github.com/public-apis/public-apis" title="Public APIs List" subtitle="Browse hundreds of free APIs for your project" icon="code" >}}
{{< card link="https://chromewebstore.google.com/search/json%20formatter" title="JSONFormatter Chrome Extension" subtitle="Format JSON responses in your browser for easy reading" icon="code" >}}
{{< /cards >}}
