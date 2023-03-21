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

If you're using ES2015 modules in Node.js, you can `import` a CommonJS module and it will work; however, if you're using CommonJS you **cannot** `require` an ES2015 module. For this reason, I always use ES2015 modules in my Node.js projects.

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

Note that the string of the module being imported can be the name of a module that's installed in the `node_modules/` directory in your project (this is where NPM installs modules by default), a relative or absolute path to a file to import, or a URL.

In the above case, the name being imported is `input`, which is between curly braces. That's because the Simple IO package uses named exports.

If you don't like the name `input` for the function, you can change it like this:

```js
import { input as inp } from "@jasonsbarr/simple-io";
```

If you wanted to import multiple functions from the library, you just separate them with commas:

```js
import { input, readFile, writeFile } from "@jasonsbarr/simple-io";
```

If you wanted to import all the functions from Simple IO as a single object, you would do this:

```js
import * as IO from "@jasonsbarr/simple-io";
```

This would give you access to all the functions from the package as methods on the `IO` object. You don't have to call the object `IO`; you can give it any name you like.

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

You can use any name you want when importing.

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

### Importing in The Browser

If you want to use ES2015 modules in the browser, you have to give your `script` element a `type` of "module":

```html
<script src="module.js" type="module">
```

Now you can use `import` statements in `module.js`.

Note that the browser will **not** resolve modules to the `node_modules/` directory, so you'll need to provide specific paths to any files you want to `import` in files your browser loads.

Alternatively, you can use a bundler like Webpack or Parcel, which we'll cover later in the book.

### Imports Must Be Top-Level Statements

One last thing to note about ES2015 `import` statements: they must be at the top level of a module. That means they can't be inside another statement, like the `if` statements we'll look at in this chapter. You can't put them inside of a block like this:

```js
{
    import { thisWillFail } from "nope";
}
```

### The Input Function

We've imported our `input` function as a named export from an ES2015 module:

```js
import { input } from "@jasonsbarr/simple-io";
```

To use the function, simply call it with a textual prompt as an argument. When you run the program, it will show the prompt to the user and wait for input.

The function receives its input from the command line as a string, which means you'll have to cast it to any other type you need.

Here's a sample program to try it out:

```js
const name = input("What is your name? ");
console.log("I am " + name);

const quest = input("What is your quest? ");
console.log(quest);

const velocity = input("What is the airspeed velocity of an unladen swallow? ");
console.log(Number(velocity));
```

The output might look something like this:

> What is your name? I am Arthur, King of the Britons
> What is your quest? I seek the Holy Grail!
> What is the airspeed velocity of an unladen swallow? NaN

The answer to the 3rd question is, of course, `NaN` because JavaScript can't convert "Is that an African or a European swallow?" to a number.

It would be nice if we had a way to check and make sure the answer to the 3rd question is a valid number. That's where conditional statements come in!

## Conditional Statements

You've already seen the conditional expression, so you're probably wondering how conditional statements are different.

The most basic difference is simply that the former is an expression, which means it evaluates to a specific value. The latter is a statement, so it is executed.

Another difference is that each branch of a conditional expression can only be a single expression, whereas each branch of a conditional statement can be a block statement that contains multiple statements.

Finally, with conditional statements the alternate (`else`) branch is optional, whereas with a conditional expression you must have both branches.

You create a conditional statement with the `if` keyword, followed by an expression that evaluates to a truthy or falsy value, followed by a statement.

```js
if (x > 10) {
    console.log("It is greater than 10");
    return true;
}
```

You can add an optional alternative branch:

```js
if (x > 10) {
    console.log("It is greater than 10");
    return true;
} else {
    console.log("It is not greater than 10");
    return false;
}
```

Note that if the body of either the `if` or `else` branch is a single statement, you can omit the curly braces and just write the single statement. I recommend always using the curly braces though, so that way if you want to add additional statements later you don't get confused. I've seen that happen a lot! If you don't have the curly braces and then try to add additional statements, the interpreter will look at it like this:

```js
// this is what you have:
if (someCondition)
    statementOne;
    statementTwo;
    statementThree;

// this is what the interpreter sees:
if (someCondition) {
    statementOne;
}
statementTwo;
statementThree;
```

So the 2nd and 3rd statements will be executed every time regardless of whether the condition is met, which probably isn't what you want. Remember that white space (e.g. spaces and tabs) means nothing to the interpreter. Unlike in Python, indenting your code nicely only helps other developers who look at your code later (which includes you 6 months from now). It doesn't actually tell the interpreter anything about your intent.

#### Chaining Conditionals

You can also chain multiple conditional statements together with `else if`.

```js
if (x > 10) {
    console.log("it is greater than 10");
} else if (x < 10) {
    console.log("It is less than 10");
} else {
    console.log("It must be exactly 10");
}
```

You can use as many `else if` statements as you need. This, however, is probably overkill:

```js
if (x === 0) {
    console.log("it is zero");
} else if (x === 1) {
    console.log("it is one");
} else if (x === 2) {
    console.log("it is two");
} // and so on...
```

If you're checking for a specific value, a `switch` statement might be a better fit. We'll look at those later in the chapter.

### Checking for Numeric Input

Now, if you want to get a number from your user with the `input` function, you can use a conditional statement.

```js
import { input } from "@jasonsbarr/simple-io";

let age = Number(input("How old are you? "));

if (Number.isNaN(age)) {
    console.log("You must enter a valid number!");
    age = Number(input("How old are you? "));
}
```

As you can see, we still have an issue here: if the user again enters invalid input, we have no way to prompt them a second time for a valid number!

We'll see how to deal with this later in the chapter.

## Switch Statements

A `switch` statement is similar to a conditional statement, except that instead of checking a different condition for each clause you check a single value and switch based on the value itself.

Each possible value gets a `case` clause, with an optional `default` clause to handle all values not specified by cases. I strongly recommend that you always use a `default` case, even (perhaps especially) when you don't think you need one.

Here's what that can look like:

```js
const name = input("Which direction would you like to go? ");

switch (name.toUpperCase()) {
    case "N":
        console.log("You're going north!");
        break;
    case "S":
        console.log("You're going south!");
        break;
    case "E":
        console.log("You're going east!");
        break;
    case "W":
        console.log("You're going west!");
        break;
    default:
        console.log("That is not a valid direction");
        break;
}
```

Note the use of a `break` statement in every case. If you don't include either the `break` statement or a `return` statement in your case, the interpreter will fall through and execute the next case, which usually isn't what you want. If you omitted the `break` statements above and the user entered "N," this would be your output:

> You're going north!
> You're going south!
> You're going east!
> You're going west!
> That is not a valid direction

Obviously that's not the output you were expecting.

Note that the interpreter will not require you to use a block statement with curly braces for cases that include multiple statements, though if you have enough of them in a case you'll probably want to use braces so the code is easier to read. I use braces if I have 3 or more statements in a case clause.

## Constant Time Algorithms

## Iteration

### While Loops

### For Loops

### For...Of Loops

## Linear Time Algorithms

## Quadratic Time Algorithms

## Errors and Error Handling

### Throwing Errors

### Using `try`/`catch`/`finally`

## JavaScript Evaluation Model

### Execution Contexts and The Call Stack

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

Next, we evaluate each argument expression one at a time, from left to right as they are given between the parentheses of the call expression:

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

### Uncaught Errors and Unwinding The Stack

## Recap

## Exercises
