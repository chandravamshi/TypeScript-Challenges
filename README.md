# TypeScript-Challenges
I occasionally solve TypeScript challenges. I'll upload my solution for the chllanges in this repository.
* Some files are txt, which contains link to my solution, problem description and test cases.


## Useful Resources to learn or improve TypeScript Skills

### Challenges
* https://typehero.dev/
* https://github.com/type-challenges/type-challenges/tree/main

### Blogs or articles 
* https://dev.to/macsikora
* https://www.totaltypescript.com/
* https://react-typescript-cheatsheet.netlify.app/


## Concepts

### Readonly

```
type MakeReadOnly<T> = {
  readonly [K in keyof T]: T[K];
};
const mutableObj: MutableExample = { name: 'John', age: 25 };
const readOnlyObj: ReadOnlyExample = mutableObj;

// This will result in a TypeScript error, as you cannot assign to readonly properties
readOnlyObj.name = 'Jane';
```
In this example, MakeReadOnly takes an object type (Example) and returns a new type (ReadOnlyExample) where all properties are readonly. The obj object can be assigned properties during creation, but those properties cannot be modified afterward.

##### Explanation
Let's break down the expression {readonly [P in keyof T]: T[P]}; and understand how it works step by step:

1. keyof T: This part gets all the keys of the type T. For example, if T is { name: string; age: number; }, then keyof T will be the union type "name" | "age".
2.  P in keyof T: This is a mapped type. It iterates over each key in the union type obtained from keyof T, and for each key, it assigns it to the type variable P.
3.  T[P]: For each P (which is a key of T), it gets the type of the corresponding property in T. For example, if P is "name", then T[P] is equivalent to T["name"], which is the type of the name property in T.
4.  readonly [P in keyof T]: T[P]: The readonly modifier is applied to each property. This means that once an object is created with this type, you cannot modify the value of its properties after creation.
5.  { ... }: This part encapsulates the whole mapped type definition.
