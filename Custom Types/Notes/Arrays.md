# Custom Types

## Array Type Annotations

The TypeScript type annotation for array types is fairly straightforward: we put `[]` after the element type.  In this code, `names` is an Array that can only contain `string`s:

```ts
let names: string[] = ['Danny', 'Samantha'];
```

An alternative method is to use the `Array<T>` syntax, where `T` stands for the type.

```ts
let names: Array<string> = ['Danny', 'Samantha'];
```

In the code above, the type, `T`, is `string`.

As we may expect, we get a type error if we try to assign an array of `number`s to a `string[]` variable:

```ts
let names: string[] = [1,2,3]; // Type Error!
```

That was just like an assignment error with primitive types. TypeScript  arrays, however, can also throw errors when elements of the wrong type  are added:

```ts
let names: string[] = ['Damien'];
names.push(666) // Type Error!
```

## Multi-dimensional Arrays 

So far we’ve looked at `string[]`arrays, but we could also have arrays that only contain `number`s (`number[]`) or `boolean`s  (`boolean[]`). In fact, we can make arrays of any type whatsoever. We can also declare multidimensional arrays: arrays of arrays (of some type). 

```ts
let arr: string[][] = [['str1', 'str2'], ['more', 'strings']];
```

Think of the `string[][]` above as short for `(string[])[]`, that is, an array where every element has type `string[]`. We’ll explore more complex array types in later lessons.

The empty array (`[]`) is compatible with any array type: 

```ts
let names: string[] = []; // No type errors.
let numbers: number[] = []; // No type errors.
names.push('Isabella');  
numbers.push(30);
```

## Tuples

So far we’ve looked at arrays with all elements of the same type. But, as we know, JavaScript arrays  are flexible and can have elements of different types. With TypeScript,  we can also define arrays with a fixed sequence of types: 

```ts
let ourTuple: [string, number, string, boolean] = ['Is', 7 , 'our favorite number?' , false]; 
```

In TypeScript, when an array is typed with elements of specific types, it’s called a *tuple*. The tuple above (`ourTuple`) contains the elements: `'Is'`, `7` , `'our favorite number?'` , `false` and the tuple has a type of `[string, number, string, boolean]`. Tuple types specify both the lengths and the orders of compatible tuples, and will cause an error if either of these conditions are not met:

```ts
let numbersTuple: [number, number, number] = [1,2,3,4]; // Type Error! numbersTuple should only have three elements.
let mixedTuple: [number, string, boolean] = ['hi', 3, true] // Type Error! The first elements should be a number, the second a string, and the third a boolean. 
```

As far as JavaScript is concerned, tuples act just like arrays. They both have `.length` properties. We can access (or change) the elements of both using `[index]`.  But despite their similarities, tuples and arrays do not have  compatible types within TypeScript. Specifically, we can’t assign an  array to a tuple variable, even when the elements are of the  correct  type: 

```ts
let tup: [string, string] = ['hi', 'bye'];
let arr: string[] = ['there','there'];
tup = ['there', 'there']; // No Errors.
tup = arr; // Type Error! An array cannot be assigned to a tuple.
```

## Array Type Inference

TypeScript can infer variable types from initial values and return  statements. Even still, we may not know exactly what type inference to  expect when dealing with arrays. For example: 

```ts
let examAnswers= [true, false, false];
```

What is the type of `examAnswers`? It seems it could equally well be `boolean[]` or `[boolean, boolean, boolean]`. In reality, it is always the first of these, since this is the less restrictive type. This enables us to expand the array:

```ts
examAnswers[3] = true; // No type error.
```

Since tuples have fixed lengths, we wouldn’t be able to add additional boolean elements to a tuple:

```ts
let tupleOfExamAnswers: [boolean, boolean, boolean] = [true, false, false];
tupleOfExamAnswers[3] = true; // Type error! The tuple only has 3 elements.
```

We also get the same kind of type inference when we use the `.concat()` method:

```ts
let tup: [number, number, number] = [1,2,3];
let concatResult = tup.concat([4,5,6]); // concatResult has the value [1,2,3,4,5,6].
```

In the code above, TypeScript infers the variable `concatResult` as an array of numbers, not a tuple.

The takeaway here is that type inference returns arrays. When we want tuples, we need to use explicit type annotations

## Rest Parameters

Assigning types to rest parameters  is similar to assigning types to arrays. Here’s a rest parameter example without types:

```ts
function smush(firstString, ...otherStrings){
  let output = firstString;
  for(let i = 0; i < otherStrings.length; i++){
    output = output.concat(otherStrings[i]);
  }
  return output;
}
```

This function concatenates all of its arguments. For example, calling: `smush('hi ', 'there')` returns the value `'hi there'`.” The *rest parameter* `otherStrings` lets the function work with any number of parameters greater than zero: 

```ts
smush('a','h','h','H','H','H','!','!'); // Returns: 'ahhHHH!!'.
```

The function works well, but it is not type safe. We don’t want a misguided coder to make a mistake like `smush(1,2,3)`, when that would cause an error. TypeScript to the rescue! Type annotations for a rest parameter are identical to type annotations for arrays. The function with a correctly typed rest parameter is then:

```ts
function smush(firstString, ...otherStrings: string[]){
  /*rest of function*/
}
```

With this change, TypeScript will treat `otherStrings` as an array of strings. This means that  `smush(1,2,3)` will result in a type error because `[2,3]` is not an array of strings.

## Spread Syntax

Like the finest wines and cheeses, TypeScript’s tuples pair beautifully with JavaScript’s [spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax). This is most useful for function calls that use lots of arguments, like this: 

```ts
function gpsNavigate(startLatitudeDegrees:number, startLatitudeMinutes:number, startNorthOrSouth:string, startLongitudeDegrees: number, startLongitudeMinutes: number, startEastOrWest:string, endLatitudeDegrees:number, endLatitudeMinutes:number , endNorthOrSouth:string, endLongitudeDegrees: number, endLongitudeMinutes: number,  endEastOrWest:string) {
  /* navigation subroutine here */
}
```

The function call `gpsNavigate(40, 43.2, 'N', 73, 59.8, 'W', 25, 0, 'N', 71, 0, 'W')` calculates a route from the Codecademy offices in New York City (40  degrees  43.2 minutes north, 73 degrees 59.8 minutes west) to selected  coordinates in the Bermuda Triangle. We all agree that this function  call is awkward to read. 

Instead, we can use tuple variables that represent the starting and ending coordinates:

```ts
let codecademyCoordinates: [number, number, string, number, number, string] = [40, 43.2, 'N', 73, 59.8, 'W'];
let bermudaTCoordinates: [number, number, string, number, number, string] = [25, 0 , 'N' , 71, 0, 'W'];
```

These tuple type annotations guarantee that the types of the elements will be valid function parameters for `gpsNavigate()`.

Now, we use JavaScript’s spread syntax to write a very readable function call: