---
title: "JavaScript Operators Explained"
slug: "operators"
description: "A complete guide to JavaScript operators — arithmetic, comparison, logical, assignment, ternary, nullish coalescing, and more."
weight: 3
section: "js"
tags: ["javascript", "basics"]
---

Operators are the symbols that let you perform operations on values — math, comparisons, logic, assignment, and more. JavaScript has a lot of them, but most are straightforward once you see them in context.

## Arithmetic operators

Used for math. These work on numbers.

```js
10 + 3   // 13  addition
10 - 3   // 7   subtraction
10 * 3   // 30  multiplication
10 / 3   // 3.3333... division
10 % 3   // 1   remainder (modulo)
10 ** 3  // 1000 exponentiation
```

The modulo operator `%` returns the remainder after division. It's useful for checking if a number is even or odd:

```js
8 % 2 === 0  // true  — even
7 % 2 === 0  // false — odd
```

### Increment and decrement

```js
let x = 5;

x++;  // post-increment: returns 5, then x becomes 6
++x;  // pre-increment: x becomes 7, then returns 7
x--;  // post-decrement: returns 7, then x becomes 6
--x;  // pre-decrement: x becomes 5, then returns 5
```

The difference between pre and post matters when you use the result:

```js
let a = 5;
let b = a++;  // b = 5, a = 6
let c = ++a;  // c = 7, a = 7
```

In most cases you'll just write `i++` in loops and not worry about the difference.

## Assignment operators

The basic assignment operator is `=`. The compound assignment operators combine an operation with assignment:

```js
let x = 10;

x += 5;   // x = x + 5  → 15
x -= 3;   // x = x - 3  → 12
x *= 2;   // x = x * 2  → 24
x /= 4;   // x = x / 4  → 6
x %= 4;   // x = x % 4  → 2
x **= 3;  // x = x ** 3 → 8
```

These are just shorthand. `x += 5` is identical to `x = x + 5`.

### Logical assignment operators

Added in ES2021 — useful for setting default values:

```js
// Assigns only if the left side is falsy
x ||= "default";   // x = x || "default"

// Assigns only if the left side is truthy
x &&= "new value"; // x = x && "new value"

// Assigns only if the left side is null or undefined
x ??= "fallback";  // x = x ?? "fallback"
```

`??=` is the most useful of the three — it sets a default without overwriting `0` or `""` the way `||=` would.

## Comparison operators

Comparison operators return a boolean (`true` or `false`).

```js
5 > 3    // true   greater than
5 < 3    // false  less than
5 >= 5   // true   greater than or equal
5 <= 4   // false  less than or equal
5 === 5  // true   strict equality
5 !== 3  // true   strict inequality
5 == "5" // true   loose equality (with coercion)
5 != "3" // true   loose inequality
```

### === vs ==

This is critical. Always use `===` (strict equality) instead of `==` (loose equality).

`==` performs type coercion — it converts both sides to the same type before comparing. The rules are complex and produce counterintuitive results:

```js
0 == false      // true
"" == false     // true
null == undefined // true
0 == ""         // true
0 == "0"        // true
"" == "0"       // false  ← wait, what?
```

`===` never coerces — it returns `false` if the types are different:

```js
0 === false       // false
null === undefined // false
5 === "5"         // false
```

Use `===` everywhere. Only use `==` if you specifically want to check for both `null` and `undefined` at once (`value == null`).

## Logical operators

Used to combine boolean expressions.

```js
true && true   // true   AND — both must be true
true && false  // false
true || false  // true   OR — at least one must be true
false || false // false
!true          // false  NOT — inverts the boolean
!false         // true
```

### Short-circuit evaluation

This is important. JavaScript evaluates logical expressions left to right and stops as soon as the result is determined.

`&&` stops at the first falsy value:

```js
false && doSomething()  // doSomething() never runs
```

`||` stops at the first truthy value:

```js
true || doSomething()   // doSomething() never runs
```

This behavior is used constantly in real code. A common pattern is providing a default value with `||`:

```js
const name = userInput || "Anonymous";
```

And conditionally calling a function with `&&`:

```js
isLoggedIn && showDashboard();
```

### Logical operators return values, not booleans

This surprises people. `&&` and `||` don't return `true` or `false` — they return one of the actual operand values:

```js
"Alice" || "Bob"   // "Alice"  — first truthy value
null || "Bob"      // "Bob"    — first truthy value
"Alice" && "Bob"   // "Bob"    — last truthy value if both truthy
null && "Bob"      // null     — first falsy value
```

## The nullish coalescing operator (??)

`??` returns the right side only if the left side is `null` or `undefined`. It's a more precise version of `||`.

```js
null ?? "default"      // "default"
undefined ?? "default" // "default"
0 ?? "default"         // 0  ← does NOT treat 0 as falsy
"" ?? "default"        // "" ← does NOT treat "" as falsy
false ?? "default"     // false
```

The difference from `||`:

```js
0 || "default"   // "default"  — 0 is falsy, so falls through
0 ?? "default"   // 0          — 0 is not null/undefined, so keeps it
```

Use `??` when `0`, `false`, or `""` are valid values you want to keep.

## The ternary operator

The ternary operator is a compact if/else in one line:

```js
condition ? valueIfTrue : valueIfFalse
```

Examples:

```js
let age = 20;
let status = age >= 18 ? "adult" : "minor";  // "adult"

let score = 85;
let grade = score >= 90 ? "A"
          : score >= 80 ? "B"
          : score >= 70 ? "C"
          : "F";  // "B"
```

Use the ternary for simple expressions. For complex logic, use a regular `if` statement — chaining more than two ternaries hurts readability.

## Optional chaining (?.)

Safely accesses nested properties without throwing an error if something in the chain is `null` or `undefined`.

```js
const user = null;

user.name           // TypeError: Cannot read properties of null
user?.name          // undefined — safe, no error

const data = { user: { profile: { name: "Alice" } } };
data?.user?.profile?.name   // "Alice"
data?.account?.balance      // undefined — account doesn't exist
```

You can also use it with method calls and array access:

```js
user?.getName()    // undefined if user is null, calls getName() otherwise
arr?.[0]           // undefined if arr is null, returns first element otherwise
```

Optional chaining is essential when working with API responses where nested data might be missing.

## The spread operator (...)

Expands an array or object into individual elements.

```js
const a = [1, 2, 3];
const b = [4, 5, 6];

const combined = [...a, ...b];  // [1, 2, 3, 4, 5, 6]
const copy = [...a];            // [1, 2, 3] — shallow copy

const obj1 = { x: 1, y: 2 };
const obj2 = { ...obj1, z: 3 }; // { x: 1, y: 2, z: 3 }
```

## The typeof and instanceof operators

`typeof` checks the type of a primitive value:

```js
typeof "hello"   // "string"
typeof 42        // "number"
typeof true      // "boolean"
typeof undefined // "undefined"
typeof null      // "object"  ← historical bug
typeof {}        // "object"
typeof []        // "object"
```

`instanceof` checks if an object was created from a specific constructor:

```js
[] instanceof Array    // true
{} instanceof Object   // true
new Date() instanceof Date  // true

"hello" instanceof String  // false — primitives are not instances
```

## Operator precedence

When multiple operators appear in one expression, JavaScript follows precedence rules (like BODMAS in math). Higher precedence operators run first.

```js
2 + 3 * 4    // 14, not 20 — * runs before +
(2 + 3) * 4  // 20 — parentheses override precedence
```

Common precedence order (high to low):

```
()           Parentheses
**           Exponentiation
* / %        Multiplication, division, remainder
+ -          Addition, subtraction
> < >= <=    Comparison
=== !== == != Equality
&&           Logical AND
||           Logical OR
??           Nullish coalescing
= += -=      Assignment
```

When in doubt, use parentheses. They make intent clear and prevent bugs.

## Common mistakes

**Using = instead of === in conditions:**

```js
if (x = 5) { }   // assignment, always true — bug!
if (x === 5) { } // comparison — correct
```

**Expecting + to add numbers when one is a string:**

```js
let input = "10";  // form inputs are strings
input + 5;         // "105" — concatenation, not addition
Number(input) + 5; // 15 — correct
```

**Using || for defaults when 0 is a valid value:**

```js
function setVolume(vol) {
  const volume = vol || 50;  // if vol is 0, volume becomes 50 — bug!
  const volume = vol ?? 50;  // if vol is 0, volume stays 0 — correct
}
```

## Summary

| Operator | Name | Example | Result |
|---|---|---|---|
| `+` `-` `*` `/` `%` `**` | Arithmetic | `10 % 3` | `1` |
| `===` `!==` `>` `<` | Comparison | `5 === "5"` | `false` |
| `&&` `\|\|` `!` | Logical | `null \|\| "x"` | `"x"` |
| `??` | Nullish coalescing | `0 ?? "x"` | `0` |
| `?.` | Optional chaining | `null?.name` | `undefined` |
| `? :` | Ternary | `a > b ? a : b` | larger value |
| `=` `+=` `-=` | Assignment | `x += 5` | `x = x + 5` |
| `typeof` | Type check | `typeof 42` | `"number"` |

The three things worth memorizing: always use `===` not `==`, use `??` instead of `\|\|` when `0` and `""` are valid values, and use `?.` when accessing nested object properties from an API.
