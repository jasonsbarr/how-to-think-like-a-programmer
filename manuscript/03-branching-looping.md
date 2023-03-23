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
import { input as gimme } from "@jasonsbarr/simple-io";
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
/**
 * Get input from a user
 * @param {string} prompt
 * @returns {string}
 */
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

One more thing to note about ES2015 `import` statements: they must be at the top level of a module. That means they can't be inside another statement, like the `if` statements we'll look at in this chapter. You can't put them inside of a block like this:

```js
{
    import { thisWillFail } from "nope";
}
```

### Importing a Module Executes All Code in The Module

Finally, it's important to remember that when you import a module all the code in that module will be executed. This includes both the declaration of any functions or variables exported from the module as well as any other code not contained in a definition.

The JavaScript interpreter keeps track of which modules have been executed, so you can import the same module into multiple different files (each of which is also a module) without executing the code multiple times. This is important because if the module contained code that did something like writing to a database you can be sure the code will only execute once, so there won't be multiple writes to the database.

A file that is not an ES2015 module is treated as a script by the JavaScript interpreter, and its code executes in the global execution context. I'll explain execution contexts at the end of this chapter.

### The Input Function

We've imported our `input` function as a named export from an ES2015 module:

```js
import { input } from "@jasonsbarr/simple-io";
```

To use the function, simply call it with a textual prompt as an argument. When you run the program, it will show the prompt to the user and wait for input.

The function receives its input from the command line as a string, which means you'll have to cast it to any other type you need.

Here's a sample program to try it out. Note the extra space at the end of the argument, so the user's input isn't right up against the end of the prompt:

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

You can add an optional alternative branch with the `else` keyword:

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

Remember our problem from earlier? If you want to make sure get a number from your user with the `input` function, now you can use a conditional statement.

```js
import { input } from "@jasonsbarr/simple-io";

let age = Number(input("How old are you? "));

if (Number.isNaN(age)) {
    console.log("You must enter a valid number!");
    age = Number(input("How old are you? "));
}
```

Here, we use the `Number.isNaN` method to check if the result of casting their input to a number is `NaN`.

You can see documentation for the `Number.isNaN` method at [the MDN Number page](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number), as well as all the other methods and properties you can use with number types and the Number constructor.

As you can see, though, we still have an issue here: if the user again enters invalid input, we have no obvious way to prompt them a second time for a valid number!

We'll see how to deal with that later in the chapter.

## Switch Statements

A `switch` statement is similar to a conditional statement, except that instead of checking a different condition for each clause you check a single value and execute a case based on the value itself.

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

Let's take a break from new syntax for a minute and talk about algorithms.

You'll remember from the introduction to this book than an algorithm is the specific set of steps used to compute a result from input.

Algorithms can be categorized in a number of ways. In this book we'll mostly talk about *time complexity*, which is a fancy way of saying how long it takes for an algorithm to run.

We could also talk about *space complexity*, which means the amount of memory it takes to run an algorithm. When one increases the other often does as well, but not always.

There are a lot of different ways we could measure this.

We could simply time how long it takes to run a computation:

```js
const start = Date.now();
someLongRunningComputation(lotsOfInput);
const end = Date.now();

console.log("That took " + end - start + " milliseconds to run.");
```

That tells us how long it took to run the computation, but unfortunately it doesn't tell us very much about the algorithm itself. For the above case, if you want to run it more efficiently all you have to do is run it on a machine with a faster processor.

When talking about time complexity, programmers tend to discuss algorithms in terms of the number of steps it takes to perform a computation relative to the amount of input given to the function that performs it.

The key here is to understand that we want to measure the efficiency of the algorithm itself, not the actual elapsed time it takes to make the computation. A more efficient algorithm will run faster on a given machine than a less efficient one, regardless of how powerful the computer itself that's running the computation is.

Before we introduced conditional statements to our arsenal we could only write very boring programs. Nothing more than a sequence of single instructions. A program simply took as long to run as there were steps to execute. This is known as a *constant time algorithm*. If a computation takes *k* steps to perform, then its running time is constant because the input to the function doesn't matter. Regardless of whether the input is huge or tiny, the computation still takes the same number of steps.

With the addition of conditional statements, things get a little more interesting. Now the number of steps performed depends on which branch of a conditional is taken.

However, our algorithms are still constant time even taking this into account. That's because the upper bound of the algorithm's running time is still defined by the number of steps it executes, and not by the input to the program.

To describe time complexity we use *Big O notation*. O is short for "order of approximation," and it characterizes a function according to its growth rate based on input.

Since a constant time function doesn't change based on input, we say a constant time function is O(1).

It doesn't matter how many steps are in the function; we still approximate the time complexity as 1.

We need to add the ability to repeat code in order to create algorithms that are other than constant time. We've already seen one way to introduce repetition into our code, with recursion. Now we'll look at another way to do it, one you'll probably see much more often in actual code.

## Iteration

You learned above how to use a conditional statement to make sure a user's input was a valid number; however, the problem remained that if they entered invalid input more than once we didn't know how to enforce the rule multiple times.

One thing we could try is to simply keep nesting conditionals and hope they eventually enter the correct input before we run out of `if` statements:

```js
let age = Number(input("How old are you? "));

if (Number.isNaN(age)) {
    age = Number(input("How old are you? "));

    if (Number.isNaN(age)) {
        age = Number(input("How old are you? "));

        if (Number.isNaN(age)) {
            age = Number(input("How old are you? "));

            // and so on...
        }
    }
}
```

"Okay," I hear you thinking. "That kinda works, but it's ridiculous." And you're absolutely right. Doing it this way violates one of the most important guidelines for programmers: *DRY*, or "Don't Repeat Yourself." If you find yourself copying and pasting code, or writing the same exact (or very similar) code again and again, you're probably missing out on an opportunity to abstract away some bit of functionality into its own function that could be used in multiple places.

Plus there's no way to be sure you'll have enough `if` checks for them to finally get it right! Even if you nest 1,000 conditional statements with the exact same prompt, they could always try the wrong input 1,001 times and then you're back to square one.

If you're thinking to yourself, "Didn't we already introduce a way to repeat code when we looked at recursive functions in the last chapter?" then you're absolutely right and kudos to you. Recursion is definitely one way we could tackle this problem:

```js
/**
 * Gets a number from user input
 * @param {string} userInput
 * @returns {number}
 */
function getAge(userInput) {
    const value = Number(userInput);

    if (Number.isNaN(value)) {
        return getAge(input("How old are you? "));
    }

    return value;
}

const age = getAge(input("How old are you? "));
console.log("You are " + age + " years old!");
```

Note that we don't need an `else` clause above because the `return` statement in the `if` block ensures that the rest of the function won't be executed.

There's absolutely nothing wrong with this solution, and good for you if you thought of it yourself.

There's another way to handle the problem though. That's by using *iteration*.

Iteration and recursion are very similar. Both give you ways to repeat bits of code until a condition is met. In fact, I've even heard computer scientists call iteration a special case of recursion, and vice versa. But in my experience programmers are more likely to use iteration to solve most problems, especially when performance is an issue. With recursion, you have a little bit of extra processing overhead that results from calling a function again. It's rarely an issue in practice, but there are occasional cases where it makes a difference (mostly when processing absolutely huge amounts of data or performing a massive number of repetitions).

Plus for beginning programmers iteration often seems more intuitive than recursion because thinking recursively can be more complex than thinking in terms of iteration.

Iteration means putting your code into a loop that runs repeatedly until a condition is met. There are two kinds of loops in JavaScript: `while` loops and `for` loops.

### While Loops

While loops are more general than for loops, because you simply loop while a condition is met. You're responsible for handling everything that happens in order to make sure the condition actually ceases to be true at some point. If the condition never becomes falsy, then your code will loop forever. This is called an infinite loop.

To write a while loop, simply use the keyword `while` followed by an expression in parentheses that evaluates to a truthy or falsy value, then another statement. I recommend using a block statement for the same reason as with `if` statements above: if you want to add additional statements to your loop body, the interpreter won't know to include them as part of the loop without the curly braces.

```js
while (condition) {
    statementOne;
    statementTwo;
    statementThree;
}
```

As long as `condition` is truthy, the loop will continue to execute.

One way to set up a loop is to use a counter and break out of the loop once the counter hits a certain value:

```js
// counts up from 0 to 9
let i = 0;
while (i < 10) {
    console.log(i);
    i++;
}
```

Note that the above loop will stop running once `i` is equal to 10, which means when `i` is 10 the loop body will not execute. That's why it stops counting at 9. If you want it to also print 10, you'll need to use `(i <= 10)` as your condition.

Any truthy value will cause the loop to run again, which makes while loops very flexible. If you have hierarchical values, you can easily traverse the hierarchy:

```js
// getParent returns a value or null
let parent = getParent(child);

while (parent) {
    doSomething(parent);
    parent = getParent(parent);
}
```

Since `null` is falsy, when `getParent` reaches the end of the hierarchy and returns null the loop will stop executing.

You can simply set the condition to `true` and it will run forever:

```js
while (true) {
    statementOne;
    statementTwo;
    statementThree;
}
```

Obviously you don't want to do this without a way to break out of the loop, which is where `break` statements come in.

#### `break` and `continue` Statements

If you want to stop a loop regardless of whether its condition has become falsy, use a `break` statement:

```js
let i = 1;
while (true) {
    if (i % 2 === 0) { // checks if number is even
        break;
    }

    console.log(i++);
}
```

Note that since the `++` is in postfix position (after `i`) the number will be printed first and **then** incremented. If the operator was in prefix position (before `i`), the number would be incremented first and then printed.

If you want to stop processing the current iteration of a loop but you don't want to terminate the loop entirely, use a `continue` statement:

```js
/**
 * Add the even numbers in a range (non-inclusive of end)
 * @param {number} start
 * @param {number} end
 * @returns {number}
 */
function addEvens(start, end) {
    let i = start;
    let sum = 0;
    while (i < end) {
        if (i % 0 !== 2) {
            continue;
        }

        sum += i;
    }

    return sum;
}

console.log(addEvens(1, 11)); //-> prints 30
```

Note that if you have a loop nested inside another loop the `break` or `continue` statement will only affect the loop it's executed in. So if you're breaking out of the innermost loop, it will continue on with the remaining code for any outer loops that contain the inner loop.

The pattern of using a counter to determine when to break out of a loop is so common that JavaScript has a special loop just for that: the `for` loop.

### For Loops

A for loop uses a counter to determine how many times the loop body executes.

If you use a counter with a while loop, you have to remember two things:

1. Initialize the counter
2. Change the counter's value inside the loop

Forgetting the 2nd is especially bad because if you leave that step out you'll end up with an infinite loop.

The for loop makes this pattern much easier.

You start with the `for` keyword, then in parenthesis you have 3 clauses: an initializer, a condition, and an incrementor (or decrementor). Then you follow the parentheses with the loop body (I recommend using a block statement with curly braces for the same reasons as above).

```js
// logs the numbers 0 through 9
for (let i = 0; i < 10; i++) {
    console.log(i);
}
```

You can initialize multiple variables in the initializer:

```js
// sums up the numbers 1 through 10
for (let i = 1, sum = 0; i <= 10; i++) {
    sum += i;
}
```

You can increment by more than 1:

```js
// logs even numbers from 0 to 18
for (let i = 0; i < 20; i += 2) {
    console.log(i);
}
```

Or you can count down instead of up:

```js
// logs numbers from 10 to 1 counting down
for (let i = 10; i > 0; i--) {
    console.log(i);
}
```

You can also use `break` and `continue` statements inside a for loop:

```js
/**
 * Sums even numbers between start and end
 * @param {number} start
 * @param {number} end
 * @returns {number}
 */
function addEvens(start, end) {
    for (let i = start, sum = 0; i < end; i++) {
        if (i % 2 !== 0) {
            continue;
        }

        sum += i;
    }

    return sum;
}

console.log(addEvens(1, 11)); //-> prints 30
```

### For...Of Loops

There's a variation of the for loop that was added to JavaScript in 2015, commonly known as the `for...of` loop.

You can use a for...of loop with any iterable object, and it will simply iterate over the object's values.

The only iterable we've encountered thus far is the string type, and a for...of loop iterates over each Unicode scalar value in the string.

#### Wait, What?

Strings seem like a simple type, right? After all, it's just text. But it turns out that encoding text in binary code is incredibly complex and fraught with all kinds of difficulties. For example, think of how many different alphabets there are in all the world's languages. We have to be able to represent all of them in a way that makes sense to the computer. Since the beginning of the computer age, programmers have come up with numerous ways to represent text... most of them incompatible with each other.

We could write an entire book about text encoding, but I'll just say the way software makers have agreed to encode text is via the Unicode standard.

Unicode maps text to numbers according to various different encoding schemes, the most common of which is UTF-8. When text is encoded as UTF-8, all "characters," which means a Unicode scalar value that represents some bit of text in some alphabet, are mapped to a sequence of 1, 2, 3, or 4 bytes (8 bits each).

A bit is the smallest size value you can represent in a computer, which in binary code is just a 1 or 0. A byte is 8 bits. You've probably seen your computer's RAM or storage space given in terms of megabytes or gigabytes, which are 1 million or 1 billion bytes, respectively. You can represent any number from 0-255 with 1 byte.

JavaScript doesn't use UTF-8. For all practical purposes, we can say JavaScript uses UTF-16.

In UTF-16, Unicode scalars are represented by either 1 or 2 2-byte (16 bits each) sequences. JavaScript's string type is actually more complicated than that, but it's unlikely you'll ever need to worry about the difference unless you end up writing a library of string functions (which I have done).

It's important to note that a Unicode scalar may not correspond exactly to what we think of as a "character" in text. It's a character for the computer's purposes, but some user-perceived characters may actually be made up of multiple Unicode scalars.

One example of this is emoji. When you see the 😀 emoji you see a single character, but it's actually made up of 2 Unicode scalars.

That means if you iterate over that emoji with a for...of loop there will actually be 2 iterations of the loop.

If you write in English or another language that uses an alphabet derived from Latin then the vast majority of characters you use will be made up of a single Unicode scalar, but it's important to be aware of the fact that things can get more complicated with more complex characters.

Unicode scalars map to numeric values called *code points*, and you can find the code point value of a character using the `String.prototype.codePointAt` method. You can see more methods on strings themselves and the String constructor at [the MDN String page](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String).

#### Strings and For...Of Loops

To iterate over a string with a for...of loop, use the `for` keyword followed by parentheses containing an initializer, the `of` keyword, and the iterable object (in this case a string), then the body of the loop (which I recommend you put in a block statement with curly braces for the same reasons as above):

```js
for (let char of "Hello, world!") {
    console.log(char);
}
```

## JavaScript Evaluation Model

Now that we've covered the important topics of conditional statements and iteration, let's start constructing our mental model of a JavaScript interpreter and how to evaluate JavaScript code. We'll start with executing functions.

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

After that, we substitute the evaluated value of each argument for every case where the parameter name in the same position occurs in the function body.

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

### Execution Contexts and Environments

Now that you understand how a single function call is evaluated in terms of its arguments, let's zoom out a little to look at the bigger picture.

As you know, each function creates its own scope when it is defined. The function's parameters and internal variables are defined in this internal scope.

The internal scope is part of the *execution context* of the function. There are 3 types of execution context: global, module, and local.

The global execution context is exactly what it sounds like: the execution context that contains the execution of all code in the program, whether it's in a script, module, or function.

The module execution context is the execution context for all the code in a given ES2015 module. Remember, an ES2015 module is made up of the code in any file that uses one or more `import` or `export` statements.

The function execution context is the same, but for an individual function.

An execution context is an object created behind the scenes by the JavaScript interpreter that keeps track of everything needed to execute the code currently being executed. There are 3 main things that happen when an execution context is constructed:

1. Creation of the context's variable object (namespace)
2. Constructing the scope chain (environments)
3. Setting the value of `this`

JavaScript code is executed in 2 passes made by the interpreter: the compilation pass and the execution pass.

The execution context is constructed during the compilation pass.

All you need to know right now about step 1 is that it's exactly what it sounds like: the interpreter creates a namespace object that contains the names of all the variables defined in the code unit being executed. This includes the names of function declarations. I'll go into more detail about this later in the book, but this is enough information for now.

I'll explain what step 3 means in the next chapter, because it's important for understanding how objects work, but it's not especially crucial to know right now.

The important step for us right now is step 2: constructing the scope chain.

Every time a function is created, whether it's a function declaration or a function expression, a new scope is created for the function.

This scope includes all the function's parameters, variables, and any functions declared inside the function itself.

As I briefly mentioned in the last chapter, the scope of a function also includes references to any variables defined in the function's outer (or containing) scope. This allows you to use any external variables in a containing scope within the function itself and even update external variables from the function itself.

If you nest functions inside of functions, each successive internal function's scope has a reference to its outer function's scope.

Maintaining a reference to a function's outer scope variables is called *closure*. You may see particular functions that use their outer reference referred to themselves as closures, because the function "closes over" the variables in its outer scope(s).

This allows you to do something like this:

```js
/**
 * @callback adder
 * @param {number} num2
 * @returns {number}
 */

/**
 * Constructs a function that adds any value to num
 * when num is passed in its construction
 * @param {number} num
 * @returns {adder}
 */
function makeAdder(num) {
    return (num2) => num + num2;
}
// constructs a function that adds 5 to its argument
const add5 = makeAdder(5);

add5(10); //-> 15
```

The `add5` function maintains a reference to the argument value given for the `num` parameter in the function that constructs it, so any number passed to `add5` will have 5 added to it.

You can nest any number of functions and the scope chain will be constructed from the innermost to outermost function, with each scope having a reference to all variables defined in any outer scope.

### The Call Stack

When you call a function, the JavaScript interpreter takes the execution context for that function along with the arguments provided in the function call and uses them to create a *call stack frame*. This happens during the interpreter's execution pass.

You saw above how calling a function causes its argument expressions to be evaluated one at a time, from left to right. In order to do the step of substituting the argument values for the parameter names in the function body, the interpreter makes a copy of the function's local environment that's part of its execution context. Then it assigns the argument values to the parameter names in the new copy of the environment. This new object is a call stack frame, which is then placed on top of the call stack.

The call stack is a stack data structure the interpreter maintains internally that keeps track of which function is currently being executed and what functions have yet to be executed.

A stack is a data structure modeled after a stack of items where you place new items on top of the stack of items, then when you need to use an item you take it off the top of the stack. You'll see how to create your own stacks in chapter 20.

The global stack frame, which is constructed from the global execution context, is always at the bottom of the stack.

When the interpreter comes to a function call, it constructs the call stack frame for this execution of the function and places it on top of the stack, a.k.a. on top of the global stack frame. Then it traverses the body of the function to see if any additional function calls are present in the execution of this function.

If it finds an additional function call, it constructs a stack frame for that function call and places it on top of the call stack. So now we have the global stack frame at the bottom, the containing function's stack frame next, and the internal function's stack frame on top.

This process continues until there are no additional function calls nested within the other function calls.

Then the interpreter executes the function whose stack frame is on top and returns its value, removes and discards that stack frame, and continues executing functions, returning their values, and removing frames until every function call has been executed. The process of removing and discarding a stack frame is called *popping* off the stack.

Eventually the interpreter gets back to the global stack frame, and then it continues executing code in the global execution context. When it encounters another function call, it begins the process again.

<!-- TODO: get examples from PythonTutor.com or another visualizer and create diagrams illustrating the execution context -->

## Recap

Now you have a solid understanding of almost everything you need to know about JavaScript to write substantial programs. Understanding control flow enables you to write more complex algorithms, and knowing how the interpreter executes functions will enable you to comprehend and debug even the most complex programs.

In the next chapter we'll look at objects, which are a kind of *compound type*. All the data we've seen so far has been comprised of primitive types. Compound types allow you to combine data of different types into more complex data types and structures, which allows you to represent data in more flexible and useful ways.

## Exercises
