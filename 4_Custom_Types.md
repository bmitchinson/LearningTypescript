# Custom Types
- 3 ways to define a custom type: Interface, Classes, Enums

### Custom Types with interfaces
Easiest of the three, same as interface in other typed langs.
>  It acts as a contract that describes the data and the behaviors that the 
object exposes for others to interact with.

```
interface Todo {
  name: string;
  completed?: boolean; // Optional property
}
```

- Interfaces are used for compile time checks only, no effect on runtime code.

- Assign a new variable the `Todo` interface: `var todo: Todo;`

- Instead of defining type of the variable: `var todo: Todo = {};` you could 
specify the type of the object using the casting syntax like this `var todo = <Todo>{};`

- Define functions on an interface, just do a function signature but without
the function keyword

```
interface ITodoService {
  delete(todoId: number): void;
}
```

- In chapter 4, classes implement interfacing

- You can use structural typing to match objects to interfaces implicitly

### Using interfaces to describe functions

- Since JS functions are an object, they could use interfaces as well.

```
interface example {
  // In order to define the function itself, add a function property without a name
  (selector: string): HTMLElement;
  version: number;
}

var thing = <example>function(selector) {
    // Find DOM ele
}

thing.version = 1.12;

var element = thing('#container')
```

### Extending interface definitions

```
interface Todo {
  name: string;
  completed?: boolean;
}

interface jQuery {
  (selector: (string | any)): jQueryElement;
  fn: any;
  version: number;
}

interface jQueryElement {
  data(name: string): any;
  data(name: string, data: any): jQueryElement;
}

// Anything in additional interface won't overwrite, it will simply "tac on"
interface jQueryElement {
  todo(): Todo;
  todo(todo: Todo): jQueryElement;
}
// Generally, you'd want to do this to code that you don't own, to extend libraries

var todo = { name: "Pick up drycleaning" };
var container = $('#container');
container.data('todo', todo);
var savedTodo = container.data('todo');
```

### Defining constant values with enums

- Set meaningful and constant values and avoid "magic" values. "3 means complete"
```
enum TodoState {
  New = 1,
  Active,
  Complete,
  Deleted
}
```

### Defining anonymous types

- You can declare interfaces inline, anywhere that accepts a type: an "Anonymous Type"
- Ex: Variable that should take an object that needs a name property: 
`var todo: { name: string };`

- This example from earlier can be made to allow anything with a length property of number type:
```
function totalLength(x: (string | any[]), y: (string | any[])): number {
  var total: number = x.length + y.length;
  return total;
}
```
becomes more reusable: (later we'll use generics to make sure x + y are the same type)
```
function totalLength(x: {length: number}, y: {length: number}): number {
  var total: number = x.length + y.length;
  return total;
}
```