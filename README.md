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
* [Unknown](#unknown)
* [If](#if)
* [Exclude](#exclude)
* [Tuple Vs Array](#tuple-vs-array)
* [First of Array](#first-of-array)
* [Readonly](#readonly)
* [PromiseLike](#promiselike)
* [Concat](#concat-js-arrayconcat-)

---


### Unknown

1. **Definition**: `unknown` is a type in TypeScript that represents a value whose type is not known at compile time. It is the type-safe counterpart of the `any` type. Variables of type `unknown` can hold any value, similar to `any`, but you can't perform operations on them without first narrowing their type.

2. **Example**:
   ```typescript
   let userInput: unknown;
   userInput = 5; // valid
   userInput = 'hello'; // valid
   ```

#### Type Checking and Narrowing `unknown`:

1. **Type Checking**: Since the type of an `unknown` variable is not known, TypeScript won't allow you to perform most operations on it without narrowing its type first. This ensures type safety.

2. **Type Narrowing**: You can narrow the type of an `unknown` variable using type assertions, type guards, or control flow analysis.

   - **Type Assertions**:
     ```typescript
     let userInput: unknown;
     let username = userInput as string; // Assertion: tells TypeScript that `userInput` is a string
     ```

   - **Type Guards**:
     ```typescript
     function isString(value: unknown): value is string {
       return typeof value === 'string';
     }

     let userInput: unknown;
     if (isString(userInput)) {
       // Within this block, TypeScript knows `userInput` is a string
       console.log(userInput.toUpperCase());
     }
     ```

   - **Control Flow Analysis**:
     ```typescript
     let userInput: unknown;
     if (typeof userInput === 'string') {
       // Within this block, TypeScript knows `userInput` is a string
       console.log(userInput.toUpperCase());
     }
     ```

#### Differences from `any`:

1. **Type Safety**: `unknown` provides more type safety compared to `any`. While `any` allows unrestricted operations, `unknown` requires type checking before performing operations, reducing the risk of runtime errors.

2. **Type Inference**: Variables of type `unknown` retain their type information. Unlike `any`, where TypeScript gives up type checking, `unknown` variables still undergo type inference and are checked for type compatibility.

Overall, `unknown` is a useful tool for working with values of uncertain type while maintaining type safety in TypeScript.

**Let's understand the concept of type inference and how it applies to variables of type `unknown` with examples:**

Type inference is the TypeScript compiler's ability to deduce the types of variables based on their usage within the code. When you declare a variable without explicitly specifying its type, TypeScript tries to infer the most appropriate type based on the value assigned to it and how that value is used.

Now, let's see how type inference works with variables of type `unknown`:

```typescript
let myVar = 10; // TypeScript infers the type of myVar as number
```

In this example, TypeScript infers the type of `myVar` as `number` because its initial value is `10`, which is a numeric literal.

Now, let's see how type inference works with variables of type `unknown`:

```typescript
let myUnknownVar: unknown = 'Hello'; // The type of myUnknownVar is explicitly set to unknown
let myString: string = myUnknownVar; // Error: Type 'unknown' is not assignable to type 'string'
```

In this example, `myUnknownVar` is explicitly declared as type `unknown`. Even though it's assigned a value of type `string`, TypeScript doesn't automatically infer its type as `string`. When you try to assign `myUnknownVar` to a variable of type `string` (`myString`), TypeScript throws a compilation error because it's not safe to assume that `myUnknownVar` will always hold a value of type `string`.

Now, let's contrast this with the behavior of variables of type `any`:

```typescript
let myAnyVar: any = 'Hello'; // The type of myAnyVar is explicitly set to any
let myString: string = myAnyVar; // No error
```

In this example, `myAnyVar` is explicitly declared as type `any`. TypeScript allows you to assign it to a variable of type `string` (`myString`) without any compilation errors. Unlike `unknown`, where TypeScript retains type information and enforces type checking, with `any`, TypeScript essentially disables type checking and treats the variable as having the type of "anything", allowing assignments without type checks.

In summary, while both `unknown` and `any` are types that represent "anything", TypeScript treats them differently during type inference and type checking. `unknown` maintains type information and enforces type safety, while `any` bypasses type checking altogether.

---

### If 

```typescript
type If<Condition extends boolean, TrueType, FalseType> = Condition extends true ? TrueType : FalseType;
```

Type Parameters

- `Condition`: Represents the boolean condition that determines which type to select.
- `TrueType`: Represents the type to be returned if the condition is `true`.
- `FalseType`: Represents the type to be returned if the condition is `false`.

Type Inference

- The `Condition` parameter is evaluated to check if it extends `boolean`. If it does not extend `boolean`, TypeScript will throw an error.
- If `Condition` is `true`, the `TrueType` is returned. Otherwise, the `FalseType` is returned.

Usage

```typescript
type IsNumber<T> = T extends number ? true : false;

type Result1 = If<IsNumber<42>, 'Number', 'Not a Number'>; // Result1: 'Number'
type Result2 = If<IsNumber<'Hello'>, 'Number', 'Not a Number'>; // Result2: 'Not a Number'
```

In these examples:
- `IsNumber<42>` evaluates to `true`, so `Result1` is `'Number'`.
- `IsNumber<'Hello'>` evaluates to `false`, so `Result2` is `'Not a Number'`.

This way, the `If` utility type allows us to conditionally select types based on boolean conditions in TypeScript.

---
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

---

### PromiseLIke
1. **PromiseLike Interface**:
   - `PromiseLike` is an interface in TypeScript.
   - It represents an object that looks like a promise, meaning it has a `then` method.
   - The `then` method is used for asynchronous actions and allows you to specify what to do when a promise is fulfilled (resolved) or rejected.

2. **Purpose**:
   - `PromiseLike` is used to define types that behave like promises but may not necessarily be instances of the built-in `Promise` class.
   - This allows TypeScript to work with a broader range of asynchronous APIs and functions.

3. **Usage**:
   - You can use `PromiseLike` in type annotations to specify that a value is expected to have a `then` method and behave like a promise.
   - For example:
     ```typescript
     function asyncFunction(): PromiseLike<number> {
       // Some asynchronous logic
       return {
         then: (onfulfilled: (arg: number) => any) => {
           // Simulating a resolved promise
           onfulfilled(42);
         }
       };
     }
     ```

4. **Type Inference**:
   - When you specify a type as `PromiseLike<T>`, TypeScript infers that the type represents an object that may have a `then` method, typically used for chaining asynchronous operations.
   - This allows you to work with custom promise-like objects that adhere to the same asynchronous API as native promises.

5. **Compatibility**:
   - TypeScript's `PromiseLike` interface makes it easier to work with various libraries and APIs that use promise-like objects, even if they are not instances of `Promise`.
   - It provides a level of interoperability by allowing TypeScript to understand and validate asynchronous code that uses custom promise-like objects.

6. **Example**:
   ```typescript
   type MyPromiseLike<T> = {
     then(onfulfilled: (value: T) => any): any;
   };

   function processPromise(promise: MyPromiseLike<number>): void {
     promise.then((value) => {
       console.log(`Received value: ${value}`);
     });
   }
   ```

In summary, `PromiseLike` is a TypeScript interface that defines the shape of objects representing promises or promise-like behavior. It enables TypeScript to work with a wider range of asynchronous patterns and enhances the interoperability of TypeScript code with various libraries and APIs.


---

### Concat (JS `Array.concat` )

1. `type Concat<A extends readonly unknown[], B extends readonly unknown[]>`: This defines a generic type `Concat` that takes two type parameters `A` and `B`, which are arrays of unknown readonly elements.

2. `([...A, ...B])`: This uses the spread operator (`...`) to concatenate the elements of array `A` followed by the elements of array `B`. The resulting array contains all the elements from `A` followed by all the elements from `B`.

Now, let's understand how this works with an example:

```typescript
type Result = Concat<[1], [2]>; // Expected to be [1, 2]
```

When we pass `[1]` as `A` and `[2]` as `B`, the `Concat` type concatenates the elements of `A` and `B`:

- `A` is `[1]`, so `[...A]` becomes `[1]`.
- `B` is `[2]`, so `[...B]` becomes `[2]`.
- `[...A, ...B]` combines `[1]` and `[2]`, resulting in `[1, 2]`.

So, the `Result` type evaluates to `[1, 2]`, as expected.

This implementation effectively replicates the behavior of JavaScript's `Array.concat` function in the TypeScript type system by concatenating two arrays in left-to-right order.

Detailed Explanation 

In the type constraint `A extends readonly unknown[]`, the `unknown` type is used to represent a value whose type is not known at compile time. Here's a breakdown of what each part of the constraint means:

- **`A`**: This is a type parameter representing the type that is being constrained. It can be any type, but it must adhere to the constraint specified.

- **`extends`**: In the context of type constraints, `extends` is used to enforce that the type `A` must conform to a certain shape or structure.

- **`readonly`**: This keyword specifies that the array type `A` must be readonly, meaning that its elements cannot be modified after initialization.

- **`unknown[]`**: This denotes an array whose element types are all of type `unknown`. In other words, `A` must be an array where each element's type is unknown.

Putting it all together, `A extends readonly unknown[]` ensures that the type `A` is an array (readonly) whose elements are of unknown type. This constraint allows for flexibility in the type of array `A`, as it can hold values of any type without revealing their specific types.

For example:
```typescript
type ExampleArray = readonly unknown[]; // Valid type that adheres to the constraint

const arr1: ExampleArray = [1, 'two', true]; // Valid, elements can be of any type
const arr2: ExampleArray = ['hello', 42]; // Also valid
const arr3: ExampleArray = []; // Valid, can be an empty array
```

This constraint provides versatility in dealing with arrays of uncertain element types, allowing for greater flexibility while maintaining type safety.

--- 
