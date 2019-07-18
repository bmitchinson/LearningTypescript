# Decorators

> Proposed syntax to implement decorator design pattern to modify the behavior of a class, method, property or parameter in a declaritive fashion.

> This powerful approach allows you to define common behavior in a central place and then easily apply it across your application to reduce duplicate code, and make your code more readable and maintainable, all at the same time.

### Implementing method decorators

Example of usage without TS:

> This is your basic method decorator pattern. It replaces the original method with another method that duplicates the original methods behavior by actually calling that original method, and then returning its result.

```
var ogMethod = TodoService.prototype.add;

TodoService.prototype.add = function(...args) {

  // Add logic to maybe print input

  let returnValue = ogMethod.apply(this, args);

  // Add more if ya want to print after ogMethod was called

  return returnValue;
}
```

Wrapping behavior to an existing method. TS Has Class, Property, Method, and
Parameter, Decorator types.

> In order to apply a decorator to a method, you simply apply an @ symbol, followed by a decorator name in front of the member that you want to decorate

Ex: How to implement a method decorator

```
within class
  ...(method definitions...)
  add(todo: Todo): Todo
  @log // <- that's it
  add(input): Todo { ... }
  ...
end class
...
(private functions)
function log(target: Object, methodName: string, descriptor:
TypedPropertyDescriptor<Function> ) {
  descriptor.value // value is the method itself (ogMethod in prev example)
                   // descriptor.configurable, enumerable, get, set, value, writable
  var ogMethod = descriptor.value;
  descriptor.value = function(...args) {
    console.log(`${methodName}(args)`)
    let returnValue = originalMethod.apply(this, args);
    console.log(`${methodName}(args) => ${returnValue}`)
    return returnValue
  }
}
// Param explanation for above:
// target - object member lives on (instance of todo service)
// method - name of method to be decorated
// descriptor - object that contains all of the metadata for the method you're
//              looking to modify.
```

Toggling "experimental decorators" might still be necessary in tsconfig?
Proposals are in [stage 2](https://github.com/tc39/proposal-decorators)

[Reminder on how tc39 proposal stages work](https://2ality.com/2015/11/tc39-process.html)

### [Implementing class decorators](https://www.linkedin.com/learning/typescript-essential-training/implementing-class-decorators?u=42459020)

Skip

### [Implementing property decorators](https://www.linkedin.com/learning/typescript-essential-training/implementing-property-decorators?u=42459020)

Skip

### [Implementing decorator factories](https://www.linkedin.com/learning/typescript-essential-training/implementing-decorator-factories?u=42459020)

Skip