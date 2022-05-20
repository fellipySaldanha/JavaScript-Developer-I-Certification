# Explore Variables and Types

## **Data types**

- Seven data types that are [primitives](https://developer.mozilla.org/en-US/docs/Glossary/Primitive):
    1. [Boolean](https://developer.mozilla.org/en-US/docs/Glossary/Boolean). `true` and `false`.
    2. [null](https://developer.mozilla.org/en-US/docs/Glossary/Null). A special keyword denoting a null value. (Because JavaScript is case-sensitive, `null` is not the same as `Null`, `NULL`, or any other variant.)
    3. [undefined](https://developer.mozilla.org/en-US/docs/Glossary/undefined). A top-level property whose value is not defined.
    4. [Number](https://developer.mozilla.org/en-US/docs/Glossary/Number). An integer or floating point number. For example: `42` or `3.14159`.
        1. Besides regular numbers, there are so-called “special numeric values” which also belong to this data type:
            1. `Infinity` represents the mathematical [Infinity](https://en.wikipedia.org/wiki/Infinity) ∞.
            2. `NaN` represents a computational error. It is a result of an incorrect or an undefined mathematical operation.
    5. [BigInt](https://developer.mozilla.org/en-US/docs/Glossary/BigInt). An integer with arbitrary precision. For example: `9007199254740992n`.
    6. [String](https://developer.mozilla.org/en-US/docs/Glossary/String). A sequence of characters that represent a text value. For example: "Howdy"
        1. In JavaScript, there are 3 types of quotes.
            1. Double quotes: `"Hello"`.
            2. Single quotes: `'Hello'`.
            3. Backticks: ``Hello``.
    7. [Symbol](https://developer.mozilla.org/en-US/docs/Glossary/Symbol) (new in ECMAScript 2015). A data type whose instances are unique and immutable.

## ****Variables****

JavaScript has three kinds of variable declarations.

`var` - Declares a variable, optionally initializing it to a value.
`let` - Declares a block-scoped, local variable, optionally initializing it to a value.
`const` - Declares a block-scoped, read-only named constant.

We can declare multiple variables in one line:

```jsx
let user = 'John', age = 25, message = 'Hello';
```

That might seem shorter, but we don’t recommend it. For the sake of better readability, please use a single line per variable.

JavaScript is **case-sensitive**

There are two limitations on variable names in JavaScript:

1. The name must contain only letters, digits, or the symbols `$` and `_`.
2. The first character must not be a digit.

**An assignment without `use strict`**

It’s possible to create a variable by a mere assignment of the value without using `let`. This still works now if we don’t put `use strict`in our scripts to maintain compatibility with old scripts.

```jsx
// note: no "use strict" in this example

num = 5; // the variable "num" is created if it didn't exist

alert(num); // 5
```

This is a bad practice and would cause an error in strict mode:

```jsx
"use strict";

num = 5; // error: num is not defined
```

capital-named constants are only used as aliases for “hard-coded” values.

```jsx
const pageLoadTime = /* time taken by a webpage to load */;
const COLOR_ORANGE = "#FF7F00";
```
## ****Links****
[Variables](https://javascript.info/variables)

[Data types](https://javascript.info/types)