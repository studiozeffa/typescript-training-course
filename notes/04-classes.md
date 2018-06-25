# Promises

- A promise represents a value or error that will occur in the future.
- In many cases, they are a better async alternative to callbacks.
- You can create a promise with `new Promise()`.
- The promise constructor is passed two functions, `resolve` and `reject`. Call these to resolve or reject the promise at any time.

``` js
const p = new Promise(resolve => {
  setTimeout(() => resolve('hello'), 200);
});

p.then(msg => console.log(msg));  // Logged after 200ms
```

<!-- break -->

# Async/await

- Make async code behave as though it is synchronous.
- `await` an async operation: calling function only continues when async operation has completed.
- Can use try/catch with async code.

``` js
const sayHello = () => new Promise(resolve => {
  setTimeout(() => resolve('hello'), 500);
});
const waitToSayHello = async function() {
  const resp = await sayHello();
  console.log(resp);  // 'hello' after 500ms
}
waitToSayHello();
```
<!-- break -->

# Classes

- Syntactic sugar for OO prototypes.
- Features: inheritance, super calls, constructors, instance methods, statics, getters/setters.

``` js
class Animal {
  speak() {
    console.log('I have no voice.');
  }
}

class Dog extends Animal {
  speak() {
    console.log('woof.');
  }
}
```

<!-- break -->

``` js
class Dog extends Animal {
  constructor(name) {
    super(name);
    this.legs = 4;
  }
  get legs() {
    return this._legs;
  }
  set legs(val) {
    this._legs = val;
    console.log(`I have ${val} legs`);
  }
  static family() {
    return 'Canis';
  }
}

const dog = new Dog('Boris');
console.log(dog.legs);      // => '4'
console.log(Dog.family());  // => 'Canis'
```

<!-- break -->

## Typing Classes

- Properties and methods in classes can be type annotated:

``` ts
class Car {
  model: string;
  currentSpeed = 0; // inferred as number
  constructor(model: string) {
    this.model = model;
  }
  drive(speed: number): void {
    this.currentSpeed = speed;
  }
}

let supercar = new Car('ferrari'); // OK
supercar = new Car();   // Compile error: model is required
supercar.drive(200);    // OK
supercar.drive('100');  // Compile error, speed must be a number
```

<!-- break -->

## Visibility modifiers

- TypeScript allows class properties and methods to be set to `public`, `private` or `protected`:
  - `public`: any calling code can access this property / call this method. This is the default visibility if no modifier is used.
  - `private`: only methods within the same class can access this property / call this method.
  - `protected`: only methods within the same class or a subclass can access this property / call this method.
- Remember that JavaScript does not currently implement visibility modifiers, so this is enforced at _compile-time_ only by TypeScript.

<!-- break -->

### Example

``` ts
class Car {
  protected model: string;
  private currentSpeed = 0; // inferred as number
  constructor(model: string) {
    this.model = model;
  }
  public drive(speed: number): void {
    this.currentSpeed = speed;
  }
}
class Ferrari extends Car {
  constructor() {
    super('ferrari');
    console.log(this.model);         // OK
    console.log(this.currentSpeed);  // Compile error
  }
}
```

<!-- break -->

### Example (cont...)

``` ts
const f = new Ferrari;
f.drive(200);                 // OK, 'public'
console.log(f.model);         // Compile error, 'protected'
console.log(f.currentSpeed);  // Compile error, 'private'
```

<!-- break -->

## Readonly modifiers

- Similar to interfaces, a class property can be defined as `readonly` to prevent it from being set anywhere other than when it as declared, or in the constructor.

``` ts
class Car {
  readonly model = '';  // OK, can be set at definition

  constructor(model: string) {
    this.model = model; // OK, can be set in constructor
  }
}

const supercar = new Car('ferrari');  // OK
supercar.model = 'porsche';           // Compile error
```

<!-- break -->

## Parameter properties

- Setting an instance property on a class from an argument passed to the constructor is a common pattern.
- TypeScript provides a shorthand way of achieving this - by annotating a constructor argument with a visibility modifier, the argument is automatically set as an instance property on the class.

``` ts
class Car {
  constructor(public model: string) {}
}

let supercar = new Car('ferrari');
console.log(supercar.model);  // => 'ferrari'
```

<!-- break -->

## Abstract classes

- TypeScript provides the `abstract` keyword to author a class which cannot be directly instantiated, only subclassed.

``` ts
abstract class Car {
  constructor(public model: string) {}
}
class Ferrari extends Car {
  constructor() {
    super('ferrari');
  }
}

const supercar = new Car('ferrari'); // Compile error
const f = new Ferrari();             // OK
```

<!-- break -->

## Classes and interfaces

- An interface in TypeScript can also be modelled as a class.

``` ts
class Response {
  code: number,
  message?: string
};
let x: Response = {
  code: 201,
  message: 'Created'
};
```

<!-- break -->

### Implementing interfaces

- A class can implement an interface. This will automatically type all properties and methods according to the implementation.

``` ts
interface IResponse {
  code: number,
  message?: string,
  refetch: () => Promise<IResponse>
};

class Response implements IResponse {
  constructor(private code: number, private message?: string) {}

  refetch() {
    return Promise.resolve(new Response(200, 'OK'));
  }
}
```

<!-- break -->

## Further Reading

- [TypeScript Handbook: Classes](https://www.typescriptlang.org/docs/handbook/classes.html)
