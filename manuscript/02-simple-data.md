{class: part}
# Basic Programming

Your coding journey proper begins in this section! We'll start with the basic building blocks of a program: the data and logic. Every program consists of those 2 things. We'll start building up simple programs right from the beginning so you can start exercising your programmer brain right away.

# Working with Simple Data: Types, Expressions, and Statements

In this chapter you'll begin learning about data and the logic used to manipulate and process data in your programs.

What is data?

It seems like a simple question, but the answer can be surprisingly difficult to pin down.

For this book, we'll define data as information that can be divided into categories according to the operations you can perform with it and process with your program logic.

## Atomic Expressions

The simplest data is just a number, bit of text, or another value by itself.

```js
42
"hello"
true
null
```

A data value like this is a complete JavaScript expression in itself.

What's an expression?

An expression is an operation you can perform in your program that evaluates to a value.

These simple expressions that consist of a single piece of data each are *atomic*, meaning they are complete and valid in and of themselves.

They're not very useful by themselves, though. Let's look at some of the operations you can perform on them.

## Binary Expressions

We'll start with simple binary expressions. They're called "binary" because they have 2 operands.

A binary expression has the form of operand, operator, operand. In JavaScript you can do all the simple mathematical expressions you'd expect with numbers:

```js
3 + 2 //-> 5
2 * 3 //-> 6
2 ** 4 // ** is the exponent operator, so this evaluates to 16
10 / 3 //-> 3.3333...
```

You can also "add" strings of text together:

```js
"Hello" + "World" //-> "HelloWorld" - note there is no space. Spaces have to be added explicitly.
```

Mathematical operations have the precedence you would expect following the order of operations:

```js
2 + 5 * 2 //-> 12
```

You can use parentheses to force a different order of operations:

```js
(2 + 5) * 2 //-> 14
```

If you're wondering about the `//` above, that starts a *comment*. A comment is just that: a comment programmers use to convey information to other programmers when they view the code. Comments are not processed by the JavaScript interpreter.

You can start single-line comments with the double slash, `//`. You can also write multiline comments that start with `/*` and end with `*/`.

```js
/* This is
  a
  multiline comment */
```

Programmers often use comments as a way to inform other programmers about how they should expect a bit of logic (function) to work and what kinds of data it uses/returns.

You can also add spaces or tabs in your code to make it easier for other programmers to read. Don't mix spaces and tabs, though. It won't make any difference to the interpreter, because the interpreter simply ignores white space, but it will confuse the hell out of other programmers (including possibly yourself 6 months from now when you come back and reread your code while maintaining it).

```js
/**
 * Calculates the absolute value of a number
 * @param {number} num
 * @returns {number}
 */
function absoluteValue(num) {
    // ...
}
```

The comment for the above function signature uses a *docblock* to document the function. I'll explain what that means when we look at designing functions in chapter 3.

JavaScript used to only have 1 numeric type, number, which can represent either integers or decimal numbers. Recently they've added bigints to the standard. A value of type number is limited in how large they can get on either side of 0.

To be precise, the number type in JavaScript is a 64 bit IEEE floating point number. If that means nothing to you, it's ok &ndash; that's the last time we'll talk about that and it won't be on the test (there is no test, unless you have a professor who's awesome enough to use this book for a course).

If the value is too far from 0, it evaluates to the one of the special number values `Infinity` or `-Infinity`.

Bigints are integer numbers that can grow indefinitely large in either direction. They are written as integers with an `n` at the end.

```js
1000000000000000000000000000000000000000000000000000000000000000000000000000000000 //-> evaluates to Infinity
1000000000000000000000000000000000000000000000000000000000000000000000000000000000n //-> evaluates to this integer
```

You can't use numbers and bigints together in the same mathematical operation. If you try, it will cause an error.

```js
10.5 + 10n // ERROR
```

If you want to use a number value and a bigint value together, you have to convert one to the other.

```js
let x = 10.5 + Number(10n) //-> x === 20.5
typeof x //-> "number"
```

I've introduced a few new things in the code block above. I'll explain them later in this chapter. For now, just recognize that those expressions will evaluate to the values I've put in the comments.

## Types

I mentioned types briefly above, but now let's talk about them in more detail.

Types are categories for data that are separated according to the kinds of operations you can perform on the data. With numeric types, you do numeric operations. With the string type, you do string operations. These are called *primitive types* because they're complete values that cannot be broken down any further. "Hello" is always "Hello" no matter what you try to do to it.

Other primitive types include booleans, symbols, and `undefined`. There's also another value called `null` that behaves like a primitive type in many ways.

### Numeric Types

As I mentioned above, JavaScript has 2 primitive numeric types: number and bigint.

As you've seen, you can do mathematical operations on numeric values, but you can't mix the 2 types together in the same mathematical operation.

Mathematical operations include addition, subtraction, multiplication, and division, just like you'd expect. There are also 2 additional mathematical operators: remainder and exponentiation.

`%` is the remainder operator. Sometimes you'll see it called "modulo" (although that isn't *technically* correct). It evaluates to the remainder left when dividing 2 numbers, and always evaluates to an integer.

```js
10 % 3 //-> 1
```

The `**` operator handles exponentiation. It raises the 1st number to the power of the 2nd number.

```js
3 ** 4 //-> 81
```

### Numeric Unary Operations

You can also increment and decrement numbers using the `++` and `--` operators. If the operation is performed on a variable, it assigns the result of the operation to that variable.

```js
10++ //-> 11
10-- //-> 9
x++ // x is now equal to its previous value + 1
```

The increment and decrement operators can be used in either prefix (before the value) or postfix (after the value) position.

#### Comparison

In addition to mathematical operations, numeric values can be compared:

```js
5n > 3n //-> true
5n < 3n //-> false
5n <= 6n //-> true
17 >= 3.2 //-> true
5 === 5 //-> true, note that there are 3 equals signs
5 !== 3 //-> true
```

These operations evaluate to booleans, which we'll see here shortly.

Note that with comparison operations you actually **can** mix numbers and bigints:

```js
10n > 5 //-> true
```

That's because when you do a comparison, the interpreter pretends for a second that they're the same type. This is called "type coercion," and I'll cover it in detail here in a minute.

You probably noticed earlier that I used operators with multiple equals signs, `===` and `!==`. These mean "is equal to" and "is not equal to." They measure strict equality, which means they check to see if the operands have both the same value *and* the same type.

The single equals sign has a different meaning that we'll cover shortly.

There are also comparison operators with fewer equals signs, `==` and `!=`. They only check the values, which can be coerced to different types in certain situations. You're going to want to use strict equality nearly every time you do an equals or not-equals check.

#### Dividing By Zero

Unfortunately, JavaScript will let you divide by zero without throwing an error:

```js
10 / 0 //-> Infinity
```

#### NaN

There is also a special value of the number type `NaN`, which means "Not a Number." You get `NaN` when you try to perform an invalid numeric operation or cast an invalid value to a number. This example tries to cast the string "hello" to a number:

```js
Number("hello") //-> NaN
```

`NaN` is viral, meaning any mathematical operation with `NaN` as one of its operands will evaluate to `NaN`. It's also the only value in JavaScript that is not equal to any other value, **including itself**.

```js
NaN === NaN //-> false
```

If you want to check equality and it's possible that one or both of your values is `NaN`, use the `Object.is` method:

```js
Object.is(NaN, NaN) //-> true
```

Getting the remainder of any number and zero evaluates to `NaN`.

```js
10 % 0 //-> NaN
```

### The String Type

The string type holds textual data. Some languages have separate types for individual characters and strings, but JavaScript only has the string type. A single character in JavaScript is just a string with a single character in it.

As shown earlier, you can "add" strings. More correctly, you can concatenate them using the `+` operator.

```js
"Hello" + "World" //-> HelloWorld
```

You can also compare strings:

```js
"hello" === "hello" //-> true
```

You can also do greater than and less than comparisons on strings. This comparison checks one character at a time to see which character's numeric value is greater or less than the other. All characters have numeric values under the hood, but you seldom need to worry about that.

```js
"a" < "b" //-> true
"hello" > "world" //-> false
"ABC" < "XYZ" //-> true
```

There is one gotcha with comparing strings. Due to how the characters' numeric values are represented, lowercase characters are always greater than uppercase characters.

```js
"B" > "a" //-> false
```

Uppercase letters' numeric values begin at 65, whereas lowercase ones start at 97, so the above comparison looks like `66 > 97` when you take their numeric values into account. That's obviously not true.

### Booleans

Booleans are an extremely simple type. There are only 2 values: `true` and `false`.

It doesn't seem like having only 2 possible values would be all that useful, but it turns out that booleans unlock whole new worlds of programming possibilities.

You surely noticed above that the comparison operations each evaluated to `true` or `false`. Those operations always evaluate to boolean values.

#### Boolean Operators

There are also boolean operators: `&&`, `||`, and `!`.

`!` is a unary operator, which means it only takes one operand. This unary operation evaluates to the opposite of whatever its operand is.

```js
!true //-> false
!false //-> true
!10 //-> false - I will explain this later in the chapter
```

`&&` and `||` are the "and" and "or" operators, respectively.

```js
true && true //-> true
true && false //-> false
true || false //-> true
false || false //-> false
```

These two operators do what's called short circuit evaluation. That means if the first operand satisfies a certain condition, the second operand is never evaluated.

With `&&`, if the left hand value is false, the right side will not be evaluated. Otherwise, the expression evaluates to the value of the right hand expression.

```js
10 > 50 && functionThatThrowsAnError() //-> false - it doesn't matter what's on the right, it will never be evaluated
100 > 10 && "hello" //-> "hello"
```

With `||`, if the left hand value is true then the right side will not be evaluated. In this case, the expression evaluates to the value of the left hand expression. If the left side is false, the expression evaluates to the value of the right hand expression.

```js
false || 3 //-> 3
true || "I will never be evaluated" //-> true
```

Weirdly enough, you can also add booleans:

```js
true + true //-> 2
```

Don't do that, though. It's silly.

### Symbols

Symbols are a relatively new primitive type that was added to the language recently. Symbols are not equal to any other value. You create them using the `Symbol` constructor function:

```js
Symbol("hi")
```

A symbol created this way is always unique. You can create 2 symbols from the same string and they will still not be equal:

```js
Symbol("hi") === Symbol("hi") //-> false
```

You can also use the `for` method (which is a function):

```js
Symbol.for("hi") === Symbol.for("hi") //-> true
```

Every symbol created with `Symbol.for("hi")` is equal to every other symbol created the same way.

You probably won't use symbols too often, but they're useful sometimes because you can use them to add keys to objects that are guaranteed to be unique. Objects are a compound data type, and we'll cover them in chapter 4.

### Null and Undefined

`null` and `undefined` are similar to one another. Both represent the absence of any value, but in different ways.

`undefined` represents a value that has not been defined. It is also returned by functions that do not have a return value, as you learned in the previous chapter.

`null` is used to intentionally set a value to be empty. Confusingly, `null` in JavaScript is of type "object." The reason is because `null` is supposed to be a value used in places where there **could** be an object value. Personally, I think this was a mistake and they should have just used 1 empty type, but that's how the language was defined in the beginning so now we're stuck with it.

#### The Nullish Coalescing Operator

`??` is the nullish coalescing operator. It's similar to `||`, except that instead of evaluating the right side expression when the left side is false, it evaluates the right side if the left side evaluates to `null` or `undefined`.

```js
5 + (x ?? 0) //-> if x is null or undefined, evaluates to 5. Otherwise, evaluates to x + 5.
```

## Type Coercion

I briefly mentioned type coercion above. Now I'll present it with a little more detail.

Type coercion is when the JavaScript interpreter changes the type of a value to fit with the operation being performed on it.

It's easier to show examples than to explain it:

```js
5 + "5" //-> "55"
true + 1 //-> 2
"" == false //-> true
0 == false //-> true
null == undefined //-> true
0 == "" //-> true
```

This is why I said before that you should almost always prefer using the strict equality operators `===` and `!==`. The regular equality operators `==` and `!=` will coerce types to try to make the operation work.

The strict equality operators compare values **and** types, and so will work the way you probably expect it to work.

Type coercion is another one of those features that was added to the language in the beginning that many programmers wish we could get rid of. It's a common source of bugs (mistakes) in code, and they can be pretty hard to track down and fix sometimes.

I guarantee that at least once in your career you will think your program is adding 2 numbers together when in fact it's trying to add a number and a string, so it coerces the number to a string and concatenates the 2 values.

### Truthy and Falsy Values

JavaScript has "truthy" and "falsy" values. That means in a context where you would expect a boolean the falsy values are coerced to `false`. Truthy values are coerced to `true`.

The only falsy values are 0, "" (an empty string), `null`, `undefined`, and `false`. Everything else is truthy.

Sometimes having truthy and falsy values is convenient and allows you to write more succinct code. Other times it can cause unexpected problems, such as when you're checking for a numeric value and get 0, and forget to handle the case where 0 is falsy.

Short circuit evaluation with the `&&` and `||` operators works with truthy and falsy values. That enables some useful programming tricks I'll demonstrate later in the chapter.

Since JavaScript has truthy and falsy values, you can use the unary `!` operator to cast any value to a boolean.

```js
!5 //-> false
!!5 //-> true - note that you can cast any value to its equivalent boolean with !!<value>
```

### The `typeof` operator

You can use the `typeof` unary operator to get the type of a value as a string, which can be useful:

```js
typeof "hello" //-> "string"
```

Unfortunately, sometimes `typeof` gives a result you might not expect like this:

```js
typeof null //-> "object"
```

`typeof` also does not distinguish between different object types. As far as it's concerned, a value is either a primitive (in which case it has a name like "string") or an object (in which case it is "object"). We'll cover objects in detail in chapter 4. For now, just recognize that if you want to get the type of an object (e.g. a `Date`) you'll need to do a little extra work.

Note that using the `typeof` operator is the only valid way to reference a variable name that has not been defined.

```js
typeof undefinedVariableName; //-> "undefined"
```

## Conditional (or Ternary) Expressions

The conditional expression allows you to test a value and choose which of 2 expressions you want to evaluate based on whether the test is truthy or falsy. It is the only operator that takes 3 operands, so sometimes you'll see it called the ternary operator.

The conditional operator uses `?` and `:` between its operands.

If the first operand is truthy, the expression after `?` is evaluated. If it's falsy, the expression after `:` is evaluated. In either case, the other expression is left unevaluated.

```js
true ? "hello" : "goodbye" //-> "hello"
x > 5 ? "it is greater than five" : "it is not greater than five" //-> depends on the value of x
```

## Statements

All the logic we've seen so far has taken the form of expressions. JavaScript also has statements. Statements are bits of logic that are executed, but do not evaluate to a value. A JavaScript program is essentially a list of statements that are executed according to the order dictated by the combination of the program's data and logic.

A statement nearly always ends with a semicolon. JavaScript will often let you omit the semicolon, but the rules are tricky and it's easy to accidentally leaves one out in a way that changes the meaning of your program.

When you're just getting started, you should always use semicolons where you're supposed to use them.

### Expression Statements

The simplest statement is the expression statement. An expression statement is the statement containing a "top level" expression where the statement consists entirely of a single expression (which can be a compound expression like `1 + 2 * 3`).

All the expressions we've used as examples so far have also been expression statements.

### Variables

Variables are names that represent certain bits of data. Sometimes you'll see people refer to a variable as a kind of "box" that holds data. I prefer to think of it as a name that points to data. However you decide to think of it, just know that a name represents some data.

In the following examples, the space between curly braces is the inner scope and the form `let [varname] = [value]` defines a variable.

#### Assignment and Falsy Values

If you're assigning a variable to a value that could be `null` or `undefined`, you can set a default value by using the `??` operator:

```js
let x = value1 ?? 0;
```

If the value could be falsy and you want to make sure you have a truthy value, use `||`:

```js
let x = value1 || 1;
```

Before we had the nullish coalescing operator, our only option for default values was to use `||`, which could cause problems if a falsy value other than `null` or `undefined` was acceptable.

#### Variables and Scope

A variable exists in a certain scope. The scope of a variable is the space within your program code in which the variable is valid.

Code in an inner scope has access to the variables in its containing scopes, but code in an outer scope cannot access variables in an inner scope:

```js
let x = 5;
{
    let y = 10;
    x + y; //-> 15
}
x + y // ERROR
```

You can give a variable in an inner scope the same name as one in the outer scope, which will prevent the inner scope from being able to access the outer variable. This is called *shadowing*:

```js
let x = 10;
{
    let x = 5;
    x + 1; //-> 6
}
x + 1; //-> 11
```

A variable is visible throughout its entire scope, no matter where in the scope it is defined. You can't use a variable before its initialization. You might expect `x + 1` in this example to use the value from the outer scope, but it does not:

```js
let x = 10;
{
    x + 1; // ERROR, because x is defined in this scope, but it hasn't been declared yet
    let x = 5;
}
```

JavaScript has 3 kinds of scope: global scope, function scope, and block scope. Variables in the global scope are accessible throughout the entire program. We'll cover function and block scopes when we get to block statements and functions later in this chapter.

#### Valid Variable Names

Valid characters for variable names include $, _, and any Unicode letter or number. However, variable names cannot begin with a number.

#### Variable Declarations

A variable declaration has 3 parts: the declaration keyword, the variable name, and an initializer expression. Sometimes the initializer expression is optional.

The initializer expression uses a single equals sign, but it's a mistake to think of it as meaning equality. It indicates that the variable name points to the value on the right. I tend to think of it as "gets," as in `x = 5` means "x gets 5."

Declare variables using either the `let` or `const` keyword:

```js
let x = 10;
let y; //-> declares y, but does not initialize it
const z = "hi there";

y = false; //-> this is an expression statement containing an assignment expression

const a; //-> ERROR
```

Why does `const a;` cause an error? It's because `const` declares a variable with a **constant binding**, which means the name cannot be reassigned to another value.

Since the name cannot be reassigned you can't give it a value later, which means it has to have an initializer in the declaration.

With `let` you declare a variable binding that can be changed later in the program. This is often (though not always) a bad idea, because it's good for your program to be able to know exactly what value a variable may have. That makes it much easier to understand and reason about your code. The ability to read your code and know exactly what possible values a name can refer to is called *referential transparency*.

However, there are some cases where it's necessary to change a variable's value.

#### Assignment Expressions

If you do have to change a variable's value, use an assignment expression, which looks just like the initializer in a variable declaration:

```js
let x;
x = 10;
```

There are also augmented assignment expressions where you perform an operation on the variable and then assign the result of that operation to the variable. Here are some examples:

```js
x += 3; // assigns the value of x + 3 to x
x -= 2;
x *= 10;
```

There's an augmented assignment operator for nearly every binary operator, including `&&=` and `??=`.

#### Variable Declarations Without Initializers

You can declare a variable with `let` and not initialize it, but you can't use it before it is assigned a value.

```js
let x;
x + 3; //-> ERROR
x = 10;
```

The space in the code between where such a variable is declared and where it is assigned a value is called the *Temporal Dead Zone*. If you use a variable in its Temporal Dead Zone, it will throw an error.

#### Declaring Multiple Variables

You can declare multiple variables at once:

```js
let a = 1, b = 2, c = "what?";
```

#### Variables Are Not Bound to Types

Finally, for a variable binding JavaScript will not prevent you from assigning a value with a completely different type to the same variable.

```js
let greeting = "hello";
greeting = 5;
greeting; //-> 5
```

I can't think of a single time in my entire career when doing this has been a good idea, but JavaScript will let you do it.

#### The `var` Keyword

Sometimes you'll see the keyword `var` used to declare a variable, mostly in code written before 2015. That's when `let` and `const` were added to the language. All you need to know is that it's the old way of declaring variables and it behaves mostly like `let`. You should always use either `let` or `const` in modern JavaScript programs (preferably `const` in most situations).

Try to use descriptive variable names that indicate to someone reading the code what the value is used for. Sometimes this can be more difficult than you might think!

### Block Statements

A block statement is a sequence of statements inside curly braces. A block statement can include any kind of statement, inluding other block statements. Block statements are often used to allow the use of multiple statements where only one statement is technically valid.

A block statement has its own inner scope.

```js
let scope = "global";
{
    let scope = "outer block";
    {
        let scope = "inner block";
    }
}
```

You can use a block statement anywhere, but they are most often used in function bodies and the bodies of `if` statements, loops, and `try`/`catch` statements. You'll learn about those in the next chapter.

A block statement usually does not require a semicolon after the closing brace. The exception is when using a function expression, in which case if the expression is the last part of the statement it's in you'll need to put a semicolon after it.

## Functions and Call Expressions

A function is an encapsulated bit of logic that operates within its own scope and can be used in different places.

Functions are the most important means of abstraction in JavaScript.

You can create functions in 2 ways:

- With a function declaration
- With a function expression

### Function Declaration

A function declaration is a statement. It begins with the `function` keyword, then the name of the function, a comma-separated list of *formal parameters* in parentheses, and ends with a block statement. Formal parameters are variables used inside the function body that can be replaced by arguments given when the function is called. Here is a function declaration for a function to calculate the absolute value of a number:

```js
/**
 * Gets the absolute value of a number
 * @param {number} num
 * @returns {number}
 */
function absoluteValue(num) {
    return (num < 0) ? -num : num;
}
```

This function ends with a `return` statement. A `return` statement contains the expression the entire function evaluates to when it is called. If a return statement is not executed inside the function, the function call evaluates to `undefined`.

### Call Expressions

You execute a function by giving the function itself followed by parentheses with any arguments it may require inside. In this case it's the function name, but the function value can be any expression that evaluates to a function:

```js
absoluteValue(-15); //-> 15
```

A function can take any number of arguments. JavaScript won't stop you from using either more or fewer arguments than the number of formal parameters defined for the function. Extra arguments are simply ignored, and if there are too few arguments the ones not accounted for are given the value `undefined`.

The expression for evaluating a function is called a *call expression*.

Trying to use `null` or `undefined` in a call expression as if it were a function will throw an error.

### Function Expression

There are two types of function expressions: the original `function` function expressions, and arrow function expressions.

#### `function` Function Expressions

A `function` function expression is written similarly to a function declaration, except that it is in *expression position*. That means any position where a statement cannot be used, but an expression can.

To rewrite the `absoluteValue` function above as a function expression:

```js
/**
 * Gets the absolute value of a number
 * @param {number} num
 * @returns {number}
 */
const absoluteValue = function(num) {
    return (num < 0) ? -num : num;
}; // note the semicolon here, which terminates the variable declaration
```

You can optionally use a named function expression, which adds a name between the `function` keyword and the opening parenthesis:

```js
/**
 * Gets the absolute value of a number
 * @param {number} num
 * @returns {number}
 */
const absoluteValue = function _absoluteValue(num) {
    return (num < 0) ? -num : num;
};
```

You **can** use the same name as the variable, but you don't have to.

#### Arrow Function Expressions

An arrow function expression is so named because it has a "fat arrow" in it. It has the form `(parameters) => body`.

If there is only one parameter, the parentheses can be omitted:

```js
/**
 * Gets the absolute value of a number
 * @param {number} num
 * @returns {number}
 */
const absoluteValue = num => (num < 0) ? -num : num;
```

If there are multiple parameters the parentheses are required. You can also end an arrow function expression with a block statement. Function expressions are the only expressions that can include statements inside them.

```js
/**
 * Gets the larger of 2 numbers
 * @param {number} n1
 * @param {number} n2
 * @returns {number}
 */
const max = (n1, n2) => {
    return (n2 > n1) ? n2 : n1;
};
```

If the body is just a single expression, you don't have to use a block statement as the arrow function body. If you have one or more statements, you have to use a block statement.

Arrow functions and `function` functions are almost exactly equivalent. The differences between them don't matter for now, but we'll discuss them later in the book.

### Functions and Scope

A function creates its own scope for the body of the function. The main purpose of the scope is so the parameters of a function can be bound to arguments when the function is called, and the parameter names can be treated just like variables inside the function body. Statements inside a function scope can access variables in outer scopes, just like with block scopes.

```js
const x = 1;

/**
 * Increments x by a number
 * @param {number} num
 * @returns {number}
 */
function increment(num) {
    return num + x; // valid!
}
```

Not only can statements inside a function body reference variables in outer scopes, the function also retains a reference to those variables throughout the lifetime of the function from when it's created to when it goes out of memory. This usually happens when the program either finishes or exits the scope in which the function was created.

That means you can do things like this:

```js
let x = 0;

/**
 * Increments x by 1
 */
function incrementX() {
    x++;
}
```

This function increments the value of `x` in the outer scope, even though the operation takes place in the inner scope. If `incrementX` is called anywhere in the program, in any scope where it's valid, the value of `x` in *this* scope will be increased by 1.

This property of functions is called *closure*, and we'll explore it in greater depth later in the book because it's a very important part of JavaScript.

### Recursive Functions

A recursive function is a function that calls itself. Recursive functions are used a lot in algorithms and certain kinds of programs, like interpreters.

```js
/**
 * Gets the factorial of n
 * @param {number} n
 * @returns {number}
 */
function factorial(n) {
    return (n === 1) ? 1 : n * factorial(n - 1);
}

factorial(5); //-> 120
```

Recursive functions should always have one or more base cases and one or more recursive cases. If you write a recursive function, you need to make sure the function will always eventually reach a base case so it doesn't continue forever.

## Member Expressions and Methods

An object is a piece of compound data that has properties which are key/value pairs. Member expressions are expressions that access a property (or member) of an object. We'll cover objects in more depth in chapter 4, but I wanted to cover member expressions now so you can understand how to use some JavaScript primitive operations that wouldn't make sense otherwise.

The most common form of member expression is `object.property` with both `object` and `property` both as valid variable names and the dot is the member access operator. Of course, `object` can be any expression that evaluates to an object.

You can also write a member expression with bracket notation, which is `object[expression]` where `expression` must evaluate to the name of the object's property. If an object property's name is not a valid variable name, you have to use bracket notation. An object's property keys can be strings or symbols.

A method is simply an object property that is a function. Functions are also objects in JavaScript, so you can use them anywhere you can use an object value.

You call a method just like you would a regular function, only with a member expression before the parentheses. This code calls the "now" method on the `Date` constructor. `Date` is a built-in JavaScript function that creates objects that represent time and date values. Functions are also objects, so a constructor function can have its own properties and methods. `Date.now()` returns the number of milliseconds since 12:00 AM on January 1, 1970 (the beginning of the UNIX epoch).

```js
const timestamp = Date.now(); // 1679085365794 as of a few seconds before I wrote this
```

Trying to access a nonexistent object property evaluates to `undefined`.

### Almost Any Type Can Be Used as An Object

All built-in JavaScript types except `null` and `undefined` can be used like objects with properties and methods.

The way JavaScript does this is by creating "wrapper" objects around non-object (i.e. primitive) type values and using the properties on that object. The wrapper object is then immediately discarded and only the primitive value remains.

```js
const greeting = "hello";
greeting.length //-> 5
greeting.toUpperCase(); //-> "HELLO"
```

### Optional Member Access

If you try to access a property on `null` or `undefined`, it will throw an error. To avoid this problem, you can use the optional member access operator, which is `?.`. If you use it, then if the value in the object position is `null` or `undefined` the expression simply evaluates to `undefined`.

```js
const obj = null;
obj?.property1 //-> undefined
obj?.property1?.property2?.property3 //-> undefined
obj.property1 // ERROR
```

Before we had the optional member access operator, we had to use a hacky solution involving `&&` if there was a chance the object could be `null` or `undefined`:

```js
obj && obj.property1 // null & undefined are falsy, so if obj is one of them the right side is never evaluated due to short circuit evaluation
```

### The `Math` Object

The `Math` object is an object built into JavaScript that contains useful properties and methods for mathematical operations on numbers. Since it predates the use of bigints in JavaScript, you can only use it with the number type. Here are some of its properties and methods:

```js
Math.PI; //-> 3.14159...
Math.E; //-> 2.71828...
Math.abs(-15); //-> 15
Math.max(5, 10); //-> 10
Math.log2(10); //-> 3.321928094887362
```

For complete documentation of the `Math` object's properties and methods, with examples, see [its Mozilla Developer Network entry](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math).

## Recap

In this chapter we've had a whirlwind tour of JavaScript primitive functionalities, including types, expressions, and statements. We've covered a lot, but believe it or not we're just getting started.

It may surprise you to learn that with just the information we've covered in this chapter you can now use JavaScript to compute anything that is computable by any programming language ever invented. That's because the subset of JavaScript we've looked at so far is *Turing-complete*, which is just another way of saying it can compute anything that can be computed. It's named after the famous mathematician Alan Turing, who is considered by many to be the father of computer science. He created a hypothetical model of a computing machine decades before the first digital computer was built.

To define a Turing-complete programming language you must have a set of primitive instructions, a means of branching (like conditional expressions) and a means of repetition (like recursion). Our current understanding of JavaScript includes all these things.

In chapter 2 we'll look at control flow constructs that expand upon our ability to direct the flow of a program to execute different branches of code or repeat instructions, as well as how to handle errors that could arise in our programs. We'll also start to build the evaluation model for our hypothetical JavaScript interpreter.

## Exercises

<!-- TODO: exercises -->
