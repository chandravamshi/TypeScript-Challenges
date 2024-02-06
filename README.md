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


### First of Array

```
type First<T extends any[]> = T extends [infer First, ...infer Rest] ? First : never;

// Example 
type arr1 = ['a', 'b', 'c'];
type arr2 = [1, 2, 3];

type head1 = First<arr1>; // inferred type: string ('a')
type head2 = First<arr2>; // inferred type: number (1)

```
##### Explanation
Let's break down the line type First<T extends any[]> = T extends [infer First, ...infer Rest] ? First : never; step by step:

1. type First<T extends any[]>: This declares a new type alias First that takes a generic type T, which must be an array.

2. T extends [infer First, ...infer Rest]: This is a conditional type check. It checks if the type T matches a pattern where it's an array with at least one element. The infer keyword is used to capture the type of the first element as First and the remaining elements as Rest.

3. infer First: The infer keyword is used to infer the type of the first element of the array T and assign it to the type variable First.

4. ...infer Rest: The rest operator (...) captures the remaining elements of the array T after the first element and assigns them to the type variable Rest.

5. ? First : never;: This is the ternary conditional operator. If the condition T extends [infer First, ...infer Rest] is true, it returns the inferred type First. Otherwise, it returns never.

### PropertyKey

1. Definition: PropertyKey is a built-in TypeScript type that represents the type of values that can be used as property names in JavaScript.
2. Examples of PropertyKeys:
    PropertyKeys can be:
      Strings: 'name', 'age'
      Numbers: 42, 3.14
      Symbols: Symbol('unique')
3. Usage in TypeScript: It's often used in scenarios where you want to allow a variety of values to be used as keys in objects or as indices in arrays.
4.  Basic Usage:
 ```
const propertyName: PropertyKey = 'name';
```

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

### Tuple to Object
```
const tuple = ['tesla', 'model 3', 'model X', 'model Y'] as const;

type result = TupleToObject<typeof tuple>;
// Expected: { 'tesla': 'tesla', 'model 3': 'model 3', 'model X': 'model X', 'model Y': 'model Y'}

type TupleToObject<T extends readonly PropertyKey[]> = { [K in T[number]]: K };
```
##### Explanation
1. Type Parameter: T extends readonly PropertyKey[]: This ensures that T is an array of PropertyKey types, meaning the keys of the resulting object must be valid property keys.
2. Mapped Type: { [K in T[number]]: K }: This is a mapped type that iterates over each element K in the array T. T[number] gets the union type of all elements in T, and K is then each individual element in this union.
3. Object Shape: { [K in T[number]]: K } ensures that the resulting object has keys and values where the key is the same as the value.
