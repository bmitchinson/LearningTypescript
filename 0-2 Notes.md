### Intro - Env Config - ES6 Overview

- Typescript is a superset of Javascript
  - Adds static typing
- Dynamic vs. Static
  - Static types end up catching the errors before they're attempted.
  - Everything's explicit

ES6:
- Has default params
- Template strings
```
var displayName = `Todo #${todo.id}`
```
- reminder that you can use let pretty much anywhere you're using var
- loops (for of)
```
for (var index in array) {
  var value = array[index];
  console.log(`${index}: ${value}`);
}
```
becomes: 
```
for (var value of array) {
  console.log(value);
}
```
- arrow functions are just lambdas
  - end up saving you from having to pass `this` around

- Rename object during destructuring:
```
todo = { name: 'hey', completed: false}
{name: title, done} = todo // Re-names name property -> title 
```
- destructuring in function params
```
function countdown(initial, final = 0, interval = 1) {
  ...
}
```
becomes 
```
function countdown({ initial, final = 0, interval = 1}) {
  ...
}
```
- spread is cool
- Computer properties with dynamic names
```
var oxPrefix = 'os_';
var support = (_a = {},
  _a[osPrefix + 'Windows'] = isSupported('Windows'),
  _a[osPrefix + 'iOS'] = isSupported('iOS'),
  _a[osPrefix + 'Android'] = isSupported('Android'),
  _a
)
```