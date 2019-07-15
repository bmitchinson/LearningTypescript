# Classes
Using classes to implement custom types, with inheritance abstraction and 
encapsulation.

### Understanding prototypical inheritance
To share behavior between object instances, you define that behavior on the 
prototype object, and then link other object instances to that object.

When trying to access an object's member, JS checks the object, then the object's
prototype, then the prototype's prototype, until it reaches Object.prototype

Most often prototypes are assigned in the constructor, when using the `new`
keyword. 

```
function TodoService() {
  this.todos = [];
}
TodoService.prototype.getAll = function() {
  return this.todos;
}
var service = new TodoService();
service.getAll()
```

### Defining a class
The class syntax does that previous example, with sugar.

```
class TodoService {

  todos: Todo[]; // TS: TodoService has a property named todos that can contain
                 //     an array of todo type objects, but it doesn't actually
                 //     create the property. That still happens in the constructor.
                 //     
                 //     You could type and define with "todos: Todo[] = [];"
                 //     ( At this same scope outside of the constructor )

  constructor() {
    this.todos = [];
  }

  getAll(){

  }
}
```

By using TS, the entire above example can be reduced to something much smaller,
thanks to the `private` keyword. Using `private` defines a constructor param and
a class property in all one expression.

```
class TodoService {
  constructor(private todos: Todo[]) {

  }
  getAll() {
    return this.todos;
  }
}
```

TS Turns all of this into prototype based syntax, like in the understanding
prototypical inheritance example.

### Applying static properties

Static example is a unique object ID remember? Used to be done just in the global
namespace in JS.

```
class TodoService {
  static lastId: number = 0; // ES6 + typing

  static getNextId() {
    return TodoService.lastId += 1;
  }
...
```

Still should avoid static variables so that things don't get brittle.

### Making properties smarter with accessors
Accessors being get and set methods

```
var todo = {
  name: "Pick up drycleaning",
  get state() {
    return TodoState.complete;
  },
  set state(newState) {
    this._state = newState
  }
}
```

As a consumer, if I use `todo.state`, I don't notice, but i'm actually using
the `get state()` accessor. Or `set` in the case of reassigning `todo.state`.

Normally though, you don't want much "getter setter" logic in an object literal.
If you really need that it's probably best to just make an instance of a class.

### Inheriting behavior from a base class

ES6 introduces extending classes.

```
class TodoStateChange {
  ...
  changeState(todo: Todo): Todo {
  ...
  }
}
class CompleteTodoStateChanger extends TodoStateChanger {
  // If you choose not to define a constructor, you inherit the constructor
  //   of the parent class (TodoStateChange)

  // To override, just use the same function signature. No need for any keywords.

  // super object can be used to reference a method from the parent class,
  //   even if you're in the process of overriding itself. 
  changeState(...){
    return super(...) // returns the changeState from TodoStateChange
  }
}
```

Aside from type info, all of this is pretty much ES6.

ES6 does not support defining / inheriting from abstract classes: TS does...

### Implementing an abstract class

Works how you would assume, just add `abstract` to a class definition, and TS 
will prevent any instantiations. 

```
abstract class TodoStateChanger {
  ...
  abstract canChangeState(todo: Todo): boolean; // "hey, every class that extends
                                                //   this needs a def of this method.
}
```

Since abstract is only a TS thing, some "rouge" js code elsewhere could still
instantiate TodoStateChanger. It is transpiled down to a normal class.

### Controlling visibility with access modifiers

You can use access modifiers like `private` on any member of a typescript class.
Not just variables you want to be local.

```
class TodoService {
  private static lastId: number = 0;

  constructor(private todos: Todo[]){ // Auto assign constructor param to local private
  }

  private get nextId(){} // When using access modifiers on getter / setters,
                         //   They have to be the same on both the get and set.

  private set nextId(){}
}
```
The three TS modifiers (Same as JS):
- `Private`: Most restrictive, only methods on the same class def may access. This includes denying classes that extend or inherit.
- `Protected`: Same as private but includes anything that extends or inherits
- `Public`: May be accessed from any other type. Default JS behavior, default access.

Could use any of these accessors for the constructor param variable shortcut

JS Doesn't actually support private, it's just a TS thing. Useful to express
intent in your code though.

- 5 min mark -

### Implementing interfaces