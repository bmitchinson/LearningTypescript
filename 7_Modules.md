# Modules

### Understanding the need for modules in JS
Everything used to be global. Gross, stop it.

Three main methods of JS encapsulation:
- Module Pattern vs revealing Module Pattern
- Namespaces
- ECMAScript 2015 modules/module Loaders

### Organizing your code with namespaces
Simplest method of encapsulation

You're free to declare the same namespace, multiple times either in the same file
or multiple, allowing you to organize your code however you like.

In order to use types outside of the scope (or namespace) you define them, you
need to expose them using the export keyword.

When you try to reference types across namespaces, even with export, you have to
reference them with their namespace as a prefix.

Ex: Accessing `Todo` located in `namespace TodoApp.Model` from `namespace DataAccess`:
`TodoApp.Model.Todo`

The alternative of this, is to create an alias for the type by 
importing from an outside namespace. Ex: 

```
import Model = TodoApp.Model
import Todo = Model.Todo
```

### Using namespaces to encapsulate private members (Internal module approach)

IIFEs used behind the curtain to make namespaces do their thing.
"Used to encapsulate code while it executes and choose which parts will be exposed" 

You can still pass in parameters when using an IIFE `(function blah(ope) ...)(param);`

If a variable is in a separate namespace (even one with the same shared name), 
you need to export it to be able to use it.

The end result of all of this is that I've got two private members that live outside of my class, completely encapsulated from the rest of the world, but fully accessible to any classes or functions that are defined within the same namespace declaration.

```
namespace DataAccess {

    import Model = TodoApp.Model;
    import Todo = Model.Todo;

    let _lastId: number = 0;

    function generateTodoId() { return _lastId += 1; }

    export interface ITodoService {
        add(todo: Todo): Todo;
        delete(todoId: number): void;
        getAll(): Todo[];
        getById(todoId: number): Todo;
    }

    class TodoService implements ITodoService {

        constructor(private todos: Todo[]) {}

        add(todo: Todo): Todo {
            todo.id = generateTodoId();
            this.todos.push(todo);
            return todo;
        }

        delete(todoId: number): void {
            var toDelete = this.getById(todoId);
            var deletedIndex = this.todos.indexOf(toDelete);
            this.todos.splice(deletedIndex, 1);
        }

        getAll(): Todo[] {
            var clone = JSON.stringify(this.todos);
            return JSON.parse(clone);
        }

        getById(todoId: number): Todo {
            var filtered = this.todos.filter(x => x.id == todoId);
            if (filtered.length) {
                return filtered[0];
            }
            return null;
        }
    }
}
```

### Understanding the difference between internal and external modules

Namespaces in the internal module approach allows you to group components together and 
get them out of the global scope by bringing them all together under one umbrella 
object.

External modules share the same goals of encapsulation, but *uses the file itself*
as the module scope.

Both internal (namespace) + external (traditional imports) require components to be exported.

"External" syntax types: Require (like Node.js) and ECMAScript15 standard

Whichever you use, TS generates the same code, recommend to use ECMA ("UTP")

### Switching from internal to external modules

It's a bit redundant to apply internal namespaces to external modules, since
they're already modularized by the file itself.

in tsconfig you can specify the module setting, but you're really free to 
use any type.

### Importing modules using CommonJS syntax (aka 'require' syntax)

`import Model = require('/.model')`: This brings everything that's been exported
in `/.model` into the `Model` object. No file extension needed. 
(Nothing about defaults yet)

You can't assign an alias to parts of the `Model` import, aside from manual 
reassignment in sperate lines.

Ex:
```
import Model = require('/.model')
import Todo = Model.Todo;
```

An alias *can* be assigned immediately upon import with ECMA15 syntax:

### Importing modules using ECMAScript 15 syntax

Also uses import by it's relative path and name: `import * as Model from './model'`;

Now just like the previous example with CommonJS, `Model.Todo` is available.

Instead of using `Todo` through `Model.Todo` though, it can be assigned an alias
upon import: `import { Todo as TodoTask, TodoState } from './model'`

(From chapter 8:


Default module: used when there's really only a need for one export:
`export default class TodoListComponent` -> `import nameOfDefaultExport from './TodoListComponent'`

Although you can only have one default export, you can combine it with other 
exports, resulting in an import statement like: `import TodoService, { ITodoService } from './TodoListComponent'`.

)

This lets you import selectively as well, (reducing code size in the end!). In
the above example, not everything from `./model` is imported, as it's not all 
needed.

You could also do simply `import ./Example`.
> Really, the only scenario you'd want to do this is when you have a script that modifies the environment in some way, that you're dependent on. In those cases, the import statement is still relevant, because you do depend on that module getting loaded. You're just not relying on any particular exports that the module provides.

### Loading external modules

Within TS, internal and both types of external get transpiled down to one constant
module type anyway (set in `tsconfig`).

Since ECMA15 Syntax isn't "widely supported" yet, and the standard definition for
loading modules (loader specification) is under review, we need a Module Loader.

There's a module config option for modules in TS for every major module loader.
Choosing what goes in this config option relies on your loader.

`System.js` attempts to implement the proposed ECMAScript specification.

Before using a module loader, you'd have to import each js file in a script tag
within your HTML. Now though, you can use on import, for your loader.

Example of [how he uses SystemJS](https://www.linkedin.com/learning/typescript-essential-training/loading-external-modules?u=42459020) 
through a CDN import within HTML.