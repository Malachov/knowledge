# Js reference

## Table of Contents

- [Js reference](#js-reference)
  - [Table of Contents](#table-of-contents)
  - [Docs, learning material](#docs-learning-material)
  - [Installation](#installation)
    - [Volta](#volta)
    - [Dependencies](#dependencies)
  - [Syntax](#syntax)
    - [Misc](#misc)
      - [Values and operators](#values-and-operators)
      - [String methods](#string-methods)
      - [String internals](#string-internals)
      - [Base64 encoding](#base64-encoding)
      - [Array methods](#array-methods)
      - [Lodash](#lodash)
      - [Array iteration](#array-iteration)
      - [Iterable protocol](#iterable-protocol)
      - [Equality](#equality)
      - [Number parsing](#number-parsing)
      - [Maps and sets](#maps-and-sets)
      - [Object entries](#object-entries)
      - [Property flags and descriptors](#property-flags-and-descriptors)
      - [Property getters and setters](#property-getters-and-setters)
      - [Internal slots and methods](#internal-slots-and-methods)
      - [Proxy and Reflect](#proxy-and-reflect)
      - [Binary data](#binary-data)
      - [WeakRef and FinalizationRegistry](#weakref-and-finalizationregistry)
      - [Destructuring](#destructuring)
      - [Rest parameters and spread syntax](#rest-parameters-and-spread-syntax)
      - [JSON](#json)
      - [Promises](#promises)
      - [Promise chaining](#promise-chaining)
      - [Promise combinators](#promise-combinators)
        - [Promise.all](#promiseall)
        - [Promise.allSettled](#promiseallsettled)
        - [Promise.race](#promiserace)
        - [Promise.any](#promiseany)
      - [Async/await](#asyncawait)
      - [Generator functions](#generator-functions)
      - [Async iteration and generators](#async-iteration-and-generators)
      - [Errors and try-catch](#errors-and-try-catch)
      - [Symbols](#symbols)
      - [Custom primitive conversion](#custom-primitive-conversion)
      - [Function behavior](#function-behavior)
        - [Function parameters](#function-parameters)
        - [Function custom properties](#function-custom-properties)
        - [Function call context](#function-call-context)
        - [Function arguments](#function-arguments)
        - [Borrowing a method](#borrowing-a-method)
        - [Function binding and partial functions](#function-binding-and-partial-functions)
        - [Currying](#currying)
        - [Reference type](#reference-type)
        - [How `this` is determined](#how-this-is-determined)
        - [Arrow functions and `this`](#arrow-functions-and-this)
        - [Constructor functions](#constructor-functions)
        - [Prototypal inheritance](#prototypal-inheritance)
        - [Class syntax](#class-syntax)
        - [Mixins](#mixins)
        - [Instanceof](#instanceof)
      - [Modules](#modules)
        - [Imports and exports](#imports-and-exports)
        - [Default exports](#default-exports)
        - [Re-exports](#re-exports)
    - [TypeScript](#typescript)
      - [Imports](#imports)
      - [Interfaces](#interfaces)
      - [Classes](#classes)
      - [Functions](#functions)
  - [Conditions](#conditions)
    - [Switch](#switch)
    - [HTML interaction](#html-interaction)
  - [Frameworks](#frameworks)
    - [Angular](#angular)
      - [NgModules and standalone components](#ngmodules-and-standalone-components)
      - [Change detection](#change-detection)
      - [Model inputs and two-way binding](#model-inputs-and-two-way-binding)
      - [Template syntax](#template-syntax)
      - [Directives and bindings](#directives-and-bindings)

## Docs, learning material

Very good learning sources:

- [JS](https://javascript.info)

## Installation

### Volta

```shell
curl https://get.volta.sh | bash

# Set default version
volta install node@24

# Set project default version (to package.json)
volta pin node@24
```

### Dependencies

Use `npm install` from the project directory to add a dependency. npm records the dependency in
`package.json` and updates `package-lock.json`:

```shell
# Runtime dependency
npm install lodash

# Development-only dependency
npm install --save-dev vitest
```

The resulting `package.json` contains the dependency under either `dependencies` or
`devDependencies`. Commit both `package.json` and `package-lock.json` when the project tracks the
lockfile.

Run a locally installed package command with `npm exec -- <command>`. The `--` separates npm's
options from the command and its arguments:

```shell
npm exec -- tsc --noEmit
```

## Syntax

### Misc

#### Values and operators

Backticks create template literals. They support interpolation with `${...}`, multiline strings, and
expressions, so they provide more functionality than single or double quotes:

```js
const name = "Peter";
const greeting = `Hello, ${name}!`;
const multiline = `First line
Second line`;

let str2 = `Hello
World`;
```

#### String methods

These string methods return booleans and are case-sensitive:

```js
const message = "Hello, world!";

message.endsWith("!"); // true
message.startsWith("Hello"); // true
message.includes("world"); // true
"a,b,c".split(","); // ["a", "b", "c"]
```

`split` returns an array of substrings. With no separator, it returns an array containing the whole
string; with an empty separator, it splits between characters:

```js
"hello world".split(" "); // ["hello", "world"]
"hello".split(""); // ["h", "e", "l", "l", "o"]
```

#### String internals

JavaScript strings are immutable sequences of UTF-16 code units. Escape sequences can represent
characters by their code units: `\xNN` uses two hexadecimal digits, while `\uNNNN` uses four:

```js
alert("\u00A9"); // ©, the same as "\xA9" using four-digit hex notation
```

Characters outside the Basic Multilingual Plane use a surrogate pair and may therefore have a
`.length` greater than the number of user-perceived characters. Use `Array.from` or the spread
syntax when iterating by Unicode code point:

```js
const musicalSymbol = "𝄞";

musicalSymbol.length; // 2 UTF-16 code units
Array.from(musicalSymbol).length; // 1 Unicode code point
```

#### Base64 encoding

`btoa` encodes a binary string as Base64, and `atob` decodes a Base64 string back to a binary
string. These browser APIs are intended for byte-oriented data, not arbitrary Unicode text:

```js
const encoded = btoa("Hello"); // "SGVsbG8="
const decoded = atob(encoded); // "Hello"
```

For Unicode text, encode the text as UTF-8 bytes before Base64 encoding and decode the bytes after
`atob`. In Node.js, use `Buffer` instead:

```js
const encoded = Buffer.from("Hello").toString("base64");
const decoded = Buffer.from(encoded, "base64").toString("utf8");
```

#### Array methods

`pop` removes and returns the last item. `push` appends one or more items and returns the new array
length. `shift` removes and returns the first item, while `unshift` prepends one or more items and
returns the new array length. All four methods mutate the array:

```js
const fruits = ["apple", "banana"];

fruits.push("cherry"); // 3; fruits is now ["apple", "banana", "cherry"]
fruits.pop(); // "cherry"; fruits is now ["apple", "banana"]
fruits.unshift("orange"); // 3; fruits is now ["orange", "apple", "banana"]
fruits.shift(); // "orange"; fruits is now ["apple", "banana"]
```

`splice` changes an array in place by removing, replacing, or inserting items. It returns the items
that were removed. `slice` returns a shallow copy of a range without changing the original array:

```js
const numbers = [1, 2, 3, 4];

numbers.splice(1, 2, "two", "three"); // [2, 3]
numbers; // [1, "two", "three", 4]

numbers.slice(1, 3); // ["two", "three"]
numbers; // unchanged
```

`concat` returns a new array by combining arrays or values. It does not mutate its inputs:

```js
const first = [1, 2];
const second = [3, 4];

first.concat(second, 5); // [1, 2, 3, 4, 5]
```

`forEach` calls a function once for each element and returns `undefined`. Use it for side effects,
not when a transformed array is needed:

```js
numbers.forEach((number, index) => {
  console.log(index, number);
});
```

`indexOf` returns the first matching index or `-1`; `includes` returns whether an array contains a
value. Both use strict equality for ordinary values:

```js
numbers.indexOf("two"); // 1
numbers.includes("three"); // true
numbers.indexOf("missing"); // -1
```

`find` returns the first element matching a predicate, or `undefined` when there is no match.
`filter` returns a new array containing every matching element:

```js
const firstLargeNumber = numbers.find((number) => number > 2);
const largeNumbers = numbers.filter((number) => number > 2);
```

`map` returns a new array containing the callback results, preserving the original array:

```js
const doubled = [1, 2, 3].map((number) => number * 2); // [2, 4, 6]
```

`sort` mutates the array and, without a comparator, compares elements as strings. Provide a numeric
comparator when sorting numbers:

```js
["pear", "apple", "orange"].sort(); // ["apple", "orange", "pear"]
[10, 2, 1].sort(); // [1, 10, 2] because values are compared as strings
[10, 2, 1].sort((a, b) => a - b); // [1, 2, 10]
[10, 2, 1].sort((a, b) => b - a); // [10, 2, 1]
```

`reduce` combines an array into one value. Supplying an initial value makes the accumulator's type
and empty-array behavior explicit:

```js
const total = [1, 2, 3].reduce((sum, number) => sum + number, 0); // 6
```

#### Lodash

Import Lodash as `_` and call its utilities through that namespace:

```js
import _ from "lodash";

_.chunk([1, 2, 3, 4], 2); // [[1, 2], [3, 4]]
_.get({ user: { name: "Peter" } }, "user.name"); // "Peter"
```

`Array.from` creates a new array from an iterable or an array-like value. It can also transform each
item while creating the array:

```js
Array.from("hello"); // ["h", "e", "l", "l", "o"]
Array.from({ length: 3 }, (_, index) => index + 1); // [1, 2, 3]
```

#### Array iteration

`for...of` iterates over the values produced by an iterable. For an array, that means the elements:

```js
for (let fruit of fruits) {
  alert(fruit);
}
```

`for...in` iterates over enumerable property keys. On an array, those keys are usually indexes as
strings, so it is generally the wrong choice when you want the array's elements. It can also include
other enumerable properties, so use `for...of` for array values and `for...in` for inspecting object
keys:

```js
for (let index in fruits) {
  console.log(index); // "0", "1", ...
}
```

#### Iterable protocol

`for...of`, `Array.from`, and the spread syntax use an object's `[Symbol.iterator]` method to obtain
an iterator. The iterator's `next()` method returns objects with `value` and `done` properties:

```js
const countToThree = {
  [Symbol.iterator]() {
    let current = 1;

    return {
      next() {
        return current <= 3 ? { value: current++, done: false } : { value: undefined, done: true };
      },
    };
  },
};

for (const number of countToThree) {
  console.log(number); // 1, 2, 3
}
```

#### Equality

`===` is strict equality: it compares values without coercing their types. `==` is loose equality:
it may convert operands to compatible types before comparing them. Prefer `===` and `!==` so type
conversion is explicit:

```js
0 == "0"; // true: the string is converted to a number
0 === "0"; // false: the types differ
null == undefined; // true: a special loose-equality rule
null === undefined; // false
```

#### Number parsing

`parseInt` parses an integer from the beginning of a string. Pass a radix, normally `10`, so the
intended numeral system is explicit. `parseFloat` parses a decimal number from the beginning of a
string:

```js
parseInt("42", 10); // 42
parseInt("42px", 10); // 42
parseFloat("3.14rem"); // 3.14
```

Both functions stop when the numeric portion ends. Use `Number(value)` when the entire value should
be a valid number, because it does not silently accept a trailing suffix such as `"px"`.

#### Maps and sets

`Map` stores key-value pairs and allows keys of any type. Use its methods rather than assigning with
`map[key]`: bracket assignment creates an ordinary property on the Map object, not a Map entry.

```js
const scores = new Map();
const player = { name: "Peter" };

scores.set(player, 42);
scores.set("bonus", 10);
scores.get(player); // 42
scores.has("bonus"); // true
scores.delete("bonus"); // true
scores.size; // 1
```

Iterate over a Map with `for...of`, or use `keys()`, `values()`, or `entries()` when a particular
view is needed. Each entry is a two-item `[key, value]` array:

```js
for (const [key, value] of scores) {
  console.log(key, value);
}

for (const key of scores.keys()) {
  console.log(key);
}
```

`Set` stores unique values. `add` returns the Set, while `has`, `delete`, and `clear` inspect or
change its contents:

```js
const tags = new Set(["js", "web", "js"]);

tags.add("frontend");
tags.has("js"); // true
tags.delete("web"); // true
tags.size; // 2

for (const tag of tags) {
  console.log(tag);
}

tags.clear();
```

JavaScript does not raise a normal arithmetic error for division by zero:

```js
1 / 0; // Infinity
0 / 0; // NaN (Not-a-Number)
Number.isNaN(0 / 0); // true
```

`null` is an intentional absence of an object value. `undefined` usually means that a value has not
been assigned or that a requested property does not exist. They are distinct values:

```js
const emptyValue = null;
let notAssigned;

typeof emptyValue; // "object" (a historical JavaScript quirk)
typeof notAssigned; // "undefined"
typeof missingVariable; // "undefined" only when the identifier is not accessed directly
```

`typeof` returns a string describing the type category of an expression. It is safe for checking
whether an undeclared identifier exists only when the identifier is passed directly to `typeof`:

```js
if (typeof maybeDefined === "undefined") {
  // maybeDefined is not available.
}
```

The logical OR operator `||` returns the first truthy value:

```js
const result = value1 || value2 || value3;
```

The logical AND operator `&&` returns the first falsy value. If every value is truthy, it returns
the last value. Both `&&` and `||` short-circuit, so later expressions are evaluated only when
needed, and both operators return one of their operands rather than necessarily returning a boolean:

```js
const displayName = user && user.name;
const configuredPort = environmentPort || 3000;

isReady && startServer(); // Calls startServer() only when isReady is truthy.
```

Double negation converts a value to a boolean:

```js
alert(!!"non-empty string"); // true
```

The nullish coalescing operator `??` returns `a` unless it is `null` or `undefined`; otherwise it
returns `b`. Unlike `||`, it preserves other falsy values such as `0`, `false`, and `""`:

```js
const result = a ?? b;
```

`Object.assign` copies enumerable own properties from one or more source objects into a target
object. It performs a shallow copy and mutates the target:

```js
const defaults = { color: "blue", options: { dense: false } };
const settings = { color: "green" };

Object.assign(settings, defaults);

settings; // { color: "blue", options: { dense: false } }
settings.options === defaults.options; // true: nested objects are shared.
```

To create a new shallowly merged object, use an empty object as the target or use object spread:

```js
const mergedSettings = Object.assign({}, defaults, settings);
const equivalentSettings = { ...defaults, ...settings };
```

#### Object entries

`Object.entries` returns an array of an object's enumerable own string-keyed `[key, value]` pairs.
It is useful for iterating over an object's keys and values:

```js
const profile = { name: "Peter", age: 30 };

Object.entries(profile); // [["name", "Peter"], ["age", 30]]

for (const [key, value] of Object.entries(profile)) {
  console.log(key, value);
}
```

`Object.fromEntries` performs the reverse conversion: it creates an object from an iterable of
`[key, value]` pairs. It is useful for transforming entries before rebuilding an object:

```js
const upperCaseProfile = Object.fromEntries(
  Object.entries(profile).map(([key, value]) => [key, String(value).toUpperCase()]),
);

upperCaseProfile; // { name: "PETER", age: "30" }
```

#### Property flags and descriptors

Every object property has flags that control how it behaves:

- `writable`: whether the property's value can be changed.
- `enumerable`: whether the property appears in operations such as `Object.keys` and
  `Object.entries`.
- `configurable`: whether the property can be deleted or have its descriptor changed.

Properties created with object literals or ordinary assignment have all three flags set to `true`.
Use `Object.defineProperty` to define a property with explicit flags. Any omitted flags default to
`false`:

```js
const user = {};

Object.defineProperty(user, "id", {
  value: 42,
  writable: false,
  enumerable: true,
  configurable: false,
});

user.id = 7; // Ignored in non-strict mode; throws in strict mode.
Object.keys(user); // ["id"]
Object.getOwnPropertyDescriptor(user, "id");
// { value: 42, writable: false, enumerable: true, configurable: false }
```

`Object.getOwnPropertyDescriptor` reads one property's descriptor. The plural
`Object.getOwnPropertyDescriptors` reads all own property descriptors, which is useful when copying
properties while preserving their flags:

```js
const copy = Object.defineProperties({}, Object.getOwnPropertyDescriptors(user));
```

#### Property getters and setters

An accessor property uses a getter to calculate a value when it is read and a setter to validate or
transform a value when it is assigned. Accessors do not store a value automatically, so they often
use a separate backing property:

```js
const account = {
  _balance: 0,

  get balance() {
    return this._balance;
  },

  set balance(value) {
    if (value < 0) {
      throw new Error("Balance cannot be negative");
    }

    this._balance = value;
  },
};

account.balance = 100;
account.balance; // 100
```

The getter is called with no arguments when the property is read. The setter receives the assigned
value. A getter-only property cannot be assigned normally, while a setter-only property can accept a
value but does not provide one when read.

#### Internal slots and methods

The JavaScript specification describes some object state and behavior with internal slots and
internal methods. They are conceptual implementation details, not ordinary properties that can be
read with dot notation:

- **Internal slots** are hidden pieces of state, written with double brackets such as
  `[[Prototype]]` or `[[MapData]]`.
- **Internal methods** are hidden operations, such as `[[Get]]`, `[[Set]]`, and `[[Call]]`, that the
  language uses when evaluating expressions.

For example, `map.get(key)` is a normal method call, while the Map's internal data storage and the
steps used to read a property are specified with internal slots and methods. The notation explains
language behavior; it does not mean that `object.[[Prototype]]` is valid JavaScript.

Built-in objects can depend on their internal slots. A Proxy forwards property access but does not
automatically provide the internal slots expected by a built-in method:

```js
const map = new Map();
const proxy = new Proxy(map, {});

proxy.set("test", 1); // TypeError: Map.prototype.set needs Map internal slots
```

Bind built-in methods back to the original target when a Proxy needs to expose them:

```js
const map = new Map();

const proxy = new Proxy(map, {
  get(target, property, receiver) {
    const value = Reflect.get(target, property, receiver);
    return typeof value === "function" ? value.bind(target) : value;
  },
});

proxy.set("test", 1);
proxy.get("test"); // 1
```

#### Proxy and Reflect

`Proxy` wraps an object and lets a handler intercept operations such as property reads, writes, and
function calls. `Reflect` provides the corresponding default operation, so handlers can customize
one part of an operation while preserving normal JavaScript behavior. For example, a handler's `get`
method intercepts the object's internal `[[Get]]` operation:

```js
const user = {
  _name: "Guest",
  get name() {
    return this._name;
  },
};

const userProxy = new Proxy(user, {
  get(target, property, receiver) {
    return Reflect.get(target, property, receiver);
  },
});

const admin = {
  __proto__: userProxy,
  _name: "Admin",
};

admin.name; // "Admin": receiver is admin, so the getter uses admin._name
```

The `receiver` argument preserves the original object used for the property access, including when
the property is an inherited getter. A Proxy cannot intercept strict equality: `proxy === target`
still compares the two object identities directly.

#### Binary data

`ArrayBuffer` is a raw fixed-length block of bytes. `TypedArray` is the family of typed views, such
as `Uint8Array`, that provide numeric access to the buffer. `DataView` provides explicit control
over numeric types and byte order:

```js
const buffer = new ArrayBuffer(4); // 4 bytes
const view = new Uint8Array(buffer);

view[0] = 255;
view[0]; // 255

const data = new DataView(buffer);
data.setUint16(1, 258, true); // Store a little-endian 16-bit integer.
data.getUint16(1, true); // 258
```

#### WeakRef and FinalizationRegistry

`WeakRef` holds a non-owning reference to an object. Calling `.deref()` returns the object while it
is still alive, or `undefined` after garbage collection. A `FinalizationRegistry` can notify code
after an object is reclaimed, but garbage collection timing is nondeterministic, so it must not be
used for required cleanup or program correctness:

```js
const registry = new FinalizationRegistry((resourceId) => {
  console.log(`Resource ${resourceId} was collected`);
});

let resource = { name: "cache entry" };
const weakResource = new WeakRef(resource);
registry.register(resource, "cache-entry");

weakResource.deref()?.name; // "cache entry" while resource is alive
resource = null;
```

`structuredClone` creates a deep clone of supported structured data, so nested objects are not
shared with the original. It does not clone functions:

```js
const original = {
  name: "Peter",
  preferences: { theme: "dark" },
};

const copy = structuredClone(original);
copy.preferences.theme = "light";

original.preferences.theme; // "dark"
copy.preferences === original.preferences; // false
```

#### Destructuring

Destructuring assigns values from arrays or properties from objects to variables. Array
destructuring follows position, while object destructuring follows property names:

```js
const colors = ["red", "green", "blue"];
const [firstColor, , thirdColor] = colors;

const user = { name: "Peter", age: 30 };
const { name, age } = user;
```

You can rename object properties, provide defaults, and collect remaining values:

```js
const { name: displayName, role = "user" } = user;
const [first, ...remaining] = colors;

displayName; // "Peter"
role; // "user"
remaining; // ["green", "blue"]
```

#### Rest parameters and spread syntax

Rest parameters collect remaining function arguments into an array. Spread syntax expands an
iterable into individual values, or copies enumerable properties into a new array or object:

```js
function sum(...numbers) {
  return numbers.reduce((total, number) => total + number, 0);
}

sum(1, 2, 3); // 6

let arr = [3, 5, 1];

alert(Math.max(...arr)); // 5
const copy = [...arr];
const settings = { ...user, active: true };
```

Rest must be the last parameter. Spread creates shallow copies; nested objects and arrays remain
shared:

```js
const original = { preferences: { theme: "dark" } };
const copyOfOriginal = { ...original };

copyOfOriginal.preferences === original.preferences; // true
```

#### JSON

`JSON.stringify` converts a JavaScript value to a JSON string. `JSON.parse` converts a valid JSON
string back into a JavaScript value:

```js
const data = { name: "Peter", scores: [10, 20] };
const json = JSON.stringify(data); // '{"name":"Peter","scores":[10,20]}'
const parsed = JSON.parse(json);

parsed.scores[0]; // 10
```

JSON supports objects, arrays, strings, numbers, booleans, and `null`. `undefined`, functions, and
symbols are omitted from objects and become `null` in arrays. JSON serialization also does not
preserve object prototypes or methods.

#### Promises

A `Promise` represents the eventual result of an asynchronous operation. The first function passed
to `.then` handles fulfillment, and the second handles rejection:

```js
const promise = new Promise((resolve, reject) => {
  setTimeout(() => resolve("done!"), 1000);
});

promise.then(
  (result) => alert(result), // Shows "done!" after 1 second.
  (error) => alert(error), // Does not run when the promise fulfills.
);
```

Use `.catch` to handle a rejection from the Promise or from an earlier `.then` callback. `.finally`
runs after either fulfillment or rejection, so it is useful for cleanup:

```js
promise
  .then((result) => console.log(result))
  .catch((error) => console.error(error))
  .finally(() => console.log("Finished"));
```

#### Promise chaining

The `.then` method always returns a new Promise, which makes it possible to chain asynchronous
operations. Return a regular value when the next step can continue immediately. Return another
Promise when the next step starts an asynchronous operation and the chain must wait for it:

```js
fetchUser()
  .then((user) => user.name) // Return a value; the next .then receives the name.
  .then((name) => fetchPermissions(name)) // Return a Promise; the chain waits for it.
  .then((permissions) => console.log(permissions));
```

Returning a Promise is also useful when a callback needs to transform the result after another
asynchronous operation:

```js
fetchUser()
  .then((user) => {
    return fetchPermissions(user.name);
  })
  .then((permissions) => permissions.filter((permission) => permission.active));
```

An error in a Promise chain, whether it comes from a rejection or an uncaught error thrown in a
handler, skips subsequent fulfillment handlers and travels to the first rejection handler or
`.catch`:

```js
fetchUser()
  .then(() => {
    throw new Error("Unexpected failure");
  })
  .then(() => console.log("This does not run"))
  .catch((error) => console.error(error)); // Handles thrown errors and rejections.
```

#### Promise combinators

Promise combinators accept an iterable, such as an array. The iterable may contain ordinary values
as well as Promises; ordinary values are treated as already fulfilled Promises.

##### [Promise.all](https://javascript.info/promise-api#promise-all)

`Promise.all` fulfills when every item fulfills and returns their results in the original order. It
rejects as soon as one item rejects:

```js
const results = await Promise.all([fetchUser(), 42, "ready"]);
// results contains the user, 42, and "ready" in that order.
```

##### [Promise.allSettled](https://javascript.info/promise-api#promise-allsettled)

`Promise.allSettled` waits for every item and reports whether each one fulfilled or rejected. The
returned result is fulfilled even when some items fail:

```js
const results = await Promise.allSettled([fetchUser(), Promise.reject(new Error("Failed"))]);

for (const result of results) {
  if (result.status === "fulfilled") {
    console.log(result.value);
  } else {
    console.error(result.reason);
  }
}
```

##### [Promise.race](https://javascript.info/promise-api#promise-race)

`Promise.race` settles as soon as the first item settles. It fulfills or rejects with that item's
result:

```js
const firstResponse = await Promise.race([fetchFromPrimary(), fetchFromBackup()]);
```

##### [Promise.any](https://javascript.info/promise-api#promise-any)

`Promise.any` fulfills as soon as the first item fulfills. It rejects with an `AggregateError` only
when every item rejects:

```js
const firstSuccessfulResponse = await Promise.any([fetchFromPrimary(), fetchFromBackup()]);
```

#### Async/await

An `async` function always returns a Promise. Returning a value fulfills that Promise, while
throwing an error rejects it. `await` pauses the async function until a Promise settles, without
blocking the rest of the JavaScript runtime:

```js
async function loadUserName() {
  const user = await fetchUser();
  return user.name;
}

loadUserName().then((name) => console.log(name));
```

Use `try...catch` around `await` when the async function should handle a rejection:

```js
async function loadUser() {
  try {
    return await fetchUser();
  } catch (error) {
    console.error(error);
    return null;
  }
}
```

#### Generator functions

A generator function is declared with `function*`. Calling it returns a generator object, and each
call to `.next()` resumes execution until the next `yield` or `return`:

```js
function* generateSequence() {
  yield 1;
  yield 2;
  return 3;
}

const generator = generateSequence();

const one = generator.next();

alert(JSON.stringify(one)); // {"value":1,"done":false}
```

The argument passed to `.next(value)` becomes the result of the suspended `yield` expression. The
first `.next()` starts the generator, so its argument is ignored; later calls can send values back
into the generator:

```js
function* askForValue() {
  const value = yield "What is your name?";
  return `Hello, ${value}!`;
}

const generator = askForValue();
generator.next(); // { value: "What is your name?", done: false }
generator.next("Peter"); // { value: "Hello, Peter!", done: true }
```

#### Async iteration and generators

An async generator uses `async function*` and can yield values that arrive asynchronously. Use
`for await...of` to consume its async iterable one value at a time:

```js
async function* fetchPages(urls) {
  for (const url of urls) {
    yield await fetch(url);
  }
}

async function logPages(urls) {
  for await (const response of fetchPages(urls)) {
    console.log(response.status);
  }
}
```

#### Errors and try-catch

`try...catch` handles errors thrown synchronously while the `try` block is executing. The caught
error is usually an `Error` object with properties such as `name`, `message`, and `stack`:

```js
const json = '{ "age": 30 }';

try {
  const user = JSON.parse(json);

  if (!user.name) {
    throw new SyntaxError("Incomplete data: no name");
  }

  console.log(user.name);
} catch (error) {
  console.log(`${error.name}: ${error.message}`);
}
```

Use `throw` to signal that an operation cannot continue. JavaScript permits throwing any value, but
throwing an `Error` or a subclass gives callers a consistent error object and stack trace:

```js
function parsePort(value) {
  const port = Number(value);

  if (!Number.isInteger(port) || port < 1 || port > 65535) {
    throw new RangeError("Port must be an integer from 1 to 65535");
  }

  return port;
}

class ValidationError extends Error {
  constructor(message) {
    super(message);
    this.name = "ValidationError";
  }
}
```

When a `catch` block can handle only some error types, handle the known type and rethrow every other
error. The outer caller can then handle errors that this layer does not understand:

```js
try {
  const user = JSON.parse(json);

  if (!user.name) {
    throw new ValidationError("Incomplete data: no name");
  }
} catch (error) {
  if (error instanceof ValidationError) {
    console.log(`Validation failed: ${error.message}`);
  } else {
    throw error; // Preserve unexpected errors and their original stack.
  }
}
```

The `finally` block runs after `try` and `catch`, whether an error was thrown or not. It also runs
when the `try` or `catch` block returns, so it is useful for cleanup such as releasing a resource:

```js
function readValue() {
  try {
    return "value";
  } finally {
    console.log("Cleanup runs before the function returns");
  }
}

readValue(); // Logs cleanup, then returns "value".
```

An error thrown inside a later timer callback is not caught by a surrounding `try...catch`, because
the callback runs after the `try` statement has already finished. Catch the error inside the
callback, or use a Promise and `await` it inside `try...catch`:

```js
try {
  setTimeout(() => {
    throw new Error("This is not caught here");
  }, 1000);
} catch (error) {
  console.log("This handler does not catch the timer error");
}
```

#### Symbols

A `Symbol` is a unique primitive value. It is useful as an object property key when a library needs
to add metadata without colliding with ordinary string-named properties:

```js
const internalId = Symbol("internalId");
const record = {
  name: "Peter",
  [internalId]: 42,
};

const sameName = Symbol("internalId");
record[internalId]; // 42
record[sameName]; // undefined: symbols are unique.
```

Symbol-keyed properties are not included by ordinary string-key enumeration such as
`Object.keys(record)`. They can still be inspected deliberately with
`Object.getOwnPropertySymbols(record)` or `Reflect.ownKeys(record)`.

#### Custom primitive conversion

`Symbol.toPrimitive` is a well-known symbol that lets an object define how it should be converted to
a primitive value. JavaScript calls the method when an operation needs a primitive, such as string
interpolation, explicit conversion with `String` or `Number`, or arithmetic. The method receives a
`hint` describing the requested conversion:

- `"string"` means a string is preferred, for example with `String(value)` or `` `${value}` ``.
- `"number"` means a number is preferred, for example with `Number(value)` or unary `+`.
- `"default"` leaves the operation's preferred type unspecified. The `+` operator commonly uses this
  hint.

The method must return a primitive value, such as a string, number, bigint, boolean, `undefined`, or
`null`. Returning another object causes a `TypeError`. If an object does not define
`Symbol.toPrimitive`, JavaScript falls back to its ordinary `valueOf()` and `toString()` methods.

```js
const user = {
  name: "John",
  money: 1000,

  [Symbol.toPrimitive](hint) {
    console.log(`hint: ${hint}`);
    return hint === "string" ? `{name: "${this.name}"}` : this.money;
  },
};

String(user); // "{name: \"John\"}"; hint is "string"
Number(user); // 1000; hint is "number"
user + 500; // 1500; hint is "default"
`${user}`; // "{name: \"John\"}"; hint is "string"
```

The method is not a general serialization hook: it only controls implicit or explicit primitive
conversion. For JSON output, define the object's JSON representation separately, for example with
`toJSON()`.

#### Function behavior

##### Function parameters

Missing arguments have the value `undefined`, and extra arguments are ignored unless a rest
parameter collects them. Default parameters are used when the corresponding argument is `undefined`,
including when the argument is omitted; `null` does not trigger the default:

```js
function describe(name, age = 18) {
  return `${name} is ${age}`;
}

describe("Peter"); // "Peter is 18"
describe("Peter", undefined); // "Peter is 18"
describe("Peter", null); // "Peter is null"
describe(); // "undefined is 18"
```

Use separate parameters when the function has a small, fixed, ordered set of required values. Use
one object parameter when arguments are optional, numerous, or easier to identify by name. An object
parameter is also easier to extend without changing the meaning of each argument position:

```js
function createUser(name, age, role) {
  return { name, age, role };
}

function createUserFromOptions({ name, age = 18, role = "user" } = {}) {
  return { name, age, role };
}

createUser("Peter", 30, "admin");
createUserFromOptions({ name: "Peter", role: "admin" });
```

Destructured parameters can have defaults for individual properties and for the whole object. The
`= {}` default prevents an error when the caller omits the object entirely.

##### Function custom properties

Functions are objects, so they can have custom properties. This can be useful for small amounts of
state associated with one function:

```js
function sayHi() {
  alert("Hi");

  // Count how many times the function runs.
  sayHi.counter++;
}

sayHi.counter = 0; // Initial value.

sayHi(); // Hi
sayHi.counter; // 1
```

This state belongs to the function object itself. It is not private; callers can read or change the
property directly.

##### Function call context

`func.call(context, arg1, arg2, ...)` calls `func` immediately with `context` as its `this` value
and passes the remaining arguments individually:

```js
function introduce(greeting, punctuation) {
  return `${greeting}, ${this.name}${punctuation}`;
}

introduce.call({ name: "Peter" }, "Hello", "!"); // "Hello, Peter!"
```

For strict-mode functions, passing `null` or `undefined` leaves `this` as that value. For ordinary
non-strict functions, JavaScript may substitute the global object.

`func.apply(context, [arg1, arg2, ...])` is similar to `call`, but receives the arguments as a
single array or array-like value. Use `call` when the arguments are already individual values and
`apply` when they are collected in an array:

```js
function add(first, second) {
  return this.base + first + second;
}

add.apply({ base: 10 }, [2, 3]); // 15
```

##### Function arguments

Traditional non-arrow functions expose their received arguments through the array-like `arguments`
object. It has numeric indexes and a `length` property, but it is not a real array, so modern code
generally prefers named parameters and rest parameters:

```js
function showArguments(first) {
  arguments[0]; // The same value as first.
  arguments.length; // Number of arguments passed by the caller.
}

showArguments("a", "b");
```

Arrow functions do not have their own `arguments`; an arrow function uses `arguments` from an outer
non-arrow function, if one exists. Use a rest parameter to collect arguments in an arrow function:

```js
const sum = (...numbers) => numbers.reduce((total, number) => total + number, 0);
```

##### [Borrowing a method](https://javascript.info/call-apply-decorators#method-borrowing)

Method borrowing calls a method from one object with another object as its `this` value. This is
useful when a method is generic and only depends on `this` having the required shape:

```js
const arrayLike = { 0: "a", 1: "b", length: 2 };

const values = Array.prototype.join.call(arrayLike, "-"); // "a-b"
```

The method is not copied onto `arrayLike`; `.call` temporarily supplies the receiver for that
invocation.

##### [Function binding and partial functions](https://javascript.info/bind#partial-functions)

`bind` creates a new function instead of calling the original immediately. It permanently supplies
`this` and can also pre-fill leading arguments:

```js
const user = { name: "Peter" };

function greet(greeting, punctuation) {
  return `${greeting}, ${this.name}${punctuation}`;
}

const greetPeter = greet.bind(user);
greetPeter("Hello", "!"); // "Hello, Peter!"
```

Pre-filling some arguments creates a partial function. The remaining arguments are supplied when the
bound function is called:

```js
function multiply(first, second) {
  return first * second;
}

const double = multiply.bind(null, 2);
double(5); // 10
```

##### Currying

Currying transforms a function with multiple arguments into a sequence of functions that each take
one argument. It is useful when a partially configured function will be reused:

```js
function multiply(first) {
  return (second) => first * second;
}

const double = multiply(2);
double(5); // 10
```

##### Reference type

When JavaScript evaluates `object.method()`, it keeps both the function and the object that supplied
it as a temporary reference. That reference lets the call set `this` to `object`. Extracting the
method first removes that call context:

```js
const user = {
  name: "Peter",
  sayHi() {
    return this.name;
  },
};

user.sayHi(); // "Peter"
const sayHi = user.sayHi;
sayHi(); // undefined in strict mode: the object reference was lost
```

##### How `this` is determined

The value of `this` is not determined by where a regular function is written. It is determined by
how the function is called:

- **Regular functions:** `this` depends on **how you call the function**.
- **Arrow functions:** `this` depends on **where you define the function**.

For a regular function, check the call site in this order:

1. **`new`:** `this` is the newly created object.
2. **Explicit binding:** `call`, `apply`, or `bind` supplies `this`.
3. **Implicit binding:** when called as `object.method()`, `this` is `object`.
4. **Default binding:** in non-strict code, `this` is the global object. In strict code and ES
   modules, `this` is `undefined`.

For example, an arrow function used as an object property does not become a method of that object.
It captures `this` from the scope where the arrow is created:

```js
const user = {
  name: "Rafael",
  greet: () => {
    console.log(`Hi, I'm ${this?.name}`);
  },
};

user.greet(); // "Hi, I'm undefined": the arrow did not capture user as its this
```

The optional chaining in this example keeps the demonstration safe when the surrounding `this` is
`undefined`. Without it, a top-level arrow in an ES module would throw when it evaluates
`this.name`.

By contrast, a regular function gets an implicit `this` only while it is called through the object.
Extracting the function removes that call-site object:

```js
const user = {
  name: "Rafael",
  greet: function () {
    console.log(`Hi, I'm ${this?.name}`);
  },
};

const greetFn = user.greet; // Extract the method.
greetFn(); // "Hi, I'm undefined": there is no object before the call
user.greet(); // "Hi, I'm Rafael": the call has implicit binding
```

Use `bind` when a regular function must keep a particular receiver after it is extracted:

```js
const greetRafael = user.greet.bind(user);
greetRafael(); // "Hi, I'm Rafael"
```

##### Arrow functions and `this`

Arrow functions do not have their own `this`. When an arrow function refers to `this`, it uses the
`this` value from the surrounding scope. A normal method call provides the surrounding `this` to an
arrow created inside that method:

```js
const user = {
  firstName: "Ilya",
  sayHi() {
    const arrow = () => console.log(this.firstName);
    arrow();
  },
};

user.sayHi(); // Ilya
```

This is different from a regular nested function. A regular callback does not automatically inherit
the method's `this`; it gets its own `this` according to the way the callback is called. Array
methods call callbacks as ordinary functions, so a regular callback in strict code has
`this === undefined`:

```js
class DataProcessor {
  constructor() {
    this.data = [1, 2, 3, 4, 5];
    this.multiplier = 2;
  }

  processWrong() {
    return this.data.map(function (number) {
      return number * this.multiplier; // TypeError: this is undefined
    });
  }

  processRight() {
    return this.data.map((number) => number * this.multiplier);
    // The arrow captures the DataProcessor instance.
  }
}

new DataProcessor().processRight(); // [2, 4, 6, 8, 10]
```

An arrow in an object literal does not capture the object. An object literal creates an object, but
it does not create a new lexical scope for `this`. The arrow therefore captures `this` from the
surrounding scope, which is the global object in a classic script or `undefined` in an ES module:

```js
const counter = {
  count: 0,
  increment: () => {
    // This is not the counter object.
    if (this === counter) {
      this.count++;
    }
  },
};

counter.increment();
console.log(counter.count); // 0: this was not counter
```

With the direct `this.count++` version, a classic script would change a global `count` property,
while an ES module would throw because its top-level `this` is `undefined`. Neither version changes
`counter.count`.

Use an ordinary method when the object should provide `this` at the call site:

```js
const counter = {
  count: 0,
  increment() {
    this.count++;
  },
};

counter.increment();
counter.count; // 1
```

Arrow functions are useful for callbacks and event handlers that need to preserve an instance. A
class field arrow is initialized separately for each instance, and its initializer captures that
instance as `this`:

```js
class Component {
  constructor() {
    this.value = 42;
  }

  handleClick = () => {
    console.log(this.value); // 42
  };

  setupListener(button) {
    button.addEventListener("click", this.handleClick);
  }
}
```

Class field syntax requires modern JavaScript support or transpilation with Babel or TypeScript.
This is not the same as automatically binding a class method: a regular class method still loses its
receiver when it is passed as a callback unless it is bound explicitly.

An arrow's `this` cannot be changed with `call`, `apply`, or `bind`; those methods can still pass
arguments, but the arrow keeps the `this` value from its surrounding scope:

```js
function createArrow() {
  return () => this.name;
}

const context = { name: "Peter" };
const arrow = createArrow.call(context);

arrow.call({ name: "Mary" }); // "Peter": still uses context, not the new object.
```

##### Constructor functions

Before classes, constructor functions were commonly used to create objects. By convention, their
names start with a capital letter and they are called with `new`:

```js
function User(firstName) {
  this.firstName = firstName;
}

User.prototype.greet = function () {
  return `Hello, ${this.firstName}!`;
};

const user = new User("Peter");
user.greet(); // Hello, Peter!
user instanceof User; // true
```

`new` creates an object, binds it as `this` while the function runs, and connects it to the
constructor's prototype. Capitalization is a convention, not a language requirement, but it signals
that a function is intended to be used with `new`. Classes provide the modern syntax for the same
general pattern and must also be called with `new`.

##### Prototypal inheritance

JavaScript objects can inherit properties and methods from another object. An object's
`[[Prototype]]` is the object JavaScript consults when a property is not found directly on the
object. This lookup continues up the prototype chain until the property is found or the chain ends
at `null`.

See [Prototypal inheritance](https://javascript.info/prototype-inheritance#prototype) for a deeper
walkthrough.

Use `Object.getPrototypeOf` to inspect an object's prototype and `Object.setPrototypeOf` to change
it. Changing prototypes after object creation can hurt performance, so prefer creating objects with
the intended prototype when possible:

```js
const animal = {
  eats: true,
};

const rabbit = Object.create(animal);
Object.getPrototypeOf(rabbit) === animal; // true
rabbit.eats; // true: found through [[Prototype]]

const anotherRabbit = {};
Object.setPrototypeOf(anotherRabbit, animal);
anotherRabbit.eats; // true
```

`__proto__` is a legacy accessor for an object's prototype. It is still encountered in existing
code, but prefer `Object.getPrototypeOf`, `Object.setPrototypeOf`, or `Object.create` for new code:

```js
rabbit.__proto__ === animal; // true, but legacy style
```

The `prototype` property is different from an object's `[[Prototype]]`. A constructor function's
`prototype` object becomes the `[[Prototype]]` of instances created with `new`:

```js
function User(name) {
  this.name = name;
}

User.prototype.sayHi = function () {
  return `Hi, ${this.name}`;
};

const user = new User("Peter");
user.sayHi(); // "Hi, Peter": inherited from User.prototype
Object.getPrototypeOf(user) === User.prototype; // true
```

The method is shared through the prototype instead of being created separately for every instance.
An instance can still have its own property with the same name, which shadows the inherited one.

##### Class syntax

`class` provides clearer syntax for constructor functions and prototype methods. Methods declared in
the class body are placed on the class's `prototype`, so instances share them. A class constructor
must be called with `new`:

```js
class User {
  constructor(name) {
    this.name = name;
  }

  sayHi() {
    return `Hi, ${this.name}`;
  }
}

const user = new User("Peter");
user.sayHi(); // "Hi, Peter"
Object.getPrototypeOf(user) === User.prototype; // true
```

Use `extends` to inherit from another class and `super` to call the parent constructor or method:

```js
class Admin extends User {
  sayHi() {
    return `${super.sayHi()} (admin)`;
  }
}

new Admin("Peter").sayHi(); // "Hi, Peter (admin)"
```

The `static` keyword creates a property or method on the class itself, not on its instances. Access
static members through the class name:

```js
class MathHelper {
  static version = "1.0";

  static double(value) {
    return value * 2;
  }
}

MathHelper.version; // "1.0"
MathHelper.double(4); // 8
```

`super` refers to the parent class. In a derived constructor, call `super(...)` before using `this`.
In an overridden method, `super.method()` calls the parent implementation:

```js
class Admin extends User {
  constructor(name) {
    super(name);
    this.role = "admin";
  }

  sayHi() {
    return `${super.sayHi()} (admin)`;
  }
}
```

JavaScript classes can also have private fields and methods prefixed with `#`. They are enforced at
runtime and can only be accessed from the class body:

```js
class Account {
  #balance = 0;

  deposit(amount) {
    this.#balance += amount;
  }

  getBalance() {
    return this.#balance;
  }
}

const account = new Account();
account.deposit(100);
account.getBalance(); // 100
// account.#balance; // SyntaxError: private outside the class
```

The `#` syntax is different from a leading underscore. An underscore is only a naming convention;
`object._value` remains publicly accessible.

##### Mixins

A mixin is an object containing reusable methods that are copied onto a class prototype. It provides
composition without requiring a class to inherit from multiple parents:

```js
const sayHiMixin = {
  sayHi() {
    alert(`Hello ${this.name}`);
  },
  sayBye() {
    alert(`Bye ${this.name}`);
  },
};

class User {
  constructor(name) {
    this.name = name;
  }
}

Object.assign(User.prototype, sayHiMixin);

new User("Dude").sayHi(); // Hello Dude!
```

Mixin methods are shared through the prototype and usually expect the consuming class to provide the
properties they use, such as `name` here.

##### Instanceof

`value instanceof Constructor` checks whether `Constructor.prototype` appears in the value's
prototype chain. It is useful for checking instances created by a class or constructor function:

```js
const user = new User("Peter");

user instanceof User; // true
user instanceof Object; // true
({}) instanceof User; // false
```

`instanceof` checks the prototype chain, not just the constructor name or object shape. Objects from
another realm, such as a different browser window, can therefore require different checks.

A default parameter is evaluated every time the function is called without that parameter. This is
useful for creating a fresh value per call:

```js
function createTimestamp(date = new Date()) {
  return date;
}

createTimestamp(); // Evaluates new Date() for this call.
```

A function with an empty `return` or with no `return` statement returns `undefined`:

```js
function stop() {
  return;
}

function doNothing() {}

stop(); // undefined
doNothing(); // undefined
```

#### Modules

Prefer `import` and `export` for modern JavaScript and TypeScript modules. They are the ES module
standard, support static analysis and tree-shaking, and work in browsers and modern Node.js when the
project is configured for ES modules:

```js
import { readFile } from "node:fs/promises";

export async function readText(path) {
  return readFile(path, "utf8");
}
```

##### Imports and exports

Named exports can be imported by name. Modules are evaluated once, and importing the same named
export from two files gives both files access to the same exported binding and object:

```js
// admin.js
export const admin = { name: "Guest" };

// 1.js
import { admin } from "./admin.js";
admin.name = "Pete";

// 2.js
import { admin } from "./admin.js";
console.log(admin.name); // "Pete"
// Both files reference the same admin object.
```

An imported binding is read-only in the importing module, but mutations to an exported object are
visible through every import of that object. Use `export` on declarations or export a list at the
end of a module:

```js
const firstName = "Peter";
function greet() {
  return `Hello, ${firstName}`;
}

export { firstName, greet };
```

##### Default exports

A module can have one default export. The importer chooses the local name and does not use braces:

```js
// greet.js
export default function greet(name) {
  return `Hello, ${name}`;
}

// app.js
import greet from "./greet.js";
greet("Peter");
```

Named and default exports can be used together when a module has one primary value and additional
supporting exports:

```js
export default class User {}
export const role = "admin";
```

##### Re-exports

An `index.js` file can re-export a module's public API, allowing callers to import from the folder's
entry point instead of knowing its internal file layout:

```js
// index.js
export { admin } from "./admin.js";
export { default as greet } from "./greet.js";

// app.js
import { admin, greet } from "./index.js";
```

`require` and `module.exports` are the older CommonJS module system. Use them when maintaining a
CommonJS project or when a dependency and its runtime configuration require CommonJS:

```js
const { readFile } = require("node:fs/promises");

module.exports = { readText };
```

Do not mix the two systems casually. Follow the module format configured by the project, such as the
`type` field in `package.json` or the TypeScript module configuration.

```js
// Ternary operator

const isAdult = age < 18 ? "too young" : "old enough";

const exports = {
  publicPath: process.env.NODE_ENV === "production" ? "/production-sub-path/" : "/",
};

// Optional Chaining

// When foo is defined, foo.bar.baz() will be computed; but when foo is null or undefined, stop what we’re doing and just return undefined
const x = foo?.bar.baz();

// If an object or property is not available, use a default value.

transfersDataSource?.numberOfItems ?? 0;
```

### TypeScript

#### Imports

Modules can export values from one file and import them from another. An `index.ts` file is often
used as the public entry point for a folder.

Example structure:

```text
src/
  greeting/
    greeting.ts
    index.ts
  Home.tsx
```

`greeting.ts` contains the implementation:

```ts
export interface Greeting {
  message: string;
}

export function createGreeting(name: string): Greeting {
  return { message: `Hello, ${name}!` };
}
```

`index.ts` re-exports the public parts of the folder:

```ts
export { createGreeting, type Greeting } from "./greeting";
```

Another file can import from the folder. TypeScript resolves `./greeting` to `./greeting/index.ts`:

```ts
import { createGreeting, type Greeting } from "./greeting";

const greeting: Greeting = createGreeting("Peter");
console.log(greeting.message);
```

The `type` keyword marks an import that is used only by the type checker. It is removed from the
JavaScript output. You can also import directly from the implementation file with
`./greeting/greeting`, but importing from the folder keeps the internal file structure private and
makes the index file the module's public API.

#### Interfaces

An interface describes the shape that an object or class must have. It does not create an object at
runtime.

```ts
interface StudentRecord {
  readonly studentId: number;
  firstName: string;
  lastName?: string; // Optional property
  introduce(): string; // Method signature
}

interface GraduateStudent extends StudentRecord {
  thesisTitle: string;
}

const record: StudentRecord = {
  studentId: 1,
  firstName: "Peter",
  introduce() {
    return `My name is ${this.firstName}.`;
  },
};
```

`readonly` prevents assignment through the TypeScript type after initialization. It does not make a
deeply nested object immutable at runtime.

#### Classes

```ts
class Student implements StudentRecord {
  public static readonly schoolType = "secondary";

  private _firstName: string;
  protected schoolName: string;
  public readonly studentId: number;
  public lastName?: string;

  constructor(studentId: number, firstName: string, schoolName: string, lastName?: string) {
    this.studentId = studentId;
    this._firstName = firstName;
    this.schoolName = schoolName;
    this.lastName = lastName;
  }

  // Getter: read a computed value like a normal property.
  public get firstName(): string {
    return this._firstName;
  }

  // Setter: validate or transform a value before storing it.
  public set firstName(value: string) {
    const trimmedValue = value.trim();

    if (trimmedValue.length === 0) {
      throw new Error("First name cannot be empty.");
    }

    this._firstName = trimmedValue;
  }

  public get fullName(): string {
    return [this.firstName, this.lastName].filter(Boolean).join(" ");
  }

  public introduce(): string {
    return `My name is ${this.fullName}.`;
  }

  protected schoolLabel(): string {
    return `${this.schoolName} student`;
  }

  private normalizedName(): string {
    return this.fullName.toLowerCase();
  }
}

const student = new Student(1, "Peter", "Central School", "Smith");
student.firstName = " Peter "; // Uses the setter.
console.log(student.fullName); // Peter Smith
console.log(student.introduce());
console.log(Student.schoolType); // Access a static property on the class.

// Allowed: public property.
student.lastName = "Jones";

// Errors caught by TypeScript:
// student.studentId = 2;       // readonly
// student.schoolName;          // protected
// student.normalizedName();    // private
```

The access modifiers control TypeScript access:

- `public`: accessible from anywhere; it is the default for class members.
- `private`: accessible only inside the declaring class.
- `protected`: accessible inside the class and its subclasses.
- `readonly`: can be assigned during initialization or in the constructor, but not reassigned later.
- `static`: belongs to the class itself, not to each instance.

JavaScript has no built-in `protected` modifier. A leading underscore, such as `_value`, is only a
convention that signals an internal property; it does not prevent outside code from accessing the
property. TypeScript's `protected` modifier is checked by the compiler and allows access inside the
declaring class and its subclasses:

```ts
class Parent {
  protected readonly id = 42;
  protected _label = "parent"; // The underscore is optional and only conventional.
}

class Child extends Parent {
  getId(): number {
    return this.id;
  }
}

const child = new Child();
child.getId(); // 42
// child.id; // TypeScript error: protected outside Parent and Child
child._label; // Allowed by JavaScript; _label is not private.
```

Access modifiers can be combined when their meanings are compatible. For example,
`protected readonly` allows subclasses to read a property while preventing reassignment through the
TypeScript type:

```ts
class Account {
  protected readonly id = 42;
}
```

`private` in TypeScript is also primarily a compile-time restriction. Use JavaScript `#private`
fields when runtime-enforced privacy is required.

The constructor can also create properties directly with parameter properties:

```ts
class Course {
  constructor(
    public readonly code: string,
    private teacher: string,
    protected room: string,
  ) {}
}
```

This is shorthand for declaring those properties and assigning the constructor parameters to
`this.code`, `this.teacher`, and `this.room`.

#### Functions

TypeScript checks parameter and return types for functions. Optional parameters use `?`, default
parameters have a fallback value, and rest parameters collect remaining arguments into an array.

```ts
function formatName(firstName: string, lastName?: string): string {
  return [firstName, lastName].filter(Boolean).join(" ");
}

function addTax(amount: number, taxRate = 0.2): number {
  return amount * (1 + taxRate);
}

function sum(...values: number[]): number {
  return values.reduce((total, value) => total + value, 0);
}

const multiply = (left: number, right: number): number => left * right;

// Generic functions preserve the type of the value they receive.
function first<T>(values: T[]): T | undefined {
  return values[0];
}

formatName("Peter", "Smith");
addTax(100);
sum(1, 2, 3);
multiply(2, 3);
first(["a", "b"]); // string | undefined
```

## Conditions

### Switch

When a `case` matches, JavaScript starts executing there and continues into the following cases
until it reaches a `break`, `return`, or the end of the `switch`. This is called **fall-through**:

```js
function describeNumber(number) {
  switch (number) {
    case 4:
      console.log("Exactly!");
    // No break: execution falls through to case 5.
    case 5:
      console.log("Am I there?");
    default:
      console.log("Unknown number");
  }
}

describeNumber(4); // Exactly!, Am I there?, Unknown number
```

Use `break` when the `switch` should end but the surrounding function should continue. Use `return`
when the matching case produces the function's result and the function should end immediately. A
`return` is especially important when code after the `switch` must not run:

```js
function labelNumber(number) {
  switch (number) {
    case 4:
      return "Exactly!";
    case 5:
      return "I'm not there";
    default:
      return "Unknown number";
  }
}

labelNumber(4); // "Exactly!"
```

### HTML interaction

```html
<!--Create new elements and populate it-->
<div id="demo">some content</div>

<script>
  // Create a new element and populate it.
  const paragraph = document.createElement("p");
  paragraph.textContent = "Some new text";

  // Add the paragraph to the container when it exists.
  const container = document.querySelector("#demo");
  container?.append(paragraph);

  // Change an image source when the image exists.
  const qrImage = document.querySelector("#qr");
  if (qrImage instanceof HTMLImageElement) {
    qrImage.src = "/images/new";
  }

  // Remove a class from all matching elements.
  function makeButtonVisible() {
    document.querySelectorAll(".nonactive").forEach((element) => {
      element.classList.remove("nonactive");
    });
  }
</script>
```

## Frameworks

### Angular

#### NgModules and standalone components

An `NgModule` is an Angular class whose `@NgModule` metadata groups declarations and configures
template dependencies and dependency injection. Its main fields are:

- `declarations`: non-standalone components, directives, and pipes owned by the module.
- `imports`: other modules and standalone components, directives, or pipes used by its declarations.
- `exports`: declarations that importing modules may use.
- `providers`: services made available through dependency injection.

```typescript
@Component({
  selector: "app-legacy-card",
  standalone: false,
  template: "<p>Legacy card</p>",
})
export class LegacyCard {}

@NgModule({
  declarations: [LegacyCard],
  imports: [CommonModule],
  exports: [LegacyCard],
})
export class LegacyCardModule {}
```

A standalone component is not declared in an `NgModule`. It lists its own template dependencies in
`imports`, and another standalone component can import it directly:

```typescript
@Component({
  selector: "app-profile-card",
  imports: [DatePipe],
  template: "<p>{{ createdAt | date }}</p>",
})
export class ProfileCard {
  createdAt = new Date();
}
```

Standalone is the default for components in Angular 19 and later, so `standalone: true` is usually
omitted. Angular recommends standalone components for new code; NgModules remain supported for
existing applications and libraries.

#### Change detection

Change detection updates the rendered view when component state changes. In this repository's
Angular 21, the strategies are `Default` and `OnPush`:

```typescript
@Component({
  selector: "app-status",
  changeDetection: ChangeDetectionStrategy.OnPush,
  template: "{{ status() }}",
})
export class StatusComponent {
  readonly status = signal("ready");
}
```

- `Default` is the default in Angular 21. Angular checks the component as part of the normal
  application change-detection pass.
- `OnPush` lets Angular skip checking the component subtree until it has a reason, such as a new
  input value, an event handled in the subtree, a changed signal read by the template, or an
  explicit mark for checking. An `AsyncPipe` also marks its component for checking when a new value
  arrives.

Angular 22 changed the default to `OnPush` and renamed `Default` to `Eager` in the current API.

#### Model inputs and two-way binding

Use `input()` when a component receives a value from its parent and does not update that value. Use
`model()` when the child needs to update the value and notify the parent. A model signal also
exposes a corresponding `checkedChange` output for a model named `checked`.

```typescript
@Component({
  selector: "app-toggle",
  template: `
    <button type="button" (click)="toggle()">
      {{ checked() ? "On" : "Off" }}
    </button>
  `,
})
export class ToggleComponent {
  readonly checked = model(false);

  toggle(): void {
    this.checked.update((current) => !current);
  }
}

@Component({
  imports: [ToggleComponent],
  template: '<app-toggle [(checked)]="enabled" />',
})
export class SettingsComponent {
  readonly enabled = signal(false);
}
```

The `[(checked)]` syntax passes the parent's value to the child and writes child changes back to the
parent. For a signal in the parent, bind the signal itself (`enabled`), not its current value
(`enabled()`).

#### Template syntax

Angular's built-in `@if`, `@for`, and `@switch` control flow is available since Angular 17 and
stable since v18. `NgIf` (used as `*ngIf`) and `NgFor` are deprecated since v20 in favor of these
blocks. `NgStyle` is still supported and is not currently marked deprecated; direct style bindings
are preferred for simpler bindings and avoid the directive:

```html
@if (user(); as currentUser) {
<p>Hello {{ currentUser.name }}</p>
} @else {
<p>Please sign in</p>
}

<div [style.color]="textColor" [style.width.px]="width"></div>
<div [style]="styleMap"></div>
```

`@if` and `[style]` are Angular template syntax, not portable syntax shared by frontend frameworks.
Other frameworks have their own control-flow and style-binding conventions; ordinary HTML `style`
attributes and CSS remain broadly shared.

#### Directives and bindings

A directive is an Angular class that adds behavior to an element or changes which elements appear in
the view. It is a way to compose behavior onto an element, not a way for the element to inherit from
an HTML tag or another class. A native attribute such as `disabled` is interpreted by the browser;
an Angular directive is instantiated by Angular when its selector matches a template element.

Angular has three directive categories:

- **Components** are directives with a template. They define a view and are commonly used as custom
  elements, such as `<app-profile-card>`.
- **Attribute directives** attach to an existing element and change its behavior, appearance, or
  properties without replacing its content. The `appHighlight` example is an attribute directive.
- **Structural directives** add, remove, or repeat template content. Older examples include `*ngIf`
  and `*ngFor`; modern Angular templates use built-in blocks such as `@if` and `@for` for this
  control flow.

In the directive decorator, `selector: '[appHighlight]'` is a CSS-style attribute selector: Angular
matches elements that have an `appHighlight` attribute. Apply it by writing the attribute in the
template:

```typescript
import { Directive, signal } from "@angular/core";

@Directive({
  selector: "[appHighlight]",
  host: {
    "(mouseenter)": "isHovered.set(true)",
    "(mouseleave)": "isHovered.set(false)",
    "[style.background-color]": 'isHovered() ? "yellow" : null',
  },
})
export class HighlightDirective {
  protected isHovered = signal(false);
}
```

```html
<p appHighlight>Highlight me!</p>
```

The `host` metadata adds event listeners to the element carrying the directive. The handlers update
the `isHovered` signal. The style binding reads that signal: it sets the background to yellow while
hovered and removes the inline style when the expression returns `null`.

Square brackets in a directive selector and in an Angular template have related-looking but
different jobs:

- In `selector: "[appHighlight]"`, the brackets are part of a CSS attribute selector. They mean
  "match an element that has this attribute." The usage `<p appHighlight>` is just an attribute; it
  does not need square brackets.
- In an Angular template, `[property]="expression"` is Angular property binding. Angular evaluates
  the expression and assigns the result to a DOM property or a component/directive input:

  ```html
  <img [src]="imageUrl" [alt]="description" />
  ```

  A directive can also receive a bound value this way if it declares a matching input, for example
  `<p [appHighlight]="highlightColor">` when the directive has an `appHighlight` input.

- Use `[attr.name]="expression"` when binding a literal HTML attribute rather than a DOM property,
  for example `[attr.aria-label]="label"`. Angular also provides specialized forms such as
  `[class.active]` and `[style.color]`.
- Parentheses denote event bindings, such as `(click)="save()"`. Combined `[(value)]` syntax is
  Angular's two-way binding convention.

Square brackets have no special binding meaning in plain HTML. They are Angular template syntax when
used in an Angular template; other frameworks may define different template syntax.

See the [Angular directives guide](https://angular.dev/guide/directives) for more details.
