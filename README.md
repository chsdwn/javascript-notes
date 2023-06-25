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

## 04. Data Types

### 01. Methods of Primitives

```js
1..toFixed(2); // "1.00"
"Ali".toLocaleUpperCase("tr"); // "ALİ"

typeof 0; // "number"
typeof new Number(0); // "object"

const zero = new Number(0);
zero && "zero is an object"; // "zero is an object"
```

### 02. Numbers

```js
1_000_000_000 === 1e9; // true
0.000_000_000_1 === 1e-10; // true

// Hexadecimal
0xff === 255; // true
0xFF === 255; // true

// Binary
0b111_111_11 === 255; // true

// Octal
0o377 === 255; // true
```

#### `toString(base)`

```js
const num = 255;

// base = 16: 0..9, A..F
num.toString(16); // "255"

// base = 2: 0, 1
num.toString(2); // "11111111"

// base = 36(maximum): 0..9 A..Z
123_456..toString(36); // "2n9c"
```

#### Rounding

|    # | `Math.floor()` | `Math.ceil()` | `Math.round()` | `Math.trunc()` |
| ---: | -------------: | ------------: | -------------: | -------------: |
|  3.1 |              3 |             4 |              3 |              3 |
|  3.6 |              3 |             4 |              4 |              3 |
| -1.1 |             -2 |            -1 |             -1 |             -1 |
| -1.6 |             -2 |            -1 |             -2 |             -1 |

#### Imprecise calculations

```js
1e500; // Infinity

0.1 + 0.2 = 0.3; // false

0.1.toFixed(20); // "0.10000000000000000555"
0.2.toFixed(20); // "0.20000000000000001110"
0.3.toFixed(20); // "0.29999999999999998890"
0.4.toFixed(20); // "0.40000000000000002220"
0.5.toFixed(20); // "0.50000000000000000000"
0.6.toFixed(20); // "0.59999999999999997780"
0.7.toFixed(20); // "0.69999999999999995559"
0.8.toFixed(20); // "0.80000000000000004441"
0.9.toFixed(20); // "0.90000000000000002220"

9_999_999_999_999_999; // 10000000000000000

-0 === 0; // true
```

#### Tests: `isFinite` and `isNaN`

```js
isNaN(NaN); // true
isNaN("str"); // true

NaN === NaN; // false

isFinite("15"); // true
isFinite("str"); // false, value: NaN
isFinite(Infinity); // false
```

- `Number.isNaN()` and `Number.isFinite()` do not autoconvert their argument into a number.
- `Number.isNaN(value)`: returns `true` if value belongs to the `number` type and it is `NaN`.
- `Number.isFinite(value)`: returns `true` if value belongs to the `number` type and it isn't `NaN`, `Infinity`, `-Infinity`.

```js
Number.isNaN(NaN); // true
Number.isNaN("str"); // false, because "str" is string

Number.isFinite(123); // true
Number.isFinite(Infinity); // false
Number.isFinite(2 / 0); // false
Number.isFinite("15"); // false, because "15" is string
```

```js
Object.is(NaN, NaN); // true
NaN === NaN; // false

Object.is(-0, 0); // false
-0 === 0; // true
```

#### `parseInt()` and `parseFloat()`

```js
parseInt("100px"); // 100
parseFloat("12.5em"); // 12.5

parseInt("12.3"); // 12.3
parseFloat("12.3.4"); // 12.3

parseInt("a123"); // NaN

// parseInt(str, radix)
parseInt("0xff", 16); // 255
parseInt("ff", 16); // 255
parseInt("2n9c", 36); // 123456
```

#### Other math functions

```js
Math.random(); // [0, 1)

Math.max(3, 5, -10, 0, 1); // 5
Math.min(1, 2); // 1

Math.pow(2, 10); // 2^10 = 1024
```

### 03. Strings

#### Special characters

| Character        | Description                                            |
| ---------------- | ------------------------------------------------------ |
| `\n`, `r`        | New line                                               |
| `\t`             | Tab                                                    |
| `\b`, `\f`, `\v` | Backspace, From Feed, Vertical Tab - not used nowadays |

#### Accessing characters

```js
const name = "Ali";

str[0]; // "A"
str.at(0); // "A"

str[str.length - 1]; // "i"
str.at(-1); // "i"

for (const char of name) char; // "A", "l", "i"
```

#### Searching for a substring

```js
const str = "Widget with id";

str.indexOf("id", 2); // 12
str.lastIndexOf("id", 11); // 1
```

#### Getting a substring

```js
const str = "stringify";

// str.slice(start [, end]): allows negatives
str.slice(2); // "ringify"
str.slice(0, 5); // "strin"
str.slice(-4, -1); // "gif"

// str.substring(start [, end]): converts negatives to 0
str.substring(2, 6); // ring
str.substring(6, 2); // ring

// str.substr(start [, length]): allows negative start
str.substr(2, 4); // ring
str.substr(-4, 2); // gi
```

#### Comparing strings

```js
"Z".codePointAt(0); // 90
"z".codePointAt(0); // 122

String.fromCodePoint(90): // Z
String.fromCodePoint(0x5a): // Z

"ö" > "z"; // true
"ö".localeCompare("z", "tr"); // -1
```

```js
"a".repeat(3); // "aaa"
```

### 04. Arrays

- Get last element
```js
[1, 2, 3].at(-1); // 3
```

#### Methods `pop`/`push` and `shift`/`unshift`

```js
const fruits = ["Apple", "Orange", "Pear"];

// pop()
const pear = fruits.pop(); // "Pear"
fruits; // "Apple", "Orange"

// push(...items)
const ananas = fruits.push("Ananas"); // "Ananas"
fruits; // "Apple", "Orange", "Ananas"

// shift()
const apple = fruits.shift(); // "Apple"
fruits; // "Orange", "Ananas"

// unshift(...items)
const length = fruits.unshift("Watermelon"); // 3
fruits; // "Watermelon", "Orange", "Ananas"
```

#### The ways to misuse an array
- Add a non-numeric property like `arr.test = 5`.
- Make holes, like add `arr[0]` and then `arr[1000]` (and nothing between them).
- Fill the array in the reverse order, like `arr[1000]`, `arr[999]` and so on.

#### A word about `length`

```js
const arr = [1, 2, 3, 4, 5];

arr.length = 2;
arr; // [1, 2]

arr.length = 5;
arr; // [1, 2,,,]
```

#### `new Array()`

```js
const arr = new Array("Apple", "Orange");

const arr2 = new Array(2);
arr2[0]; // undefined
arr2.length; // 2
```

#### `toString()`

```js
const arr = [1, 2, 3];
String(arr); // "1,2,3"
```

#### Don't compare arrays with `==`

```js
[] == []; // false
[0] == [0]; // false

0 == []; // true, 0 == ""
0 == [0]; // true, 0 == 0
"0" == []; // false, "0" == ""
```

### 05. Array Methods

#### `splice`

```js
const arr = ["I", "go", "home"];

delete arr[1];
arr[1]; // undefined
arr; // ["I", ,"home"]
arr.length; // 3
```

```js
// arr.splice(start [, deleteCount, elem1, ..., elemN])
const arr = ["I", "go", "home"];
const go = arr.splice(1, 1); // ["go"]
arr; // ["I", "home"]
```

```js
const arr = [1, 2, 5];
arr.splice(-1, 0, 3, 4);
arr; // [1, 2, 3, 4, 5]
```

#### `slice`

```js
const arr = ["t", "e", "s", "t"];
arr.slice(1, 3); // ["e", "s"]
arr.slice(-2); // ["s", "t"]
```

#### `concat`

```js
[1, 2].concat([3, 4], 5, 6); // [1, 2, 3, 4, 5, 6]
```

```js
const arr = {
  0: "a",
  1: "b",
  [Symbol.isConcatSpreadable]: true,
  length: 2,
};
[1, 2].concat(arr); // [1, 2, "a", "b"];
```

#### Iterate: `forEach`

```js
["a", "b"].forEach((item, index, arr) => {
  item; // "a", "b"
  index; // 0, 1
  arr; // ["a","b"], ["a","b"]
});
```

#### Searching in array

```js
const arr = [1, 0, false, 0, NaN];

arr.indexOf(0); // 1
arr.indexOf(2); // -1
arr.lastIndexOf(0); // 3

arr.indexOf(NaN); // -1
arr.includes(NaN); // true
```

#### `find` and `findIndex`/`findLastIndex`

```js
["a", "b"].find((item, index, arr) => {
  item; // "a", "b"
  index; // 0, 1
  arr; // ["a","b"], ["a","b"]
});
```

```js
const users = [
 { id: 1, name: "Ali" },
 { id: 2, name: "Veli" },
 { id: 3, name: "Ali" },
];

users.find((u) => u.name === "Ali"); // { id: 1, name: "Ali" }
users.findIndex((u) => u.name === "Ali"); // 0
users.findLastIndex((u) => u.name === "Ali"); // 2
```

#### `filter`

```js
["a", "b"].filter((item, index, arr) => {
  item; // "a", "b"
  index; // 0, 1
  arr; // ["a","b"], ["a","b"]
});
```

```js
const users = [
 { id: 1, name: "Ali" },
 { id: 2, name: "Veli" },
 { id: 3, name: "Ali" },
];

users.filter((u) => u.name === "Ali"); // [{ id: 1, name: "Ali" }, { id: 3, name: "Ali" }]
```

#### `map`

```js
["a", "b"].map((item, index, arr) => {
  item; // "a", "b"
  index; // 0, 1
  arr; // ["a","b"], ["a","b"]
});
```

```js
[1, 2].map((num) => num * 2); // [2, 4]
```

#### `sort`

```js
const arr = [1, 2, 15];
arr.sort(); // Sorted by string
arr; // [1, 15, 2];

arr.sort((a, b) => a - b);
arr; // [1, 2, 15];
```

```js
const arr = ["ö", "z", "a"];
arr.sort();
arr; // ["a", "z", "ö"]

arr.sort((a, b) => a.localeCompare(b, "tr"));
arr; // ["a", "ö", "z"]
```

#### `reverse`

```js
const arr = [1, 2, 3];
arr.reverse();
arr; // [3, 2, 1]
```

#### `split`

```js
"1, 2, 3, 4".split(", ", 2); // ["1", "2"]
```

#### `reduce`/`reduceRight`

```js
const sum = [1, 2, 3].reduce((accumulator, item, index, arr) => {
  accumulator; // 0, 1, 3
  item; // 1, 2, 3
  index; // 0, 1, 2
  arr; // [1,2,3], [1,2,3], [1,2,3]
  return accumulator + item;
}, 0);
sum; // 6
```

#### `some`/`every`

```js
[1, 2, 3].some((num) => num % 2 === 0); // true
[1, 2, 3].every((num) => num % 2 === 0); // false
```

#### `fill`

```js
[1, 2, 3].fill("a", 1, 1); // [1, "a", 2]
```

#### `copyWithin`

```js
const arr = ["a", "b", "c", "d", "e"];
arr.copyWithin(0, 3, 5); // copy ["d", "e"] to index 0
arr; // ["d", "e", "c", "d", "e"]
```

#### `flat`/`flatMap`

```js
[1, 2, [3, 4]].flat(); // [1, 2, 3, 4]
[1, 2, [3, 4, [5, 6]]].flat(2); // [1, 2, 3, 4, 5, 6]
[1, 2, [3, 4, [5, 6, [7, 8, [9, 10]]]]].flat(Infinity); // [1,2,3,4,5,6,7,8,9,10]
```

```js
[1, 2, 3].flatMap((num) => Array(num).fill(num)); // [1, 2, 2, 3, 3, 3]
```

#### `Array.isArray`

```js
typeof {}; // "object"
typeof []; // "object"

Array.isArray({}); // false
Array.isArray([]); // true
```

#### Most methods support `thisArg`

- Almost all array methods that call functions accept an optional parameter `thisArg` except `sort`.

```js
arr.find(() => {}, thisArg);
arr.filter(() => {}, thisArg);
arr.map(() => {}, thisArg);
```

```js
const army = {
  minAge: 18,
  maxAge: 27,
  canJoin(user) {
    const age = user.age;
    return age >= this.minAge && age < this.maxAge;
  },
};

const users = [
  { age: 16 },
  { age: 20 },
  { age: 23 },
  { age: 30 },
];

const soldiers = users.filter((user) => army.canJoin(user), army);
soldiers; // [{ age: 20 }, { age: 23 }]
```

### 06. Iterables

#### `Symbol.iterator`

```js
const range = {
  from: 1,
  to: 3,
  [Symbol.iterator]() {
    this.current = this.from;
    return this;
  },
  next() {
    if (this.current <= this.to) {
      return { done: false, value: this.current++ };
    }
    return { done: true };
  },
};

for (const num of range) num; // 1, 2, 3
```

#### String is iterable

```js
const str = "𝒳😂";
for (const char of str) char; // "𝒳", "😂"

str.length; // 4
str.charCodeAt(0); // 55349
```

#### Calling an iterator explicitly

```js
const str = "Hello";
const iterator = str[Symbol.iterator]();
while(true) {
  const res = iterator.next();
  if (res.done) break;
  res.value; // "H", "e", "l", "l", "o"
}
```

#### Iterables and array-likes

```js
const iterable = {
  from: 1,
  to: 3,
  [Symbol.iterator]() {},
};

const arrayLike = {
  0: "a",
  1: "b",
  length: 2,
};
```

#### `Array.from(obj[, mapFn, thisArg])`

```js
const arrayLike = {
  0: "a",
  1: "b",
  length: 2,
};

const arr = Array.from(arrayLike);
arr; // ["a", "b"]
```

```js
const iterable = {
  from: 1,
  to: 3,
  [Symbol.iterator]() {
    this.current = this.from;
    return this;
  },
  next() {
    if (this.current > this.to) return { done: true };
    return { done: false, value: this.current++ };
  },
};

const arr = Array.from(iterable, (num) => num * num);
arr; // [1, 4, 9]
```

```js
const str = "𝒳😂";
str.length; // 4

const chars = Array.from(str);
chars.length; // 2
chars[0]; // "𝒳"
chars[1]; // "😂"

for (const char of str) char; // "𝒳", "😂"

str.slice(1, 3); // "##" (two pieces from different surrogate pairs)
str.slice(0, 2); // "𝒳"
```

### 07. Map and Set

#### Map

- `Map` allows keys of any type. (`Object` keys converted to string).

##### Methods and properties:
- `new Map()`: creates the map.
- `map.set(key, value)`: stores the value by the key.
- `map.get(key)`: returns the value by the key, `undefined` if `key` doesn't exist in map.
- `map.has(key)`: returns `true` or `false`.
- `map.delete(key)`: removes the element.
- `map.clear()`: removes everything from the map.
- `map.size`: returns the current element count.

```js
const map = new Map();

map
  .set("1", "abc")
  .set(1, 123);

map.get("1"); // "abc"
map.get(1); // 123

const obj = { name: "Ali" };
map.set(obj, 20);
map.get(obj); // 20

map.keys().next().value; // "1"
map.values().next().value; // "abc"
map.entries().next().value; // ["1", "abc"]

for (const [key, value] of map) {
  key; // "1", 1, { name: "Ali" }
  value; // "abc", 123, 20
}

map.forEach((value, key, map) => {
  key; // "1", 1, { name: "Ali" }
  value; // "abc", 123, 20
  map.size; // 3, 3, 3
})
```

#### `Object.entries`: `Map` from `Object`

```js
const map = new Map([
  ["1", "abc"],
  [1, 123],
  [true, false],
]);
```

```js
const obj = {
  name: "Ali",
  age: 20,
};

const map = new Map(Object.entries(obj));
map.get("name"); // "Ali"
map.get("age"); // 20
```

```js
const map = new Map([["a", 1], ["b", 2]])
const obj = Object.fromEntries(map); // omit .entries()
obj; // { "a": 1, "b": 2 }
```

#### Set

- Collection of unique values.

##### Methods and properties:
- `new Set([iterable])`: creates the set.
- `set.add(value)`: adds a value, returns the set itself.
- `set.delete(value)`: removes the value, returns `true` if `value` existed or `false`.
- `set.has(value)`: returns `true` if value exists or `false`.
- `set.clear()`: removes everything from the set.
- `set.size`: elements count.

```js
const set = new Set();

const ali = { name: "Ali" };
const veli = { name: "Veli" };

set.add(ali);
set.add(veli);
set.add(ali);

set.size; // 2
```

#### Iteration over `Set`

```js
const set = new Set(["a", "b", "c"]);
set; // { "a", "b", "c" }

for (const char of set) char; // "a", "b", "c"

set.forEach((value, valueAgain, set) => {
  value; // "a", "b", "c"
  valueAgain; // "a", "b", "c"
  set; // { "a", "b", "c" }, { "a", "b", "c" }, { "a", "b", "c" }
});

set.keys().next().value; // "a"
set.value().next().value; // "a"
set.entries().next().value; // ["a", "a"]
```

### 08. WeakMap and WeakSet

#### `WeakMap`

- `WeakMap` keys must be objects, not primitive values.

```js
const ali = { name: "Ali" };

const weakMap = new WeakMap();
weakMap.set(ali, 20);

ali = null;
// ali is removed from memory
```

##### Methods and properties:
- `weakMap.set(key, value)`
- `weakMap.get(key)`
- `weakMap.delete(key)`
- `weakMap.has(key)`

#### `WeakSet`

```js
const weakSet = new WeakSet();

const ali = { name: "Ali" };

weakSet.add(ali);
weakSet.has(ali); // true

ali = null;
// ali is removed from memory
```

##### Methods and properties:
- `weakSet.add(value)`
- `weakSet.delete(key)`
- `weakSet.has(key)`

### 09. Object.keys, values, entries

```js
const id = Symbol("id");
const obj = {
  [id]: 123,
  name: "Ali",
  age: 20,
};

Object.keys(obj); // ["name", "age"]
Object.values(obj); // ["Ali", 20]
Object.entries(obj); // [["name", "Ali"], ["age", 20]]
```

#### Transforming objects

```js
const prices = {
  banana: 1,
  orange: 2,
  apple: 4,
};

const doublePrices = Object.fromEntries(
  Object.entries(prices).map(([key, value]) => [key, value * 2])
);
doublePrices; // { "banana": 2, "orange": 4, "apple": 8 }
```

### 10. Destructuring Assignment

```js
const arr = ["Ali", 20, true, {}];
const [name, age, ...rest] = arr;

const obj = {
  name: "Ali",
  age: 20,
  isAdmin: true,
  address: {}
};
const { age: userAge, age = 18, salary: income = 1_000, ...rest } = obj;

const greet = ({ name = "Ali" } = {}) => {}
```

### 11. Date and Time

#### Creation

```js
const Jan01_1970 = new Date(0);
const Dec31_1969 = new Date(-24 * 60 * 60);

new Date("2000-01-31"); // 31 Jan 2000
new Date(2000, 0, 1, 0, 0, 0); // 1 Jan 2000, 00:00:00
```

#### Access date components

- `getFullYear()`: `2000`
- `getMonth()`: 0..11
- `getDate()`: 1..31
- `getHours()`, `getMinutes()`, `getSeconds()`, `getMilliseconds()`
- `getDay()`: 0..6: `0` Sunday, `6` Saturday
- `getUTCFullYear()`, `getUTCMonth()`, `getUTCDay()`
- `getTime()`: `0` is 1 Jan 1970.
- `getTimezoneOffset()`: `60` is UTC-1, `-180` is UTC+3.
- `getYear()`: deprecated. Use `getFullYear()` instead.

#### Setting date components

- Every one of them except `setTime()` has a UTC-variant.
- `setFullYear(year, [month], [date])`
- `setMonth(month, [date])`
- `setDate(date)`
- `setHours(hour, [min], [sec], [ms])`
- `setMinutes(min, [sec], [ms])`
- `setSeconds(sec, [ms])`
- `setMilliseconds(ms)`
- `setTime(ms)`

#### Autocorrection

```js
const date = new Date(2000, 0, 32); // 1 Feb 2000
date.setDate(-3); // 28 Jan 2000
```

#### `Date.now()`

```js
const ms = Date.now(); // milliseconds count from 1 Jan 1970
```

#### `Date.parse` from a string

- `YYYY-MM-DDTHH:mm:ss.sssZ`
- `YYYY-MM-DD`: year-month-day.
- `T`: delimeter.
- `HH:mm:ss.sss`: hours, minutes, seconds and milliseconds.
- `Z`: UTC+0

```js
const ms = Date.parse("2000-01-13T15:45:59.865+03:00");
ms; // 947767559865
```

### 12. JSON Methods, toJSON

#### `JSON.stringify`

```js
const student = {
  name: "Ali",
  age: 20,
  isAdmin: false,
  courses: ["haskell", "elixir"],
  address: null
};

JSON.stringify(student);
/*
{
  "name": "Ali",
  "age": 20,
  "isAdmin": false,
  "courses": ["haskell", "elixir"],
  "address": null
}
*/
```

- Methods, symbols and undefined properties are skipped.

```js
const user = {
  greet() {},
  [Symbol("id")]: 123,
  something: undefined,
};

JSON.stringify(user); // "{}"
```

- No circular references.

```js
const room = { number: 42 };
const meetup = { participants: ["ali", "veli"] };

meetup.place = room;
room.occupiedBy = meetup;

JSON.stringify(meetup); // Error: cyclic object value
```

#### Excluding and transforming: replacer

- `JSON.stringify(value[, replacer, space])`

```js
const meetup = {
  title: "Conference",
  participants: [{ name: "Ali", age: 20 }, { name: "Veli", age: 40 }],
  space: 40
};

JSON.stringify(meetup, ["title", "participants", "name"], 4);
/*
{
    "title": "Conference",
    "participants": [{ "name": "Ali" }, { "name": "Veli" }]
}
*/
```

#### Custom `toJSON`

```js
const room = {
  number: 42,
  toJSON() {
    return this.number;
  },
};

JSON.stringify(room); // 42
```

#### `JSON.parse`

- `JSON.parse(str, [revier])`

```js
let str = '{"title":"Conference","date":"2000-01-01T12:00:00.000Z"}';

const meetup = JSON.parse(str, (key, value) => {
  if (key === "date") return new Date(value);
  return value;
});

meetup; // { "title": "Conference", "date": "2000-01-01T12:00:00.000Z" }
```

## 05. Advanced Working with Functions

### 02. Rest Parameters and Spread Syntax

```js
const sumAll = (...nums) => {
  let sum = 0;
  for (let num of nums) sum += num;
  return sum;
}
sumAll(1, 2, 3); // 6
```

#### The `arguments` variable

```js
function sumAll() {
  console.log(arguments) // { 0: 1, 1: 2, 2: 3, length: 3 }
  console.log(arguments.length) // 3
  console.log(arguments[0], arguments[1], arguments[2]) // 1, 2, 3

  let sum = 0;
  for (let num of arguments) sum += num;
  return sum;
}
sumAll(1, 2, 3); // 6
```

### 03. Variable Scope, Closure

- Closure: functions that remember their outer variables and can access them. They automatically remember where they were created using a hidden `[[Environment]]` property.

#### Code blocks

```js
{
  const name = "Ali";
  console.log(name); // Ali
}
console.log(name); // Error: name is not defined

const age = 20;
const age = 30; // Error: Cannot redeclare block-scoped variable
```

#### Lexical environment

- All functions remember the Lexical Environment in which they were made.

```js
// # global LexicalEnvironment --[outer]-> null #
// - makeCounter: function
// - counter: function
function makeCounter() {
  // # LexicalEnvironment of makeCounter() call --[outer]-> global
  // - count: 0 (becomes 1 after seconds counter() call)
  let count = 0;
  return function() {
    // # LexicalEnvironment: [[Environment]] --[outer]-> makeCounter()
    // - <empty>
    return count++;
  }
}

const counter = makeCounter();
console.log(counter()); // 0
console.log(counter()); // 1
```

### 04. The Old `var`

#### `var` has no block scope

```js
{
  var name = "Ali";
  console.log(name); // "Ali"
}
console.log(name); // "Ali"

for (var i = 0; i < 3; i++) console.log(i) // 0, 1, 2
console.log(i); // 3
```

#### `var` tolerates redeclarations

```js
var age = 20;
var age = 30;
console.log(age) // 30
```

#### `var` variables can declared below their use

```js
const greet = () => {
  name = "Ali";

  console.log(name, age); // "Ali" undefined

  var name; // hoisted to the top of the function
  var age = 20; // declarations are hoisted, but assignments are not
}
greet();
```

#### IIFE

- Emulates block-level visibility for `var`.

```js
(function() {
  var name = "Ali";
  console.log(name); // "Ali"
})();
console.log(name); // Error: name is not defined

(function() {
  console.log("IIFE"); // "IIFE"
}());
!function() {
  console.log("Bitwise"); // "Bitwise"
}();
+function() {
  console.log("Unary plus"); // "Unary plus"
}();
```

### 06. Function Object, NFE

#### The `name` property

```js
function sum() {}
console.log(sum.name); // "sum"

const greet = function() {}
console.log(greet.name); // "greet"

const farewell = () => {}
console.log(farewell.name); // "farewell"

function fn(callback = function() {}) {
  console.log(callback.name); // "callback"
}
fn();

const arr = [function() {}];
console.log(arr[0].name); // [empty string]
```

#### The `length` property

```js
const a = (a1, ...rest) => {}
console.log(a.length); // 1

function b(a1, a2, ...rest) {}
console.log(b.length); // 2
```

#### Custom properties

```js
const greet = () => {
  greet.counter++;
  return "Hi";
}
greet.counter = 0;

console.log(greet()); // "Hi"
console.log(greet()); // "Hi"
console.log(greet.counter); // 2
```

#### Named Function Expression

```js
const greet = function print(name) {
  if (name) {
    console.log(name);
  } else {
    print("Ali");
  }
}
greet(); // "Ali"
greet("Veli"); // "Veli"
```

### 07. The `new Function` Syntax

```js
const sum = new Function("a", "b", "return a + b");
console.log(sum(1, 2)); // 3

const sum2 = new Function("a,b", "return a + b");
console.log(sum2(1, 2)); // 3
```

### 09. Decorators and Forwarding, `call`/`apply`

- Decorator: a special function that takes another function and alters its behavior.

```js
const worker = {
  name: "worker",
  process(size) {
    console.log(`${this.name}.process(${size})`)
    const arr = [];
    for (let i = 0; i < size; i++) arr.push(i);
    return arr;
  }
}

const cachingDecorator = (fn) => {
  const cache = new Map();
  return function() {
    // # Borrowing Array.join method
    const key = [].join.apply(arguments, [","]);
    if (cache.has(key)) return cache.get(key);

    const result = fn.call(this, ...arguments);
    cache.set(key, result);
    return result;
  }
}

worker.process = cachingDecorator(worker.process);
worker.process(1000); // worker.process(1000)
worker.process(1000);
```

#### Decorators and function properties

- If the original function had properties on it, like `fn.calledCount`, then the decorated one will not provide them.

### 10. Function Binding

#### Losing `this`

```js
const veli = { name: "Veli" }
const ali = {
  name: "Ali",
  greet() {
    return { 
      isGlobal: this === globalThis,
      message: `Hi, ${this.name}!`
    };
  }
}
console.log(ali.greet()); // { isGlobal: false, message: "Hi, Ali!" }

const greetAli = ali.greet;
console.log(greetAli()); // { isGlobal: true, message: "Hi, undefined!" }

const greetVeli = ali.greet.bind(veli);
console.log(greetVeli()); // { isGlobal: false, message: "Hi, Veli!" }
```

#### Partial function

```js
const sum = (a, b) => a + b;

const plusTwo = sum.bind(null, 2);
console.log(plusTwo(3)); // 5
```
