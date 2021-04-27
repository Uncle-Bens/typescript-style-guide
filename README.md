# TypeScript Style Guide and Coding Conventions in DGT

Key Sections:

* [Variable](#variable-and-function)
* [Class](#class)
* [Interface](#interface)
* [Type](#type)
* [Namespace](#namespace)
* [Enum](#enum)
* [Single vs. Double Quotes](#quotes)
* [Tabs vs. Spaces](#spaces)
* [Use semicolons](#semicolons)
* [Annotate Arrays as `Type[]`](#array)
* [File Names](#filename)
* [`type` vs `interface`](#type-vs-interface)
* [`null` vs. `undefined`](#null-vs-undefined)
* [Conditional Evaluation](#conditionals)

## Variable and Function
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
    Bar: number;
    Baz() { }
}
```
**Good**
```ts
class Foo {
    bar: number;
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

* Use one quote (`''`).

## Spaces

* Use `2` spaces. Not tabs.

> Reason: More JavaScript teams do this (e.g. [airbnb](https://github.com/airbnb/javascript), [idiomatic](https://github.com/rwaldron/idiomatic.js), [standard](https://github.com/feross/standard), [npm](https://github.com/npm/npm), [node](https://github.com/nodejs/node), [google/angular](https://github.com/angular/angular/), [facebook/react](https://github.com/facebook/react)). The TypeScript/VSCode teams use 4 spaces but are definitely the exception in the ecosystem.

## Semicolons

* Not use semicolons.


## Array

* Annotate arrays as `foos: Foo[]` instead of `foos: Array<Foo>`.

> Reasons: It's easier to read. It's used by the TypeScript team. Makes easier to know something is an array as the mind is trained to detect `[]`.

## Filename
* Use a pattern```feature.type.ts```.
* Use dashes to separate words in the descriptive name. 
* Use dots to separate the descriptive name from the type.
As example: ```primary-button.presets.ts```

## type vs. interface

* Use `type` when you *might* need a union or intersection:

```
type Foo = number | { someProperty: number }
```
* Use `interface` when you want `extends` or `implements` e.g.

```
interface Foo {
  foo: string;
}
interface FooBar extends Foo {
  bar: string;
}
class X implements FooBar {
  foo: string;
  bar: string;
}
```

## Null vs. Undefined

* Prefer not to use either for explicit unavailability

> Reason: these values are commonly used to keep a consistent structure between values. In TypeScript you use *types* to denote the structure

**Bad**
```ts
let foo = { x: 123, y: undefined };
```
**Good**
```ts
let foo: { x: number, y?: number } = { x:123 };
```

* Use `undefined` in general (do consider returning an object like `{valid:boolean, value?:Foo}` instead)

**Bad**
```ts
return null;
```
**Good**
```ts
return undefined;
```

* Use `null` where it's a part of the API or conventional

## Conditional Evaluation
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

* Use shortcuts

**Bad**
```
if ( array.length > 0 ) ...
if ( array.length === 0 ) ...
```
**Good**
```
if ( array.length ) ... 
if ( !array.length ) ...
```
**Bad**
```
if ( string !== "" ) ...
if ( string === "" ) ...
```
**Good**
```
if ( string ) ...
if ( !string ) ...
```
**Bad**
```
if ( foo === true ) ...
if ( foo === false ) ...
```
**Good**
```
if ( foo ) ...
if ( !foo ) ...
```
*Be careful, the last example will also match: 0, "", null, undefined, NaN. If you _MUST_ test for a boolean false, then use
```
 if ( foo === false ) ...
```


* Otherwise use whatever makes you happy that day.
