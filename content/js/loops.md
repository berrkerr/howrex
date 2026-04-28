---
title: "JavaScript Loops Explained"
slug: "loops"
description: "Learn how to use for, while, do/while, for...of, and for...in loops in JavaScript with practical examples."
weight: 5
section: "js"
tags: ["javascript", "basics"]
---

Loops let you repeat a block of code multiple times without writing it out manually. JavaScript has several loop types — each suited to different situations.

## for loop

The most common loop. Use it when you know in advance how many times to repeat.

```js
for (let i = 0; i < 5; i++) {
  console.log(i);
}
// 0, 1, 2, 3, 4
```

The three parts inside `for()`:

1. **Initialization** — `let i = 0` — runs once before the loop starts
2. **Condition** — `i < 5` — checked before each iteration, loop stops when false
3. **Update** — `i++` — runs after each iteration

### Looping over an array by index

```js
const fruits = ["apple", "banana", "cherry"];

for (let i = 0; i < fruits.length; i++) {
  console.log(i, fruits[i]);
}
// 0 "apple"
// 1 "banana"
// 2 "cherry"
```

### Looping backwards

```js
const nums = [1, 2, 3, 4, 5];

for (let i = nums.length - 1; i >= 0; i--) {
  console.log(nums[i]);
}
// 5, 4, 3, 2, 1
```

## while loop

Repeats as long as a condition is true. Use it when you don't know in advance how many iterations you need.

```js
let count = 0;

while (count < 5) {
  console.log(count);
  count++;
}
// 0, 1, 2, 3, 4
```

The condition is checked **before** each iteration. If it's false from the start, the body never runs.

```js
let x = 10;

while (x < 5) {
  console.log(x); // never runs
}
```

### Practical while example

```js
let input = "";

while (input !== "quit") {
  input = prompt("Type something (or 'quit' to exit):");
}
```

### Infinite loop warning

Always make sure the condition will eventually become false — otherwise you'll create an infinite loop that freezes the browser or crashes Node.js:

```js
// Infinite loop — never changes i
while (true) {
  console.log("forever");
}

// Accidentally infinite — forgot to increment
let i = 0;
while (i < 5) {
  console.log(i);
  // i++ is missing!
}
```

## do...while loop

Like `while`, but the body runs **at least once** — the condition is checked after the first iteration.

```js
let count = 0;

do {
  console.log(count);
  count++;
} while (count < 5);
// 0, 1, 2, 3, 4
```

The difference matters when the condition starts as false:

```js
let x = 10;

do {
  console.log(x); // runs once, logs 10
} while (x < 5);  // then checks — false, loop ends
```

`do...while` is less common but useful for things like menus where you always want to show the prompt at least once.

## for...of loop

The cleanest way to loop over any iterable — arrays, strings, Maps, Sets, and more.

```js
const fruits = ["apple", "banana", "cherry"];

for (const fruit of fruits) {
  console.log(fruit);
}
// "apple"
// "banana"
// "cherry"
```

No index needed, no `.length`, no `i++`. Just the values directly.

### Looping over a string

```js
for (const char of "hello") {
  console.log(char);
}
// "h", "e", "l", "l", "o"
```

### Getting the index with for...of

Use `entries()` to get both index and value:

```js
const fruits = ["apple", "banana", "cherry"];

for (const [i, fruit] of fruits.entries()) {
  console.log(i, fruit);
}
// 0 "apple"
// 1 "banana"
// 2 "cherry"
```

### Looping over a Map

```js
const scores = new Map([
  ["Alice", 95],
  ["Bob", 87],
  ["Carol", 92],
]);

for (const [name, score] of scores) {
  console.log(`${name}: ${score}`);
}
// "Alice: 95"
// "Bob: 87"
// "Carol: 92"
```

## for...in loop

Loops over the **keys** of an object.

```js
const user = { name: "Alice", age: 30, city: "Istanbul" };

for (const key in user) {
  console.log(key, user[key]);
}
// "name" "Alice"
// "age" 30
// "city" "Istanbul"
```

### Do not use for...in on arrays

`for...in` iterates over enumerable properties — including any custom properties added to `Array.prototype`. This can produce unexpected results:

```js
const arr = [1, 2, 3];

for (const i in arr) {
  console.log(i);  // "0", "1", "2" — strings, not numbers
}
```

Use `for...of` for arrays, `for...in` for plain objects.

## Array methods vs loops

For arrays specifically, higher-order methods are often cleaner than explicit loops:

```js
const nums = [1, 2, 3, 4, 5];

// forEach — like for...of, no return value
nums.forEach(n => console.log(n));

// map — transform each element, returns new array
const doubled = nums.map(n => n * 2);      // [2, 4, 6, 8, 10]

// filter — keep elements matching condition
const evens = nums.filter(n => n % 2 === 0); // [2, 4]

// find — first element matching condition
const first = nums.find(n => n > 3);        // 4

// reduce — combine all elements into one value
const sum = nums.reduce((acc, n) => acc + n, 0); // 15
```

These are covered in depth in the Arrays article. For simple iteration, `for...of` and `forEach` are interchangeable — `for...of` supports `break` and `continue`, `forEach` does not.

## break and continue

### break

Exits the loop immediately:

```js
for (let i = 0; i < 10; i++) {
  if (i === 5) break;
  console.log(i);
}
// 0, 1, 2, 3, 4
```

Useful for stopping a search once you've found what you need:

```js
const users = ["Alice", "Bob", "Carol"];
let found = null;

for (const user of users) {
  if (user === "Bob") {
    found = user;
    break;  // no need to keep searching
  }
}
```

### continue

Skips the current iteration and moves to the next:

```js
for (let i = 0; i < 6; i++) {
  if (i % 2 === 0) continue;  // skip even numbers
  console.log(i);
}
// 1, 3, 5
```

### break and continue don't work with forEach

This is a common gotcha:

```js
[1, 2, 3, 4, 5].forEach(n => {
  if (n === 3) break;  // SyntaxError — can't use break in forEach
});

// Use for...of instead
for (const n of [1, 2, 3, 4, 5]) {
  if (n === 3) break;  // works fine
}
```

## Nested loops

Loops inside loops. Each iteration of the outer loop runs the full inner loop.

```js
for (let i = 1; i <= 3; i++) {
  for (let j = 1; j <= 3; j++) {
    console.log(`${i} x ${j} = ${i * j}`);
  }
}
// 1 x 1 = 1
// 1 x 2 = 2
// 1 x 3 = 3
// 2 x 1 = 2
// ... and so on
```

Be cautious with nested loops on large datasets — the number of operations grows fast (outer × inner iterations).

## Which loop should you use?

| Situation | Best choice |
|---|---|
| Known number of iterations | `for` |
| Unknown number of iterations | `while` |
| Must run at least once | `do...while` |
| Iterate array or iterable values | `for...of` |
| Iterate object keys | `for...in` |
| Transform/filter an array | `map`, `filter`, `reduce` |
| Need `break` or `continue` | `for`, `for...of`, `while` |

## Common mistakes

**Off-by-one errors:**

```js
// Intended to loop 5 times, but loops 6 times
for (let i = 0; i <= 5; i++) { }  // 0,1,2,3,4,5 — 6 iterations
for (let i = 0; i < 5; i++) { }   // 0,1,2,3,4   — 5 iterations
```

**Modifying an array while looping over it:**

```js
const arr = [1, 2, 3, 4, 5];

// Don't do this — skips elements
for (let i = 0; i < arr.length; i++) {
  if (arr[i] % 2 === 0) arr.splice(i, 1);
}

// Use filter instead
const odd = arr.filter(n => n % 2 !== 0);
```

**Using var instead of let in a for loop:**

```js
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}
// Logs: 3, 3, 3 — var leaks out of the loop block

for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}
// Logs: 0, 1, 2 — let is block-scoped
```

**Using for...in on arrays:**

```js
const arr = [10, 20, 30];

for (const i in arr) {
  console.log(typeof i); // "string" — keys are strings in for...in
}

// Use for...of
for (const val of arr) {
  console.log(typeof val); // "number" — actual values
}
```

## Summary

```js
// Classic for loop
for (let i = 0; i < n; i++) { }

// while — unknown iterations
while (condition) { }

// do...while — always runs once
do { } while (condition);

// for...of — iterate values
for (const item of iterable) { }

// for...in — iterate object keys
for (const key in object) { }

// break — exit loop early
// continue — skip to next iteration
```

For most array work, reach for `for...of` or array methods like `map` and `filter`. Use a classic `for` loop when you need the index or precise control over iteration. Use `while` when looping until a condition changes.
