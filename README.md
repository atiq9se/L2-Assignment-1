1.What are some differences between interfaces and types in TypeScript?

Interfaces support declaration merging we can define an interface multiple times, and TypeScript will merge the declarations:

interface User {
  name: string;
}

interface User {
  age: number;
}

interface User {
  name: string;
  age: number;
}


Types do not merge, Type aliases are one-off declarations and can’t be merged:

type User = {
  name: string;
};

type User = {
  age: number; // ❌ Error: Duplicate identifier 'User'
};


Both interface and type support extension, but the syntax differs:
Interface extension:
interface Person {
  name: string;
}

interface Employee extends Person {
  role: string;
}

Type intersection:
type Person = {
  name: string;
};

type Employee = Person & {
  role: string;
};

Types can represent primitives, unions, and tuples:

type ID = string | number;
type Point = [number, number];


Interfaces cannot represent primitive types or union types.


Classes can implement interfaces:

interface Printable {
  print(): void;
}

class Report implements Printable {
  print() {
    console.log("Printing...");
  }
}
While we can use a type alias with implements, it must resolve to a compatible object shape. Interfaces are more natural and intended for this purpose.


While interfaces and types share many capabilities, knowing their differences allows you to make more intentional design decisions in your TypeScript code. Use interfaces for clear, extensible object contracts, and types for more flexible or advanced type constructs.



2.What is the use of the keyof keyword in TypeScript? Provide an example 

The keyof keyword in TypeScript is used to create a union type of all property keys of a given object type. It’s particularly useful when we want to work with object keys in a type-safe way—for example, to restrict access to certain properties or implement generic utility functions.

Given a type T, keyof T will return a union of string (or number) literal types representing the keys of T.

Example:
type User = {
  id: number;
  name: string;
  isAdmin: boolean;
};

type UserKeys = keyof User;
// Equivalent to: "id" | "name" | "isAdmin"
Now UserKeys is a union of the literal strings "id" | "name" | "isAdmin".

Example: Generic Property Access

function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const user = {
  id: 1,
  name: "Alice",
  isAdmin: true
};

const name = getProperty(user, "name");  // type is string
const isAdmin = getProperty(user, "isAdmin");  // type is boolean
// const invalid = getProperty(user, "email");  // ❌ Error: Argument of type '"email"' is not assignable to parameter of type 'keyof {...}'
This pattern ensures you only access properties that exist on the object, with full type safety and autocomplete support.
