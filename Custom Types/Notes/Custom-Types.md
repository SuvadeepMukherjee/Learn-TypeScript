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

