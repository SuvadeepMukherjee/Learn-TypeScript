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