# Real-World App Development

### Introducing the sample JS application

Focus on mindset over syntax. Showing complete Todo application in ES5, going
to convert to TS

### Converting existing JS code to TS

Creating classes out of large function objects, generic type method, access mods

[link](https://www.linkedin.com/learning/typescript-essential-training/converting-existing-javascript-code-to-typescript?u=42459020)


### Generating declaration files

Surprise, not all libraries are written in TS.

To tell TS about an existing variable, without actually declaring it with a type,
use the `declare` keyword: `declare var $: any`.

This gets rid of errors, but doesn't give strong typing. To get that, use a 
TS Declaration file:

> A file that describes a library that's not written in TypeScript. The file describes the type information about the JavaScript code that it goes along with. They don't actually define an implementation themselves. They just describe an implementation that lives in another file. 

Have their own "interesting" syntax, but TS can actually generate declaration
files for you: Set `declaration` in the tsconfig file to true. After compiling
TS, you'll get the js files as output, as well as a `.d.ts`.

> "When I open this file, I see that TypeScript includes the same interfaces that I defined pretty much untouched. The only difference is that for any type that represents actual code, things like enums, variables, and class definition, TypeScript attaches the declare keyword in front of them to indicate that the type definition just describes an implementation that lives elsewhere. These kinds of declarations that don't define an implementation are called ambient declarations."

Use case of providing `.d.ts`: a way to document minified/bundled js. Provide
the small js, but include that declaration file too for info. "best of both worlds".

### Referencing third-party libraries

>"Going to show you tsd to download TS decs for any popular OS library"

[tsd has been deprecated](https://github.com/DefinitelyTyped/tsd/issues/269),
in favor of [typings which is also deprecated](https://github.com/typings/typings/issues).

[TS files here?](https://github.com/Microsoft/TypeSearch) Not updated since Jan 18, provided by microsoft though?

Looks like everyone is on to [DefinitelyTyped](http://definitelytyped.org/) now?

Oh, no DefinitelyTyped is the resource that tsc relied on to work? 
Looks like even tho tsc is deprecated it still might be an okay option for 
lightweight usage? Will have to experiment.

### Converting to external modules

Can move private + static functions out of the class definition, so that they're
"truly private".

including a CDN import: `import '//code.jquery.com/jquery-1.12.1.min.js`.
(Since the module loader supports URLs or relative paths)
This obviously doesn't actually import any types. This import lets the module 
loader know that this module (in which jquery is imported from url) expects 
jquery to have been loaded and made available before the current module itself 
gets loaded.

In this example, TS knows about all the jquery types because of the `d.ts` 
installed earlier.

Default module: used when there's really only a need for one export:
`export default class TodoListComponent` -> `import nameOfDefaultExport from './TodoListComponent'`

Although you can only have one default export, you can combine it with other 
exports, resulting in an import statement like: `import TodoService, { ITodoService } from './TodoListComponent'`.

### Debugging TS with source maps

Purpose. JS output errors don't make much since after being transpiled. Source
maps allow you to relate that JS output back to it's original TS.