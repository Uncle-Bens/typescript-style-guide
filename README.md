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
* [Destructuring](#destructuring)
* [Strings](#strings)
* [Functions](#functions)
* [Arrow Functions](#arrow-functions)

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

```ts
type Foo = number | { someProperty: number }
```
* Use `interface` when you want `extends` or `implements` e.g.

```ts
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
```ts
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
```ts
this.reviewScore = 9
const totalScore = new String(this.reviewScore) // typeof totalScore is "object" not "string"
const totalScore = this.reviewScore + '' // invokes this.reviewScore.valueOf()
const totalScore = this.reviewScore.toString() // isn’t guaranteed to return a string
```
**Good**
```ts
this.reviewScore = 9
const val = String(this.reviewScore)
```
**Bad**
```ts
const inputValue = '4'
const val = new Number(inputValue)
const val = +inputValue
const val = inputValue >> 0
const val = parseInt(inputValue)
```
**Good**
```ts
const inputValue = '4'
const val = Number(inputValue)
```
**Bad**
```ts
const age = 0
const val = new Boolean(age)
```
**Good**
```ts
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
```ts
if (isValid === true) ...
```
**Good**
```ts
if (isValid) ...
```
**Bad**
```ts
if (name) ...
```
**Good**
```ts
if (name !== "") ...
```
**Bad**
```ts
if (collection.length) ...
```
**Good**
```ts
if (collection.length > 0) ...
```

* Ternaries should not be nested and generally be single line expressions.

**Bad**
```ts
const foo = maybe1 > maybe2
  ? "bar"
  : value1 > value2 ? "baz" : null
```
**Good**
```ts
const maybeNull = value1 > value2 ? "baz" : null
const foo = maybe1 > maybe2
  ? "bar"
  : maybeNull
```

* When mixing operators, enclose them in parentheses. The only exception is the standard arithmetic operators: +, -, and ** since their precedence is broadly understood. We recommend enclosing / and * in parentheses because their precedence can be ambiguous when they are mixed
> Reason: This improves readability and clarifies the developer’s intention.
**Bad**
```ts
const foo = a && b < 0 || c > 0 || d + 1 === 0;
```
**Good**
```ts
const foo = (a && b < 0) || c > 0 || (d + 1 === 0);
```

* In case your control statement (if, while etc.) gets too long or exceeds the maximum line length, each (grouped) condition could be put into a new line. The logical operator should begin the line.
> Reason: requiring operators at the beginning of the line keeps the operators aligned and follows a pattern similar to method chaining. This also improves readability by making it easier to visually follow complex logic.
**Bad**
```ts
if ((foo === 123 || bar === 'abc') && doesItLookGoodWhenItBecomesThatLong() && isThisReallyHappening()) {
  thing1();
}
```
**Good**
```ts
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
```ts
const items = getItems(),
    goSportsTeam = true,
    dragonball = 'z'
```
**Good**
```ts
const items = getItems()
const goSportsTeam = true
const dragonball = 'z'
```

* Don’t chain variable assignments
**Bad**
```ts
let a = b = c = 1
```

* Avoid using unary increments and decrements (++, --)

* Avoid linebreaks before or after = in an assignment. If your assignment violates max-len, surround the value in parens.
**Bad**
```ts
const foo
  = "superLongLongLongLongLongLongLongLongString"
```
**Good**
```ts
const foo = "superLongLongLongLongLongLongLongLongString"
```

## Objects
* Use the literal syntax for object creation.
**Bad**
```ts
const item = new Object()
```
**Good**
```ts
const item = {}
```

* Use property value shorthand
**Bad**
```ts
const lukeSkywalker = 'Luke Skywalker'
const obj = {
  lukeSkywalker: lukeSkywalker,
}
```
**Good**
```ts
const lukeSkywalker = 'Luke Skywalker'
const obj = {
  lukeSkywalker
}
```

* Group your shorthand properties at the beginning of your object declaration.
**Bad**
```ts
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
```ts
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
```ts
console.log(object.hasOwnProperty(key))
```
**Good**
```ts
console.log(Object.prototype.hasOwnProperty.call(object, key))
```

* Prefer the object spread operator over ```Object.assign``` to shallow-copy objects. Use the object rest operator to get a new object with certain properties omitted.
**Bad**
```ts
const original = { a: 1, b: 2 };
const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }
```
**Good**
```ts
const original = { a: 1, b: 2 };
const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
```

## Arrays
* Use the literal syntax for array creation
**Bad**
```ts
const items = new Array()
```
**Good**
```ts
const items = []
```

* Use ``Array#push`` instead of direct assignment to add items to an array
**Bad**
```ts
const someStack = []
someStack[someStack.length] = "abracadabra"
```
**Good**
```ts
const someStack = []
someStack.push("abracadabra")
```
* Use array spreads ... to copy arrays.

**Good**
```ts
const itemsCopy = [...items]
```

* To convert an iterable object to an array, use spreads ... instead of Array.from
**Good**
```ts
const foo = document.querySelectorAll('.foo');
const nodes = [...foo];
```

* Use return statements in array method callbacks
**Bad**
```ts
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
```ts
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


## Destructuring

Use object destructuring when accessing and using multiple properties of an object. jscs: [`requireObjectDestructuring`
(http://jscs.info/rule/requireObjectDestructuring)

> Why? Destructuring saves you from creating temporary references for those properties.

**Bad**
```ts
function getFullName(user) {
  const firstName = user.firstName;
  const lastName = user.lastName;

  return `${firstName} ${lastName}`;
}
```

**Good**
```ts
function getFullName(user) {
  const { firstName, lastName } = user;
  return `${firstName} ${lastName}`;
}

function getFullName({ firstName, lastName }) {
  return `${firstName} ${lastName}`;
}
```

Use array destructuring. jscs: [`requireArrayDestructuring`](http://jscs.info/rule/requireArrayDestructuring)

**Bad**
```ts
const arr = [1, 2, 3, 4];

const first = arr[0];
const second = arr[1];
```
**Good**
```ts
const [first, second] = arr;
```

Use object destructuring for multiple return values, not array destructuring. jscs: [`disallowArrayDestructuringReturn`
(http://jscs.info/rule/disallowArrayDestructuringReturn)

    > Why? You can add new properties over time or change the order of things without breaking call sites.
**Bad**
```ts
function processInput(input) {
  // then a miracle occurs
  return [left, right, top, bottom];
}

// the caller needs to think about the order of return data
const [left, __, top] = processInput(input);
```
**Good**
```ts
function processInput(input) {
  // then a miracle occurs
  return { left, right, top, bottom };
}

// the caller selects only the data they need
const { left, top } = processInput(input);
```

## Strings

Use double quotes `""` for strings.

**Bad**
```ts
//template literals should contain interpolation or newlines
const name = `Capt. Janeway`;
```
**Good**
```ts
const name = "Capt. Janeway";
```

Strings that cause the line to go over 100 characters should not be written across multiple lines using string concatenation.

> Why? Broken strings are painful to work with and make code less searchable.

**Bad**
```ts
const errorMessage = "This is a super long error that was thrown because \
of Batman. When you stop to think about how Batman had anything to do \
with this, you would get nowhere \
fast.";

const errorMessage = "This is a super long error that was thrown because ' +
  'of Batman. When you stop to think about how Batman had anything to do ' +
  'with this, you would get nowhere fast.";
```

**Good**
```ts
const errorMessage = "This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.";
```

When programmatically building up strings, use template strings instead of concatenation. eslint: [`prefer-template`](http://eslint.org/docs/rules/prefer-template.html) [`template-curly-spacing`](http://eslint.org/docs/rules/template-curly-spacing) jscs: [`requireTemplateStrings`](http://jscs.info/rule/requireTemplateStrings)

> Why? Template strings give you a readable, concise syntax with proper newlines and string interpolation features.

**Bad**
```ts
function sayHi(name) {
  return "How are you, " + name + "?";
}

function sayHi(name) {
  return ["How are you, ", name, "?"].join();
}

function sayHi(name) {
  return `How are you, ${ name }?`;
}
```

**Good**
```ts
function sayHi(name) {
  return `How are you, ${name}?`;
}
```

Never use `eval()` on a string, it opens too many vulnerabilities.

Do not unnecessarily escape characters in strings. eslint: [`no-useless-escape`](http://eslint.org/docs/rules/no-useless-escape)

> Why? Backslashes harm readability, thus they should only be present when necessary.

**Bad**
```ts
const foo = '\'this\' \i\s \"quoted\"';
```

**Good**
```ts
const foo = '\'this\' is "quoted"';
const foo = `'this' is "quoted"`;
```

## Functions

Use named function expressions instead of function declarations. eslint: [`func-style`](http://eslint.org/docs/rules/func-style) jscs: [`requireFunctionDeclarations`](http://jscs.info/rule/requireFunctionDeclarations)

> Why? Function declarations are hoisted, which means that it’s easy - too easy - to reference the function before it is defined in the file. This harms readability and maintainability. If you find that a function’s definition is large or complex enough that it is interfering with understanding the rest of the file, then perhaps it’s time to extract it to its own module! Don’t forget to name the expression - anonymous functions can make it harder to locate the problem in an Error's call stack.

**Bad**
```ts
const foo = function () {
};

function foo() {
}
```

**Good**
```ts
const short = function longUniqueMoreDescriptiveLexicalFoo() {
  // ...
};
```

Wrap immediately invoked function expressions in parentheses. eslint: [`wrap-iife`](http://eslint.org/docs/rules/wrap-iife.html) jscs: [`requireParenthesesAroundIIFE`](http://jscs.info/rule/requireParenthesesAroundIIFE)

> Why? An immediately invoked function expression is a single unit - wrapping both it, and its invocation parens, in parens, cleanly expresses this. Note that in a world with modules everywhere, you almost never need an IIFE.

```ts
// immediately-invoked function expression (IIFE)
(function () {
  console.log('Welcome to the Internet. Please follow me.');
}());
```

Never declare a function in a non-function block (if, while, etc). Assign the function to a variable instead. Browsers will allow you to do it, but they all interpret it differently, which is bad news bears. eslint: [`no-loop-func`](http://eslint.org/docs/rules/no-loop-func.html)

**Note:** ECMA-262 defines a `block` as a list of statements. A function declaration is not a statement. [Read ECMA-262's note on this issue](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

**Bad**
```ts
if (currentUser) {
  function test() {
    console.log('Nope.');
  }
}
```

**Good**
```ts
let test;
if (currentUser) {
  test = () => {
    console.log('Yup.');
  };
}
```

Never name a parameter `arguments`. This will take precedence over the `arguments` object that is given to every function scope.

**Bad**
```ts
function nope(name, options, arguments) {
  // ...stuff...
}
```

**Good**
```ts
function yup(name, options, args) {
  // ...stuff...
}
```

  Never use `arguments`, opt to use rest syntax `...` instead. eslint: [`prefer-rest-params`](http://eslint.org/docs/rules/prefer-rest-params)

> Why? `...` is explicit about which arguments you want pulled. Plus, rest arguments are a real Array, and not merely Array-like like `arguments`.

**Bad**
```ts
function concatenateAll() {
  const args = Array.prototype.slice.call(arguments);
  return args.join('');
}
```

**Good**
```ts
function concatenateAll(...args) {
  return args.join('');
}
```

Use default parameter syntax rather than mutating function arguments.

**Bad**
```ts
function handleThings(opts) {
  // No! We shouldn't mutate function arguments.
  // Double bad: if opts is falsy it'll be set to an object which may
  // be what you want but it can introduce subtle bugs.
  opts = opts || {};
  // ...
}

function handleThings(opts) {
  if (opts === void 0) {
    opts = {};
  }
  // ...
}
```

**Good**
```ts
function handleThings(opts = {}) {
  // ...
}
```

Avoid side effects with default parameters.

> Why? They are confusing to reason about.

**Bad**
```ts
var b = 1;
function count(a = b++) {
  console.log(a);
}
count();  // 1
count();  // 2
count(3); // 3
count();  // 3
```

Always put default parameters last.

**Bad**
```ts
function handleThings(opts = {}, name) {
  // ...
}
```

**Good**
```ts
function handleThings(name, opts = {}) {
  // ...
}
```

Never use the Function constructor to create a new function. eslint: [`no-new-func`](http://eslint.org/docs/rules/no-new-func)

Why? Creating a function in this way evaluates a string similarly to eval(), which opens vulnerabilities.

**Bad**
```ts
var add = new Function('a', 'b', 'return a + b');

var subtract = Function('a', 'b', 'return a - b');
```

Spacing in a function signature. eslint: [`space-before-function-paren`](http://eslint.org/docs/rules/space-before-function-paren) [`space-before-blocks`](http://eslint.org/docs/rules/space-before-blocks)

> Why? Consistency is good, and you shouldn’t have to add or remove a space when adding or removing a name.

**Bad**
```ts
const f = function(){};
const g = function (){};
const h = function() {};
```

**Good**
```ts
const x = function () {};
const y = function a() {};
```

Never mutate parameters. eslint: [`no-param-reassign`](http://eslint.org/docs/rules/no-param-reassign.html)

> Why? Manipulating objects passed in as parameters can cause unwanted variable side effects in the original caller.

**Bad**
```ts
function f1(obj) {
  obj.key = 1;
};
```

**Good**
```ts
function f2(obj) {
  const key = Object.prototype.hasOwnProperty.call(obj, 'key') ? obj.key : 1;
};
```

Never reassign parameters. eslint: [`no-param-reassign`](http://eslint.org/docs/rules/no-param-reassign.html)

> Why? Reassigning parameters can lead to unexpected behavior, especially when accessing the `arguments` object. It can also cause optimization issues, especially in V8.

**Bad**
```ts
function f1(a) {
  a = 1;
}

function f2(a) {
  if (!a) { a = 1; }
}
```

**Good**
```ts
function f3(a) {
  const b = a || 1;
}

function f4(a = 1) {
}
```

Prefer the use of the spread operator `...` to call variadic functions. eslint: [`prefer-spread`](http://eslint.org/docs/rules/prefer-spread)

> Why? It's cleaner, you don't need to supply a context, and you can not easily compose `new` with `apply`.

**Bad**
```ts
const x = [1, 2, 3, 4, 5];
console.log.apply(console, x);
```

**Good**
```ts
const x = [1, 2, 3, 4, 5];
console.log(...x);
```

**Bad**
```ts
new (Function.prototype.bind.apply(Date, [null, 2016, 08, 05]));
```

**Good**
```ts
new Date(...[2016, 08, 05]);
```

## Arrow Functions

When you must use function expressions (as when passing an anonymous function), use arrow function notation. eslint: [`prefer-arrow-callback`](http://eslint.org/docs/rules/prefer-arrow-callback.html), [`arrow-spacing`](http://eslint.org/docs/rules/arrow-spacing.html) jscs: [`requireArrowFunctions`](http://jscs.info/rule/requireArrowFunctions)

> Why? It creates a version of the function that executes in the context of `this`, which is usually what you want, and is a more concise syntax.

> Why not? If you have a fairly complicated function, you might move that logic out into its own function declaration.

**Bad**
```ts
[1, 2, 3].map(function (x) {
  const y = x + 1;
  return x * y;
});
```

**Good**
```ts
[1, 2, 3].map((x) => {
  const y = x + 1;
  return x * y;
});
```

If the function body consists of a single expression, omit the braces and use the implicit return. Otherwise, keep the braces and use a `return` statement. eslint: [`arrow-parens`](http://eslint.org/docs/rules/arrow-parens.html), [`arrow-body-style`](http://eslint.org/docs/rules/arrow-body-style.html) jscs:  [`disallowParenthesesAroundArrowParam`](http://jscs.info/rule/disallowParenthesesAroundArrowParam), [`requireShorthandArrowFunctions`](http://jscs.info/rule/requireShorthandArrowFunctions)

> Why? Syntactic sugar. It reads well when multiple functions are chained together.

**Bad**
```ts
[1, 2, 3].map(number => {
  const nextNumber = number + 1;
  `A string containing the ${nextNumber}.`;
});
```

**Good**
```ts
[1, 2, 3].map(number => `A string containing the ${number}.`);

[1, 2, 3].map((number) => {
  const nextNumber = number + 1;
  return `A string containing the ${nextNumber}.`;
});

[1, 2, 3].map((number, index) => ({
  index: number
}));
```

In case the expression spans over multiple lines, wrap it in parentheses for better readability.

> Why? It shows clearly where the function starts and ends.

**Bad**
```ts
['get', 'post', 'put'].map(number => Object.prototype.hasOwnProperty.call(
    httpMagicObjectWithAVeryLongName,
    httpMethod
  )
);
```

**Good**
```ts
['get', 'post', 'put'].map(number => (
  Object.prototype.hasOwnProperty.call(
    httpMagicObjectWithAVeryLongName,
    httpMethod
  )
));
```

If your function takes a single argument and doesn’t use braces, omit the parentheses. Otherwise, always include parentheses around arguments. eslint: [`arrow-parens`](http://eslint.org/docs/rules/arrow-parens.html) jscs:  [`disallowParenthesesAroundArrowParam`](http://jscs.info/rule/disallowParenthesesAroundArrowParam)

> Why? Less visual clutter.

**Bad**
```ts
[1, 2, 3].map((x) => x * x);
```

**Good**
```ts
[1, 2, 3].map(x => x * x);

[1, 2, 3].map(number => (
  `A long string with the ${number}. It’s so long that we’ve broken it ` +
  'over multiple lines!'
));
```

**Bad**
```ts
[1, 2, 3].map(x => {
  const y = x + 1;
  return x * y;
});
```

**Good**
```ts
[1, 2, 3].map((x) => {
  const y = x + 1;
  return x * y;
});
```

Avoid confusing arrow function syntax (`=>`) with comparison operators (`<=`, `>=`). eslint: [`no-confusing-arrow`](http://eslint.org/docs/rules/no-confusing-arrow)

**Bad**
```ts
const itemHeight = item => item.height > 256 ? item.largeSize : item.smallSize;

const itemHeight = (item) => item.height > 256 ? item.largeSize : item.smallSize;
```

**Good**
```ts
const itemHeight = item => (item.height > 256 ? item.largeSize : item.smallSize);

const itemHeight = (item) => {
  const { height, largeSize, smallSize } = item;
  return height > 256 ? largeSize : smallSize;
};
```
