# Generics

- Allows us to create a structure with a generic type, depending on what is defined.
- Uses angled bracket notation, with the convention `<T>`.
- For example, a function's return type can be inferred depending on what is passed in:

``` ts
function identity<T>(arg: T): T {
  return arg;
}

let a = identity('a');  // a is a string
let t = identity(true); // t is a boolean
```

<!-- break -->

## Built-in generics

- Two generics you will often come across are `Array` and `Promise`:

``` ts
let keys: Array<string>;        // An array of strings
keys = ['one', 'two', 'three']; // OK
keys.push(4);                   // Compile error

let resp: Promise<boolean>; // A promise resolved with a boolean
resp = Promise.resolve(true);     // OK
resp = Promise.resolve('hello');  // Compile error
```

<!-- break -->

## Generic classes

- A class can also be annotated with a generic parameter.
- This allows any property or method in the class to use the passed generic, ensuring type safety throughout the entire class.
- For example:

``` ts
class Stack<T> {
  // T is used throughout to ensure that
  // items in the stack are all of the same type.
  private stack: Array<T> = [];
  push(item: T) {
    this.stack.push(item);
  }
  pop(): T {
    return this.stack.pop();
  }
}
```

<!-- break -->

## Generic classes (cont...)

- Now, when using a Stack:

``` ts
const myStack = new Stack<number>();

myStack.push(1);      // OK
myStack.push('one');  // Compile error

const item = myStack.pop();   // => 1, item type is number
```

<!-- break -->

## Generic constraints

- Use inheritance to set a generic type that must have one or more typed properties.
- For example:

``` ts
interface Countable {
  length: number;
}

function count<T extends Countable>(arg: T): number {
  return arg.length;  // T will always have `.length`
}

count(1);       // => 1
count('one');   // => 3
count([0,0,0]); // => 3
count(true);    // Compiler error
```

<!-- break -->

# Decorators

- A function that will be applied to classes via an annotation.
- Primarily used to add behaviour to a class.
- Currently a stage-2 proposal for JavaScript.
- Experimental - could change! Although unlikely to drastically do so, given that they are a core part of authoring an Angular 2+ app.
- There are 4 types of decorator:
  - class
  - method
  - parameter
  - property
- We'll only discuss the class & method decorators here, see the [official docs](https://www.typescriptlang.org/docs/handbook/decorators.html) for a full breakdown of all decorator types.

<!-- break -->

## Class decorator

- Annotates a class definition.
- Decorator is passed the class constructor as its only argument.
- Decorator function is called once for the lifetime of the program, when the class is defined.

``` ts
function logClassName(constructor: Function) {
  console.log(constructor.name);
}

@logClassName
class Car {}  // results in console log 'Car'
```

<!-- break -->

## Method decorator

- Annotates a class method definition.
- Decorator is passed three arguments:
  - `target`: the class prototype.
  - `name`: the method name.
  - `descriptor`: the method [descriptor](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty).
- Decorator function is called once for the lifetime of the program, when the class is defined.

``` ts
function logMethodName(target: any, name: string) {
  console.log(name);
}
class Car {
  @logMethodName
  drive() {}      // results in console log 'drive'
}
```

<!-- break -->

## Decorator factories

- Create a function which returns a decorator function.
- When annotating a class, call the annotation - this will return the decorator to apply to the class.

``` ts
function logClassName(prefix: string) {
  return function (constructor: Function) {
    console.log(`${prefix}: ${constructor.name}`);
  }
}

@logClassName('INFO')
class Car {}  // results in console log 'INFO: Car'

```

<!-- break -->

## Further Reading

- [TypeScript Handbook: Generics](https://www.typescriptlang.org/docs/handbook/generics.html)
- [TypeScript Handbook: Decorators](https://www.typescriptlang.org/docs/handbook/decorators.html)
