---
title: "JavaScript: Arrays, Objects & Data"
weight: 2
toc: true
---

## Introduction

In Session 1, you learned the building blocks of JavaScript: variables, data types, functions, and conditionals. Those are essential, but they only let you work with one piece of data at a time. Real applications need to store and process **collections** of data — a list of students, a set of products, a series of scores.

This is where **arrays** and **objects** come in. Arrays let you store ordered lists of values, and objects let you group related data together. Combined with **array methods** like `map`, `filter`, and `forEach`, you gain the ability to process entire datasets with a single expression.

These concepts are not just academic — they are used in every JavaScript framework, every API response, and every React component you will build in this course.

---

## Arrays

An **array** is an ordered list of values. Each value sits at a numbered position called an **index**.

### Creating Arrays

```javascript
const colors = ["red", "green", "blue"];
const scores = [95, 82, 73, 68, 91];
const mixed = ["Alice", 20, true]; // Possible, but usually stick to one type
```

### Accessing Elements

Array indices start at **0**, not 1. This is a universal convention across nearly all programming languages.

```javascript
const colors = ["red", "green", "blue"];

console.log(colors[0]);     // "red"   (first item)
console.log(colors[1]);     // "green" (second item)
console.log(colors[2]);     // "blue"  (third item)
console.log(colors.length); // 3
```

```
Index:   0        1         2
      ┌────────┬─────────┬──────┐
      │ "red"  │ "green" │"blue"│
      └────────┴─────────┴──────┘
```

{{< callout type="important" >}}
The last index in an array is always `length - 1`. A 5-item array has indices 0, 1, 2, 3, 4.
{{< /callout >}}

### Modifying Arrays

Even when declared with `const`, you can change an array's **contents** (add, remove items). `const` only prevents reassigning the variable itself.

```javascript
const fruits = ["apple", "banana"];

// Add to end
fruits.push("cherry");
console.log(fruits); // ["apple", "banana", "cherry"]

// Remove from end
const removed = fruits.pop();
console.log(removed); // "cherry"
console.log(fruits);  // ["apple", "banana"]

// Check if item exists
console.log(fruits.includes("apple")); // true
console.log(fruits.includes("grape")); // false
```

### Common Array Methods Reference

| Method | What It Does | Returns |
|--------|-------------|---------|
| `push(item)` | Add item to end | New length |
| `pop()` | Remove last item | The removed item |
| `includes(item)` | Check if item exists | `true` or `false` |
| `indexOf(item)` | Find position of item | Index, or `-1` if not found |
| `length` | Number of items (property, not method) | Number |

{{< callout type="warning" >}}
Accessing an index that does not exist returns `undefined` — JavaScript will not throw an error. Always check `.length` if you are unsure whether an index is valid.
```javascript
const colors = ["red", "green", "blue"];
console.log(colors[99]); // undefined (no error!)
```
{{< /callout >}}

---

## Loops

Loops let you run the same code once for each item in an array. Without loops, processing a list of 100 items would require 100 lines of code.

### The `for` Loop

The classic loop gives you control over the index:

```javascript
const names = ["Alice", "Bob", "Charlie", "Diana"];

for (let i = 0; i < names.length; i++) {
    console.log(`${i + 1}. ${names[i]}`);
}
// Output:
// 1. Alice
// 2. Bob
// 3. Charlie
// 4. Diana
```

The three parts inside the parentheses:
1. **Initialization:** `let i = 0` — start at index 0
2. **Condition:** `i < names.length` — keep going while true
3. **Update:** `i++` — add 1 to `i` after each iteration

### The `for...of` Loop

The modern approach — cleaner when you do not need the index:

```javascript
const names = ["Alice", "Bob", "Charlie", "Diana"];

for (const name of names) {
    console.log(`Hello, ${name}!`);
}
```

### When to Use Which

| Situation | Use |
|-----------|-----|
| You need the index number (numbering, counting) | `for` loop |
| You just need each value | `for...of` loop |
| You want cleaner, modern syntax | `for...of` loop |

{{< callout type="info" >}}
Use `for...of` most of the time. Only reach for the classic `for` loop when you specifically need the index.
{{< /callout >}}

### Loop + Conditional Example

Combining loops with conditionals from Session 1:

```javascript
const scores = [85, 92, 73, 96, 81];
let highest = 0;

for (const score of scores) {
    if (score > highest) {
        highest = score;
    }
}

console.log(`Highest score: ${highest}`); // 96
```

---

## Array Methods: forEach, map, filter

JavaScript arrays come with built-in methods that replace manual loops with cleaner, more expressive code. These three methods are the most important in modern JavaScript — you will use them constantly in React.

Each method takes a **callback function**: a function you pass as an argument. The method calls your function once for each item in the array.

### forEach — Do Something with Each Item

`forEach` replaces a basic loop. It calls your function once per item and returns nothing.

```javascript
const names = ["Alice", "Bob", "Charlie"];

names.forEach((name) => {
    console.log(`Hello, ${name}!`);
});
// Hello, Alice!
// Hello, Bob!
// Hello, Charlie!
```

You can also access the index as a second parameter:

```javascript
names.forEach((name, index) => {
    console.log(`${index + 1}. ${name}`);
});
// 1. Alice
// 2. Bob
// 3. Charlie
```

### map — Transform Each Item

`map` creates a **new array** by transforming each element. The original array stays unchanged.

```javascript
const scores = [85, 92, 73, 96, 81];

const doubled = scores.map((score) => score * 2);
console.log(doubled); // [170, 184, 146, 192, 162]
console.log(scores);  // [85, 92, 73, 96, 81] — unchanged!

const labels = scores.map((score) => `Score: ${score}`);
console.log(labels); // ["Score: 85", "Score: 92", ...]
```

{{< callout type="important" >}}
`map` is the **most important array method for React**. In React, you render lists by mapping over arrays: `students.map(s => <Card name={s.name} />)`.
{{< /callout >}}

### filter — Keep Items That Pass a Test

`filter` creates a **new array** containing only the items where your callback returned `true`.

```javascript
const scores = [85, 92, 73, 96, 81];

const passing = scores.filter((score) => score >= 80);
console.log(passing); // [85, 92, 96, 81]

const failing = scores.filter((score) => score < 80);
console.log(failing); // [73]
```

If no items pass the test, `filter` returns an empty array `[]`.

### Chaining Methods

Because `filter` and `map` both return arrays, you can chain them:

```javascript
const scores = [85, 92, 73, 96, 81];

const passingLabels = scores
    .filter((score) => score >= 80)
    .map((score) => `${score} — Pass`);

console.log(passingLabels);
// ["85 — Pass", "92 — Pass", "96 — Pass", "81 — Pass"]
```

### Quick Comparison

| Method | Returns | Original Changed? | Use When... |
|--------|---------|--------------------|-------------|
| `forEach` | Nothing (`undefined`) | No | You want to **do** something (print, log) |
| `map` | New array (same length) | No | You want to **transform** each item |
| `filter` | New array (maybe shorter) | No | You want to **keep** certain items |

{{< callout type="info" >}}
**Memory tip:** forEach = "for each, DO this" | map = "map to new values" | filter = "filter OUT what fails"
{{< /callout >}}

---

## Objects

An **object** is a collection of related data stored as **key-value pairs**. Think of it like a form with labeled fields — each field has a name (key) and a value.

### Creating Objects

```javascript
const student = {
    name: "Alice",
    age: 20,
    major: "Computer Science",
    gpa: 3.7,
    isEnrolled: true
};
```

Keys are on the left, values on the right, separated by colons. Pairs are separated by commas.

### Accessing Properties

**Dot notation** (most common):
```javascript
console.log(student.name);  // "Alice"
console.log(student.age);   // 20
```

**Bracket notation** (needed when the key is stored in a variable):
```javascript
const field = "major";
console.log(student[field]); // "Computer Science"
```

{{< callout type="warning" >}}
`student.field` looks for a property literally named `"field"`. `student[field]` uses the **value** of the variable `field` as the key. These are different!
{{< /callout >}}

### Modifying Properties

```javascript
student.gpa = 3.8;                // Update existing property
student.email = "alice@neiu.edu"; // Add a new property
```

Like arrays, `const` prevents reassigning the variable, but you can still change the object's properties.

---

## Arrays of Objects

The combination of arrays and objects is the **most common data pattern in programming**. Each object in the array represents one record (a student, a product, a blog post).

### Creating Arrays of Objects

```javascript
const students = [
    { name: "Alice", score: 95 },
    { name: "Bob", score: 73 },
    { name: "Charlie", score: 88 },
    { name: "Diana", score: 62 }
];
```

### Accessing Data

Combine array indexing with dot notation:

```javascript
console.log(students[0]);        // { name: "Alice", score: 95 }
console.log(students[0].name);   // "Alice"
console.log(students[1].score);  // 73
```

### Using Array Methods with Objects

This is where everything comes together:

```javascript
// Get all names
const names = students.map((s) => s.name);
console.log(names); // ["Alice", "Bob", "Charlie", "Diana"]

// Find students scoring 80+
const honor = students.filter((s) => s.score >= 80);
console.log(honor);
// [{ name: "Alice", score: 95 }, { name: "Charlie", score: 88 }]

// Print each student's info
students.forEach((s) => {
    console.log(`${s.name}: ${s.score}`);
});
```

{{< callout type="important" >}}
This pattern — an array of objects processed with `map`, `filter`, and `forEach` — is how virtually every web application works. APIs return arrays of objects. Databases return arrays of objects. React renders lists by mapping over arrays of objects. Master this pattern and you understand how real apps work.
{{< /callout >}}

---

## Common Patterns

### Pattern 1: Transform Data with map

**Use case:** You have raw data and need to create a derived version.

```javascript
const getGrade = (score) => {
    if (score >= 90) return "A";
    if (score >= 80) return "B";
    if (score >= 70) return "C";
    if (score >= 60) return "D";
    return "F";
};

const roster = [
    { name: "Alice", score: 95 },
    { name: "Bob", score: 73 }
];

const withGrades = roster.map((student) => {
    return {
        name: student.name,
        score: student.score,
        grade: getGrade(student.score)
    };
});
// [{ name: "Alice", score: 95, grade: "A" }, { name: "Bob", score: 73, grade: "C" }]
```

### Pattern 2: Filter + Count

**Use case:** Find how many items match a condition.

```javascript
const passing = students.filter((s) => s.score >= 60);
console.log(`${passing.length} out of ${students.length} students passing`);
```

### Pattern 3: Filter + Map (Chain)

**Use case:** Get specific data from a subset.

```javascript
const honorNames = students
    .filter((s) => s.score >= 90)
    .map((s) => s.name);
// Only names of students with 90+
```

---

## Troubleshooting & Common Mistakes

{{% details title="Off-by-one error: accessing array[length]" closed="true" %}}
```javascript
const colors = ["red", "green", "blue"];
console.log(colors[colors.length]); // undefined!
// Should be: colors[colors.length - 1] for the last item
```
The last valid index is `length - 1`, not `length`.
{{% /details %}}

{{% details title="Forgetting that map returns a new array" closed="true" %}}
```javascript
const scores = [85, 92, 73];
scores.map((s) => s * 2); // This returns [170, 184, 146] but you didn't save it!

// Fix: assign the result to a variable
const doubled = scores.map((s) => s * 2);
```
{{% /details %}}

{{% details title="Using forEach when you need map" closed="true" %}}
```javascript
// Wrong: forEach returns undefined
const result = names.forEach((name) => name.toUpperCase()); // result is undefined!

// Right: use map to create a new array
const result = names.map((name) => name.toUpperCase());
```
{{% /details %}}

{{% details title="Object dot notation vs bracket notation confusion" closed="true" %}}
```javascript
const student = { name: "Alice", major: "CS" };
const field = "major";

console.log(student.field);   // undefined (looks for property named "field")
console.log(student[field]);  // "CS" (uses the VALUE of variable field)
```
Use bracket notation when the key is stored in a variable.
{{% /details %}}

{{% details title="Missing return in map callback" closed="true" %}}
```javascript
// Wrong: curly braces without return
const doubled = scores.map((s) => { s * 2 }); // [undefined, undefined, ...]

// Fix option 1: add return
const doubled = scores.map((s) => { return s * 2; });

// Fix option 2: remove curly braces (implicit return)
const doubled = scores.map((s) => s * 2);
```
Arrow functions with curly braces `{}` require an explicit `return`. Without braces, the return is implicit.
{{% /details %}}

---

## Key Takeaways

{{< callout emoji="🎯" >}}
- **Arrays** are ordered lists. Index starts at **0**. Use `push`/`pop` to add/remove, `.length` for count.
- **Loops**: Use `for...of` when you just need values, `for` when you need the index.
- **forEach** does something with each item (returns nothing).
- **map** transforms each item into a new array (same length, original unchanged).
- **filter** keeps items that pass a test (new array, possibly shorter).
- **Objects** store data as key-value pairs. Access with dot notation (`obj.key`) or bracket notation (`obj[variable]`).
- **Arrays of objects** are the most common data pattern — master `map` and `filter` on arrays of objects and you are ready for React.
- None of these array methods change the original array. They create new arrays (immutability).
{{< /callout >}}

---

## Additional Resources

{{< cards >}}
  {{< card link="https://javascript.info/array" title="javascript.info: Arrays" subtitle="Interactive tutorial on JavaScript arrays" icon="book-open" >}}
  {{< card link="https://javascript.info/array-methods" title="javascript.info: Array Methods" subtitle="Comprehensive guide to map, filter, forEach, and more" icon="book-open" >}}
  {{< card link="https://javascript.info/object" title="javascript.info: Objects" subtitle="Everything about JavaScript objects" icon="book-open" >}}
  {{< card link="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array" title="MDN: Array Reference" subtitle="Official documentation for all array methods" icon="code" >}}
  {{< card link="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_objects" title="MDN: Working with Objects" subtitle="Official guide to JavaScript objects" icon="code" >}}
{{< /cards >}}
