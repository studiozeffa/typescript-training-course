# TypeScript Introduction

- TypeScript adds new features and static types to JavaScript.
- It compiles down to JavaScript that can run in a variety of different target environments.
- It also supports definition files, allowing types to be added to existing libraries without needing to modify the libraries themselves.
- It is built and maintained by a team at Microsoft.

<!-- break -->

## Brief History

- The language was first released by Microsoft as a public beta in October 2012.
- The source code was hosted on codeplex.com (remember that?) and it's main competitors were Dart and CoffeeScript.
- It's original aim was twofold:
  - Provide static typing to JavaScript.
  - Provide features such as classes and modules that were not yet part of JavaScript.
- It was adopted quickly by many developers and v1 final was released in April 2014.

<!-- break -->

## Brief History (cont...)

- In July 2014, Microsoft joined the 21st century, and moved the source code over to GitHub.
- In March 2015, Google announced that Angular 2 would be built using TypeScript.
- September 2016 saw v2 released, with the keynote feature being the ability to prevent a variable from being set to `null`.
- v3 is expected in July 2018, which (amongst other things) brings a new type `unknown` to the language.

<!-- break -->

## `let` / `const`

- Brought in to replace `var`, due to confusing scoping rules.
- `let` allows changing after assignment, `const` is for constants.
- Both correctly scope to _blocks_, unlike `var`. For example:

``` js
var a = 'bob';
let b = 'bruce';

if (1 < 2) {
  var a = 'jim';
  let b = 'jack';
  console.log(a);  // 'jim'
  console.log(b);  // 'jack'
}

console.log(a);  // 'jim' ... ;(
console.log(b);  // 'bruce' ... :)
```

<!-- break -->

## Template Strings

- An improvement to regular single/double quoted strings.
- Features: string interpolation, newlines and more.

``` js
const str = `I am a string`;

const multiline = `I am a string...
                   on multiple lines!`

const food = 'pizza';

const like = `I like ${food}`;
```

<!-- break -->

## Typing

- Add a type to a variable, argument or function using a colon `:`.
- TypeScript has a number of built-in types, or you can define your own.

``` ts
/**
 * The TS compiler will expect arguments a and b
 * to be numbers, and the function to return a number.
 * The compiler will throw an error if this isn't satisfied.
 */
function add(a: number, b: number): number {
  return a + b;
}
```

<!-- break -->

## Built-in types

TypeScript includes the following types:

- `boolean`, `number`, `string`
- `object`
- `Array` (or `[]`)
- Tuple
- `enum`
- `any`, `never`
- `void`, `null`, `undefined`

A full reference [is available here](https://www.typescriptlang.org/docs/handbook/basic-types.html).

<!-- break -->

### `boolean`

- Used to represent a value which is `true` or `false`.

### `number`

- Used to represent a floating point number.

### `string`

- Used to represent a set of text characters.

<!-- break -->

### `Object`

- Can be used to model any non-primitive type.
- Useful for `Object.create` but not much else (you should instead define a custom type).

### `Array` (or `[]`)

- Used to represent an array of data of the same type.
- For example, `Array<number>` or `string[]`.
- `Array<type>` is known as a 'generic array type'. It's the preferred syntax as it is more obvious at a glance that the type is an array.

<!-- break -->

### Tuple

- Used to represent an array of fixed length, where each element in the array is known.
- Elements outside of the tuple definition can be one of the types defined in the tuple, but not anything else.
- For example:

``` ts
let x: [string, number] = [];
x = ["cloud", 9]; // This is OK
x = [9, "cloud"]; // Compile error, 9 is not a string
x[2] = "route";   // OK, route is a string
x[3] = 66;        // OK, 66 is a number
x[4] = false;     // Compile error, not a string or number
```

<!-- break -->

### `enum`

- Used to give a more meaningful names to a set of numeric values.
- You can directly set a variable to an enum value, or look up the enum key by value.
- Very handy for e.g. status codes.

``` ts
enum HttpStatus {
  OK = 200,
  NotFound = 404
}
let code: HttpStatus = HttpStatus.OK;
let name: string = HttpStatus[404]; // => 'NotFound'
```

<!-- break -->

### `any`

- Used as an escape hatch - tells TypeScript to ignore type-checking.
- Useful for things like big complicated server responses, untyped third party APIs or places where you don't care about type checking.

### `never`

- Represents a type that 'never occurs'.
- Primarily used for functions which never return, but instead only throw an exception. For example:

``` ts
function issueError(msg: string): never {
  throw new Error(msg);
}
```

<!-- break -->

### `void`

- The absence of a type.
- Mainly used for functions which return without a value.
- Contrast this to functions with the return type `never`:
  - `never` means that the function doesn't return at all, e.g. if it just throws an exception
  - `void` means the function does return, but without a value.

``` ts
function issueWarning(): void {
  alert('Warning Warning Warning!!!');
}
```

<!-- break -->

### `undefined` and `null`

- By default, can be assigned to variables with a different type. For example:

``` ts
let msg: string;
msg = undefined; // OK
msg = null;      // OK
```

- TypeScript v2 introduced a new compiler flag, `strictNullChecks`. When turned on, the above example would need to be modified to use a union type:

``` ts
let msg: string | null | undefined;
msg = undefined; // OK
msg = null;      // OK
```

<!-- break -->

## Type Inference

- If you initialise a variable without setting a type, TypeScript will infer the type based on the value:

``` ts
let msg = 'hello';  // msg is inferred as type string
msg = 7;            // Compiler error
```

<!-- break -->

## Modules

- Export JavaScript variables and functions, and import them in other files.

``` ts
// utils.ts
export function add(x: number, y: number): number {
  return x + y;
}
export function subtract(x: number, y: number): number {
  return x - y;
}

// app.ts
import { add, subtract } from './utils';
add(2, 3);       // => 5
subtract(3, 2);  // => 1
```

<!-- break -->

## Further Reading

- [TypeScript Handbook: Variable Declaration](https://www.typescriptlang.org/docs/handbook/variable-declarations.html)
- [TypeScript Handbook: Basic Types](https://www.typescriptlang.org/docs/handbook/basic-types.html)
- [TypeScript Handbook: Enums](https://www.typescriptlang.org/docs/handbook/enums.html)
- [TypeScript Handbook: Type Inference](https://www.typescriptlang.org/docs/handbook/type-inference.html)
- [TypeScript Handbook: Modules](https://www.typescriptlang.org/docs/handbook/modules.html)