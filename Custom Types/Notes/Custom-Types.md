# Custom Types 

## Enums

Our first example of a complex type is also one of the most useful: *enums*.  We use enums when we’d like to **enum**erate all the possible values that a variable could have. This is in contrast to most of the other types we have studied. A variable of the `string` type can have any string as a value; there are infinitely many possible strings, and it would be impossible to list them all. Similarly, a  variable of the `boolean[]` type can have any array of booleans as its value; again, the possibilities are infinite. 

```ts
enum Direction {
  North,
  South,
  East,
  West
}
```

There are many situations when we might want to limit the possible  values of a variable. For example, the code above defines the enum `Direction`, representing four compass directions: `Direction.North`, `Direction.South`, `Direction.East`, and `Direction.West`. Any other values, like `Direction.Southeast`, are not allowed. Check out the example below: 

```ts
let whichWayToArcticOcean: Direction;
whichWayToArcticOcean = Direction.North; // No type error.
whichWayToArcticOcean = Direction.Southeast; // Type error: Southeast is not a valid value for the Direction enum.
whichWayToArcticOcean = West; // Wrong syntax, we must use Direction.West instead. 
```

As shown above, an enum type can be used in a type annotation like any other type.

Under the hood, TypeScript processes these kinds of enum types using `number`s. Enum values are assigned a numerical value according to their listed order. The first value is assigned a number of `0`, the second a number of `1`, and onwards

For example, if we set `whichWayToArticOcean = Direction.North`, then `whichWayToArticOcean == 0` evaluates to `true`. Furthermore, we can reassign `whichWayToArticOcean` to a number value, like  `whichWayToArticOcean = 2`, and it does not raise a type error. This is because `Direction.North`, `Direction.South`, `Direction.East`, and `Direction.West` are equal to 0, 1, 2, and 3, respectively. 

We can change the starting number, writing something like 

```ts
enum Direction {
  North = 7,
  South,
  East,
  West
}
```

Here, `Direction.North`, `Direction.South`, `Direction.East`, and `Direction.West` are equal to 7, 8, 9, and 10, respectively. 

We can also specify all numbers separately, if needed: 

```ts
enum Direction {
  North = 8,
  South = 2,
  East = 6,
  West = 4
}
```

## String Enums vs Numeric Enums

The enums we have studied so far are referred to as *numeric enums*, since they are based on `number`s. TypeScript also allows us to use enums based on `string`s, referred to as *string enums*. They are  defined very similarly:

```ts
enum DirectionNumber { North, South, East, West }
enum DirectionString { North = 'NORTH', South = 'SOUTH', East = 'EAST', West = 'WEST' }
```

With numeric enums, the numbers could be assigned automatically, but  with string enums we must write the string explicitly, as shown above.  Technically, any string will do: `North = 'JabberWocky'` is a valid value definition. However, it is much better to use the convention shown here (`North = 'NORTH'`), where the string value of the enum variable is just the capitalized  form of the variable name. This way, error messages and logs will be  much more informative.

It is recommend to always use string enums  because numeric enums allow for some behaviors that can let bugs sneak  into our code. For example, numbers can be assigned directly to numeric  enum variables:   

```ts
let whichWayToAntarctica: DirectionNumber;
whichWayToAntarctica = 1; // Valid TypeScript code.
whichWayToAntarctica = DirectionNumber.South; // Valid, equivalent to the above line.
```

Strangely, even assigning arbitrary numbers, as in `whichWayToAntarctica = 943205`, will not lead to type errors. 

String enums are *much* more strict. With string enums, variables cannot be assigned to strings at all!

```ts
let whichWayToAntarctica: DirectionString;
whichWayToAntarctica = '\ (•◡•) / Arbitrary String \ (•◡•) /'; // Type error!
whichWayToAntarctica = 'SOUTH'; // STILL a type error!
whichWayToAntarctica = DirectionString.South; // The only allowable way to do this.
```

## Object Types

TypeScript’s *object types* are extremely useful, as they allow  us extremely fine-level control over variable types in our programs.  They’re also the most common custom types, so we’ll have to understand  them if we want to read other people’s programs.

Here’s a type annotation for an object meant to represent a person:

```ts
let aPerson: {name: string, age: number};
```

The type annotation looks like an object literal, but instead of values appearing after properties, we have types.  Notice that the variable `aPerson` has yet to be assigned a value. Trying to assign a value to `aPerson` that doesn’t have `name` and `age` properties of the specified types will lead to a type error:

```ts
aPerson = {name: 'Aisle Nevertell', age: "wouldn't you like to know"}; // Type error: age property has the wrong type.
aPerson = {name: 'Kushim', yearsOld: 5000}; // Type error: no age property. 
aPerson = {name: 'User McCodecad', age: 22}; // Valid code. 
```

Above, in the case of Kushim, the object had  properties of the correct types. Still, a type error was thrown because  the properties didn’t have the correct names. 

TypeScript places no restrictions on the types of an object’s properties. They can be enums, arrays, and even other object types!

```ts
let aCompany: {
  companyName: string, 
  boss: {name: string, age: number}, 
  employees: {name: string, age: number}[], 
  employeeOfTheMonth: {name: string, age: number},  
  moneyEarned: number
};
```

##  Type Aliases

One great way to customize the types in our programs is to use *type aliases*. These are alternative type names that we choose for convenience. We use the format `type <alias name> = <type>`:

```ts
type MyString = string;
let myVar: MyString = 'Hi'; // Valid code.
```

Coming up with alternate names for `string` may not be very useful, but this can be done with any type whatsoever.  Type aliases are truly useful for referring to complicated types that  need to be repeated, especially object types and tuple types.  Recall  our earlier company example:

```ts
let aCompany: { 
  companyName: string, 
  boss: { name: string, age: number }, 
  employees: { name: string, age: number }[], 
  employeeOfTheMonth: { name: string, age: number },  
  moneyEarned: number
};
```

There’s so much needless repetition here! (And the more times we repeat  something, the more opportunity there is for typos.) This can be cleaned up with type aliases:

```ts
type Person = { name: string, age: number };
let aCompany: {
  companyName: string, 
  boss: Person, 
  employees: Person[], 
  employeeOfTheMonth: Person,  
  moneyEarned: number
};
```

Everyone knows the famous Shakespeare quotation “What’s in a name? That which we call a `string` by any other name would smell as sweet”. TypeScript aliases are nothing more than names. They have absolutely no influence over how types work. For example, the following code does not lead to type errors:

```ts
type MyString = string; 
type MyOtherString = string;
let firstString: MyString = 'test';
let secondString: MyOtherString = firstString; // Valid code.
```

The reason this works is that `MyString` and `MyOtherString` are not distinct types. They are just alternative names for the same thing. 

Using type aliases, we can make our code much simpler to understand. 

## Function Types

One of the neat things about JavaScript is that functions can be assigned to variables

```ts
let myFavoriteFunction = console.log; // Note the lack of parentheses.
myFavoriteFunction('Hello World'); // Prints: Hello World
```

One of the neat things about TypeScript is that we can precisely control the kinds of functions assignable to a variable. We do this using *function types*, which specify the argument types and return type of a function. Here’s  an example of a function type that is only compatible with functions  that take in two string arguments and return a number.

```ts
type StringsToNumberFunction = (arg0: string, arg1: string) => number;
```

This syntax is just like arrow notation for functions, except instead of the return value we put the return type. In this case, the return type  is `number`. Because this is just a type, we did not write the function body at all. A variable of type `StringsToNumberFunction` can be assigned any compatible function:

```ts
let myFunc: StringsToNumberFunction;
myFunc = function(firstName: string, lastName: string) {
  return firstName.length + lastName.length;
};

myFunc = function(whatever: string, blah: string) {
  return whatever.length - blah.length;
};
// Neither of these assignments results in a type error.
```

As we can see above, it doesn’t matter what we name the function parameters, so long as they have the correct types (`string    ` and `string`). Therefore, it doesn’t matter what we name the parameters in the type annotation (above, we chose `arg0` and `arg1`).

There’s something **important**  to remember here. We must never be tempted to omit the parameter names  or the parentheses around the parameters in a function type annotation,  even if there is only one parameter. This code will not run!

```ts
type StringToNumberFunction = (string)=>number; // NO
type StringToNumberFunction = arg: string=>number; // NO NO NO NO
```

Function types are most useful when applied to [callback functions](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function).

## Generic Types

TypeScript’s *generics* are ways to create collections of types  (and typed functions, and more) that share certain formal similarities.  These collections are parameterized by one or more type variables. Now  that that’s cleared up, let’s move on to the review!

Hmm, maybe we should discuss this in a bit more detail. Actually, we have already seen an example of a *generic type* being used. Remember the array type syntax `Array<T>`? This is generic because we can substitute any type (either pre-defined or custom)  in the place of `T`. For example `Array<string>` is an array of strings.

Generics give us the power to define our own collections of object types. Here’s an example:

```ts
type Family<T> = {
  parents: [T, T], mate: T, children: T[]
};
```

This code defines a collection of object types, with a different type for every value of `T`. The generic `Family<T>` cannot actually be used as a type in a type annotation. Instead, we must substitute `T` with some type of our choosing, for example `string`.   Then, `Family<string>` is *exactly* the same as the object type given by setting `T` to `string`: `{parents:[string,string], mate:string, children: string[]}`. So the following assignment will be error free:

```ts
let aStringFamily: Family<string> = {
  parents: ['stern string', 'nice string'],
  mate: 'string next door', 
  children: ['stringy', 'stringo', 'stringina', 'stringolio']
}; 
```

In general, writing generic types with  `type typeName<T>` allows us to use `T` within the type annotation as a type placeholder. Later, when the generic type is used, `T` is replaced with the provided type. (Writing `T` is just a convention. We could just as easily use `S` or `GenericType`. )

## Generic Functions 

We can also use generics to create collections of typed functions. *Generic functions* like these are probably easiest to understand via an example. And for  once, the example is actually useful! Imagine we wanted to create a  function that returns arrays filled with a certain value. Let’s just  write the JavaScript for now:

```ts
function getFilledArray(value, n) {
  return Array(n).fill(value);
}
```

Here, `getFilledArray('cheese', 3)` evaluates to  `['cheese', 'cheese', 'cheese']`. No problem, right? Well, we run into a problem when we try to specify  the function’s return type. We know it should be an array of whatever `value`‘s type is—do we have to write a separate type annotation for every type of `value`? Nope. Here, we are rescued by generic functions! 

```ts
function getFilledArray<T>(value: T, n: number): T[] {
  return Array(n).fill(value);
}
```

The above code tells TypeScript to make sure that `value` and the returned array have the same type `T`. When the function is invoked, we will provide `T`‘s value. For example, we can invoke the function using `getFilledArray<string>('cheese', 3)`, which sets `T` equal to `string`. This still evaluates to  `['cheese', 'cheese', 'cheese']`, but the function is now correctly typed and less prone to errors. The function `getFilledArray<string>` is precisely the same as if we had written `(value: string, n: number): string[]` in its type annotation.

In general, writing generic functions with  `function functionName<T>` allows us to use `T` within the type annotation as a type placeholder. Later, when the function is invoked, `T` is replaced with the provided type. 
