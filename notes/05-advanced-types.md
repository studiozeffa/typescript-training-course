## Intersection Types

- An intersection type merges multiple types into one.
- It is most useful when composing parts of a system together:

``` ts
interface Hero        { name: string };
interface Flyer       { fly: () => void }
interface WallClimber { climbWall: () => void }

const superman: Hero & Flyer;
superman.name = 'Superman'; // OK
superman.fly();             // OK
superman.climbWall();       // Compile error

const batman: Hero & WallClimber;
batman.name = 'Batman'; // OK
batman.climbWall();     // OK
batman.fly();           // Compile error
```

<!-- break -->

## Union Types

- A union type is the `OR` of the typing world - use it to declare that a variable could be one of a set of types.
- Declare a union type by delimiting types with a vertical bar `|`.
- For example:

``` ts
let msg: string | null;
msg = 'hello';  // OK
msg = null;     // OK
msg = 3;        // Compile error
```

- Union types are also useful if you have a function that can accept multiple forms of an argument, or different combinations of arguments (i.e. an overloaded function).

<!-- break -->

## Literal types

- Literal types are useful when you want to assert that a variable is one of a set of values.

``` ts
let allowedCode: 200 | 400 | 404;
allowedCode = 200;  // OK
allowedCode = 300;  // Compile error

let allowedMsg = 'OK' | 'Bad Request' | 'Not Found';
allowedMsg = 'Bad Request';   // OK
allowedMsg = 'Server Error';  // Compile error
```

<!-- break -->

## `type` keyword

- You can alias intersection, union or literal types with `type`:

``` ts
type Superman = Hero & Flyer;
let superman: Superman;

type Message = string | null;
let msg: Message;

type AllowedCode = 200 | 400 | 404;
let allowedCode: AllowedCode;
```

<!-- break -->

## Discriminated Unions

- By setting a variable to be the same name & type on a set of interfaces, TypeScript will automatically cast to the correct type where necessary:

``` ts
interface Sandwich {
  type: 'sandwich',
  fillings: Array<string>
};
interface Curry {
  type: 'curry',
  name: string,
  spice: 'mild' | 'medium' | 'hot'
}

type Order = Sandwich | Curry;
```

<!-- break -->

## Discriminated Unions (cont...)

- We can now set up a function which will switch on the `type`, and according to the value, TypeScript will infer the correct type according to the `Order` discriminated union.

``` ts
function takeOrder(order: Order): string {
  switch(order.type) {
    case 'sandwich':
      // order is now inferred as type Sandwich
      return order.fillings.join(', ');
    case 'curry':
      // order is now inferred as type Curry
      return `${order.name}: ${order.spice}`;
  }
}
```

<!-- break -->

## Index types

- Useful when referring to objects by their key, e.g. in a 'pluck' operation.

``` ts
interface Response {
  code: number,
  message?: string,
};
type ResponseProps = keyof Response;  // 'code' | 'message'

let response: Response = { code: 200, message: 'OK' };
function pluck(r: Response, keys: Array<ResponseProps>) {
 // Perform pluck
}

pluck(response, ['code']);  // OK
pluck(response, ['code', 'data']); // Compile error, no 'data' key
```

<!-- break -->

## Further Reading

- [TypeScript Handbook: Advanced Types](https://www.typescriptlang.org/docs/handbook/advanced-types.html)
