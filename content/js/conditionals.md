---
title: "if, else, and switch in JavaScript"
slug: "conditionals"
description: "Learn how to control program flow in JavaScript using if, else if, else, switch statements, and the ternary operator."
section: "js"
tags: ["javascript", "basics"]
---

Conditionals let your code make decisions — run this block if something is true, run that block if it's false. JavaScript gives you two main tools for this: `if/else` and `switch`.

## if statement

The simplest conditional. Runs a block of code only if the condition is `true`.

```js
let age = 20;

if (age >= 18) {
  console.log("You can vote.");
}
```

If `age` is less than 18, nothing happens — the block is skipped entirely.

## if / else

Add an `else` block to handle the false case:

```js
let age = 15;

if (age >= 18) {
  console.log("You can vote.");
} else {
  console.log("Too young to vote.");
}
```

Exactly one of the two blocks will always run.

## if / else if / else

Chain multiple conditions with `else if`:

```js
let score = 72;

if (score >= 90) {
  console.log("A");
} else if (score >= 80) {
  console.log("B");
} else if (score >= 70) {
  console.log("C");
} else {
  console.log("F");
}
// "C"
```

JavaScript checks each condition top to bottom and runs the first one that is `true`. Once a match is found, the rest are skipped. The final `else` is the catch-all if nothing matches.

## Truthy and falsy values

The condition inside `if()` doesn't have to be a boolean. JavaScript converts any value to `true` or `false` automatically.

**Falsy values** — these all evaluate to `false` in a condition:

```js
false
0
""          // empty string
null
undefined
NaN
```

**Everything else is truthy** — including `"0"`, `[]`, and `{}`.

```js
if (0) { }        // skipped — 0 is falsy
if ("hello") { }  // runs — non-empty string is truthy
if ([]) { }       // runs — empty array is truthy
if (null) { }     // skipped — null is falsy
```

This lets you write compact checks:

```js
const user = getUser();

if (user) {
  console.log("User found:", user.name);
}
```

Instead of the verbose `if (user !== null && user !== undefined)`.

## Comparison and logical operators in conditions

Combine conditions with `&&` (AND) and `||` (OR):

```js
let age = 25;
let hasID = true;

if (age >= 18 && hasID) {
  console.log("Entry allowed.");
}

let isAdmin = false;
let isModerator = true;

if (isAdmin || isModerator) {
  console.log("Access granted.");
}
```

Use `!` to negate:

```js
let isLoggedIn = false;

if (!isLoggedIn) {
  console.log("Please log in.");
}
```

## Nested if statements

You can put `if` statements inside other `if` statements:

```js
let age = 25;
let hasTicket = true;

if (age >= 18) {
  if (hasTicket) {
    console.log("Welcome!");
  } else {
    console.log("You need a ticket.");
  }
} else {
  console.log("Must be 18 or older.");
}
```

Nesting deeper than two levels hurts readability. Flatten it with `&&` when you can:

```js
if (age >= 18 && hasTicket) {
  console.log("Welcome!");
}
```

## Early return pattern

In a function, returning early avoids deep nesting and makes the happy path clearer:

```js
// Hard to read — deeply nested
function process(user) {
  if (user) {
    if (user.isActive) {
      if (user.hasPermission) {
        // actual logic here
      }
    }
  }
}

// Easier to read — early returns
function process(user) {
  if (!user) return;
  if (!user.isActive) return;
  if (!user.hasPermission) return;

  // actual logic here — only runs if all checks pass
}
```

This pattern is called a **guard clause**. It's one of the cleanest ways to write conditional logic.

## switch statement

`switch` compares one value against multiple possible cases. It's cleaner than a long `else if` chain when you're checking the same variable repeatedly.

```js
let day = "Monday";

switch (day) {
  case "Monday":
    console.log("Start of the work week.");
    break;
  case "Friday":
    console.log("Almost the weekend.");
    break;
  case "Saturday":
  case "Sunday":
    console.log("Weekend!");
    break;
  default:
    console.log("Midweek.");
}
```

### How switch works

1. JavaScript evaluates the expression after `switch`
2. It compares the result against each `case` using `===`
3. When a match is found, it runs that block
4. `break` stops execution and exits the switch
5. `default` runs if no case matches — like `else`

### The break statement is required

Without `break`, execution **falls through** to the next case:

```js
let x = 1;

switch (x) {
  case 1:
    console.log("one");
    // no break!
  case 2:
    console.log("two");
    break;
  case 3:
    console.log("three");
}
// Logs: "one" then "two" — fallthrough!
```

This is a common bug. Always add `break` unless you intentionally want fallthrough.

### Intentional fallthrough

Sometimes fallthrough is useful — multiple cases sharing the same code:

```js
let month = 4;
let days;

switch (month) {
  case 1:
  case 3:
  case 5:
  case 7:
  case 8:
  case 10:
  case 12:
    days = 31;
    break;
  case 4:
  case 6:
  case 9:
  case 11:
    days = 30;
    break;
  case 2:
    days = 28;
    break;
}

console.log(days); // 30
```

### switch uses strict equality

`switch` uses `===` internally, so type matters:

```js
let x = "1";

switch (x) {
  case 1:
    console.log("number one");
    break;
  case "1":
    console.log("string one"); // this runs
    break;
}
```

## if/else vs switch — when to use which

| Situation | Use |
|---|---|
| One or two conditions | `if/else` |
| Range checks (`> 10`, `< 5`) | `if/else` — switch can't do ranges |
| Many cases on the same variable | `switch` |
| Checking different variables | `if/else` |
| Explicit case-by-case values | `switch` |

## The ternary operator

For simple two-branch conditions, the ternary is a compact alternative to `if/else`:

```js
// if/else version
let label;
if (score >= 50) {
  label = "pass";
} else {
  label = "fail";
}

// ternary version
let label = score >= 50 ? "pass" : "fail";
```

The ternary is great for assigning values inline. Avoid it for complex logic — if you find yourself nesting ternaries, switch to `if/else`.

## Common mistakes

**Missing break in switch:**

```js
switch (color) {
  case "red":
    console.log("Stop");
  case "green":       // unintentional fallthrough
    console.log("Go");
}
// If color is "red": logs "Stop" then "Go"
```

**Comparing with == instead of === in conditions:**

```js
if (value == null) { }  // catches both null and undefined — sometimes intentional
if (value === null) { } // catches only null — usually what you want
```

**Forgetting that assignment returns a value:**

```js
if (x = 10) { }  // always true — you assigned 10, not compared
if (x === 10) { } // correct comparison
```

Most code editors and linters will warn you about this.

**Over-nesting instead of using early returns:**

```js
// Avoid
if (a) {
  if (b) {
    if (c) {
      doThing();
    }
  }
}

// Better
if (!a) return;
if (!b) return;
if (!c) return;
doThing();
```

## Summary

```js
// Basic if/else
if (condition) {
  // runs when true
} else {
  // runs when false
}

// Multiple conditions
if (a) {
  // ...
} else if (b) {
  // ...
} else {
  // catch-all
}

// switch
switch (value) {
  case x:
    // ...
    break;
  case y:
    // ...
    break;
  default:
    // ...
}

// Ternary — for simple inline conditions
let result = condition ? "yes" : "no";
```

Use `if/else` for most conditional logic. Reach for `switch` when you have four or more cases on the same value. Use the ternary for simple one-liner assignments. And use guard clauses to keep functions flat and readable.
