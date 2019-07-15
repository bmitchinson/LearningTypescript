# Generics
Create functions and classes that define a behavior to be used
across many different types, while retaining the information
about that type.

### Introduction

Ex: TS has no idea what this will return, since it could parse
anything. 
```
function clone(value) {
  let serialized = JSON.stringify(value);
  return JSON.parse(serialized);
}
```

Ex: Applying generics lets us use this code for any type, and
provides TS with additional context.

```
function clone<T>(value: T): T { // This is it. T is the generic type 
                                 //   to be referred to. Could use anything
...
}
```

### Creating generic classes

JS handles Array as a generic class:
`var array: number[] = [1,2]` is the same as `var array: Array<number> = 1,2`

Ex:
```
class KVPair<TKey, TValue> {
  constructor (public key: TKey, public value: TValue) {

  }
}

// now all of these are valid and have TS info:
let pair1 = new KVPair(1, 'First');
let pair2 = new KVPair<string, Date>('Second', Date.now()); 
// ^ Warns that .now isn't a date because generic types were explicitly labeled
let pair3 = new KVPair(3, 'Third');
```

You can define generic classes that interact with a generic type now that it's been
defined:

```
class KVPairPrinter<T, U> {
  constructor(private pairs: KVPair<T, U>[]){}
  print(){
    for (let p of this.pairs) {
      console.log(`${p.key}: ${p.value}`)
    }
  }
}
```

### Applying generic constraints

```
function totalLength(x: { length: number }, y: { length: number}) {
  var total: number = x.length + y.length;
  return total;
}
```

can become this (by using generic constraint):

```
function totalLength<T extends {length: number}>(x: { length: number }, y: { length: number} {
  var total: number = x.length + y.length;
  return total;
}
```

further cleaned into:

```
interface IHaveALength {
  length: number;
}

function totalLength<T extends IHaveALength>(x: { length: number }, y: { length: number} {
  var total: number = x.length + y.length;
  return total;
}
```