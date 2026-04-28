---
title: "JavaScript Functions Explained"
slug: "functions"
description: "Learn how to write and use functions in JavaScript — declarations, expressions, parameters, return values, and scope."
section: "js"
tags: ["javascript", "functions"]
---

A function is a reusable block of code that performs a specific task. Instead of writing the same logic repeatedly, you write it once as a function and call it whenever you need it.

Functions are the most important building block in JavaScript. Almost everything in the language — events, callbacks, promises, modules — revolves around them.

## Declaring a function

The most common way to create a function is a **function declaration**:

```js
function greet(name) {
  return "Hello, " + name + "!";
}
```

Three parts:

- **`function` keyword** — tells JavaScript you're defining a function
- **Name** — `greet` — how you'll call it later
- **Body** — the code inside `{ }` that runs when the function is called

## Calling a function

Write the function name followed by parentheses:

```js
greet("Alice");        // "Hello, Alice!"
greet("Bob");          // "Hello, Bob!"
```

Each call runs the function body from top to bottom and returns the result.

## Parameters and arguments

**Parameters** are the variable names listed in the function definition. **Arguments** are the actual values you pass when calling.

```js
function add(a, b) {    // a and b are parameters
  return a + b;
}

add(3, 5);              // 3 and 5 are arguments — returns 8
add(10, 20);            // returns 30
```

### Default parameters

If an argument is not provided, the parameter is `undefined` by default. You can set a fallback:

```js
function greet(name = "stranger") {
  return "Hello, " + name + "!";
}

greet("Alice");  // "Hello, Alice!"
greet();         // "Hello, stranger!"
```

### Rest parameters

Collect any number of arguments into an array with `...`:

```js
function sum(...numbers) {
  return numbers.reduce((total, n) => total + n, 0);
}

sum(1, 2, 3);       // 6
sum(1, 2, 3, 4, 5); // 15
```

## The return statement

`return` sends a value back to wherever the function was called. Execution stops at `return` — any code after it is ignored.

```js
function multiply(a, b) {
  return a * b;
  console.log("done"); // never runs
}

const result = multiply(4, 5); // result = 20
```

A function without a `return` statement returns `undefined`:

```js
function sayHi() {
  console.log("Hi!");
}

const result = sayHi(); // logs "Hi!", result is undefined
```

## Function expressions

You can assign a function to a variable. This is called a **function expression**:

```js
const greet = function(name) {
  return "Hello, " + name + "!";
};

greet("Alice"); // "Hello, Alice!"
```

The function itself has no name here — it's **anonymous**. The variable `greet` holds a reference to it.

### Declaration vs expression

The key difference is **hoisting**. Function declarations are hoisted — you can call them before they appear in the code. Function expressions are not.

```js
// Works — declaration is hoisted
sayHi();
function sayHi() {
  console.log("Hi!");
}

// Error — expression is not hoisted
greet();  // ReferenceError
const greet = function() {
  console.log("Hello!");
};
```

In practice: define your functions before calling them regardless of hoisting. It's clearer and avoids confusion.

## Arrow functions

Arrow functions are a shorter syntax for function expressions, introduced in ES6:

```js
// Regular function expression
const add = function(a, b) {
  return a + b;
};

// Arrow function
const add = (a, b) => {
  return a + b;
};

// Even shorter — implicit return when body is a single expression
const add = (a, b) => a + b;
```

Single parameter — parentheses are optional:

```js
const double = n => n * 2;
double(5); // 10
```

No parameters — empty parentheses required:

```js
const greet = () => "Hello!";
```

Arrow functions are used everywhere in modern JavaScript, especially as callbacks:

```js
const nums = [1, 2, 3, 4, 5];
const doubled = nums.map(n => n * 2);   // [2, 4, 6, 8, 10]
const evens = nums.filter(n => n % 2 === 0); // [2, 4]
```

Arrow functions have one important difference from regular functions: they don't have their own `this`. This matters in classes and event handlers — covered in the `this` article.

## Scope

Variables declared inside a function are **local** to that function. They don't exist outside it.

```js
function makeCounter() {
  let count = 0;  // local to makeCounter
  count++;
  return count;
}

makeCounter(); // 1
makeCounter(); // 1 — count resets each call
console.log(count); // ReferenceError — count doesn't exist here
```

Variables declared outside functions are in the **global scope** and accessible everywhere — but global variables are best avoided as they can be modified from anywhere, making bugs hard to track.

### Block scope

`let` and `const` are block-scoped — they live only inside the `{ }` where they're declared, including inside `if` blocks, loops, and functions:

```js
function example() {
  if (true) {
    let x = 10;
    const y = 20;
  }
  console.log(x); // ReferenceError
  console.log(y); // ReferenceError
}
```

## Functions as values

Functions in JavaScript are first-class values — you can pass them around like any other value: store them in variables, pass them as arguments, and return them from other functions.

### Passing functions as arguments

```js
function applyTwice(fn, value) {
  return fn(fn(value));
}

const double = n => n * 2;
applyTwice(double, 3); // 12 — double(double(3)) = double(6) = 12
```

A function passed as an argument is called a **callback**. You'll see this constantly:

```js
setTimeout(() => console.log("hello"), 1000); // callback after 1 second

document.addEventListener("click", () => {
  console.log("clicked");
});
```

### Returning functions from functions

```js
function makeMultiplier(factor) {
  return n => n * factor;
}

const triple = makeMultiplier(3);
triple(5);  // 15
triple(10); // 30
```

This is a **closure** — the returned function remembers the `factor` variable from its outer scope. Closures are covered in depth in their own article.

## Pure functions

A pure function always returns the same output for the same input and has no side effects — it doesn't modify anything outside itself.

```js
// Pure — same input always gives same output, no side effects
function add(a, b) {
  return a + b;
}

// Impure — depends on external state
let tax = 0.2;
function addTax(price) {
  return price + price * tax; // result changes if tax changes
}

// Impure — modifies external state
function addItem(cart, item) {
  cart.push(item); // mutates the array passed in
}
```

Pure functions are easier to test, debug, and reason about. Write pure functions by default and isolate side effects where necessary.

## Common mistakes

**Forgetting to return a value:**

```js
function add(a, b) {
  a + b; // calculated but not returned
}

add(2, 3); // undefined
```

**Calling a function without parentheses:**

```js
const result = multiply;   // stores the function reference, doesn't call it
const result = multiply(); // calls the function
```

**Redeclaring a function accidentally:**

```js
function process() { return "v1"; }
function process() { return "v2"; } // silently overwrites the first

process(); // "v2"
```

Use `const` with function expressions to prevent this:

```js
const process = () => "v1";
const process = () => "v2"; // SyntaxError — can't redeclare const
```

**Assuming arrow functions work like regular functions for `this`:**

```js
const obj = {
  name: "Alice",
  greet: () => {
    console.log(this.name); // undefined — arrow functions don't have their own this
  }
};
```

Use a regular function when you need `this` to refer to the object.

## Summary

```js
// Function declaration — hoisted
function name(params) {
  return value;
}

// Function expression — not hoisted
const name = function(params) {
  return value;
};

// Arrow function — shortest syntax
const name = (params) => value;

// Default parameters
function greet(name = "stranger") { }

// Rest parameters
function sum(...nums) { }

// Calling a function
name(arguments);
```

Functions are the core unit of JavaScript code. Get comfortable writing them in all three forms — declarations for named utility functions, arrow functions for callbacks and one-liners, and function expressions when you need to conditionally assign or export functions.
