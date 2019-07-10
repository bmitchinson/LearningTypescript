# Type Fundamentals

## Introducing JavaScript Types
- boolean, number, string [3 immutable primitives]
- null / undefined
- objects [completly mutable]

Functions and Arrays *are* objects (woah)
- Functions just objects that contain logic that can be executed
- Both functions and arrays get their cool methods from having a prototype

Prototypical Inheritance:
> "An object is defined that contains the base properties and behavior to be 
shared, and when new instances of that type are created, JS links those new 
instances to the properties and behaviors of the base class."

### Object Literals
Not all JS objects are created from a constructor that has a prototype:
Object Literal defines and instantiates the object at the same time, 
that's all the ...
```
objectEx = {
  prop1: 'hello'
  speak: function () { console.log('Woof'); }
}
```
...syntax is.

## Type Inference
Relies on static analysis to give you benefits in using TypeScript, even when
just writing plain JS. 

If you define using an object literal, when you try to re-define properties or 
access them, TypeScript will enforce their previously defined type.

> "Strategically applying type information to common, low level functions and 
API calls is a great way to start getting the most out of TypeScript's type 
inference in your existing JavaScript codebase without having to write any 
TypeScript specific code at all."

When TS can't infer, it gives it the `any` type. Once you have `any`, TypeScript
can't help assume anything about it. You're essentially completely opting out of 
TS.

## Specifying JS Types
- For function parameter typing: `function totalLength(x, y: string) { ...`
- You can also apply function return types without specifying all inner contents:
 `function total(x, y: string): string {...`
- array typing syntax: `x: any[]`

## Specifying function parameter types
- dual typing `function total(x: string | any[], y: string | any[]): number {`
means that "`total()` returns a number, and takes either a string or array full of
any types, as both the x and y input parameters."
- you can also wrap the types like this `x: (string | any[])` for clarity
- "Type Guard" syntax allows you to use type specific methods, by making certain
that they are `instanceof` Ex: `if (x instanceof Array) { x.push('abc') }`.
TypeScript won't compain about using push on the initially `any` typed `x` variable,
because it knows within the scope of that block that `x` must be an array.
  - If Type Gaurding a primitive, like `string` you'll need to use `typeof` in place
    of instanceof. (You could just use the *S*tring class and then still use 
    `instanceof`)

## Adding function overloads
> TypeScript also supports the idea of overloaded functions. In languages like 
C# and Java, you'd implement two completely different functions and you'd 
associate them together by simply giving them the same name. 
This won't work in JavaScript because the second definition of the function 
would just overwrite the first definition.

> The way that TypeScript allows us to define overloaded functions is to 
simply add the alternate signatures that we want to expose right on top of the 
actual function implementation.

These overloads are removed in the JavaScript, but are there just to make writing
easier.

```
function totalLength(x: string, y: string): number
function totalLength(x: any[], y: any[]): number
function totalLength(x: (string | any[]), y: (string | any[])): number {
  ...
}
```
So you still have to write only one function, it just helps in giving you
intellisense of what that function can handle / do. "It's simply metadata
to help you write better code"