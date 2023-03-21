# Working with Compound Data: Arrays and Objects

## Objects

### Simple Data Objects, a.k.a. POJOs

### Methods

### The `this` Keyword

### Iterating over Objects

### Creating Objects with Functions

### The Spread Operator

### Destructuring Objects

## Arrays

### The Need for Arbitrary-Length Data

### Constructing Arrays

### Iterating over Arrays

### Simple Array Methods

### Spreading Arrays

### Array Destructuring

## Variadic Functions

### The `arguments` Object

### Using a Rest Parameter

## Functions with Default Parameters

## Linear Time Algorithms

Now that you've seen loops and arrays, we can more easily discuss algorithms that are more complex than constant time algorithms.

Probably the most common algorithm involving a loop is the linear time algorithm.

That means the running time of an algorithm increases proportionally to an increase in the size of its input.

If a function takes 10 steps to run with an input of 10 elements, and it takes 1,000 steps to run with an input of 1,000 elements, we say it's in linear time. A linear time algorithm has a Big O of O(n).

It is often the case that an O(n) algorithm will have one main loop, or one main recursive call. Here is an example of an O(n) algorithm that gets an arbitrary amount of inputs and then processes them:

```js
/**
 * Print the list of names in uppercase
 * @param {string[]} names
 */
function yellNames(names) {
    for (let name of names) {
        console.log(name.toUpperCase());
    }
}

// now let's get our input
let names = [];
let inputName;

while (inputName !== "") {
    inputName = input("Give me a name: ");

    if (inputName) {
        names.push(inputName);
    }
}

yellNames(names);
```

Now, you might look at the above code and be confused: shouldn't the algorithm's running time be O(n * 2)? After all, there are 2 iterations for each name. One when the name is being entered, and one when it is being printed. And what about the fact that each iteration has 1 operation? Maybe it should be something like O(n(n + 1) * n(n + 1)).

That's a good observation, and in real terms doubling the number of iterations could absolutely affect how long it takes an algorithm to run in real time.

However, in terms of Big O, it's still just O(n) because there is only one set of inputs. The running time of the code is a function of how large the input is, regardless of whether that input is processed 1, 2, or 1,000 times. The number of steps it takes to process the input in each iteration isn't important.

Big O notation is an **approximation** of the running time, not a calculation of every little thing that affects how long it actually takes for the computation to run.

## JavaScript Evaluation Model

### Function Calls and `this`

### The Differences between `function` Functions and Arrow Functions

## Recap

## Exercises
