# TypeScript

![TypeScript](https://upload.wikimedia.org/wikipedia/commons/2/29/TypeScript_Logo_%28Blue%29.svg)

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

## Enums

- TypeScript supports enums.
- Enums are used to define a collection of constants.

Example

```ts
enum Direction {
  Up,
  Down,
  Left,
  Right,
}

console.log(Direction.Up); // 0
console.log(Direction.Down); // 1
console.log(Direction.Left); // 2
console.log(Direction.Right); // 3
```

By default, the value of the first enum member is `0`. The value of the subsequent enum members are incremented by `1`.

We can also specify the value of enum members.

```ts
function getInitialRight(): number {
  return 1;
}

enum Direction {
  Up = 2,
  Down = 4,
  Left = Down + 2,
  Right = getInitialRight(),
}

console.log(Direction.Up); // 2
console.log(Direction.Down); // 4
console.log(Direction.Left); // 6
console.log(Direction.Right); // 1
```

## Type Assertions

- TypeScript supports type assertions.
- Type assertions are used to tell the compiler to trust us about the type of a variable.
- Type assertions are similar to type casting in other languages.
- Type assertions are done using the `as` keyword.
- Type assertions are used when we know more about a type than TypeScript does.

Example

```ts
let message: any = "Hello World";
let count: number = (<string>message).length;

console.log(count); // 11
```

```ts
let myImage = document.querySelector("#myImage") as HTMLImageElement;
myImage.src = "cat.jpg"; // valid
```

```ts
let myImage = <HTMLImageElement>document.querySelector("#myImage");
myImage.src = "cat.jpg"; // valid
myImage.value = "cat.jpg"; // Property 'value' does not exist on type 'HTMLImageElement'.
```

## Type Annotation with Objects

- Type annotation with objects is used to specify the type of the properties of an object.

Example

```ts
let person: {
  name: string;
  age: number;
  isStudent: boolean;
} = {
  name: "John",
  age: 25,
  isStudent: true,
};

console.log(person.name); // John
console.log(person.age); // 25
console.log(person.isStudent); // true
```

`readonly` modifier can be used to make the properties of an object readonly.

```ts
let person: {
  readonly name: string;
  readonly age: number;
  readonly isStudent: boolean;
} = {
  name: "John",
  age: 25,
  isStudent: true,
};

person.name = "Jane"; // Cannot assign to 'name' because it is a read-only property.
```

## Interface

- Interface is used to define the structure of an object.

Example

```ts
interface Person {
  name: string;
  age: number;
  isStudent: boolean;
}

let person: Person = {
  name: "John",
  age: 25,
  isStudent: true,
};

console.log(person.name); // John
console.log(person.age); // 25
console.log(person.isStudent); // true
```

To define that a property is optional, we can use the `?` modifier.

```ts
interface Person {
  name: string;
  age?: number;
  isStudent: boolean;
}

let person: Person = {
  name: "John",
  isStudent: true,
};

console.log(person.name); // John
console.log(person.age); // undefined
console.log(person.isStudent); // true
```

`readonly` modifier can be used to make the properties of an object readonly.

```ts
interface Person {
  readonly name: string;
  readonly age: number;
  readonly isStudent: boolean;
}

let person: Person = {
  name: "John",
  age: 25,
  isStudent: true,
};

person.name = "Jane"; // Cannot assign to 'name' because it is a read-only property.
```

Interface reopens and merges with the existing interface.

```ts
interface Person {
  name: string;
  age: number;
  isStudent: boolean;
}

interface Person {
  email: string;
}

let person: Person = {
  name: "John",
  age: 25,
  isStudent: true,
  email: "John@example.com",
};

console.log(person); // { name: 'John', age: 25, isStudent: true, email: 'John@example.com'}
```

Interfaces can be extended.

```ts
interface user {
  name: string;
  age: number;
}

interface Moderator extends user {
  canDelete: boolean;
}

interface Admin extends Moderator {
  canSuspend: boolean;
}

let admin: Admin = {
  name: "John",
  age: 25,
  canDelete: true,
  canSuspend: true,
};

console.log(admin); // { name: 'John', age: 25, canDelete: true, canSuspend: true }
```

One interface can extend multiple interfaces.

```ts
interface user {
  name: string;
  age: number;
}

interface Moderator extends user {
  canDelete: boolean;
}

interface HR extends user {
  canHire: boolean;
}

interface Manager extends Moderator, HR {
  canSuspend: boolean;
}

const manager: Manager = {
  name: "John",
  age: 25,
  canDelete: true,
  canHire: true,
  canSuspend: true,
};

console.log(manager); // { name: 'John', age: 25, canDelete: true, canHire: true, canSuspend: true }
```

## Classes

- TypeScript supports classes.
- Classes are used to define the blueprint for creating objects.
- Classes are used to define the structure and behavior of objects.

Example

```ts
class Person {
  name: string;
  age: number;
  isStudent: boolean;

  constructor(name: string, age: number, isStudent: boolean) {
    this.name = name;
    this.age = age;
    this.isStudent = isStudent;
  }

  getDetails(): string {
    return `Name: ${this.name}, Age: ${this.age}, Is Student: ${this.isStudent}`;
  }
}

let person = new Person("John", 25, true);

console.log(person.getDetails()); // Name: John, Age: 25, Is Student: true
```

## Class Access Modifiers

- TypeScript supports access modifiers.
- Access modifiers are used to control the access to the members of a class.
- Examples of access modifiers are `public`, `private`, `protected` and `readonly`.
  - `public` - The members of a class are public by default.
  - `private` - The members of a class are private and can be accessed only within the class.
  - `protected` - The members of a class can be accessed within the class and its subclasses.
  - `readonly` - The members of a class can be read-only.

Example

```ts
class Person {
  public name: string;
  private age: number;
  protected isStudent: boolean;
  readonly email: string;

  constructor(name: string, age: number, isStudent: boolean, email: string) {
    this.name = name;
    this.age = age;
    this.isStudent = isStudent;
    this.email = email;
  }

  getDetails(): string {
    return `Name: ${this.name}, Age: ${this.age}, Is Student: ${this.isStudent}, Email: ${this.email}`;
  }
}

let person = new Person("John", 25, true, "john@example.com");

console.log(person.name); // John
console.log(person.age); // Property 'age' is private and only accessible within class 'Person'.
```

## Getters and Setters

- TypeScript supports getters and setters.
- Getters and setters are used to get and set the values of the private members of a class.
- Getters and setters are used to validate the values the user wants to set to the private members of a class.

Example

```ts
class Person {
  private _name: string;
  private _age: number;
  private _isStudent: boolean;

  constructor(name: string, age: number, isStudent: boolean) {
    this._name = name;
    this._age = age;
    this._isStudent = isStudent;
  }

  get name(): string {
    return this._name;
  }

  set name(name: string) {
    if (name.length < 3) {
      throw new Error("Name must be at least 3 characters long");
    }
    this._name = name;
  }

  get age(): number {
    return this._age;
  }

  set age(age: number) {
    if (age < 0 || age > 100) {
      throw new Error("Age must be between 0 and 100");
    }
    this._age = age;
  }

  get isStudent(): boolean {
    return this._isStudent;
  }

  set isStudent(isStudent: boolean) {
    this._isStudent = isStudent;
  }

  getDetails(): string {
    return `Name: ${this._name}, Age: ${this._age}, Is Student: ${this._isStudent}`;
  }
}

let person = new Person("John", 25, true);
console.log(person.age); // 25
console.log(person.getDetails()); // Name: John, Age: 25, Is Student: true
person.age = -1; // Error: Age must be between 0 and 100
```

## Class Static Members

- TypeScript supports static members.
- Static members are used to define properties and methods that are accessible directly from the class without creating an instance of the class. (No need to create an object to access static members)

A famous example of static members is the `Math` class.

```ts
class Mathematics {
  static PI = 3.14;

  static add(x: number, y: number): number {
    return x + y;
  }
}

console.log(Mathematics.PI); // 3.14
```

Another example is to count the number of instances of a class.

```ts
class Person {
  static count = 0;

  constructor() {
    Person.count++;
  }
}

let person1 = new Person();
let person2 = new Person();
let person3 = new Person();

console.log(Person.count); // 3
```

## Class Extends Inheritance

- TypeScript supports inheritance.
- Inheritance is used as a contract between a parent class and its child classes.

Example

```ts
interface user {
  name: string;
  age: number;
  sayHello(): string;
}

class Person implements user {
  name: string;
  age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }

  getDetails(): string {
    return `Name: ${this.name}, Age: ${this.age}`;
  }

  sayHello(): string {
    return `Hello, my name is ${this.name}`;
  } // if missed : Class 'Person' incorrectly implements interface 'user'. Property 'sayHello' is missing in type 'Person' but required in type 'user'
}

const john = new Person("John", 25);

console.log(john.getDetails()); // Name: John, Age: 25
```

## Abstract Classes

- TypeScript supports abstract classes.
- Abstract classes are used to define the structure of a class.
- Abstract classes cannot be instantiated.

Example

```ts
abstract class Person {
  name: string;
  age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }

  abstract getDetails(): string;
}

class Student extends Person {
  isStudent: boolean;

  constructor(name: string, age: number, isStudent: boolean) {
    super(name, age);
    this.isStudent = isStudent;
  }

  getDetails(): string {
    return `Name: ${this.name}, Age: ${this.age}, Is Student: ${this.isStudent}`;
  }
}

let student = new Student("John", 25, true);

let person = new Person("John", 25); // Error: Cannot create an instance of an abstract class.

console.log(student.getDetails()); // Name: John, Age: 25, Is Student: true
```

`abstract` keyword is used to define an abstract class. It is also used to define abstract methods. Abstract methods are methods that are defined in the abstract class but not implemented. Abstract methods must be implemented in the child classes.

`super` keyword is used to call the constructor of the parent class.

## Polymorphism and Method Overriding

- TypeScript supports polymorphism and method overriding.
- Polymorphism is used to define methods in the child classes that have the same name as the methods in the parent class.
- Polymorphism makes different classes have the same method name but different implementations based on the class needs.
- Method overriding is used to override the implementation of the methods in the parent class in the child classes.

Example

```ts
abstract class Person {
  name: string;
  age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }

  sayHello(): void {
    console.log(`Hello, I am person`);
  }

  abstract getDetails(): string;
}

class Student extends Person {
  isStudent: boolean;

  constructor(name: string, age: number, isStudent: boolean) {
    super(name, age);
    this.isStudent = isStudent;
  }

  getDetails(): string {
    return `Name: ${this.name}, Age: ${this.age}, Is Student: ${this.isStudent}`;
  }

  sayHello(): void {
    super.sayHello();
    console.log(`I am also student`);
  }
}

class Teacher extends Person {
  isTeacher: boolean;

  constructor(name: string, age: number, isTeacher: boolean) {
    super(name, age);
    this.isTeacher = isTeacher;
  }

  getDetails(): string {
    return `Name: ${this.name}, Age: ${this.age}, Is Teacher: ${this.isTeacher}`;
  }

  sayHello(): void {
    super.sayHello();
    console.log(`I am also teacher`);
  }
}

let student = new Student("John", 25, true);
let teacher = new Teacher("Jane", 35, true);

console.log(student.sayHello()); // Hello, I am person \n I am also student
console.log(teacher.sayHello()); // Hello, I am person \n I am also teacher
```

## Generics

- TypeScript supports generics.
- Generics are used to define a class or a method with a type parameter.

Example

```ts
function echo<T>(arg: T): T {
  return arg;
}

console.log(echo<string>("Hello World")); // Hello World
console.log(echo<number>(1)); // 1
console.log(echo<boolean>(true)); // true
```

Generics can be used with multiple type parameters.

```ts
const echo = <T, U>(arg1: T, arg2: U): string => {
  return `${arg1} - ${arg2}`;
};

console.log(echo<string, number>("Hello World", 1)); // Hello World - 1
```

## Generic Classes

- TypeScript supports generic classes.
- Generic classes are used to define classes with type parameters.

Example

```ts
class Person<T> {
  name: T;

  constructor(name: T) {
    this.name = name;
  }

  getName(): T {
    return this.name;
  }
}

let person1 = new Person<string>("John");
let person2 = new Person<number>(25);

console.log(person1.getName()); // John
console.log(person2.getName()); // 25
```

Note that telling the type of the generic class is optional. TypeScript can infer the type of the generic class from the type of the argument passed to the constructor.

```ts
let person1 = new Person("John"); // valid
let person2 = new Person(25); // valid
```

A default type can be defined for the generic class.

```ts
class Person<T = string> { ... }
```
