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

## 03. Objects: The Basics

### 01. Objects

- Object keys converted to string.

```js
const obj = {
  0: "zero",
  "lorem ipsum": "lipsum",
  a: undefined,
};
obj[0]; // "zero"
obj["0"]; // "zero"

obj.__proto__ = 0; // Cannot be set to a non-object value;

"lorem ipsum" in obj; // true

obj.a; // undefined
"a" in obj; // true
```

#### Property names limitations

```js
const obj = {
  for: 1,
  const: 2,
  return: 3,
};
obj.for + obj.const + obj.return; // 6
```

#### The `for..in` loop

```js
const user = {
  name: "Ali",
  age: 20,
};
for (const key in user) key; // name, age
```

#### Ordered like an object

```js
const numbers = {
  3: "three",
  "5": "five",
  "seven": 7,
  2: "two",
  "4": "four",
  "six": 6,
  1: "one",
};
for (const num in numbers) num; // "1", "2", "3", "4", "5", "seven", "six"
```

### 02. Object References and Copying

#### Cloning and merging, `Object.assign`

```js
const user = {
  name: "Ali",
  age: 20,
};

const userClone = {};
Object.assign(userClone, user);

const userClone2 = Object.assign({}, user);
```

#### `structuredClone`

```js
const user = {
  name: "Ali",
  age: 20
};
user.me = user;

const clonedUser = structuredClone(user);
```

```js
// Function object could not be cloned. 
structuredClone({
  fn: () => {}
});
```

### 04. Object methods, `this`

```js
const ali = { name: "Ali" };
const veli = { name: "Veli" };

function greet() { return `Hi, ${this.name}`; }

ali.greet = greet;
veli.greet = greet;

ali.greet(); // Hi, Ali
veli.greet(); // Hi, Veli
greet(); // this is undefined in strict mode
```

### 05. Contructor, Operator `new`

#### Constructor function

```js
function User(name) {
  this.name = name;
  this.age = 20;
}
const user = new User("Ali");
user.name; // "Ali"
user.age; // 20
```

#### Constructor mode test: `new.target`

```js
function User() {
  console.log(new.target);
}

User(); // undefined
new User(); // function User() {...}
```

#### Return from constructors

```js
function User() {
  this.name = "Ali";
  return { name: "Veli" }; // overrides `this`
}
new User().name; // "Veli"
```

```js
function User() {
  this.name = "Ali";
  return;
}
new User().name; // "Ali"
```

```js
function User() { this.name = "Ali"; }

const user = new User();
// same as
const user2 = new User;

user.name; // "Ali"
user2.name; // "Ali"
```

### 06. Optional Chaining `?.`

```js
obj?.prop; // returns value or undefined
obj?.[index]; // returns item or undefined
obj.method?.(); // invokes method or undefined
```

### 07. Symbol Type

```js
const id = Symbol("id");
const id2 = Symbol("id");
id == id2; // false

id.toString(); // "Symbol(id)"
id.description; // "id"
```

#### Symbols are skipped by `for..in`

```js
const id = Symbol("id");
const user = {
  name: "Ali",
  age: 20,
  [id]: 123
};

for (const key in user) key; // name, age
user[id]; // 123
```

```js
const id = Symbol("id");
const user = { [id]: 123 };

// `Object.assign` copies both string and symbol properties.
const clonedUser = Object.assign({}, user);
clonedUser[id]; // 123
```

#### Global symbols

```js
const id = Symbol.for("id");
const id2 = Symbol.for("id");
id === id2; // true
Symbol.keyFor(id); // "id"
```

#### System symbols

- `Symbol.hasIntance`
- `Symbol.isConcatSpreadable`
- `Symbol.iterator`
- `Symbol.toPrimitive`

### 08. Object to Primitive Conversion

- To do the conversion, JavaScript tries to find and call three object methods:
  1. Call `obj[Symbol.toPrimitive](hint)` if such method exists,
  2. Otherwise if hint is `string`
    - try calling `obj.toString()` or `obj.valueOf()`, whatever exists.
  3. Otherwise if hint is `number` or `default`
    - try calling `obj.valueOf()` or `obj.toString()`, whatever exists.

#### `Symbol.toPrimitive`

```js
const user = {
  name: "Ali",
  age: 20,
  [Symbol.toPrimitive](hint) {
    if (hint === "string") return this.name;
    if (hint === "number") return this.age;
    if (hint === "default") return `{ "name": "${this.name}", "age": ${this.age} }`;
  }
}

String(user); // "Ali"
Number(user); // 20
user + ""; // "{ 'name': 'Ali', 'age': 20 }"
user.toString(); // "[object Object]"
```

#### `toString`, `valueOf`

```js
const user = {
  name: "Ali",
  age: 20,

  // hint === "string"
  toString() {
    return this.name;
  },

  // hint === "number || hint === "default"
  valueOf() {
    return this.age;
  }
}

user.toString(); // "Ali"
String(user); // "Ali"

user.valueOf(); // 20
user * 1; // 20
```
