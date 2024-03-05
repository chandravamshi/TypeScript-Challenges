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
| | |
|-------------|--------------|
| [Unknown](#unknown) | [Pick](#pick-the-pick-builtin) |
| [If](#if) | [ReturnType](#returntype-the-returntype-builtin) |
| [Exclude](#exclude) | [Omit](#omit-the-omit-builtin) |
| [Tuple Vs Array](#tuple-vs-array) | [ReadOnly](#readonly-1) |
| [First of Array](#first-of-array) | [DeepReadOnly](#deepreadonly) |
| [Readonly](#readonly) | [Tuple to Union](#tuple-to-union) |
| [PromiseLike](#promiselike) | [Chainable Options](#chainable-options) |
| [Concat](#concat-js-arrayconcat-) | [Last of Array](#last-of-array) |
| [Push](#push-js-arraypush-) | [Except Last of Array](#pop) |
| [UnShift](#unshift-js-arrayunshift-) | [PromiseAll Function](#promiseall-function) |
| [Parameters](#parameters) | [Type Lookup](#type-lookup) |
| [Trim Left](#trim-left) |  [Trim](#trim) |
| [Capitalize](#capitalize) | | 







<!--
* [Unknown](#unknown)
* [If](#if)
* [Exclude](#exclude)
* [Tuple Vs Array](#tuple-vs-array)
* [First of Array](#first-of-array)
* [Readonly](#readonly)
* [PromiseLike](#promiselike)
* [Concat](#concat-js-arrayconcat-)
* [Push](#push-js-arraypush-)
* [UnShift](#unshift-js-arrayunshift-)
* [Parameters](#parameters)
* [Pick](#pick-the-pick-builtin)
* [ReturnType](#returntype-the-returntype-builtin)
* [Omit](#omit-the-omit-builtin)
* [ReadOnly](#readonly-1)
* [DeepReadOnly](#deepreadonly)
* [Tuple to Union](#tuple-to-union)
* [Chainable Options](#chainable-options)
* [Last of Array](#last-of-array)
* [Except Last of Array](#pop)
* [PromiseAll Function](#promiseAll-function)
* [Type Lookup](#type-lookup)
* [Trim Left](#trim-left)
* [Trim](#trim)
 -->

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

Certainly! Here's the explanation in a README file format:

---

 ### Push (JS `Array.push` )

**Overview**

In TypeScript, we often need to work with arrays and manipulate their contents. One common operation is to add elements to an array. While TypeScript provides the `push` method for this purpose, we might want to create a generic version of `push` to ensure type safety.

**Implementation**

To implement a generic version of `Array.push`, we use tuple types and conditional types in TypeScript.

**Approach**

We define a type named `Push` that takes two type parameters: `T` for the input array and `E` for the type of the element to be pushed onto the array.

```typescript
type Push<T extends any[], E> = [...T, E];
```

In this implementation:
- `T` represents the input array, which must be a tuple type (`T extends any[]`).
- `E` represents the type of the element to be pushed onto the array.
- `[...T, E]` uses the spread operator (`...`) to create a new tuple that includes all elements of the original array `T`, followed by the new element `E`.

**Example**

Here's how you can use the `Push` type:

```typescript
type Result = Push<[1, 2], '3'>; // Result: [1, 2, '3']
```

This implementation correctly appends the new element `'3'` to the end of the input array `[1, 2]`, resulting in `[1, 2, '3']`.

**Conclusion**

Creating a generic version of `Array.push` allows us to maintain type safety while manipulating arrays in TypeScript. By leveraging tuple types and conditional types, we can achieve this functionality in a flexible and type-safe manner.

--- 

### UnShift (JS `Array.unshift` )

To implement the type version of `Array.unshift` in TypeScript, we can leverage tuple types and conditional types. Here's how we can do it:

```typescript
type Unshift<T extends any[], E> = [E, ...T];
```

Explanation:
- We define a type named `Unshift` that takes two type parameters: `T` for the input array and `E` for the type of the element to be added to the beginning of the array.
- Using the spread operator (`...`), we prepend the new element `E` to the tuple `T`, creating a new tuple.

Example:
```typescript
type Result = Unshift<[1, 2], 0>; // Result: [0, 1, 2]
```

This implementation correctly adds the new element `0` to the beginning of the input array `[1, 2]`, resulting in `[0, 1, 2]`.

---
### Parameters

To implement the `Parameters` utility type without using it directly, we can leverage TypeScript's inference capabilities and conditional types. Here's how we can do it:

```typescript
type MyParameters<T extends (...args: any) => any> = T extends (...args: infer P) => any ? P : never;
```

Explanation:
- We define a type named `MyParameters` that takes a generic function type `T`.
- We use a conditional type to check if `T` is assignable to a function type `(args: infer P) => any`, where `P` represents the tuple of function parameters.
- If `T` is indeed a function type, we return `P`, which represents the tuple of function parameters.
- If `T` is not a function type, we return `never`.

Example:
```typescript
const foo = (arg1: string, arg2: number): void => {}

type FunctionParamsType = MyParameters<typeof foo>; // [arg1: string, arg2: number]
```

In this example, `MyParameters<typeof foo>` correctly evaluates to `[arg1: string, arg2: number]`, which represents the parameters of the `foo` function.

---
### Pick (The `Pick` builtin)

The `Pick` utility type in TypeScript is used to select a subset of properties from an existing type. It takes two type parameters: the first parameter `T` is the type from which you want to pick properties, and the second parameter `K` is a union type of keys (property names) that you want to include in the new type.

Own version of `Pick`, called `MyPick`:

```typescript
type MyPick<T, K extends keyof T> = {
  [P in K]: T[P];
};
```

Let's break down the implementation:

- `T` represents the original type from which you want to pick properties.
- `K extends keyof T` ensures that the type `K` is a union of keys that exist in `T`.
- `[P in K]` iterates over each key in the union type `K`.
- `T[P]` selects the type of the property with key `P` from the original type `T`.

Here's an example of how you can use `MyPick`:

```typescript
interface Person {
  name: string;
  age: number;
  address: string;
}

type PersonNameAndAge = MyPick<Person, 'name' | 'age'>;

// The type of PersonNameAndAge is:
// {
//   name: string;
//   age: number;
// }
```

In this example, `MyPick<Person, 'name' | 'age'>` selects only the `name` and `age` properties from the `Person` type, resulting in a new type containing only those properties.

---
### ReturnType (The `ReturnType` builtin)

To implement the `ReturnType` generic type without using the built-in `ReturnType` utility, you can extract the return type of a function by inferring the return type based on the function's signature. Here's how you can do it:

```typescript
type MyReturnType<T extends (...args: any[]) => any> = T extends (...args: any[]) => infer R ? R : never;

// Test example
const fn = (v: boolean) => {
  if (v) {
    return 1;
  } else {
    return 2;
  }
};

type a = MyReturnType<typeof fn>; // should be "1 | 2"
```

Explanation:
- `MyReturnType` is a generic type that takes a function type `T`.
- `T extends (...args: any[]) => infer R` checks if `T` is a function type. If it is, `infer R` extracts the return type of the function and assigns it to `R`.
- `? R : never` ensures that if `T` is not a function type, the result is `never`.
- In the test example, `fn` is a function that returns either `1` or `2`. `MyReturnType<typeof fn>` extracts this return type as `"1 | 2"`.

---

### Omit (The `Omit` builtin)

To implement the `Omit<T, K>` generic type without using the built-in `Omit` utility, you can manually construct a new type by picking all properties from `T` except those specified by `K`. Here's how you can do it:

```typescript
type MyOmit<T, K extends keyof T> = {
  [P in keyof T as P extends K ? never : P]: T[P];
};

// Test example
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}

type TodoPreview = MyOmit<Todo, 'description' | 'title'>;

const todo: TodoPreview = {
  completed: false,
};
```

Explanation:
- `MyOmit<T, K>` is a generic type that takes two type parameters: `T` and `K`.
- `[P in keyof T as P extends K ? never : P]` iterates over each property `P` in `T`.
- For each property `P`, it checks if `P` extends any key in `K`.
- If `P` extends any key in `K`, it assigns `never` to exclude that property from the new type.
- Otherwise, it keeps the property as it is.
- This effectively creates a new type by omitting the properties specified by `K` from the original type `T`.
---

### ReadOnly 

```markdown
# MyReadonly2<T, K>

This generic type takes two type arguments `T` and `K`, where:
- `T`: Represents the type of the object whose properties we want to make readonly.
- `K`: (Optional) Specifies the subset of properties of `T` that should be set to readonly. Defaults to all properties of `T`.

## Implementation

```typescript
type MyReadonly2<T, K extends keyof T = keyof T> = 
  { readonly [P in K]: T[P] } & 
  { [P in keyof T as P extends K ? never : P]: T[P] };
```
**Explanation**

1. **Type Parameters:**
   - `T`: Represents the type of the object whose properties we want to make readonly.
   - `K`: Represents the subset of properties of `T` that should be set to readonly. It's optional and defaults to all properties of `T` (`keyof T`).

2. **Mapped Type for Readonly Properties (`K` subset):**
   - `{ readonly [P in K]: T[P] }`: This mapped type iterates over each property `P` in the subset `K` of `T` and makes them readonly by adding the `readonly` modifier. It ensures that only the specified properties in `K` are readonly.

3. **Mapped Type for Non-Readonly Properties (`T` minus `K`):**
   - `[P in keyof T as P extends K ? never : P]: T[P]`: This mapped type iterates over all properties `P` of `T`. For each property `P`, it checks if `P` exists in the subset `K`. If yes, it evaluates to `never`, effectively removing that property from the resulting type. If not, it evaluates to `P`, keeping the property as-is with its original type.

4. **Intersection of Mapped Types:**
   - `&`: Combines the mapped types created in steps 2 and 3 into a single type. This ensures that the resulting type contains both the readonly properties specified in `K` and the non-readonly properties from `T` that are not in `K`.

Putting it all together, the implementation ensures that the properties specified in `K` are readonly, while the rest of the properties of `T` remain unchanged.

---

### DeepReadOnly 

```markdown
# DeepReadonly<T>

This generic type makes every parameter of an object `T`, including its sub-objects, readonly.

## Implementation

```typescript
type DeepReadonly<T> = {
  readonly [K in keyof T]: T[K] extends Function
    ? T[K]
    : T[K] extends object
    ? DeepReadonly<T[K]>
    : T[K];
};
```
**Explanation**
1. **Type Parameter:**
   - `T`: Represents the type of the object whose parameters we want to make readonly.

2. **Mapped Type for Readonly Properties:**
   - `readonly [K in keyof T]`: This mapped type iterates over each property `K` in `T` and makes them readonly by adding the `readonly` modifier.

3. **Conditional Type:**
   - `T[K] extends Function ? T[K] : T[K] extends object ? DeepReadonly<T[K]> : T[K]`: This conditional type checks the type of each property `T[K]`:
     - If `T[K]` is a function, it returns `T[K]` itself because functions do not have properties to make readonly.
     - If `T[K]` is an object, it recursively applies the `DeepReadonly` type to make its properties readonly.
     - If `T[K]` is neither a function nor an object, it keeps `T[K]` as-is.

---

### Tuple to Union

`TupleToUnion<T>`:

```typescript
type TupleToUnion<T extends any[]> = T[number];
```

Explanation:
- `T extends any[]`: Ensures that the input type `T` is a tuple.
- `T[number]`: Accesses the union type of all elements in the tuple `T`.

This works because `T[number]` will give us the union of all element types in the tuple `T`.

Example usage:

```typescript
type Arr = ['1', '2', '3'];

type Test = TupleToUnion<Arr>; // expected to be '1' | '2' | '3'
```

In this example, `TupleToUnion<Arr>` resolves to the union type `'1' | '2' | '3'`, which is the union of all the values in the tuple `Arr`.

Let's break down how `T[number]` accesses the union type of all elements in the tuple `T`:

1. **`T[number]`**: This syntax accesses the type of an element in the tuple `T` by indexing it with a numeric literal type. In TypeScript, when you use numeric literal types to index an array or tuple type, TypeScript returns the type of the indexed element.

2. **Union of Element Types**: When you use `T[number]`, TypeScript infers the type of each element in the tuple `T` individually. Since each element in the tuple can have a different type, TypeScript combines these types into a union type.

Here's a step-by-step example to illustrate this process:

Consider a tuple type `T` with the elements `'1'`, `'2'`, and `'3'`:

```typescript
type T = ['1', '2', '3'];
```

When we use `T[number]`, TypeScript internally accesses the type of each element in the tuple `T`:

- `T[0]`: Type of the first element `'1'` is `'1'`.
- `T[1]`: Type of the second element `'2'` is `'2'`.
- `T[2]`: Type of the third element `'3'` is `'3'`.

Now, TypeScript combines these types into a union:

```typescript
type UnionType = T[number]; // Equivalent to type UnionType = '1' | '2' | '3';
```

So, `T[number]` effectively gives us the union type of all elements in the tuple `T`.

---

### Chainable Options

We need to type an object or a class to provide two functions: `option(key, value)` and `get()`. The `option` function extends the current configuration type by the given key and value, and the `get` function retrieves the final result.

Example

```typescript
declare const config: Chainable;

const result = config
  .option('foo', 123)
  .option('name', 'type-challenges')
  .option('bar', { value: 'Hello World' })
  .get();

// The expected type of result is:
interface Result {
  foo: number;
  name: string;
  bar: {
    value: string;
  };
}
```

Solution

```typescript
type Chainable<T = {}> = {
  option<K extends string, V>(
    key: Exclude<K, keyof T>,
    value: V
  ): Chainable<Omit<T, K> & Record<K, V>>;
  get(): T;
};
```
Explanation

The `Chainable` type is designed to provide a chainable interface for configuring objects. It consists of two main functions:

1. `option(key, value)`: This function allows adding or updating key-value pairs in the configuration object.
2. `get()`: This function retrieves the final configuration object.

Detailed Explanation

- **`option` function**:
  - It takes two parameters: `key` (the name of the property to be added or updated) and `value` (the value to be assigned to the property).
  - The `key` parameter is constrained to be a string (`K extends string`) to ensure type safety.
  - The `value` parameter can have any type (`V`).
  - The `Exclude<K, keyof T>` type is used to ensure that the `key` does not already exist in the configuration object (`T`). This prevents accidental overwriting of existing properties.
  - Inside the function body, the `Omit<T, K> & Record<K, V>` type is used to create a new configuration object:
    - `Omit<T, K>` removes the `key` property (if it exists) from the current configuration object (`T`).
    - `Record<K, V>` adds the `key` property with the specified `value` to the new configuration object.
  - The function returns a new `Chainable` object with the updated configuration.

- **`get` function**:
  - It takes no parameters.
  - It retrieves the final configuration object (`T`) after all option calls.
  - The function returns the final configuration object.

Example

```typescript
// Declaring a Chainable object
declare const config: Chainable;

// Calling option() and get() functions to configure and retrieve the final result
const result1 = config
  .option('foo', 123)
  .option('bar', { value: 'Hello World' })
  .option('name', 'type-challenges')
  .get();

// The expected type of result1 is:
// interface Result1 {
//   foo: number;
//   bar: {
//     value: string;
//   };
//   name: string;
// }
```

In this example, the `config` object is used to configure a hypothetical object. Each call to the `option` function adds or updates properties in the configuration object. Finally, the `get` function retrieves the final configuration object (`result1`). The resulting object `result1` should have properties `foo`, `bar`, and `name` with the specified values.

This demonstrates how the `Chainable` type enables a chainable configuration pattern in TypeScript, ensuring type safety and ease of use.

---

### Last of Array

Problem

Create a generic type `Last<T>` that takes an array type `T` and returns its last element.

Example

```typescript
type arr1 = ['a', 'b', 'c']
type arr2 = [3, 2, 1]

type tail1 = Last<arr1> // expected to be 'c'
type tail2 = Last<arr2> // expected to be 1
```

Solution

We can use conditional types and inference to achieve this. The type `Last<T>` checks if the array type `T` matches a pattern where the last element is captured by `infer LastElement`. If the pattern matches, it returns `LastElement`; otherwise, it returns `never`.

```typescript
type Last<T extends any[]> = T extends [...infer _, infer LastElement] ? LastElement : never;
```

- `T extends any[]`: This constrains `T` to be an array type.
- `[...infer _, infer LastElement]`: This pattern matches an array type where the last element is captured by `LastElement`, and the rest of the elements (excluding the last) are captured by `infer _`.
- `? LastElement : never`: If the pattern matches successfully, it returns the last element `LastElement`. Otherwise, it returns `never`.

Usage

```typescript
type arr1 = ['a', 'b', 'c']
type arr2 = [3, 2, 1]

type tail1 = Last<arr1> // evaluates to 'c'
type tail2 = Last<arr2> // evaluates to 1
```

This solution provides a generic type `Last<T>` that accurately determines the last element of an array type `T`.

[Top](#concepts)

--- 

### Pop 

Problem

Implement a generic `Pop<T>` that takes an Array `T` and returns an Array without it's last element.

```typescript
// Example usage:
type arr1 = ['a', 'b', 'c', 'd'];
type arr2 = [3, 2, 1];

type re1 = Pop<arr1>; // expected to be ['a', 'b', 'c']
type re2 = Pop<arr2>; // expected to be [3, 2]
```
Solution

```typescript
type Pop<T extends unknown[]> =  T extends readonly [...infer Rest, infer Last]  ? Rest : T;
    // Check if T extends an array-like structure with at least one element
    // If so, return the array without its last element
    // If T is empty or not an array, return T as is
    
```

Explanation:
- The `Pop<T>` type takes a generic type parameter `T`, which is expected to be an array or a tuple.
- We use a conditional type to check if `T` extends an array-like structure with at least one element.
- If `T` matches this pattern, meaning it has at least one element, we use tuple spreading (`[...infer Rest, infer Last]`) to destructure `T` into two parts: `Rest`, which represents all elements except the last one, and `Last`, which represents the last element.
- We return `Rest`, which contains all elements of `T` except the last one.
- If `T` is empty or not an array, the conditional type fallback returns `T` as is.


 [Top](#concepts)

---

### PromiseAll Function

Type the function `PromiseAll` that accepts an array of `PromiseLike` objects, returning a promise that resolves to an array with the resolved values of the input promises.

Example

```typescript
const promise1 = Promise.resolve(3);
const promise2 = 42;
const promise3 = new Promise<string>((resolve, reject) => {
  setTimeout(resolve, 100, 'foo');
});

// Expected: Promise<[number, 42, string]>
const p = PromiseAll([promise1, promise2, promise3] as const);
```

Solution

```typescript
// Define a conditional type to unwrap promises
type MyAwaited<T> = T extends PromiseLike<infer R> ? R : T;

// Declare the PromiseAll function
declare function PromiseAll<T extends unknown[]>(
  values: readonly [...T]
): Promise<{ [K in keyof T]: MyAwaited<T[K]> }>;
```

Explanation

1. **MyAwaited Conditional Type**: We define a conditional type `MyAwaited<T>` that checks if the type `T` extends `PromiseLike<infer R>`. If so, it resolves to `R`, otherwise, it resolves to `T`.

2. **PromiseAll Function**: We declare the `PromiseAll` function that accepts an array of unknown types `T` as its argument. We use TypeScript's variadic tuple types to enforce that the input array remains immutable (`readonly`). Inside the function, we map over each element of the input array using a mapped type to unwrap promises using the `MyAwaited` conditional type.

This ensures that the resulting promise resolves to an array containing the resolved values of the input promises.

1. **Define `MyAwaited` Conditional Type**:
   ```typescript
   type MyAwaited<T> = T extends PromiseLike<infer R> ? R : T;
   ```
   - This line declares a conditional type named `MyAwaited`.
   - It takes a type `T` as input.
   - The conditional checks if `T` extends `PromiseLike<infer R>`, where `infer R` represents the type contained within the promise.
   - If `T` extends `PromiseLike`, it resolves to `R`, which is the type inside the promise.
   - If `T` does not extend `PromiseLike`, it resolves to `T` itself.

2. **Declare `PromiseAll` Function**:
   ```typescript
   declare function PromiseAll<T extends unknown[]>(
     values: readonly [...T]
   ): Promise<{ [K in keyof T]: MyAwaited<T[K]> }>;
   ```
   - This line declares the `PromiseAll` function.
   - It accepts an array of unknown types `T` as its argument.
   - The `T` type is constrained to be a tuple using TypeScript's variadic tuple syntax (`T extends unknown[]`).
   - The `values` parameter is marked as `readonly` to ensure immutability of the input array.
   - Inside the function signature, a mapped type is used to iterate over each element of the input array.
   - For each element `T[K]`, the `MyAwaited` conditional type is applied to unwrap any promises contained within.
   - The resulting type is a tuple where each element's type is replaced with its unwrapped type using `MyAwaited`.

Overall, this code defines a conditional type `MyAwaited` to unwrap promises, and declares a `PromiseAll` function that accepts an array of unknown types and returns a promise resolving to an array with the resolved values of the input promises, unwrapping them if necessary.

[Top](#concepts)

--- 

### Type Lookup

we would like to get the corresponding type by searching for the common type field in the union `Cat | Dog`. In other words, we will expect to get `Dog` for `LookUp<Dog | Cat, 'dog'>` and `Cat` for `LookUp<Dog | Cat, 'cat'>` in the following example.

```typescript
interface Cat {
  type: 'cat'
  breeds: 'Abyssinian' | 'Shorthair' | 'Curl' | 'Bengal'
}

interface Dog {
  type: 'dog'
  breeds: 'Hound' | 'Brittany' | 'Bulldog' | 'Boxer'
  color: 'brown' | 'white' | 'black'
}

type MyDogType = LookUp<Cat | Dog, 'dog'> // expected to be `Dog`
```

Solution:

1. **Define the `LookUp` type**:
   ```typescript
   type LookUp<U, T> = U extends { type: T } ? U : never;
   ```
   - Here, `LookUp` is a conditional type that takes two type parameters:
     - `U`: The union type we want to search in.
     - `T`: The type string we want to look up.

2. **Conditional Check**:
   ```typescript
   U extends { type: T } ? U : never;
   ```
   - This conditional type checks if each member of the union type `U` extends an object type with a `type` property equal to `T`.
   - If the condition is true, meaning if the member has a `type` property equal to `T`, it returns the member type `U`.
   - If the condition is false, meaning if the member does not have a `type` property equal to `T`, it returns `never`.

3. **Application**:
   ```typescript
   type MyDogType = LookUp<Cat | Dog, 'dog'>;
   ```
   - Here, `Cat | Dog` is our union type `U`, which consists of `Cat` and `Dog`.
   - We are looking up the type corresponding to the type string `'dog'`.
   - When we pass `Cat | Dog` to `LookUp`, it iterates over each member of the union and checks if it has a `type` property equal to `'dog'`.
   - In this case, only the `Dog` type has a `type` property equal to `'dog'`, so `LookUp` returns `Dog`.

In summary, the `LookUp` type allows us to find the type in a union based on a specified attribute value. It iterates over the union type, checks each member for a matching attribute value, and returns the corresponding member type if found.

[Top](#concepts)

---

### Trim Left

Implement `TrimLeft<T>` which takes an exact string type and returns a new string with the whitespace beginning removed.

```typescript
type trimed = TrimLeft<'  Hello World  '> // expected to be 'Hello World  '
```

Solution:

```typescript
type TrimLeft<S extends string> = S extends `${infer L}${infer R}` ?
  L extends ' ' | '\n' | '\t' ?
    TrimLeft<R> : `${L}${R}` : S;
```

Explanation:

1. **Template Literal Types**: 
   - We use template literal types to destructure the input string `S` into two parts: the first character `L` and the rest of the string `R`.

2. **Conditional Check for Whitespace**:
   - We check if the first character `L` is a space `' '`, newline `'\n'`, or tab `'\t'`.
   - If `L` is a whitespace character, we recursively call `TrimLeft<R>` to continue trimming the string.
   - If `L` is not a whitespace character, we concatenate it with the rest of the string `R` to preserve the non-whitespace characters.

3. **Base Case**:
   - If the input string `S` is empty or does not contain leading whitespace, we return `S` as is.

Example:

```typescript
type trimed = TrimLeft<'  Hello World  '> 
// Expected: 'Hello World  '
```

- For the input string `'  Hello World  '`, the leading whitespace characters are removed, resulting in `'Hello World  '`.
- The expected output matches the behavior of the `TrimLeft` type, as the leading whitespace is correctly removed from the input string.

[Top](#concepts)

---

### Trim

Implement `Trim<T>` which takes an exact string type and returns a new string with the whitespace from both ends removed.

For example:

```typescript
type trimmed = Trim<'  Hello World  '> // expected to be 'Hello World'
```

Solution

```typescript
type Trim<T extends string> = T extends
  | `${" " | "\n" | "\t"}${infer R}`
  | `${infer R}${" " | "\n" | "\t"}`
  ? Trim<R>
  : T;
```

Explanation

- We define a conditional type `Trim<T>` that takes an exact string type `T`.
- The conditional type checks if the string starts or ends with whitespace (space, newline, or tab). If it does, it recursively calls `Trim` on the string without the leading or trailing whitespace.
- If the string does not start or end with whitespace, it returns the original string as is.

Example

```typescript
type Test1 = Trim<'str   '>;
// Expected: 'str'

type Test2 = Trim<'     str     '>;
// Expected: 'str'

type Test3 = Trim<'   \n\t foo bar \t'>;
// Expected: 'foo bar'
```

This solution recursively removes leading and trailing whitespace until none is left, returning the trimmed string.

This should cover all cases where whitespace exists at the beginning or end of the string.

[Top](#concepts)

---

### Capitalize

Implement `Capitalize<T>` which converts the first letter of a string to uppercase and leaves the rest as-is.

For example:

```typescript
type capitalized = Capitalize<'hello world'> // expected to be 'Hello world'
```

Solution

```typescript
type Capitalize<T extends string> = T extends `${infer First}${infer Rest}`
  ? `${Uppercase<First>}${Rest}`
  : T;
```

Explanation

- We define a conditional type `Capitalize<T>` that takes an exact string type `T`.
- The conditional type checks if the string `T` can be split into two parts: the first letter `First` and the rest of the string `Rest`.
- If the string can be split, it converts the first letter `First` to uppercase using the `Uppercase` utility type and concatenates it with the rest of the string `Rest`.
- If the string cannot be split (i.e., it's an empty string or a string with only one character), it returns the original string as is.

Example

```typescript
type Test1 = Capitalize<'hello'>;
// Expected: 'Hello'

type Test2 = Capitalize<'world'>;
// Expected: 'World'

type Test3 = Capitalize<'foo bar'>;
// Expected: 'Foo bar'

type Test4 = Capitalize<'A'>;
// Expected: 'A'
```

This solution ensures that the first letter of the string is converted to uppercase while leaving the rest of the string unchanged.

[Top](#concepts)

---
