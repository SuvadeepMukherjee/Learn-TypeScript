# The tsconfig.json file 

TypeScript features are extremely useful: for one, it allows us to add types to  regular JavaScript code. It also checks for syntax errors even before  run time. It even provides tooltips that show you why some code might  throw an error. There are so many great features that come by default.  Even with all these great features, TypeScript recognizes the need for  flexibility. 

Sometimes, you don’t want all the [default rules](https://www.typescriptlang.org/docs/handbook/compiler-options.html) that TypeScript is trying to enforce — and that’s fine. That’s one reason why providing a **tsconfig.json** file is useful. Additionally, you get perks like telling the TypeScript compiler what files to run on and more! 

## Sample tsconfig.json and Breakdown

The **tsconfig.json** file is  always placed in the root of your project and we can customize what  rules we want the TypeScript compiler to enforce. Here’s a sample  **tsconfig.json** file 

```
{
  "compilerOptions": {
    "target": "es2017",
    "module": "commonjs",
    "strictNullChecks": true
  },
  "include": ["**/*.ts"]
}
```

In the JSON, there are several properties:

- ```
  "compilerOptions"
  ```

  , which is a nested object that contains the rules for the TypeScript compiler to enforce.

  - `"target"`, the value `"es2017"` means the project will be using the 2017 version of EcmaScript standards for JavaScript.
  - `"module"`, this project will be using `"commonjs"` syntax to import and export modules.
  - `"strictNullChecks"`, variables can only have `null` or `undefined` values if they are explicitly assigned those values.

- `"include"` that determines what files the compiler applies the rules to. In this case `["**/*.ts"]` means the compiler should check every single file that has a **.ts** extension.

## Usage

Another neat addition is that by including a **tsconfig.json** file, we can now use the command `tsc` without any arguments in your terminal! The compiler will automatically recognize from your **tsconfig.json** file, what specific files to run on. We can still provide specific files like `tsc fileName.ts` if that’s the only file you want the compiler to check. 

