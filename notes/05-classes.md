# Classes

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
  - `proptected`: only methods within the same class or a subclass can access this property / call this method.
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

## Abstract classes

- TypeScript provides the `abstract` keyword to author a class which cannot be directly instantiated, only subclassed.

``` ts
abstract class Car {
  model: string;
  constructor(model: string) {
    this.model = model;
  }
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
  refetch: () => Promise<Response>
};

class Response implements IResponse {
  constructor(code: number, message?: string) {
    this.code = code;
    this.message = message;
  }
  refetch() {
    return Promise.resolve(new Response(200, 'OK'));
  }
}
```

<!-- break -->

## Further Reading

- [TypeScript Handbook: Classes](https://www.typescriptlang.org/docs/handbook/classes.html)
