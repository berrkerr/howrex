---
title: "JavaScript Data Types Explained"
slug: "data-types"
description: "Learn the eight data types in JavaScript — primitives vs objects, typeof, type coercion, and common mistakes."
weight: 2
section: "js"
tags: ["javascript", "basics"]
---

Every value in JavaScript has a type. Understanding types is fundamental — they determine what you can do with a value, how JavaScript compares values, and why some bugs appear out of nowhere.

JavaScript has **eight data types**: seven primitives and one object type.

## The eight types at a glance

```js
// Primitives
let name    = "Alice";        // String
let age     = 30;             // Number
let big     = 900719925n;     // BigInt
let active  = true;           // Boolean
let x       = undefined;      // Undefined
let empty   = null;           // Null
let id      = Symbol("id");   // Symbol

// Object type
let user    = { name: "Alice" }; // Object
```

Arrays, functions, and dates are all objects under the hood.

## Primitives vs objects

The most important distinction in JavaScript types:

**Primitives** are simple values. They are immutable — you cannot change the value itself, only reassign the variable. They are copied by value.

**Objects** are collections of properties. They are mutable and copied by reference.

```js
// Primitives — copied by value
let a = 10;
let b = a;
b = 20;
console.log(a); // 10 — unchanged

// Objects — copied by reference
let obj1 = { x: 10 };
let obj2 = obj1;
obj2.x = 20;
console.log(obj1.x); // 20 — changed!
```

This is one of the most common sources of bugs in JavaScript.

## String

A string is a sequence of characters wrapped in single quotes, double quotes, or backticks.

```js
let single = 'Hello';
let double = "World";
let template = `Hello, ${double}!`; // template literal
```

Strings are immutable — you can't change individual characters, only create new strings.

```js
let str = "hello";
str[0] = "H";         // silently fails
console.log(str);     // "hello" — unchanged
str = "Hello";        // this works — reassigning the variable
```

Common string operations:

```js
let s = "JavaScript";

s.length;             // 10
s.toUpperCase();      // "JAVASCRIPT"
s.includes("Script"); // true
s.slice(0, 4);        // "Java"
s.replace("Java", "Type"); // "TypeScript"
s.split("");          // ["J","a","v","a","S","c","r","i","p","t"]
```

## Number

JavaScript has a single number type for both integers and decimals. It uses 64-bit floating point (IEEE 754).

```js
let int     = 42;
let float   = 3.14;
let neg     = -7;
let exp     = 1.5e6;  // 1,500,000
```

### Special number values

```js
Infinity        // 1 / 0
-Infinity       // -1 / 0
NaN             // "not a number" — result of invalid math
```

`NaN` is the strangest value in JavaScript — it's not equal to anything, including itself:

```js
NaN === NaN;    // false
isNaN(NaN);     // true — use this to check for NaN
Number.isNaN(NaN); // true — more reliable
```

### Floating point precision

This trips everyone up eventually:

```js
0.1 + 0.2;     // 0.30000000000000004
```

This is not a JavaScript bug — it's how IEEE 754 floating point works in every language. For financial calculations, use integer arithmetic (store cents, not dollars) or a library like `decimal.js`.

## BigInt

BigInt handles integers larger than `Number.MAX_SAFE_INTEGER` (2⁵³ - 1). Add `n` after the number:

```js
const big = 9007199254740992n;
const result = big + 1n; // 9007199254740993n
```

You can't mix BigInt and Number in arithmetic:

```js
1n + 1;   // TypeError: Cannot mix BigInt and other types
```

Use BigInt when working with very large integers — cryptography, database IDs, financial systems.

## Boolean

A boolean is simply `true` or `false`. Used in conditions and comparisons.

```js
let isLoggedIn = true;
let hasError   = false;

if (isLoggedIn) {
  console.log("Welcome back");
}
```

Any value in JavaScript can be converted to a boolean. See truthy and falsy values for the full rules.

## Undefined

A variable that has been declared but not assigned a value is `undefined`. JavaScript sets this automatically.

```js
let x;
console.log(x);         // undefined

function greet(name) {
  console.log(name);    // undefined if called as greet()
}
```

`undefined` means a variable exists but has no value yet.

## Null

`null` is an intentional absence of value. You set it explicitly to signal "no value here."

```js
let user = null;        // no user loaded yet

// later...
user = { name: "Alice" };
```

### null vs undefined

```js
let a;          // undefined — no value assigned
let b = null;   // null — intentionally empty
```

Use `null` when you want to explicitly clear a value. `undefined` is what JavaScript assigns automatically. In practice: don't assign `undefined` yourself — if you want to signal "no value", use `null`.

## Symbol

Symbols are unique, immutable identifiers. Every `Symbol()` call creates a completely unique value even with the same description.

```js
const a = Symbol("id");
const b = Symbol("id");
a === b;  // false — always unique
```

Symbols are mainly used as unique object property keys to avoid naming collisions:

```js
const ID = Symbol("id");
const user = {
  name: "Alice",
  [ID]: 123       // symbol as key — won't clash with any string key
};
```

You won't need Symbols often in everyday code, but they are used extensively in JavaScript internals and libraries.

## typeof

Use `typeof` to check the type of a value at runtime:

```js
typeof "hello"      // "string"
typeof 42           // "number"
typeof true         // "boolean"
typeof undefined    // "undefined"
typeof Symbol()     // "symbol"
typeof 42n          // "bigint"
typeof {}           // "object"
typeof []           // "object"  ← arrays are objects
typeof null         // "object"  ← famous bug, null is not an object
typeof function(){} // "function"
```

The `typeof null === "object"` result is a historical bug in JavaScript that cannot be fixed for backwards compatibility reasons. To check for null specifically:

```js
value === null  // the correct way to check for null
```

To check if something is an array:

```js
Array.isArray([]);  // true
Array.isArray({});  // false
```

## Type coercion

JavaScript automatically converts types in certain contexts — this is called **type coercion**. It's convenient but can produce surprising results.

### String coercion

The `+` operator converts to string when one operand is a string:

```js
"5" + 3;    // "53"  — number coerced to string
"5" + true; // "5true"
```

### Numeric coercion

Other arithmetic operators convert to number:

```js
"5" - 3;    // 2  — string coerced to number
"5" * 2;    // 10
true + 1;   // 2  — true is 1
false + 1;  // 1  — false is 0
```

### == vs ===

`==` performs type coercion before comparing. `===` does not — it checks value and type.

```js
5 == "5";   // true  — coerces string to number
5 === "5";  // false — different types, no coercion

null == undefined;  // true
null === undefined; // false
```

Always use `===` in your code. The coercion rules for `==` are complex and produce hard-to-debug issues.

## Common mistakes

**Adding numbers and strings accidentally:**

```js
let input = "5";  // from a form input — always a string
let result = input + 10;  // "510" — not 15!

// Fix: convert first
let result = Number(input) + 10;  // 15
```

**Checking for null incorrectly:**

```js
if (value == null) { }  // catches both null and undefined (intentional)
if (value === null) { } // catches only null (precise)
```

**Assuming typeof null is reliable:**

```js
typeof null === "object"  // true, but null is NOT an object
// Always check: value === null
```

## Summary

| Type      | Example              | typeof result |
|-----------|----------------------|---------------|
| String    | `"hello"`            | `"string"`    |
| Number    | `42`, `3.14`         | `"number"`    |
| BigInt    | `42n`                | `"bigint"`    |
| Boolean   | `true`, `false`      | `"boolean"`   |
| Undefined | `undefined`          | `"undefined"` |
| Null      | `null`               | `"object"` ⚠️ |
| Symbol    | `Symbol("id")`       | `"symbol"`    |
| Object    | `{}`, `[]`, `Date`   | `"object"`    |

The key things to remember: use `===` not `==`, watch out for string/number coercion with `+`, and always use `Array.isArray()` instead of `typeof` to check for arrays.
