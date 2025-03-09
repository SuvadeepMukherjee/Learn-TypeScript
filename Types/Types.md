# Types



## From JavaScript to TypeScript 

- Stricter programming languages will inform the developer when they change one area of code in a way that will break other areas . JavaScript will not which often leads to unexpected behaviour at runtime
- To address these shortcomings ,Microsoft developed TypeScript and released publicly in 2012 to blend the flexibility of JavaScript with the advantages of a stricter language 

## What is TypeScript ? 

- We write Typescript code in files with the extension .ts
- We run our code through the TypeScript transpolar .The transpilar will check that the code adheres to TypeScripts standards and it will display errors when it does not 
- If the TypeScript code can be converted into working JavaScript , the transpilar will output a JS version of the file(.js)
- TypeScript code is a superset of Javascript code -it has all the features of traditional js but adds some new features 
- The TypeScript transcompiler can be used on the command line by running the `tsc` command (This will create an equivalent .js file in the same directory as well as surface any errors found by the TypeScript transcompiler)  

## Type Inferences

- When we declare a variable with an initial value , the variable can never be reassigned a value of a different data type .This is an example of Type Inference : everywhere in our program , TypeScript expects the data type of the variable to match the type of the value initially assigned to it at declaration

## Type Shapes

- Because TypeScript knows what *types* our objects are, it also knows what *shapes* our objects adhere to. An object’s shape describes, among other things, what  properties and methods it does or doesn’t contain.
- The built-in types in JavaScript each have known properties and methods that always exist. All `string`s, for example, are known to have a `.length` property and `.toLowerCase()` method.

```ts
"OH".length; // 2
"MY".toLowerCase(); // "my"
```

- TypeScript’s `tsc` command will let you know if your code tries to access properties and methods that don’t exist:

```ts
"MY".toLowercase();
// Property 'toLowercase' does not exist on type '"MY"'.
// Did you mean 'toLowerCase'?
```

- Through this knowledge of type shapes, TypeScript helps us quickly locate bugs in our code. 

## Any

There are some places where TypeScript will  not try to infer what type something is—generally when a variable is  declared without being assigned an initial value. In situations where it isn’t able to infer a type, TypeScript will consider a variable to be  of type `any`. 

Variables of type `any` can be assigned to *any* value and TypeScript won’t give an error if they’re reassigned to a different type later on.

```ts
let onOrOff;

onOrOff = 1;
onOrOff = false;
```

In the code above, we declared the variable `onOrOff` without an initial value. TypeScript considers it to be of type `any`, and, therefore, doesn’t produce an error when we change the variable’s assignment from a `number` value to a `boolean` value. 

## Variable Type Annotations

In some situations, we’d like to declare a  variable without an initial value while still ensuring that it will only ever be assigned values of a certain type. If left as `any`, TypeScript won’t be able to protect us from accidentally assigning a variable to an incorrect type that could break our code. 

We can tell TypeScript what type something is or will be by using a *type annotation*. 

Variables can have *type annotations*  (also known as type declarations) added just after their names.  We  provide a type annotation by appending a variable with a colon (`:`) and the type (e.g., `number`, `string`, or `any`). 

```ts
let mustBeAString : string;
mustBeAString = 'Catdog';

mustBeAString = 1337;
// Error: Type 'number' is not assignable to type 'string'
```

In the code above, we explicitly declare `mustBeAString` to be of type `string` without assigning it an initial value. This enables us to assign it the value `'Catdog'` without complaint, but when we later attempt to assign it a numerical  value, TypeScript will give us an error message telling us that a `number` is being improperly assigned to a variable of type `string`.

Note => Type Annotations get automatically removed when comiled to JavaScript 