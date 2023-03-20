# Control Flow and Our Execution Model

## Conditional Statements

### Constant Time Algorithms

## Iteration

## While Loops

## For Loops

## For...Of Loops

## JavaScript Evaluation Model

### The Two Steps of Execution

### Hoisting

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

### Execution Contexts and Environments

## Recap

## Exercises
