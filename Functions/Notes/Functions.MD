# Functions

## Parameter Type Annotations

In TypeScript, function parameters may be given type annotations with the same syntax as variable declarations: a colon next to the  name. The type annotations ensure that the parameters are of the correct type:

```ts
function greet(name: string) {
  console.log(`Hello, ${name}!`);
}

greet('Katz'); // Prints: Hello, Katz  

greet(1337); // Error: argument '1337' is not assignable to parameter of type 'string'
```

By placing `: string` after the `name` parameter, we’re declaring that `name` is of type `string`. Any invocation of `greet()` should pass a value of type `string` as the first argument, or else an error will be thrown.

Parameters that we do not provide type annotations for are assumed to be of type `any`—the same wayvariables are.

```ts
function printKeyValue(key: string, value) {
  console.log(`${key}: ${value}`);
}

printKeyValue('Courage', 1337); // Prints: Courage: 1337
printKeyValue('Mood', 'scared'); // Prints: Mood: scared
```

Here, the parameter `value` is an `any` variable: it’s compatible with any type. 

## Optional Parameters

TypeScript normally gives an error if we don’t provide a value for all  arguments in a function. This isn’t always desirable; sometimes we’d  like to skip providing values.

```ts
function greet(name: string) {
  console.log(`Hello, ${name || 'Anonymous'}!`);
}

greet('Anders'); // Prints: Hello, Anders!
greet(); // TypeScript Error: Expected 1 arguments, but got 0.
```

When the code snippet above is compiled to JavaScript, the `greet()` function will correctly print `'Hello, Anonymous!'`. That’s because when no arguments are passed in, `name` has the falsy value `undefined`, which means that `name || 'Anonymous'` evaluates to `'Anonymous'`.  Since the final code works as intended, we want to prevent TypeScript from throwing errors:

To indicate that a parameter is intentionally optional, we add a `?` after its name. This tells TypeScript that the parameter is allowed to be `undefined` and doesn’t always have to be provided.

```ts
function greet(name?: string) {
  console.log(`Hello, ${name|| 'Anonymous'}!`);
}

greet(); // Prints: Hello, Anonymous!
```

## Default Parameters

If a parameter is assigned a default value,  TypeScript will infer the variable type to be the same as the default  value’s type.  (This is similar to how TypeScript infers the type of an  initialized variable to be the same as the type of its initial value.)

The following code snippet logs a string to greet a user’s name, and defaults to the name `'Anonymous'` if no name is provided. 

```ts
function greet(name = 'Anonymous') {
  console.log(`Hello, ${name}!`);
}
```

The function `greet()` can receive a `string` or `undefined` as its `name` parameter—if any other type is provided as an argument,  TypeScript will consider that a type error. 

This is a cleaner way of getting the same functionality we had in the previous exercise. There, we used `?` to mark the `name` parameter as optional. But parameters with default values don’t need a `?` after their name, since assigning a default value already implies that they’re optional parameters.

## Inferring Return Types

TypeScript can also infer the types of values returned by functions. It does this by looking at the types of the values after a function’s `return` statements.

```ts
function createGreeting(name: string) {
  return `Hello, ${name}!`;
}

const myGreeting = createGreeting('Aisle Nevertell');
```

Here’s how TypeScript can infer `myGreeting` above to be of type `string`:

1. `createGreeting()`’s `return` statement is followed by a  `string` variable, so `createGreeting()` is inferred to return `string`.
2. `createGreeting('Aisle Nevertell')` therefore must result in a value of type `string`.
3. `myGreeting` is initialized to `createGreeting('Aisle Nevertell')`, which is a value with the type `string`.

```ts
function ouncesToCups(ounces: number) {
  return `${ounces / 16} cups`;
}

const liquidAmount: number = ouncesToCups(3);
// Type 'string' is not assignable to type 'number'.
```

Here, TypeScript was able to infer that `liquidAmount` was being assigned a value of type `string`, despite it being declared as a variable of type `number`. This correctly results in an error being surfaced.

## Explicit Return Types

If we’d like to be explicit about what type a function returns, we can  add an explicit type annotation after its closing parenthesis. Here, we  use the same syntax as other type annotations, a colon followed by the type. TypeScript will produce an error for any  return statement in that function that doesn’t return the right type of  value.

```ts
function createGreeting(name?: string): string {
  if (name) {
    return `Hello, ${name}!`;
  }

  return undefined;
  // Typescript Error: Type 'undefined' is not assignable to type 'string'.
};
```

We can also explicitly state return types for arrow functions(which were defined in the ES6/ES2015 version of JavaScript). We’ll see the same kinds of error messages for both function types.

```ts
const createArrowGreeting = (name?: string): string => {
  if (name) {
    return `Hello, ${name}!`;
  }

  return undefined;
  // Typescript Error: Type 'undefined' is not assignable to type 'string'.
};
```

## Void Return Type

By now, we’ve made a pretty convincing case that type annotations are very useful and should always be used unless there’s a very good  reason not to. They make everything neat and aid in understanding our  code.



For these reasons, it is often preferred to use type annotations for functions, even when those functions don’t return anything. Example:

```ts
function logGreeting(name: string){
  console.log(`Hello, ${name}!`)
}
```

The function `logGreeting()` simply logs a greeting to the console. There is no returned value, so we must treat the return type as `void`. A proper type annotation for this function would look like this:

```ts
function logGreeting(name:string): void{
  console.log(`Hello, ${name}!`)
}
```

## Documenting Functions 

TypeScript recognizes JavaScript comment syntax:

```ts
// This is a single line comment 

/*
This is a 
multiline
comment
*/
```

But it’s common in TypeScript to see a third comment style:  documentation comments. A documentation comment is denoted with the  first line `/**` and a final line `*/`. It’s common for each line within the comment to start with an asterisk (`*`):

* ```ts
  /**
  
  * This is a documentation comment
    */
  ```

  

Documentation comments are especially useful for documenting functions. We place a function’s documentation comment in the code directly above  the function declaration. We can use special tags within the comment to  highlight certain aspects of the function. We can use `@param` to describe each of the function’s parameters, and we can use `@returns` to describe what the function returns:

  

```ts
/**

   * Returns the sum of two numbers.
     *
   * @param x - The first input number
   * @param y - The second input number
   * @returns The sum of `x` and `y`
     *
        */
       function getSum(x: number, y: number): number {

    return x + y;

  }
}
```

Many text editors will helpfully display documentation comments, for example, when hovering over a function name.  