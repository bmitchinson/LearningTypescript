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

### Importing modules using CommonJS syntax

### Importing modules using ECMAScript 15 syntax

### Loading external modules