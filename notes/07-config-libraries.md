# Configuration

- TypeScript can be configured via command line flags or a configuration file, `tsconfig.json`.
- When building an app with TypeScript, use a config file.
- A config file should resemble the following:

``` js
{
  "compilerOptions": {
    // ...sets compiler options, see below.
  },
  "include": [  // Glob pattern of files to compile.
    "src/**/*"
  ],
  "exclude": [  // Files to exclude from compilation.
    "**/*.spec.ts"
  ]
}
```

<!-- break -->

## Compiler Options

- Controls how the TypeScript compiler behaves.
- Many options: [see here](https://www.typescriptlang.org/docs/handbook/compiler-options.html) for a full reference.
- Commonly used options:

``` js
{
  "allowJs": true,  // Enables (limited) typechecking in js files.
  "checkJs": true,  // Useful when migrating a codebase to TS.
  "experimentalDecorators": true, // Enable decorator support.
  "module": "commonjs",   // Generate modules as CommonJS.
  "outFile": "path/to/app.js",  // Compiler output destination.
  "noImplicitAny": true,  // Require everything to be typed.
  "sourceMap": true,  // Output sourcemap with compiled code.
  "strictNullChecks": true  // Enforce `string | null` typing.
}
```

<!-- break -->

## Extending configuration

- Use the `extends` property to extend from another config file:

``` js
// tsconfig.json
{
  "compilerOptions": {
    "experimentalDecorators": true
  }
}
```
``` json
// tsconfig.nodecorators.json
{
  "extends": "./tsconfig",
  "compilerOptions": {
    "experimentalDecorators": false
  }
}
```

<!-- break -->

# Third party libraries

- Some third party libraries include type information for their API directly inside the module.
- In this case, all you need to do is `npm install` or `yarn add` the module and you get typing included.
- Many other libraries rely on user-contributions to a central project, [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped).
- Every type definition file housed in the DefinitelyTyped repo is published as an npm module with the name `@typed/{name}`.
- For example, to add types to lodash, simply run `yarn add -D @typed/lodash`.
- TypeScript knows that types installed under the `@typed` module namespace should be automatically used in the project.

<!-- break -->

## Further Reading

- [TypeScript Handbook: tsconfig](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)
- [TypeScript Handbook: Compiler Options](https://www.typescriptlang.org/docs/handbook/compiler-options.html)
- [TypeScript Handbook: Library Consumption](https://www.typescriptlang.org/docs/handbook/declaration-files/consumption.html)