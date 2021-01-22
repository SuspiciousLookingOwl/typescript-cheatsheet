# Typescript Cheatsheet

My Typescript cheatsheet, which contains some frequently used basic stuff and stuff that I always forget.

If you spot a mistake or want to add more cheatsheet, feel free to [open an issue](https://github.com/SuspiciousLookingOwl/typescript-cheatsheet/issues/new)

- [Typescript Cheatsheet](#typescript-cheatsheet)
  - [Basic Type](#basic-type)
    - [Explicit and Implicit type](#explicit-and-implicit-type)
    - [Common Types](#common-types)
  - [Union](#union)
  - [Array](#array)
  - [Interface, Utility, and Object](#interface-utility-and-object)
    - [Simple Interface](#simple-interface)
    - [Partial](#partial)
  - [Type guard](#type-guard)
  - [Function](#function)
    - [Parameter and Return Type](#parameter-and-return-type)
    - [Optional Parameter](#optional-parameter)
    - [Overloading (with conditional return type)](#overloading-with-conditional-return-type)
  - [Class](#class)
  - [Enums](#enums)
  - [Generic Type](#generic-type)
    - [Basic](#basic)
    - [Conditional Return Type](#conditional-return-type)
    - [Conditional EventEmitter Callback Type](#conditional-eventemitter-callback-type)
    - [Object value as Union Type](#object-value-as-union-type)

## Basic Type

### Explicit and Implicit type

```ts
// Explicit
const str: string = "foo";
const str: string = 1; // Error: Type 'number' is not assignable to type 'string'
```

```ts
// Implicit
const str = "foo";
const str = 1; // no error, `str`  type will become number
```

### Common Types

Read more:

- https://www.typescriptlang.org/docs/handbook/basic-types.html

```ts
const bool: boolean = false;
const num: number = 1;
const str: string = "foo";

let any: any = "string";
any = 1;
```

```ts
// type can also be a value
let str: "Some string" = "Some string";
str = "other string"; // Error: Type '"other string"' is not assignable to type '"Some string"'
```

## Union

Read more:

- https://www.typescriptlang.org/docs/handbook/unions-and-intersections.html#union-types

```ts
// Union type, in this example strOrNum can be string or number
let strOrNum: string | number = "Foo";
strOrNum = 1; // can be assigned  as number
```

```ts
// Type also can be literal
let num: 1 | 2 | 3 = 1; // num can only be 1, 2, or 3
num = 2;
num = 4; // Error: Type '4' is not assignable to type '1 | 2 | 3'
```

## Array

```ts
// Array type, in this example strings is array of string
const string: string[] = ["Foo", "Bar"];
```

```ts
// Combining union and array is also possible!
// In this example, `x` is array of string or number
const x: (string | number)[] = ["Foo", "Bar", 1, 2, 3];
// Adding other type to the array resulting in an error
x.push(true); // Error: Argument of type 'boolean' is not assignable to parameter of type 'string | number'
```

## Interface, Utility, and Object

### Simple Interface

Read more:

- https://www.typescriptlang.org/docs/handbook/interfaces.html

```ts
interface Foo {
  x: number;
  y: string;
  readonly z?: boolean;
  opt?: string; // optional key
}

let foo: Foo = {
  x: 1,
  y: "string",
  z: false,
};
foo.z = true; // Error: Cannot assign to 'z' because it is a read-only property.

// Error: Property 'y' is missing in type '{ x: number; }' but required in type 'Foo'
const foo2: Foo = {
  x: 1,
};

const foo3: Foo = {
  x: 1,
  y: "string",
  opt: "Optional key",
};
```

### Partial

```ts
interface Foo {
  x: number;
  y: string;
  z: string;
}

// all key in `Foo` become optional
const foo: Partial<Foo> = {};

foo.x = 1;
```

## Type guard

Read more:

- https://www.typescriptlang.org/docs/handbook/advanced-types.html

```ts
let foo: number | string = "string";

// `foo` is still number | string,
// which means it's possible for `foo` to be a string,
// therefore doing an arithmetic operand will gives you an error
foo++; // Error: An arithmetic operand must be of type 'any', 'number', 'bigint' or an enum type.

// Type guarding foo
if (typeof foo === "string") {
  foo; // is string
} else {
  foo; // is number
  foo++; // no more error
}
```

## Function

Read more:

- https://www.typescriptlang.org/docs/handbook/functions.html

### Parameter and Return Type

```ts
// first & second parameter is number, with number as the return type
function sum(x: number, y: number): number {
  return x + y;
}

// async function
async function asyncFunc(): Promise<string> {
  return await somethingAsync();
}
```

### Optional Parameter

```ts
// `x` is number and required
// `y` is string and optional with "bar" as the default value
// `z` is boolean and optional
function foo(x: number, y = "bar", z?: boolean) {}
```

### Overloading (with conditional return type)

Read more:

- https://www.typescriptlang.org/docs/handbook/functions.html#overloads

```ts
/**
 * if number is passed as the parameter, it will return a number
 * if string is passed as the parameter, it will return a string
 */
function foo(x: number): number;
function foo(x: string): string;
function foo(x: number | string): number | string {
  if (typeof x === "string") return `String is: ${x}`;
  else return x + 5;
}

foo("Bar"); // "String is: Bar"
foo(5); // 10
```

## Class

Read more:

- https://www.typescriptlang.org/docs/handbook/classes.html

```ts
class Foo {
  private privateVar: string;
  protected protectedVar: number;
  public publicVar: boolean;
  alsoPublicVar: string[];

  // Means `example` will be assigned later, not when declaring the class
  example!: string;

  // Means `otherExample` is an optional attribute
  otherExample?: string;

  constructor(x: string) {
    this.privateVar = x;
    this.protectedVar = 5;
    this.publicVar = true;
  }

  get exampleGetter(): string {
    this.otherExample;
    return `privateVar is : ${this.privateVar}`;
  }

  exampleMethod(x: number, y: number): number {
    return x + y;
  }
}

const foo = new Foo("foo");
foo.exampleGetter; // "privateVar is : foo"
foo.exampleMethod(5, 10); // 15
```

## Enums

Read more:

- https://www.typescriptlang.org/docs/handbook/enums.html

```ts
enum Options {
  A, // not assigned, therefore it's 0
  B, // not assigned, therefore it's 1
  C = "C",
  D = "D",
}

const opt: Options = Options.A; // 0
Options.B; // 1
Options.C; // "C"
Options.D; // "D"
```

## Generic Type

Read more:

- https://www.typescriptlang.org/docs/handbook/generics.html

### Basic

```ts
function foo<T>(x: T): T {
  return x;
}

foo(1); // is 1;
foo("str"); // is "str"

foo<number>(1); // is number
foo<string>("str"); // is string

foo<number>("str"); // Error: Argument of type 'string' is not assignable to parameter of type 'number'
```

### Conditional Return Type

```ts
type FooReturnType<T> = T extends string ? string : number;

function foo<T extends string | number>(x: T): FooReturnType<T>;
function foo(x: string | number) {
  if (typeof x === "string") {
    return "It's a string!";
  } else {
    return x + 5;
  }
}

foo("bar"); // is string
foo(1); // is number
```

```ts
// Also can do the same with object parameter
type FooReturnType<T> = T extends { type: "string" } ? string : number;

interface Options {
  type: "string" | "number";
  other?: string | number;
}

function foo<T extends Options>(x: T): FooReturnType<T>;
function foo(x: Options) {
  if (x.type === "string") {
    return "It's a string!";
  } else {
    return 5;
  }
}

foo({ type: "string" }); // is string
foo({ type: "number" }); // is number
```

### Conditional EventEmitter Callback Type

```ts
interface ClientEvents {
  ready: (status: number) => void;
  data: (data: string) => void;
}

declare interface Foo {
  on<T extends keyof ClientEvents>(event: T, listener: ClientEvents[T]): this;
  emit<T extends keyof ClientEvents>(
    event: T,
    ...args: Parameters<ClientEvents[T]>
  ): boolean;
}

class Foo extends EventEmitter {
  foo() {
    this.emit("data", 1); // Error: Argument of type 'number' is not assignable to parameter of type 'string'
    this.emit("data", "Some string");
  }
}

const foo = new Foo();

foo.on("ready", (status) => {
  status; // is number
});

// Support for intellisense on event name and callback parameter
foo.on("data", (data) => {
  data; // is string
});
```

### Object value as Union Type

```ts
type ValueOf<T> = T[keyof T];

const value = {
  a: 1,
  b: 2,
  c: 3,
} as const;

let x: ValueOf<typeof value> = 1; // x can only be 1, 2, or 3
x = 4; // Error: Type '4' is not assignable to type 'ValueOf<{ readonly a: 1; readonly b: 2; readonly c: 3; }>'
```
