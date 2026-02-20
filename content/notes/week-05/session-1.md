---
title: "JavaScript Fundamentals"
weight: 1
toc: true
---

## Introduction

So far you have used **HTML** to define page structure and **CSS** to control appearance. Neither can make a page *do* anything. For interactivity -- responding to clicks, validating forms, fetching data -- you need **JavaScript**.

```
The Three Languages of the Web:

  HTML          CSS           JavaScript
┌──────────┐  ┌──────────┐  ┌──────────┐
│Structure │  │  Style   │  │ Behavior │
│ headings │  │  colors  │  │  clicks  │
│  lists   │  │  fonts   │  │  forms   │
│  links   │  │  layout  │  │  data    │
└──────────┘  └──────────┘  └──────────┘
   bones         skin         muscles
```

JavaScript is the **only programming language** that runs natively in browsers. Created in 1995, it now powers everything from form validation to complex applications like Gmail and the React apps you will build later in this course.

---

## The Browser Console

The fastest way to start writing JavaScript is the **browser console** -- a built-in tool where you can type code and see results instantly.

{{% steps %}}

### Open DevTools

- **Mac:** `Cmd + Option + I`
- **Windows/Linux:** `F12` or `Ctrl + Shift + I`

### Go to the Console Tab

Click the **Console** tab at the top of the DevTools panel.

### Type Your First Command

```javascript
console.log("Hello, World!");
```

Press **Enter**. You should see `Hello, World!` printed below.

### Experiment

```javascript
2 + 2
"Hello" + " " + "World"
Math.random()
```

{{% /steps %}}

{{< callout type="info" >}}
`console.log()` prints a value to the console. Think of it as "show me what this value is." You will use it constantly for testing and debugging.
{{< /callout >}}

### Connecting JavaScript to HTML

Real JavaScript lives in `.js` files linked to your HTML with the `<script>` tag:

```html
<body>
    <h1>Hello!</h1>
    <!-- Add script at end of body -->
    <script src="script.js"></script>
</body>
```

{{< callout type="important" >}}
Place `<script>` at the **end of `<body>`**. This ensures all HTML elements are loaded before your JavaScript runs.
{{< /callout >}}

{{< filetree/container >}}
  {{< filetree/folder name="my-project" >}}
    {{< filetree/file name="index.html" >}}
    {{< filetree/file name="style.css" >}}
    {{< filetree/file name="script.js" >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}}

---

## Variables

A **variable** is a named container that stores a value. JavaScript has two modern keywords for creating variables:

```javascript
const name = "Alice";   // Cannot be reassigned
let score = 85;         // Can be reassigned

score = 90;             // OK -- let allows this
// name = "Bob";        // ERROR -- const prevents this
```

| Keyword | Can Reassign? | When to Use |
|---------|--------------|-------------|
| `const` | No | **Default choice.** Use for values that won't change. |
| `let` | Yes | Only when the value needs to change later. |
| `var` | Yes | **Never.** Old syntax (pre-2015) with confusing behavior. |

{{< callout type="important" >}}
**Use `const` by default.** Only use `let` when you have a specific reason the value needs to change. Never use `var`.
{{< /callout >}}

### Naming Conventions

- Use **camelCase**: `firstName`, `totalScore`, `isLoggedIn`
- Must start with a letter, `_`, or `$`
- Names are **case-sensitive** (`score` and `Score` are different)

```javascript
// Good:
const firstName = "Alice";
const isLoggedIn = true;

// Bad:
const x = "Alice";          // Too vague
const first_name = "Alice"; // snake_case is not the JS convention
```

---

## Data Types

Every value in JavaScript has a **type** that determines how it behaves.

{{< tabs items="String,Number,Boolean,null & undefined" >}}
  {{< tab >}}

**String** -- text data, wrapped in quotes.

```javascript
const greeting = "Hello, World!";   // Double quotes
const name = 'Alice';               // Single quotes
const message = `Score: ${95}`;     // Backticks (template literal)

console.log(typeof greeting); // "string"
```

**Useful string methods:**

```javascript
const text = "JavaScript";
text.length              // 10
text.toUpperCase()       // "JAVASCRIPT"
text.includes("Script")  // true
```

  {{< /tab >}}
  {{< tab >}}

**Number** -- any numeric value (integer or decimal).

```javascript
const age = 25;
const price = 9.99;

console.log(typeof age); // "number"
```

JavaScript does not distinguish between integers and decimals -- they are all `number`.

**Special values:** `Infinity`, `-Infinity`, `NaN` ("Not a Number" -- result of invalid math like `"abc" * 2`).

  {{< /tab >}}
  {{< tab >}}

**Boolean** -- `true` or `false`. The foundation of conditional logic.

```javascript
const isLoggedIn = true;
const isAdult = age >= 18; // true or false depending on age

console.log(typeof isLoggedIn); // "boolean"
```

  {{< /tab >}}
  {{< tab >}}

**null** -- intentionally empty. You set this deliberately.

**undefined** -- not yet assigned. JavaScript sets this automatically.

```javascript
let selectedUser = null;  // "I set this to nothing on purpose"
let score;                // undefined -- "no value assigned yet"
```

```
  null:       "The box is intentionally empty"
              ┌──────────┐
              │  (empty) │  <-- You put nothing in it on purpose
              └──────────┘

  undefined:  "The box was never opened"
              ┌──────────┐
              │    ???   │  <-- No value assigned yet
              └──────────┘
```

  {{< /tab >}}
{{< /tabs >}}

### Template Literals

**Template literals** use backticks and `${expression}` to embed values in strings:

```javascript
const name = "Alice";
const age = 20;

// Old way (concatenation):
console.log("Name: " + name + ", Age: " + age);

// Modern way (template literals):
console.log(`Name: ${name}, Age: ${age}`);
console.log(`Next year: ${age + 1}`);
```

{{< callout type="info" >}}
**Where is the backtick key?** Below the **Esc** key on most keyboards, on the same key as the tilde (`~`).
{{< /callout >}}

### The typeof Operator

```javascript
typeof "hello"      // "string"
typeof 42           // "number"
typeof true         // "boolean"
typeof undefined    // "undefined"
typeof null         // "object"  <-- Famous JS bug!
```

{{< callout type="warning" >}}
`typeof null` returns `"object"` instead of `"null"`. This is a [known bug](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof#typeof_null) from 1995 that was never fixed.
{{< /callout >}}

---

## Operators

### Arithmetic Operators

| Operator | Name | Example | Result |
|----------|------|---------|--------|
| `+` | Addition | `10 + 3` | `13` |
| `-` | Subtraction | `10 - 3` | `7` |
| `*` | Multiplication | `10 * 3` | `30` |
| `/` | Division | `10 / 3` | `3.333...` |
| `%` | Remainder | `10 % 3` | `1` |
| `**` | Exponentiation | `2 ** 3` | `8` |

```javascript
let count = 10;
count += 5;    // count = count + 5  (now 15)
count -= 3;    // count = count - 3  (now 12)
count++;       // count = count + 1  (now 13)
```

### Comparison Operators

| Operator | Name | Example | Result |
|----------|------|---------|--------|
| `===` | Strict equality | `5 === 5` | `true` |
| `!==` | Strict inequality | `5 !== 3` | `true` |
| `>` | Greater than | `5 > 3` | `true` |
| `<` | Less than | `5 < 3` | `false` |
| `>=` | Greater or equal | `5 >= 5` | `true` |
| `<=` | Less or equal | `5 <= 3` | `false` |

{{< callout type="error" >}}
**NEVER use `==` or `!=`.** ALWAYS use `===` and `!==`.

Double equals (`==`) converts types before comparing, causing bugs:

```javascript
5 == "5"     // true   WRONG -- a number should not equal a string
0 == false   // true   WRONG -- zero should not equal false

5 === "5"    // false  CORRECT -- different types are not equal
0 === false  // false  CORRECT
```

Triple equals (`===`) checks both **value AND type** -- always use it.
{{< /callout >}}

### Logical Operators

| Operator | Name | Meaning |
|----------|------|---------|
| `&&` | AND | Both must be `true` |
| `\|\|` | OR | At least one must be `true` |
| `!` | NOT | Flips the value |

```javascript
const age = 20;
const hasID = true;

if (age >= 18 && hasID) {
    console.log("Entry allowed");  // Both true
}

if (!hasID) {
    console.log("ID required");    // Runs if hasID is false
}
```

### Type Coercion

JavaScript sometimes converts types automatically, which can be surprising:

```javascript
"5" + 3    // "53"  (string wins -- concatenation)
"5" - 3    // 2     (math wins -- subtraction)
```

The `+` operator can mean concatenation *or* addition. When one side is a string, it concatenates. The `-`, `*`, `/` operators only do math, so they convert strings to numbers.

{{< callout type="info" >}}
You do not need to memorize coercion rules. Just use `===` and be careful with `+` on mixed types.
{{< /callout >}}

---

## Functions

A **function** is a reusable block of code. Instead of repeating the same logic, you write it once and call it whenever needed.

```
  Input (Parameters)    Function Body       Output (Return Value)
  ┌───────────┐      ┌──────────────┐     ┌───────────┐
  │ name      │─────>│ process the  │────>│  result   │
  │ score     │      │ data         │     └───────────┘
  └───────────┘      └──────────────┘
```

### Three Ways to Define Functions

{{< tabs items="Declaration,Expression,Arrow" >}}
  {{< tab >}}

**Function Declaration** -- the classic way.

```javascript
function greet(name) {
    return `Hello, ${name}!`;
}

greet("Alice"); // "Hello, Alice!"
```

- Uses the `function` keyword
- Can be called *before* its definition (hoisting)

  {{< /tab >}}
  {{< tab >}}

**Function Expression** -- stored in a variable.

```javascript
const greet = function(name) {
    return `Hello, ${name}!`;
};

greet("Alice"); // "Hello, Alice!"
```

- Assigned to a `const` variable
- Cannot be called before its definition

  {{< /tab >}}
  {{< tab >}}

**Arrow Function** -- modern, concise.

```javascript
// Full form:
const greet = (name) => {
    return `Hello, ${name}!`;
};

// Short form (implicit return):
const greet = (name) => `Hello, ${name}!`;

greet("Alice"); // "Hello, Alice!"
```

- Uses `=>` (the "fat arrow")
- One-liner? Omit `{}` and `return` for implicit return
- **Preferred for short functions**

  {{< /tab >}}
{{< /tabs >}}

**Same function, three ways:**

```javascript
function double(n) { return n * 2; }          // Declaration
const double = function(n) { return n * 2; };  // Expression
const double = (n) => n * 2;                   // Arrow
// All three: double(5) returns 10
```

{{< callout type="info" >}}
**Which to use?** Arrow functions for short functions. Declarations for longer, named functions. Expressions are less common but you will encounter them.
{{< /callout >}}

### Parameters and Return Values

```javascript
// Two parameters, one return:
const add = (a, b) => a + b;
add(5, 3); // 8

// No parameters:
const sayHi = () => console.log("Hi!");

// Multiple statements need {} and explicit return:
const calculate = (a, b) => {
    const sum = a + b;
    const product = a * b;
    return { sum, product };
};
```

### Default Parameters

```javascript
const greet = (name = "World") => `Hello, ${name}!`;

greet("Alice"); // "Hello, Alice!"
greet();        // "Hello, World!"
```

---

## Conditionals

Conditionals let your code make **decisions** based on whether a condition is true or false.

### if / else / else if

```javascript
const score = 85;

if (score >= 90) {
    console.log("Grade: A");
} else if (score >= 80) {
    console.log("Grade: B");
} else if (score >= 70) {
    console.log("Grade: C");
} else {
    console.log("Grade: F");
}
// Output: "Grade: B"
```

```
How if/else evaluates (score = 85):

  score >= 90?  ── NO ────┐
                           ▼
  score >= 80?  ── YES ──> Run this block ("Grade: B")
                           │
  score >= 70?  ── SKIP ──┘
  else          ── SKIP
```

### Ternary Operator

A shorthand for simple two-way `if/else`:

```javascript
// condition ? valueIfTrue : valueIfFalse

const age = 20;
const status = age >= 18 ? "Adult" : "Minor";
// Same as: if (age >= 18) { status = "Adult"; } else { status = "Minor"; }
```

{{< callout type="warning" >}}
Only use ternary for **simple, two-way choices**. For complex logic, use `if/else` -- readability matters more than brevity.
{{< /callout >}}

---

## Preview: Arrays

Arrays are covered fully in Week 6. Here is a brief introduction.

An **array** is an ordered list of values, where each item has a position (**index**) starting at 0.

```javascript
const colors = ["red", "green", "blue"];

colors[0]       // "red"
colors[1]       // "green"
colors.length   // 3

colors.push("yellow"); // Add to end
colors.forEach((color) => console.log(color)); // Loop through
```

---

## Preview: Objects

Also covered fully in Week 6. An **object** is a collection of **key-value pairs**.

```javascript
const student = {
    name: "Alice",
    age: 20,
    major: "Computer Science"
};

student.name           // "Alice" (dot notation)
student["age"]         // 20 (bracket notation)
student.age = 21;      // Modify a property
```

{{< callout type="info" >}}
Even though `student` is declared with `const`, you can change its properties. `const` prevents reassigning the variable itself, not modifying its contents.
{{< /callout >}}

---

## Preview: Loops

Full coverage in Week 6. Here are the basics.

```javascript
// for loop:
for (let i = 0; i < 5; i++) {
    console.log(`Iteration ${i}`);
}

// for...of (loop through array values):
const colors = ["red", "green", "blue"];
for (const color of colors) {
    console.log(color);
}
```

---

## Common Patterns

### Function Returning Different Values Based on Input

```javascript
const getGrade = (score) => {
    if (score >= 90) return "A";
    if (score >= 80) return "B";
    if (score >= 70) return "C";
    if (score >= 60) return "D";
    return "F";
};
```

No `else` needed when each branch uses `return` -- the function exits immediately.

### Template Literal for Formatted Output

```javascript
const formatPrice = (name, price, qty) => {
    const total = price * qty;
    return `${name} x${qty}: $${total.toFixed(2)}`;
};
formatPrice("Widget", 9.99, 3); // "Widget x3: $29.97"
```

### Complete Report (Combines Everything)

```javascript
const printReport = (name, score) => {
    const grade = getGrade(score);
    const passing = grade !== "F" ? "Passing" : "Failing";

    console.log("=== Grade Report ===");
    console.log(`Student: ${name}`);
    console.log(`Score: ${score}, Grade: ${grade}`);
    console.log(`Status: ${passing}`);
    console.log("====================");
};

printReport("Alice", 95);
printReport("Bob", 55);
```

---

## Troubleshooting & Common Mistakes

{{% details title="ReferenceError: x is not defined" closed="true" %}}
You are using a variable that does not exist. Check for typos, and remember JavaScript is case-sensitive (`userName` vs `username`).

```javascript
// FIX: Declare the variable before using it
const userName = "Alice";
console.log(userName);
```
{{% /details %}}

{{% details title="TypeError: x is not a function" closed="true" %}}
You are trying to call something that is not a function. Check that you did not accidentally overwrite a function name with a non-function value.

```javascript
// WRONG:
const greet = "Hello";
greet("Alice"); // TypeError!

// FIX:
const greet = (name) => `Hello, ${name}!`;
```
{{% /details %}}

{{% details title="TypeError: Assignment to constant variable" closed="true" %}}
You tried to reassign a `const` variable. Use `let` if the value needs to change.

```javascript
// WRONG:
const score = 85;
score = 90; // TypeError!

// FIX:
let score = 85;
score = 90; // OK
```
{{% /details %}}

{{% details title="SyntaxError: Unexpected token" closed="true" %}}
Usually means a typo: missing parenthesis, extra bracket, or using `=` instead of `===`.

```javascript
// WRONG:
if (score = 90) { ... }  // Assigns 90 to score!

// FIX:
if (score === 90) { ... }
```
{{% /details %}}

{{% details title="Function returns undefined" closed="true" %}}
With curly braces in arrow functions, you **must** use `return`. Without curly braces, return is implicit.

```javascript
// WRONG (no return):
const double = (n) => { n * 2; };
double(5); // undefined

// FIX:
const double = (n) => { return n * 2; };
// Or:
const double = (n) => n * 2;
```
{{% /details %}}

{{% details title="NaN showing up unexpectedly" closed="true" %}}
A math operation received a non-numeric value. Make sure your variables hold numbers before doing math.

```javascript
"hello" * 5         // NaN
undefined + 1       // NaN
Number("10") + 5    // 15 (convert string to number first)
```
{{% /details %}}

---

## Key Takeaways

{{< callout emoji="🎯" >}}
- **JavaScript adds behavior** to web pages -- HTML is structure, CSS is style, JS is interactivity
- The **browser console** (F12) is your best tool for learning and testing
- Use **`const` by default**, `let` when needed, **never `var`**
- Five basic data types: **string, number, boolean, null, undefined**
- **Template literals** use backticks and `${expression}` for cleaner strings
- **Always use `===`** for comparison (never `==`)
- **Arrow functions** (`=>`) are the modern default for short functions
- `if/else` for branching, **ternary** for simple two-way choices
- With `{}` in arrow functions, you **must** write `return` explicitly
{{< /callout >}}

---

## Additional Resources

### Official Documentation

{{< cards >}}
  {{< card link="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide" title="MDN JavaScript Guide" subtitle="Comprehensive official documentation" icon="book-open" >}}
  {{< card link="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference" title="MDN JavaScript Reference" subtitle="Complete language reference" icon="document-text" >}}
{{< /cards >}}

### Interactive Tutorials

{{< cards >}}
  {{< card link="https://javascript.info/first-steps" title="javascript.info" subtitle="Modern JavaScript Tutorial - start here" icon="academic-cap" >}}
  {{< card link="https://eloquentjavascript.net/" title="Eloquent JavaScript" subtitle="Free online textbook (Chapters 1-4)" icon="book-open" >}}
{{< /cards >}}

### Practice & Exploration

{{< cards >}}
  {{< card link="https://jsfiddle.net/" title="JSFiddle" subtitle="Online code playground for experiments" icon="code" >}}
  {{< card link="https://developer.chrome.com/docs/devtools/console/" title="Chrome DevTools Console" subtitle="Official guide to the browser console" icon="play" >}}
{{< /cards >}}
