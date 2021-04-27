# TypeScript Style Guide and Coding Conventions in DGT

Key Sections:

* [File Names](#filename)
* [Variable and Function names](#variable-and-function-name)
* [Class](#class)
* [Interface](#interface)
* [Type](#type)
* [`type` vs `interface`](#type-vs-interface)
* [Namespace](#namespace)
* [Enum](#enum)
* [Single vs. Double Quotes](#quotes)
* [Tabs vs. Spaces](#spaces)
* [Use semicolons](#semicolons)
* [Type Coersion](#type-coersion)
* [`null` vs. `undefined`](#null-vs-interface)
* [Conditional Evaluation](#conditional-evaluation)
* [Variables](#variables)
* [Objects](#objects)
* [Arrays](#arrays)

## Filename
* Use a pattern```feature-name.type.ts```.

As example: ```primary-button.presets.ts```

## Variable and Function names
* Use `camelCase` for variable and function names

> Reason: Conventional JavaScript

**Bad**
```ts
var FooVar;
function BarFunc() { }
```
**Good**
```ts
var fooVar;
function barFunc() { }
```

## Class
* Use `PascalCase` for class names.

> Reason: This is actually fairly conventional in standard JavaScript.

**Bad**
```ts
class foo { }
```
**Good**
```ts
class Foo { }
```
* Use `camelCase` of class members and methods

> Reason: Naturally follows from variable and function naming convention.

**Bad**
```ts
class Foo {
    Bar: number
    Baz() { }
}
```
**Good**
```ts
class Foo {
    bar: number
    baz() { }
}
```
## Interface

* Use `PascalCase` for name.

> Reason: Similar to class

* Use `camelCase` for members.

> Reason: Similar to class

* **Don't** prefix with `I`

> Reason: Unconventional. `lib.d.ts` defines important interfaces without an `I` (e.g. Window, Document etc).

**Bad**
```ts
interface IFoo {
}
```
**Good**
```ts
interface Foo {
}
```

## Type

* Use `PascalCase` for name.

> Reason: Similar to class

* Use `camelCase` for members.

> Reason: Similar to class


## type vs. interface

* Use `type` when you *might* need a union or intersection:

```
type Foo = number | { someProperty: number }
```
* Use `interface` when you want `extends` or `implements` e.g.

```
interface Foo {
  foo: string
}
interface FooBar extends Foo {
  bar: string
}
class X implements FooBar {
  foo: string
  bar: string
}
```

## Namespace

* Use `PascalCase` for names

> Reason: Convention followed by the TypeScript team. Namespaces are effectively just a class with static members. Class names are `PascalCase` => Namespace names are `PascalCase`

**Bad**
```ts
namespace foo {
}
```
**Good**
```ts
namespace Foo {
}
```

## Enum

* Use `PascalCase` for enum names

> Reason: Similar to Class. Is a Type.

**Bad**
```ts
enum color {
}
```
**Good**
```ts
enum Color {
}
```

* Use `PascalCase` for enum member

> Reason: Convention followed by TypeScript team i.e. the language creators e.g `SyntaxKind.StringLiteral`. Also helps with translation (code generation) of other languages into TypeScript.

**Bad**
```ts
enum Color {
    red
}
```
**Good**
```ts
enum Color {
    Red
}
```

## Quotes

* Use double quote (`""`).

## Spaces

* Use `2` spaces. Not tabs.

> Reason: More JavaScript teams do this (e.g. [airbnb](https://github.com/airbnb/javascript), [idiomatic](https://github.com/rwaldron/idiomatic.js), [standard](https://github.com/feross/standard), [npm](https://github.com/npm/npm), [node](https://github.com/nodejs/node), [google/angular](https://github.com/angular/angular/), [facebook/react](https://github.com/facebook/react)). The TypeScript/VSCode teams use 4 spaces but are definitely the exception in the ecosystem.

## Semicolons

* Not use semicolons.

## Type Coersion

* Not use "smart" coercions
> Reason: next examples should be considered "unnecessarily clever". Prefer the obvious approach of comparing the returned value.

**Bad**
```
let number = 1,
  string = "1",
  bool = false

number
// 1

number + ""
// "1"

string
// "1"

+string
// 1

+string++
// 1

string
// 2

bool
// false

+bool
// 0

bool + ""
// "false"
```

* Perform type coercion at the beginning of the statement.

**Bad**
```
this.reviewScore = 9
const totalScore = new String(this.reviewScore) // typeof totalScore is "object" not "string"
const totalScore = this.reviewScore + '' // invokes this.reviewScore.valueOf()
const totalScore = this.reviewScore.toString() // isn’t guaranteed to return a string
```
**Good**
```
this.reviewScore = 9
const val = String(this.reviewScore)
```
**Bad**
```
const inputValue = '4'
const val = new Number(inputValue)
const val = +inputValue
const val = inputValue >> 0
const val = parseInt(inputValue)
```
**Good**
```
const inputValue = '4'
const val = Number(inputValue)
```
**Bad**
```
const age = 0
const val = new Boolean(age)
```
**Good**
```
const age = 0
const val = Boolean(age)
```

## Null vs. Undefined

* Prefer not to use either for explicit unavailability

> Reason: these values are commonly used to keep a consistent structure between values. In TypeScript you use *types* to denote the structure

**Bad**
```ts
let foo = { x: 123, y: undefined }
```
**Good**
```ts
let foo: { x: number, y?: number } = { x:123 }
```

* Use `undefined` in general (do consider returning an object like `{valid:boolean, value?:Foo}` instead)

**Bad**
```ts
return null
```
**Good**
```ts
return undefined
```

* Use `null` where it's a part of the API or conventional

## Conditional Evaluation
* Conditional statements such as the if statement evaluate their expression using coercion with the ToBoolean abstract method and always follow these simple rules:

Objects evaluate to true
Undefined evaluates to false
Null evaluates to false
Booleans evaluate to the value of the boolean
Numbers evaluate to false if +0, -0, or NaN, otherwise true
Strings evaluate to false if an empty string '', otherwise true

* Use `===` and `!==` over `==` and `!=` (unless the case requires loose type evaluation)

* Use `== null` / `!= null` (not `===` / `!==`) to check for `null` / `undefined` on primitives as it works for both `null`/`undefined` but not other falsy values (like `''`, `0`, `false`) e.g.

**Bad**
```ts
if (error !== null)... // does not rule out undefined
```
**Good**
```ts
if (error != null)... // rules out both null and undefined
```

* Use *truthy* check for **objects** being `null` or `undefined`

**Bad**
```ts
if (error === null)...
```
**Good**
```ts
if (error)...
```

* Use shortcuts for booleans, but explicit comparisons for strings and numbers.

**Bad**
```
if (isValid === true) ...
```
**Good**
```
if (isValid) ...
```
**Bad**
```
if (name) ...
```
**Good**
```
if (name !== "") ...
```
**Bad**
```
if (collection.length) ...
```
**Good**
```
if (collection.length > 0) ...
```

* Ternaries should not be nested and generally be single line expressions.

**Bad**
```
const foo = maybe1 > maybe2
  ? "bar"
  : value1 > value2 ? "baz" : null
```
**Good**
```
const maybeNull = value1 > value2 ? "baz" : null
const foo = maybe1 > maybe2
  ? "bar"
  : maybeNull
```

* When mixing operators, enclose them in parentheses. The only exception is the standard arithmetic operators: +, -, and ** since their precedence is broadly understood. We recommend enclosing / and * in parentheses because their precedence can be ambiguous when they are mixed
> Reason: This improves readability and clarifies the developer’s intention.
**Bad**
```
const foo = a && b < 0 || c > 0 || d + 1 === 0;
```
**Good**
```
const foo = (a && b < 0) || c > 0 || (d + 1 === 0);
```

* In case your control statement (if, while etc.) gets too long or exceeds the maximum line length, each (grouped) condition could be put into a new line. The logical operator should begin the line.
> Reason: requiring operators at the beginning of the line keeps the operators aligned and follows a pattern similar to method chaining. This also improves readability by making it easier to visually follow complex logic.
**Bad**
```
if ((foo === 123 || bar === 'abc') && doesItLookGoodWhenItBecomesThatLong() && isThisReallyHappening()) {
  thing1();
}
```
**Good**
```
if (
  (foo === 123 || bar === 'abc')
  && doesItLookGoodWhenItBecomesThatLong()
  && isThisReallyHappening()
) {
  thing1();
}
```

## Variables
* Always use const or let to declare variables. 
> Reason: not doing so will result in global variables. We want to avoid polluting the global namespace.
 
* Use const for all of your references. 
> Reason: this ensures that you can’t reassign your references, which can lead to bugs and difficult to comprehend code.

* If you must reassign references, use let instead of var.
> Reason: let is block-scoped rather than function-scoped like var.

* Use one const or let declaration per variable or assignment.
**Bad**
```
const items = getItems(),
    goSportsTeam = true,
    dragonball = 'z'
```
**Good**
```
const items = getItems()
const goSportsTeam = true
const dragonball = 'z'
```

* Don’t chain variable assignments
**Bad**
```
let a = b = c = 1
```

* Avoid using unary increments and decrements (++, --)

* Avoid linebreaks before or after = in an assignment. If your assignment violates max-len, surround the value in parens.
**Bad**
```
const foo
  = "superLongLongLongLongLongLongLongLongString"
```
**Good**
```
const foo = "superLongLongLongLongLongLongLongLongString"
```

## Objects
* Use the literal syntax for object creation.
**Bad**
```
const item = new Object()
```
**Good**
```
const item = {}
```

* Use property value shorthand
**Bad**
```
const lukeSkywalker = 'Luke Skywalker'
const obj = {
  lukeSkywalker: lukeSkywalker,
}
```
**Good**
```
const lukeSkywalker = 'Luke Skywalker'
const obj = {
  lukeSkywalker
}
```

* Group your shorthand properties at the beginning of your object declaration.
**Bad**
```
const lukeSkywalker = 'Luke Skywalker'
const anakinSkywalker = 'Anakin Skywalker'

const obj = {
  episodeOne: 1,
  twoJediWalkIntoACantina: 2,
  lukeSkywalker,
  episodeThree: 3,
  mayTheFourth: 4,
  anakinSkywalker,
}
```
**Good**
```
const lukeSkywalker = 'Luke Skywalker'
const anakinSkywalker = 'Anakin Skywalker'

const obj = {
  lukeSkywalker,
  anakinSkywalker,
  episodeOne: 1,
  twoJediWalkIntoACantina: 2,
  episodeThree: 3,
  mayTheFourth: 4,
}
```

* Do not call Object.prototype methods directly, such as hasOwnProperty, propertyIsEnumerable, and isPrototypeOf
> Reason: these methods may be shadowed by properties on the object in question - consider { hasOwnProperty: false } - or, the object may be a null object (Object.create(null)).
**Bad**
```
console.log(object.hasOwnProperty(key))
```
**Good**
```
console.log(Object.prototype.hasOwnProperty.call(object, key))
```

* Prefer the object spread operator over ```Object.assign``` to shallow-copy objects. Use the object rest operator to get a new object with certain properties omitted.
**Bad**
```
const original = { a: 1, b: 2 };
const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }
```
**Good**
```
const original = { a: 1, b: 2 };
const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
```

## Arrays
* Use the literal syntax for array creation
**Bad**
```
const items = new Array()
```
**Good**
```
const items = []
```

* Use ``Array#push`` instead of direct assignment to add items to an array
**Bad**
```
const someStack = []
someStack[someStack.length] = "abracadabra"
```
**Good**
```
const someStack = []
someStack.push("abracadabra")
```
* Use array spreads ... to copy arrays.

**Good**
```
const itemsCopy = [...items]
```

* To convert an iterable object to an array, use spreads ... instead of Array.from
**Good**
```
const foo = document.querySelectorAll('.foo');
const nodes = [...foo];
```

* Use return statements in array method callbacks
**Bad**
```
inbox.filter((msg) => {
  const { subject, author } = msg
  if (subject === "Mockingbird") {
    return author === "Harper Lee"
  } else {
    return false
  }
});
```
**Good**
```
inbox.filter((msg) => {
  const { subject, author } = msg
  if (subject === "Mockingbird") {
    return author === "Harper Lee"
  }

  return false
});
```

* Annotate arrays as `foos: Foo[]` instead of `foos: Array<Foo>`.

> Reasons: It's easier to read. It's used by the TypeScript team. Makes easier to know something is an array as the mind is trained to detect `[]`.


