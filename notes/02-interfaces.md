# Object literals

- Object literal properties can also be typed:

``` ts
let x: { code: number, message: string };

x = {
  code: 201,
  message: 'Created'
};  // OK

x = {
  code: '404',
  message: 'Not Found'
} // Error: code is a string

x = {
  code: 404
} // Error: message is missing
```

<!-- break -->

## Optional properties

- Object literal properties can be marked as optional with the `?` character.
- For example, if the message is an optional part of the object, the type could be rewritten as follows

``` ts
let x: { code: number, message?: string };

x = {
  code: 404
} // OK, since message is optional (was defined with ?)
```

<!-- break -->

## Read-only properties

- Object literal properties can be marked as `readonly`.
- This will prevent them from being changed after the object was first created.
- For example:

``` ts
let x: { readonly code: number, message?: string };

x = {
  code: 404
}; // OK
x.code = 200; // Compiler error since property is readonly
```

- Think of `readonly` as the `const` for object properties.

<!-- break -->

## Enhanced Objects

- If the property name and variable are the same,
objects can now be created using shorthand notation.

``` js
const data = 'some data';

const obj = { data };   // Equivalent to `{ data: data }`

console.log(obj.data);  // 'some data'
```

<!-- break -->

## Destructuring

- Properties can be extracted from objects and assigned to regular variables via a technique known as destructuring.

``` js
const obj = {
  firstname: 'John',
  lastname: 'Smith',
  age: 66
};

const { firstname, lastname } = obj;

console.log(firstname); // => 'John'
console.log(lastname);  // => 'Smith'
```

<!-- break -->

## Spread operator

- Spreads the elements of an array or object.

``` js
const list1 = ['apples', 'bananas'];
const list2 = ['cereal', 'dog food'];

const combo = [...list1, ...list2];
console.log(combo); // => [ apples, bananas, cereal, dog food]
```

``` js
const defaults = { fontSize: 12, darkMode: true };
const custom = { fontSize: 16 };

const config = {
  ...defaults,
  ...custom   // Overwrites any same-named from `defaults`
};
// => { fontSize: 16, darkMode: true }
```

<!-- break -->

# Arrow Functions

- Function definition which preserves the value of `this` (known as _lexical scoping_).

``` js
const bathales = {
  beers: ['Gem', 'Wild Hare', 'Barnsey'],
  bottle: function(beer) {
    console.log(`bottling ${beer}...`);
  },
  bottleAll: function() {
    this.beers.forEach(beer => {
      this.bottle(beer);    // Value of `this` is preserved.
    });
  }
}

bathales.bottleAll();
```

<!-- break -->

# Interfaces

- To reuse an object literal type, we can create an `interface`:

``` ts
interface Response {
  code: number,
  message?: string
};

let x: Response = {
  code: 201,
  message: 'Created'
};
```

<!-- break -->

## Excess properties

- Trying to set a property on an object that doesn't have an associated type definition will cause a compiler error.
- For example:

``` ts
interface Response {
  code: number,
  message?: string
};

let x: Response = {
  code: 201,
  message: 'Created',
  data: 'Here is your lovely data'
};  // Compiler error, data is not defined in the Response
```

<!-- break -->

## Index signatures

- You can get around excess property checking by defining a so-called 'index signature' to the interface.

``` ts
interface Response {
  code: number,
  message?: string,
  [key: string]: any;
};

let x: Response = {
  code: 201,
  message: 'Created',
  data: 'Here is your lovely data'
};  // OK, index signature allows any other data to be added
```

- Since `data` is not part of the type signature, it won't be available to code completion tools in your IDE.

<!-- break -->

## Call signatures

- Object literal properties can also be functions.
- To define a function, we use a 'call signature'. For example:

``` ts
interface Response {
  code: number,
  message?: string,
  refetch: () => Promise<Response>
}

let x: Response = {
  code: 201,
  message: 'Created',
  refetch: () => fetch('http://data.com/api')
};
```

- In the above example, `refetch` is a function which returns a Promise.

<!-- break -->

## Function types

- Interfaces can also be used to create a standalone function type, which can be applied to a regular variable:

``` ts
interface Fetcher {
  (url: string) => Promise<Response>
};

const fetcher: Fetcher = (url) => fetch(url);
```

<!-- break -->

## Function parameters

- Function parameters can be defined as optional with `?`:

``` ts
const timestamper = (date?: Date) =>
  console.log(date || new Date());

timestamper();              // Prints current date
timestamper(new Date(0));   // Prints date at epoch
```

- Optional parameters can't come before required parameters:

``` ts
// Compiler error, required parameter comes after optional one
const timestamper = (date?: Date, message: string) =>
  console.log(`${date || new Date()}: ${message}`);
```

<!-- break -->

### Default parameters

- TypeScript offers the same default parameter support as was introduced in ES6:

``` ts
const timestamper = (date = new Date()) =>
  console.log(date);
```

- Using a default parameter will allow TypeScript to infer that the argument should be of the same type as the default it is set to.
- So, in the example above, the `date` parameter will have a type of `Date` inferred to it.

<!-- break -->

### Rest parameters

- Rest parameters can also be typed with TypeScript:

``` ts
const writeLogMessage =
  (message: string, ...otherData: Array<string>) =>
    console.log(message, otherData.join('\n'));
```

<!-- break -->

## Weakly typed functions

- If you don't need or know the arguments or return type of a function, you can use the `Function` type.
- This will tell TypeScript that the variable is callable, but nothing about what to call it with, or what it returns.

``` ts
const logger: Function;
logger = (msg: string) => console.log(msg);   // OK
logger = 2;   // Compile error
```

<!-- break -->

## Further Reading

- [TypeScript Handbook: Interfaces](https://www.typescriptlang.org/docs/handbook/interfaces.html)
- [TypeScript Handbook: Functions](https://www.typescriptlang.org/docs/handbook/functions.html)