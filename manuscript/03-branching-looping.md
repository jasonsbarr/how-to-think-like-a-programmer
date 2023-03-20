# Control Flow

Now that you have a solid understanding of the basic statements and expressions in JavaScript, it's time to look at some of the more complex statements. These statements allow you to implement *control flow* in your programs. You've already seen one way to do code branching (conditional expressions) and one way to repeat code (recursion), but these statements will give you the ability to have greater control over the flow of your programs.

## Getting User Input

Before getting into that, though, let's make it possible to write some more interesting programs getting user input, so we have the ability to not know in advance what values will be present in the execution of our programs. That will make things a little more true-to-life.

We'll do this using the `input` function, which is not built into JavaScript but is part of an NPM package.

First, you'll need to install the package. Make sure you're in the directory for chapter 3 in the course files. When you're in the chapter03 directory, enter:

```bash
npm install @jasonsbarr/simple-io
```

Simple IO is a package I released a couple of years ago that abstracts over some more complicated input/output functionalities in Node.js to make it easier for beginning programmers to use them.

### Using ES2015 Modules

Now you'll need to include the `input` function in your code. First, we need to let Node.js know we want to use the new ES2015 standard modules and imports, not the old style CommonJS `require` function.

#### Wait, What?

To make a long story very, *very* short, JavaScript went decades without a standardized solution for modules. Developers were left largely to their own devices in this regard, so some common patterns emerged. When Node.js came onto the scene in 2009, they needed a solution for modules and importing code. The result was the CommonJS module system, which is based on the `require` function. To include library code in the current file, you would declare a variable as the result of calling `require` with the module as its argument.

In the massive 2015 update to the JavaScript language, which is sometimes called *ES6* (or ECMAScript 6), we finally got a standardized system for modules, importing, and exporting.

Unfortunately, the standard modules are not compatible with CommonJS modules, so you have to let Node.js know you intend for your code to use ES2015 modules.

### Editing `package.json`

When you installed the Simple IO package, NPM created a file called `package.json`. The file should look something like this:

```json
{
  "dependencies": {
    "@jasonsbarr/simple-io": "^0.1.0"
  }
}
```

First, put a comma after the closing brace of the "dependencies" object. Then ENTER to go to the next line. Then add `"type": "module"` to the next line. Your `package.json` should now look like this:

```json
{
  "dependencies": {
    "@jasonsbarr/simple-io": "^0.1.0"
  },
  "type": "module"
}
```

Setting the `type` property to "module" will let Node.js know you intend for the project to use ES2015 modules.

If you don't want to set a property in `package.json`, possibly because you want to have both ES2015 and CommonJS modules as part of the same project, you can change the `.js` file extension to `.mjs` to indicate that file should be an ES2015 module. Likewise, you can change the `.js` extension to `.cjs` to let Node know that file should be handled as a CommonJS module.

### Importing from An ES2015 Module

To import from an ES2015 module, use an `import` statement. In this case, we're importing the `input` function:

```js
import { input } from "@jasonsbarr/simple-io";
```

This will allow you to use the `input` function.

### Breaking Down an `import` Statement

An `import` statement has 3 parts:

1. The `import` keyword
2. The name or names being imported
3. The module being imported from, as a string

In the above case, the name being imported is `input`, which is between curly braces. That's because the Simple IO package uses named exports. If you wanted to import all the functions from Simple IO as a single object, you would do this:

```js
import * as IO from "@jasonsbarr/simple-io";
```

This would give you access to all the functions from the package as methods on the `IO` object.

### Named Exports

The `input` function is exported from Simple IO as a named export, which means it's exported like this:

```js
export { input };
```

In your own code, if you want to use named exports just put the variable names you wish to export between curly braces.

You can also do what we did for the functions in the exercises from the previous chapters, and use the `export` keyword when defining the variable or function:

```js
export function input(prompt) {
    // ...
}
```

Defining a variable or function this way creates a named export.

### Default Exports

You can also use a default export. Default exports are for when you have a single main export you want to use, and you do this by simply adding the `default` keyword to the `export` statement. If I were to provide a default export for the entire Simple IO package, it might look like this, assuming IO is an object whose methods are all the functions from the library:

```js
export default IO;
```

If I didn't have such an object already constructed, I could do this:

```js
export default {
    input,
    // ...
};
```

The syntax for importing a default export is similar to the syntax for importing all a package's named exports at once, only without the `*`. Here's an example using the popular React UI library:

```js
import React from "react";
```

You can also use both default and named exports at the same time:

```js
import React, { useState, useEffect } from "react";
```

To make matters even more potentially confusing, if you did this when importing the React library:

```js
import * as React from "react";
```

you would get all the named exports as properties on the `React` object, **plus** a property named `default` with the value of the default export.

Understanding when to import named vs. default exports can be tricky, and you'll have to rely on a package's documentation to know when to use which one. I don't personally like default exports very much, so I don't use them. Not everyone shares my opinion though, so you'll have to be familiar with both.

In this book we will only `export` with named exports.

### The Input Function

## Conditional Statements

### Constant Time Algorithms

## Iteration

### While Loops

### For Loops

### For...Of Loops

### Linear Time Algorithms

### Quadratic Time Algorithms

## Errors and Error Handling

### Throwing Errors

### Using `try`/`catch`/`finally`

## JavaScript Evaluation Model

### The Evaluation of A Function Call

The act of executing a function with some number of arguments is called "calling" the function. Sometimes you'll also hear about "applying" a function to its arguments. Either is a valid way to talk about it.

We can think of calling a function as a process of substitution.

First, when we encounter the name of a function in a call expression, we substitute the code of the function's body for the name of the function itself, as if the code had been written again.

Let's say we have this function:

```js
function absoluteValue(num) {
    return (num < 0) ? -num : num;
}
```

If we have this variable declaration, which includes calling the `absoluteValue` function:

```js
let value = absoluteValue(-(8 + 7));
```

this is substituted for the call expression:

```js
{
    return (num < 0) ? -num : num;
}
```

Next, we replace every instance of a function parameter inside the body with the expression given as an argument in the position where that formal parameter occurred.

So now we have:

```js
{
    return (-(8 + 7) < 0) ? -(-(8 + 7)) : -(8 + 7);
}
```

Then we evaluate each argument expression one at a time, from left to right as they are given between the parentheses of the call expression.

```js
// absoluteValue(-(8 + 7)) was given, so now we have
absoluteValue(-15);
```

After that, we substitute the evaluated value of each argument for every case where the parameter name it's replaced occurs in the function body.

```js
{
    return (-15 < 0) ? -(-15) : -15;
}
```

Finally, we execute the body of the function with the code and values produced by these substitutions.

If the execution of the function ends with a return statement, we substitute the code of the function body with the value of the return statement's expression. If a return statement is not executed, we use `undefined`. So now we have `15` as the value produced by evaluating the call expression.

Then we continue executing the code from top to bottom with the return value in place as if it had been there all along, before executing the function.

```js
let value = 15;
```

### Execution Contexts, Environments, and the Call Stack

### Uncaught Errors and Unwinding The Stack

## Recap

## Exercises
