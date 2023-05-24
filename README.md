# JavaScript Notes

## 01. JavaScript Fundamentals

### 05. Data Types

```js
typeof null === "object";
typeof undefined === "undefined";
typeof function() {} === "function";
typeof [] === "object";
```

#### Primitives

- number: `1`
- bigint: `123n`
- string: `"a"`
- boolean: `true`
- null
- undefined
- symbol: `Symbol("id")`

#### Non-Primitive

- object: `{ a: 1 }`

### 07. Type Conversions

#### Numeric conversion

| Value            | To         |
| ---------------- | ---------- |
| `undefined`      | `NaN`      |
| `null`           | `0`        |
| `true`, `false`  | `1`, `0`   |
| `'\t \n'`, `abc` | `0`, `NaN` |

#### Boolean conversion

| Value                                 | To      |
| ------------------------------------- | ------- |
| `0`, `null`, `undefined`, `NaN`, `""` | `false` |
| `"0"`, `" "`                          | `true`  |

### 09. Comparisons

- `===`: checks the equality without type conversion.

```js
'Z' > 'A'; // 90 > 65 unicode comparison
'2' > '1'; // 50 > 49 unicode comparison
'Aa' > 'A';

'2' > 1; // 2 > 1
'01' == 1; // 1 == 1

true == 1; // 1 == 1
false == 0; // 0 == 0
'' == false; // 0 == 0

null == undefined; // they equal each other, but not any other value.

null > 0; // 0 > 0
null == 0;
null >= 0; // 0 >= 0

undefined > 0; // NaN > 0
undefined < 0; // NaN < 0
undefined == 0;
```

### 11. Logical Operators

- The precedence of `&&` is higher than `||`

```js
1 || 2 || 0; // 2
undefined || 0 || null; // null

1 && 0 && 1; // 0
1 && 2 && 3; // 3

1 && 2 || 0 && 1; // (1 && 2) || (0 && 1) = 1 || 0 = 1
```

### 12. Nullish Coalescing Operator

```js
0 ?? 1; // 0 !== null && 0 !== undefined ? 0 : 1 = 0

null ?? 0; // 0
undefined ?? 0; // 0

1 && 2 || 3 ?? 4; // SyntaxError: Unexpected token '??'
```

### 13. Loops `while` and `for`

```js
for (let i = 0; i < 3; i++) i; // 0, 1, 2
for (let i = 0; i < 3; ++i) i; // 0, 1, 2
```

```js
let i = 0;
while (i < 3) i++; // 0, 1, 2
i; // 3

i = 0;
while (i < 3) ++i; // 1, 2, 3
i; // 3
```

#### Labels for `break`, `continue`

```js
outer: for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    break outer;
  }
}

outer:
for (let i = 0; i < 3; i++) {}
```

### 15. Functions

- Function declaration
```js
test(); // hoisted
function test() {}
```

### 16. Function Expressions
```js
test(); // TypeError: test is not a function
var test = function() {};
```

## 02. Code Quality

### 03. Comments

```js
// Single-line

/*
 * Multiline
 */

/**
 * Returns object of x and name.
 * 
 * @param {number} x - The number x
 * @param {string} name - Your name
 * @return {Object} An object of x and name
 */
const makeObj = (x, name) => ({ x, name });
```

### 06. Polyfills and Transpilers

#### Transpilers

- A transpiler is a special piece of software that translates code to another source code.
- [Babel](https://babeljs.io/) is one of the most prominent transpiler.

```js
height = height ?? 100;

height = (height !== undefined && height !== null) ? height : 100; // transpiled
```

#### Polyfills

- A script that updates/adds new functions is called polyfill.
- [core.js](https://github.com/zloirock/core-js) is one of the most popular polyfill library.

```js
if (!Math.trunc) {
  Math.trunc = (num) => num < 0 ? Math.ceil(num) : Math.floor(num);
}
```
