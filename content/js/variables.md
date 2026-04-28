---
title: "var, let, and const in JavaScript Explained"
slug: "variables"
description: "Learn the difference between var, let, and const in JavaScript — scope, hoisting, reassignment, and which one to use."
section: "js"
tags: ["javascript", "basics", "variables"]
---

JavaScript gives you three ways to declare a variable: `var`, `let`, and `const`. They look similar but behave differently — and using the wrong one is the source of countless bugs.

Here's the short answer:

- Use **`const`** by default.
- Use **`let`** when you need to reassign the variable.
- Avoid **`var`** in modern code.

The rest of this article explains why.

## The three keywords at a glance

```js
var   age = 30;     // old, function-scoped
let   age = 30;     // modern, block-scoped, reassignable
const age = 30;     // modern, block-scoped, cannot be reassigned
```

All three create a variable. The differences are in **scope**, **hoisting**, and whether you can **reassign** the value.

## var: the legacy way

`var` was the only way to declare variables until ES6 (2015). It's still valid JavaScript, but it has quirks that lead to bugs.

### var is function-scoped

A `var` variable is visible everywhere inside the function it was declared in — even outside the block where you created it.

```js
function example() {
  if (true) {
    var x = 10;
  }
  console.log(x); // 10  ← still accessible
}
```

This surprises people. The `if` block doesn't create a new scope for `var`.

### var can be redeclared

You can declare the same `var` variable twice in the same scope without an error:

```js
var name = "Alice";
var name = "Bob";   // no error
console.log(name);  // "Bob"
```

In a large file this is a recipe for accidentally overwriting your own variables.

### var is hoisted and initialized as undefined

`var` declarations are moved to the top of their scope at runtime. The variable exists before the line you wrote it on, but its value is `undefined` until the assignment runs:

```js
console.log(name); // undefined  (not an error)
var name = "Alice";
```

This is called **hoisting**. It looks harmless but it hides bugs.

## let: block-scoped and reassignable

`let` was added in ES6 to fix `var`'s problems. It's **block-scoped**, meaning it only exists inside the `{ ... }` where it was declared.

```js
function example() {
  if (true) {
    let x = 10;
  }
  console.log(x); // ReferenceError: x is not defined
}
```

You can reassign a `let` variable, but you can't redeclare it in the same scope:

```js
let count = 1;
count = 2;          // OK — reassignment

let count = 3;      // SyntaxError: already declared
```

## const: block-scoped and locked

`const` works exactly like `let`, with one extra rule: **you cannot reassign the variable** after it's created.

```js
const PI = 3.14159;
PI = 3.14;   // TypeError: Assignment to constant variable
```

You also can't declare a `const` without a value:

```js
const x;     // SyntaxError: Missing initializer
```

### const objects and arrays are not frozen

This is the part that trips people up. `const` prevents **reassignment of the variable**, not **modification of its contents**:

```js
const user = { name: "Alice" };
user.name = "Bob";        // OK — you're not reassigning user
user.age = 30;            // OK — adding a property is fine
console.log(user);        // { name: "Bob", age: 30 }

user = { name: "Carol" }; // TypeError — this would reassign
```

Same for arrays:

```js
const items = [1, 2, 3];
items.push(4);    // OK — mutating the array
items[0] = 99;    // OK — changing an element
items = [];       // TypeError — reassignment
```

If you need a truly immutable object, use `Object.freeze()`.

## The Temporal Dead Zone

`let` and `const` are also hoisted, but unlike `var`, you cannot use them before the declaration line. The period between the start of the scope and the declaration is called the **Temporal Dead Zone (TDZ)**:

```js
console.log(x); // ReferenceError: Cannot access 'x' before initialization
let x = 10;
```

This is a feature, not a bug. It catches mistakes that `var` would silently allow.

## Which one should you use?

A simple decision rule that covers 95% of cases:

1. **Start with `const`.** Most variables don't need to be reassigned.
2. **Switch to `let`** the moment you actually need to reassign.
3. **Never use `var`** in new code unless you're maintaining a legacy codebase.

Examples:

```js
const items   = [1, 2, 3];         // never reassigned, use const
const user    = { name: "Alice" }; // the reference doesn't change

let count = 0;                     // reassigned in a loop
for (let i = 0; i < 10; i++) {
  count = count + i;
}

let result = null;                 // reassigned later
if (someCondition) {
  result = computeValue();
}
```

## Common mistakes

**Using `var` in a loop and expecting block scope:**

```js
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}
// Logs: 3, 3, 3   ← because var is function-scoped
```

Use `let` to fix this:

```js
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}
// Logs: 0, 1, 2
```

**Trying to mutate a constant primitive:**

```js
const age = 30;
age++;   // TypeError
```

Primitives (numbers, strings, booleans) cannot be mutated, so `const` locks them completely.

**Assuming `const` deep-freezes objects:**

```js
const config = { debug: false };
config.debug = true;  // works — const doesn't prevent this
```

Use `Object.freeze(config)` if you need real immutability.

## Summary

| Feature          | var              | let       | const     |
|------------------|------------------|-----------|-----------|
| Scope            | Function         | Block     | Block     |
| Reassignable     | Yes              | Yes       | No        |
| Redeclarable     | Yes              | No        | No        |
| Hoisted          | Yes (undefined)  | Yes (TDZ) | Yes (TDZ) |
| Must initialize  | No               | No        | Yes       |

In modern JavaScript: default to `const`, reach for `let` when you need to reassign, and leave `var` in 2015.
