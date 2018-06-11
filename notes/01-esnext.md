# ES.Next

- Between 1999 and 2015, there was only a single update to the JavaScript language: **ES5**, which was releaed in 2009.
- The body responsible for adding new features (known as **TC39**) decided that this was hampering the language, and in 2015 committed to yearly updates.
- The 2015 release (ES2015) contained many new features, with a steady rollout of further enhancements in 2016 and 2017.
- Collectively, the progression of JavaScript is known as **ES.Next**.
- New features are proposed and considered by TC39 before they are released.
- TypeScript has implemented many of these forthcoming features and can compile them down to ES5 or ES2015/16/17 compatible code, depending on your target browser support.

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

## Classes

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

## Arrow Functions

- Function definition which preserves the value of `this` (known as _lexical scoping_).

``` js
class Brewery {
  constructor(beers) {
    this.beers = beers;
    this.beers.forEach(beer => {
      this.bottle(beer);    // Value of `this` is preserved.
    });                     // If we used `function`, `this`
  }                         // would be undefined inside.

  bottle(beer) {
    console.log(`bottling ${beer}...`);
  }
}

const bathales = new Brewery(['Gem', 'Wild Hare', 'Barnsey']);
```

<!-- break -->

## Promises

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

## Async/await

- Make async code behave as though it is synchronous.
- `await` an async operation: calling function only continues when async operation has completed.
- Can use try/catch with async code.

``` js
const sayHello = () => new Promise(resolve => {
  setTimeout(() => resolve('hello'), 200);
});

const waitToSayHello = async function() {
  await sayHello();
  return 'done';
}

const response = waitToSayHello();  // Sets `response` after 200ms
console.log(response); // => `done`
```

<!-- break -->

## Modules

- Export JavaScript variables and functions, and import them in other files.

``` js
// utils.js
export function add(x, y) {
  return x + y;
}
export function subtract(x, y) {
  return x - y;
}

// app.js
import { add, subtract } from './utils';
add(2, 3);       // => 5
subtract(3, 2);  // => 1
```

<!-- break -->

## Further Reading

- [es6features](https://github.com/lukehoban/es6features#readme)
- [Future JavaScript: Now](https://basarat.gitbooks.io/typescript/docs/future-javascript.html)
- [Exploring ES2016 and ES2017](http://exploringjs.com/es2016-es2017.html)
- [Exploring ES2018 and ES2019](http://exploringjs.com/es2018-es2019/)