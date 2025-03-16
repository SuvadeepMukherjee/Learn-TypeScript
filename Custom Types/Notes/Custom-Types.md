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
