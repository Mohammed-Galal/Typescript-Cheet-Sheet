This is a refined and comprehensive version of your TypeScript Cheat Sheet. I have expanded the missing primitives, added critical utility types, introduced type narrowing/guards, and included advanced object-oriented features while maintaining a concise, professional format.

---

# TypeScript Comprehensive Cheat Sheet

## 1. Data Types

### Primitive & Basic Types

- **`boolean`**: `true` or `false`.
- **`number`**: Integers and floats (also `NaN`, `Infinity`).
- **`string`**: Textual data.
- **`bigint`**: Large integers (e.g., `100n`).
- **`symbol`**: Unique constants used as object keys.
- **`any`**: Opt-out of type checking (use sparingly).
- **`unknown`**: Type-safe version of `any`. You must perform a check before using it.
- **`void`**: Return type of functions that return no value.
- **`null` / `undefined`**: Standard JavaScript nullables.
- **`never`**: A value that should never occur (e.g., a function that always throws an error).

---

## 2. Custom Data Types (Type vs. Interface)

### Type Alias (`type`)

Best for Unions, Tuples, and complex functional definitions.

- **Capabilities**: Can define primitives, unions, and tuples.
- **Constraint**: Cannot be re-opened for new properties (Static).

```typescript
type ID = string | number; // Union
type Point = [number, number]; // Tuple
type Intersection = { a: number } & { b: string }; // Intersection

// Advanced: Conditional Types
type IsString<T> = T extends string ? "Yes" : "No";

// Advanced: Template Literals
type Direction = "top" | "bottom";
type Margin = `margin-${Direction}`; // "margin-top" | "margin-bottom"
```

### Interface (`interface`)

Best for defining the shape of Objects and Classes.

- **Capabilities**: Supports **Declaration Merging** (add props later).
- **Extension**: Uses `extends` for inheritance.

```typescript
interface User {
  readonly id: number; // Cannot be modified after init
  name: string;
  email?: string; // Optional property
}

// Declaration Merging
interface User {
  age: number;
} // Merged automatically into the original User

// Inheritance
interface Admin extends User {
  role: "admin" | "super";
}
```

---

## 3. Implementation Methods

### Array & Tuples

```typescript
let list: number[] = [1, 2, 3]; // Array
let genericList: Array<number> = [1, 2, 3]; // Generic Array syntax
let tuple: [string, number] = ["age", 30]; // Fixed-length, fixed-type array
```

### Enums

Used to define a set of named constants.

```typescript
enum Status {
  Pending,
  Active,
  Suspended,
} // Default starts at 0
let s: Status = Status.Active; // 1

enum ErrorCode {
  NotFound = 404,
  Unauthorized = 401,
} // Manual numbering
```

### Type Narrowing (Type Guards)

Techniques to "zoom in" on a specific type from a union.

```typescript
function process(val: string | number) {
  if (typeof val === "string") {
    return val.toUpperCase(); // TS knows it's a string here
  }
  return val.toFixed(2); // TS knows it's a number here
}
```

---

## 4. Generics

Write reusable code that works with multiple types while maintaining safety.

```typescript
// Generic Function
function identity<T>(arg: T): T {
  return arg;
}

// Generic Constraints (restricting T to certain shapes)
interface Lengthwise {
  length: number;
}
function loggingIdentity<T extends Lengthwise>(arg: T): T {
  console.log(arg.length); // Safe: arg MUST have a length property
  return arg;
}

// Generic Class
class Box<T> {
  constructor(public content: T) {}
}
const numBox = new Box<number>(100);
```

---

## 5. Utility Types

TypeScript provides built-in helpers for common type transformations.

| Utility            | Description                                               |
| :----------------- | :-------------------------------------------------------- |
| **`Partial<T>`**   | Makes all properties in `T` optional.                     |
| **`Required<T>`**  | Makes all properties in `T` required.                     |
| **`Readonly<T>`**  | Makes all properties in `T` immutable.                    |
| **`Record<K, T>`** | Constructs an object type with keys `K` and values `T`.   |
| **`Pick<T, K>`**   | Constructs a type by picking specific keys `K` from `T`.  |
| **`Omit<T, K>`**   | Constructs a type by removing specific keys `K` from `T`. |

```typescript
interface Task {
  title: string;
  desc: string;
  done: boolean;
}

const update: Partial<Task> = { done: true }; // Only need some properties
const info: Pick<Task, "title"> = { title: "Buy Milk" }; // Only has title
```

---

## 6. Advanced Object-Oriented Features

### Access Modifiers

Used in classes to control visibility.

```typescript
class Account {
  public name: string; // Accessible everywhere (default)
  private id: string; // Only within this class
  protected type: string; // Within class and subclasses

  constructor(id: string, name: string) {
    this.id = id;
    this.name = name;
  }
}

// Short-hand syntax (Parameter Properties)
class EasyAccount {
  constructor(
    private id: string,
    public name: string,
  ) {}
}
```

---

## 7. Operators and Assertions

### Keyof / Typeof

- **`keyof`**: Extracts keys of an object type as a union.
- **`typeof`**: Obtains the type from a variable instance.

```typescript
const point = { x: 10, y: 20 };
type Point = typeof point; // { x: number; y: number; }
type PointKeys = keyof Point; // "x" | "y"
```

### Assertions & Non-Null

- **`as`**: Tells the compiler "trust me, I know the type better than you."
- **`!`**: Non-null assertion operator. Tells TS a value is not null/undefined.

```typescript
const myCanvas = document.getElementById("main") as HTMLCanvasElement;
const name = user.name!; // Asserts that name will not be null/undefined
```

---

## 8. Decorators

A **Decorator** is a special kind of declaration that can be attached to a **class, method, accessor, property, or parameter.** It uses the form `@expression`, where `expression` must evaluate to a function.

> Enable via `"experimentalDecorators": true` in `tsconfig.json`.

```typescript
function Logger(target: any, key: string) {
  console.log(`Method ${key} is being watched.`);
}

class Office {
  @Logger
  printDocument() {
    console.log("Printing...");
  }
}
```

- **Usage**: Heavily used in Angular (Dependency Injection) and NestJS (Routing/Validation).
- **Modern JS**: Since TypeScript 5.0, standard ECMAScript decorators are supported without experimental flags, though their internal metadata behavior differs.

Think of a decorator as **"wrapping"** your code to add extra functionality without changing the original code's logic—similar to how a gift wrap covers a box.

---

### 1. How a Decorator looks in TypeScript

To use decorators in "traditional" TypeScript, you usually enable `"experimentalDecorators": true` in your `tsconfig.json`.

```typescript
function Logger(target: any) {
  console.log("Class was created!");
}

@Logger
class Robot {
  name = "R2D2";
}
```

---

### 2. What is the equivalent in JavaScript?

#### The Modern Equivalent (ECMAScript)

As of **2023-2024**, decorators are now a formal part of the JavaScript standard (TC39 Stage 3/4). Modern browsers and runtimes are starting to support them natively.

In modern JavaScript, the syntax looks **exactly the same** as TypeScript:

```javascript
// This is now valid modern JavaScript
@defineElement("my-widget")
class MyWidget extends HTMLElement {
  // ...
}
```

#### The "Under the Hood" Equivalent (High-Order Functions)

If you are working in an older environment (ES5 or ES6) that doesn't support the `@` symbol, the decorator is essentially just a **Higher-Order Function**.

Before decorators existed, we achieved the same result by passing a class or function into another function:

**TypeScript Decorator Style:**

```typescript
@testable
class MyClass {}
```

**Equivalent Old JavaScript Logic:**

```javascript
class MyClass {}

function testable(target) {
  target.isTestable = true;
}

// Manually "decorating" the class
const DecoratedClass = testable(MyClass) || MyClass;
```

---

### 3. Comparison of Features

| Feature       | TypeScript (Legacy/Experimental)               | Modern JavaScript (Standard)                       |
| :------------ | :--------------------------------------------- | :------------------------------------------------- |
| **Syntax**    | `@decorator`                                   | `@decorator`                                       |
| **Metadata**  | Can use `Reflect.metadata` to store type info. | Does not support design-time types.                |
| **Context**   | Gives you the `target` constructor.            | Gives you a `context` object with names and types. |
| **Execution** | Runs when the file is parsed.                  | Runs when the class is defined.                    |
| **Goal**      | Introspection and metadata.                    | Modification and "wrapping."                       |

---

### 4. Key Differences to watch out for

1.  **Experimental vs. Standard:** Most TypeScript projects use the "Experimental" version of decorators (common in Angular). However, TypeScript 5.0+ introduced support for the official "JavaScript Standard" decorators. The syntax is the same, but the internal arguments the function receives are different.
2.  **Metadata:** TypeScript can "read" your types at runtime using decorators (like seeing that a parameter is a `String`). JavaScript decorators cannot do this because JavaScript has no type system at runtime.

### Summary

- **In TypeScript:** Decorators are used heavily for Dependency Injection (like in **Angular** or **NestJS**) to identify types and inject services.
- **In JavaScript:** They are used to simplify code, such as "binding" methods to a class instance or marking properties as "read-only."

**Essentially:** A decorator is just a function that takes a piece of code (like a class) and returns a modified version of it.
