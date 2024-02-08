# TypeScript notes
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
* [Tuple Vs Array](#tuple-vs-array)
* [First of Array](#first-of-array)
* [Readonly](#readonly)
* [Exclude](#exclude)

### Exclude

The `Exclude` utility type in TypeScript removes types from a union that are assignable to another type. Here's how you can implement it on our own:

```typescript
type MyExclude<T, U> = T extends U ? never : T;

// Example usage
type Result = MyExclude<'a' | 'b' | 'c', 'a'>; // Result: 'b' | 'c'
```

Explanation:
- `T extends U ? never : T;`: This conditional type checks if each type in `T` is assignable to `U`. If it is, it replaces it with `never`, effectively removing it from the resulting type. If it's not assignable to `U`, it keeps the type unchanged.
- `Result`: When `MyExclude<'a' | 'b' | 'c', 'a'>` is evaluated, it removes `'a'` from the union `'a' | 'b' | 'c'`, resulting in `'b' | 'c'`.

More detailed explanation:
Let's break down the `MyExclude` type:

- `type MyExclude<T, U>`: This is a generic type that takes two type parameters, `T` and `U`.
- `T extends U ? never : T;`: This is a conditional type. It checks if each type in `T` is assignable to `U`. If it is, it results in `never`, effectively removing that type from the resulting type. If it's not assignable to `U`, it keeps the type unchanged.

Now, let's see how it works with the provided example:

```typescript
type Result = MyExclude<'a' | 'b' | 'c', 'a'>;
```

In this example:
- `T` is the union type `'a' | 'b' | 'c'`.
- `U` is the type `'a'`.

For each type in `T`, the conditional type checks if it's assignable to `'a'`. Here's the breakdown:

- `'a' extends 'a'` evaluates to `true`, so it results in `never`. This removes `'a'` from the resulting type.
- `'b' extends 'a'` and `'c' extends 'a'` both evaluate to `false`, so they remain unchanged.

After applying the conditional type to each type in `T`, the resulting type becomes `'b' | 'c'`, which excludes `'a'`. So, the `Result` type is `'b' | 'c'`.


---
### Tuple Vs Array

The syntax for declaring tuples and arrays can look similar because they both use square brackets. However, there are subtle differences:

1. **Tuple Declaration**:
   - In tuple declaration, the types of individual elements are specified within the square brackets.
   - The types are separated by commas.
   - Example:
     ```typescript
     const myTuple: [string, number] = ['hello', 10];
     ```
   - Here, `[string, number]` indicates that `myTuple` is a tuple with a string followed by a number.

2. **Array Declaration**:
   - In array declaration, only the type of the elements is specified within the square brackets.
   - The elements themselves are specified using normal array literal syntax.
   - Example:
     ```typescript
     const myArray: number[] = [1, 2, 3];
     ```
   - Here, `number[]` indicates that `myArray` is an array containing only numbers.

So, while both tuples and arrays use square brackets, the types inside the brackets and the way elements are assigned differentiate them.

**More about Tuple and Array**
1. **Tuples**:
   - Tuples are fixed-length collections in TypeScript, meaning they have a predetermined number of elements.
   - Each element in a tuple can have a different type.
   - Example:
     ```typescript
     const myTuple: [string, number] = ['hello', 10];
     // The above tuple has two elements: a string followed by a number.
     ```

2. **Arrays**:
   - Arrays, on the other hand, can vary in length. You can add or remove elements dynamically.
   - However, all elements in an array must have the same type.
   - Example:
     ```typescript
     const myArray: number[] = [1, 2, 3];
     // This array has three elements, and they're all numbers.
     ```

3. **Arrays and Any Length**:
   - When we say "array of any length," it means that arrays can have any number of elements, including zero.
   - The length of an array can change dynamically based on the elements added or removed.
   - Example:
     ```typescript
     const myArray: number[] = []; // Empty array
     const anotherArray: string[] = ['hello', 'world']; // Array with two elements
     ```

So, in summary:
- Tuples are fixed-length and can contain elements of different types.
- Arrays can vary in length and contain elements of the same type. They can be empty or have any number of elements.
- So, if you need a collection of elements that won't change in length or type, you can use a tuple. Otherwise, if you need more flexibility, an array would be more appropriate.

---
### First of Array

```
type First<T extends any[]> = T extends [infer First, ...infer Rest] ? First : never;

// Example 
type arr1 = ['a', 'b', 'c'];
type arr2 = [1, 2, 3];

type head1 = First<arr1>; // inferred type: string ('a')
type head2 = First<arr2>; // inferred type: number (1)

```
**Explanation**

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
---
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

**Explanation**

Let's break down the expression {readonly [P in keyof T]: T[P]}; and understand how it works step by step:

1. keyof T: This part gets all the keys of the type T. For example, if T is { name: string; age: number; }, then keyof T will be the union type "name" | "age".
2.  P in keyof T: This is a mapped type. It iterates over each key in the union type obtained from keyof T, and for each key, it assigns it to the type variable P.
3.  T[P]: For each P (which is a key of T), it gets the type of the corresponding property in T. For example, if P is "name", then T[P] is equivalent to T["name"], which is the type of the name property in T.
4.  readonly [P in keyof T]: T[P]: The readonly modifier is applied to each property. This means that once an object is created with this type, you cannot modify the value of its properties after creation.
5.  { ... }: This part encapsulates the whole mapped type definition.
---
### Tuple to Object
```
const tuple = ['tesla', 'model 3', 'model X', 'model Y'] as const;

type result = TupleToObject<typeof tuple>;
// Expected: { 'tesla': 'tesla', 'model 3': 'model 3', 'model X': 'model X', 'model Y': 'model Y'}

type TupleToObject<T extends readonly PropertyKey[]> = { [K in T[number]]: K };
```
**Explanation**
1. Type Parameter: T extends readonly PropertyKey[]: This ensures that T is an array of PropertyKey types, meaning the keys of the resulting object must be valid property keys.
2. Mapped Type: { [K in T[number]]: K }: This is a mapped type that iterates over each element K in the array T. T[number] gets the union type of all elements in T, and K is then each individual element in this union.
3. Object Shape: { [K in T[number]]: K } ensures that the resulting object has keys and values where the key is the same as the value.
