# TypeScript

## What is TypeScript?

- TypeScript is a superset of JavaScript that adds optional static typing and class-based object-oriented programming to the language.
- Developed and maintained by Microsoft.

## Why TypeScript?

- TypeScript is a superset of JavaScript, so any valid JavaScript code is also valid TypeScript code.
- Static typing helps to catch bugs at compile time.
- Object-oriented programming helps to write more modular and reusable code.

## How TypeScript works?

- TypeScript code is compiled into JavaScript code. (Browser doesn't understand TypeScript code.)

## Installing TypeScript

- Install TypeScript globally using npm:

```bash
$ npm install -g typescript
```

- Check the version of TypeScript:

```bash
$ tsc --version
```

## Your first TypeScript code

- Create a file named `hello.ts`:

```ts
function sayHello(name: string) {
  console.log(`Hello ${name}`);
}

sayHello("John");
```

- Compile the TypeScript code:

```bash
$ tsc hello.ts
```

Note that a new file named `hello.js` is created.

- Run the compiled JavaScript code:

```bash
$ node hello.js
```

Running these commands can be tedious. So, we can use the `--watch` flag to automatically compile the TypeScript code when it changes:

```bash
$ tsc hello.ts --watch
```

Note : To show the help message, run the following command:

```bash
$ tsc --help
```

## Configuring TypeScript

To configure TypeScript, we init the `tsconfig.json` file using the following command:

```bash
$ tsc --init
```

This command creates a `tsconfig.json` file with default settings.

Open the `tsconfig.json` file and take a look at the settings.

One use case of the `tsconfig.json` file is to specify the root directory and the output directory. For example, we can specify the root directory as `src` and the output directory as `dist`:

```json
{
  "rootDir": "./src" /* Specify the root folder within your source files. */,
  "outDir": "./dist" /* Specify an output folder for all emitted files. */
}
```

Typing the command `tsc -w` will compile the TypeScript code in the `src` folder and put the compiled JavaScript code in the `dist` folder. This helps to keep the source code and the compiled code separate.

There are many other setting that can be uncommented and configured in the `tsconfig.json` file, like the target, module, sourceMap, removeComments, etc.

## Statically typed variables vs dynamically typed variables

| Aspect        | Statically typed variables                                     | Dynamically typed variables                                         |
| ------------- | -------------------------------------------------------------- | ------------------------------------------------------------------- |
| Type Checking | The type of a variable is known at compile time.               | The type of a variable is known at run time.                        |
| Type Safety   | The type of a variable cannot be changed after it is declared. | The type of a variable can be changed after it is declared.         |
| Compilation   | Requires a separate compilation step                           | Usually interpreted or uses just-in-time (JIT) compilation          |
| Performance   | Usually interpreted or uses just-in-time (JIT) compilation     | Slightly slower due to runtime type checks and lack of optimization |
| Flexibility   | Less flexible                                                  | More flexible                                                       |
| Examples      | Go, Java, C, C++, C#                                           | JavaScript, Python, Ruby, PHP                                       |

## Type Annotations

- TypeScript uses type annotations to specify the type of a variable, parameter, or return value.
- Type annotations are _optional_. If we don't specify the type of a variable, TypeScript will infer the type based on the value assigned to the variable.

Examples:

```ts
let a: number = 5;
let b: string = "Hello";
let c: boolean = true;
let d: any = 5;
let e: any = "Hello";
let f: any = true;
let g: number[] = [1, 2, 3];
let h: string[] = ["Hello", "World"];
let i: boolean[] = [true, false];
let j: any[] = [1, "Hello", true];
```

TypeScript also supports assigning multiple types to a variable:

```ts
let k: number | string | boolean;
let l: number[] | string[] | boolean[];
```

To use type annotations with multi dimensional arrays, we can use the following syntax:

```ts
let m: number[][] = [
  [1, 2],
  [3, 4],
];
let n: string[][] = [["Hello", "World"], ["TypeScript"]];
let o: boolean[][] = [
  [true, false],
  [false, true],
];
```

## Functions type annotations

- TypeScript uses type annotations to specify the type of parameters and return value of a function.
- Type annotations are _optional_. If we don't specify the type of a parameter or return value, TypeScript will infer the type based on the value passed to the function and the return value of the function.

Examples:

```ts
function sum(a: number, b: number): number {
  return a + b;
}

function sayHello(name: string): void {
  console.log(`Hello ${name}`);
}

function average(...numbers: number[]): number {
  let sum = 0;
  for (let num of numbers) {
    sum += num;
  }
  return sum / numbers.length;
}

console.log(sum(1, 2)); // 3
console.log(sum(1, "2")); // Argument of type 'string' is not assignable to parameter of type 'number'.
```

## Function Optional and Default Parameters

- TypeScript supports optional and default parameters in functions.
- Optional parameters are specified using the `?` symbol.
- Default parameters are specified using the `=` symbol.

Examples:

```ts
function sum(a: number, b: number, c?: number): number {
  if (c) {
    return a + b + c;
  }
  return a + b;
}

function sayHello(name: string = "World"): void {
  console.log(`Hello ${name}`);
}

console.log(sum(1, 2)); // 3
console.log(sum(1, 2, 3)); // 6
console.log(sayHello()); // Hello World
console.log(sayHello("John")); // Hello John
```

Note that optional parameters must be specified after the required parameters. Not before.

## Function Rest Parameters

- TypeScript supports rest parameters in functions.
- Rest parameters are specified using the `...` symbol.

Examples:

```ts
function sum(...numbers: number[]): number {
  let sum = 0;
  for (let num of numbers) {
    sum += num;
  }
  return sum;
}

console.log(sum(10, 20, 100)); // 130
```

## Type Alias

- TypeScript supports type alias.
- Type alias is used to give a type a name.

Example

```ts
type StringOrNumber = string | number;

// Not a good practice, it bypasses TypeScript's type checking
function sum(a: StringOrNumber, b: StringOrNumber): StringOrNumber {
  return (a as any) + (b as any);
}

console.log(sum(1, 2)); // 3
console.log(sum("Hello", "World")); // HelloWorld
console.log(sum(1, "Hello")); // 1Hello
```

## Type Literal

- TypeScript supports type literal.
- Type literal is used to specify the exact value a variable can have.

Example

```ts
type Direction = "left" | "right" | "top" | "bottom";

function move(direction: Direction): void {
  console.log(`Moving ${direction}`);
}

move("left"); // Moving left
move("right"); // Moving right
move("top"); // Moving top
move("bottom"); // Moving bottom
move("left1"); // Argument of type '"left1"' is not assignable to parameter of type 'Direction'.
```

## Tubles

- TypeScript supports tubles.
- Tubles are used to store multiple values of different types in a single variable.

Example

```ts
let person: [string, number, boolean] = ["John", 25, true];

console.log(person[0]); // John
console.log(person[1]); // 25
console.log(person[2]); // true
person[0] = "Jane"; // valid
person.push("content"); // valid
```

If you want to restrict the number of elements in a tuble, you can use the `readonly` modifier.

```ts
let person: readonly [string, number, boolean] = ["John", 25, true];

person[0] = "Jane"; // Cannot assign to '0' because it is a read-only property.
person = ["Sara", 20, false]; // valid
person.push("Jane"); // Property 'push' does not exist on type 'readonly [string, number, boolean]'.
```

## Void and Never

- TypeScript supports `void` and `never` types.
- `void` is used to specify the return type of functions that do not return a value.
- `never` is used to specify the return type of functions that never return. (e.g. functions that always throw an error, or an infinite loop)

Example

```ts
function sayHello(name: string): void {
  console.log(`Hello ${name}`);
}

function throwError(errorMsg: string): never {
  throw new Error(errorMsg);
}

function infiniteLoop(): never {
  while (true) {}
}
```
