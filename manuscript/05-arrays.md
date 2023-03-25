# Built-In Collection Types: Maps, Sets, and Arrays

JavaScript has 3 built-in *collection* types that we will look at in this chapter. A collection type allows you to group data together in a structured manner. The 3 built-in collection types are Maps, Sets, and Arrays.

## Maps

A JavaScript Map is an object made up of key/value pairs. It's conceptually similar to an object, except that with an object you can only have property keys that are strings or symbols. The keys of a Map entry can be any type, including objects and even other Maps.

You can mix and match the types of your keys and values, but don't. Maps are most effective when you have the same type for all keys and the same type for all values. Obviously the key and value types don't have to be the same. If you **absolutely** need different types for values and you need keys with a type other than string or symbol, you have to use a Map. You should avoid getting into situations where this is necessary. If you need entry values with different types, an object is more appropriate than a Map.

### Constructing Maps

Unlike with objects, there is no literal syntax for a Map. You have to use the Map constructor:

```js
let m = new Map(); // creates an empty Map
```

You can also create a Map directly from an array of pairs, which are arrays with 2 elements representing key/value pairs:

```js
const pairs = [ [ "a", "hello" ], [ "b", "goodbye" ] ];
//-> Map { "a" => "hello", "b" => "goodbye" }
```

Obviously we'll cover constructing arrays later in the chapter, but this is an example of using array literal syntax to create an array of array pairs.

The ability to use an array of pairs to construct a Map means you can easily convert an object to a Map:

```js
const obj = { a: "hi", b: "bye" };
const m = new Map(Object.entries(obj));
```

### Working with Maps

To add entries to a Map, use the `Map.prototype.set` method with the key and value:

```js
m.set("key1", "value1");
```

The `set` method both sets the value for the given key and returns the Map itself with the new entry.

To read an entry from the Map use `Map.prototype.get` with the key:

```js
m.get("key1");
```

If the key you pass to `get` as an argument is a reference type, the interpreter will use reference equality to compare the argument to the Map's keys. That means the object must be the same object or an alias for the object. Passing it an object that is structurally the same (i.e. has the same property keys and values) will not work.

Trying to get an entry with a nonexistent key will evaluate to `undefined`, so if you're uncertain whether a Map key has been set you'll want to use the `has` method as shown below.

Note that, because all objects (including collection types) are reference types, an equals comparison on 2 Maps will only evaluate to `true` if one is an alias of the other:

```js
// This creates a map with the same entries as m, above
const m2 = new Map(Object.entries({ key1: "value1" }));

m === m2; //-> false
```

To check and see if a Map has an entry with a certain key, use `Map.prototype.has`:

```js
m.has("key2"); //-> false
```

This allows you to avoid getting `undefined` values when fetching data from a Map:

```js
let value;

if (!m.has("key2")) {
    value = "value2";
    m.set("key2", "value2");
} else {
    value = m.get("key2");
}
```

Just like with `get`, if the key is a reference type it must be the exact same object.

To remove an entry from the Map, pass its key to `Map.prototype.delete`:

```js
m.delete("key2"); //-> true
```

If the entry is successfully deleted `delete` returns `true`; otherwise it returns `false`. Just like with `get` and `has`, if you give `delete` an object it will only delete the entry if it's the exact same object or an alias.

If you need to delete all the data in a Map, use `Map.prototype.clear`:

```js
m.clear(); // m is now empty
```

It's good programming practice in general to avoid mutating your objects like this, but there are cases where it's necessary.

Finally, to see how many entries are in a Map, use the `size` property:

```js
m.size; // currently 0 after clearing
```

### Iterating over Maps

Similar to objects, a Map has methods to get its keys, values, and entries.

`Map.prototype.keys` returns the keys:

```js
for (let key of m.keys()) {
    console.log(m.get(key));
}

for (let value of m.values()) {
    console.log(value);
}

for (let entry of m.entries()) {
    console.log(entry);
}
```

The `keys` and `values` methods both return a MapIterator, which is an iterable object similar to an array. It doesn't have the same properties and methods as an array, though, so if you want to use it like an array you'll need to convert it first. The `entries` method returns a MapEntries object, which is an iterable object that contains array key/value pairs for all the entries. Again, if you want to use it with array methods you'll have to convert it. I'll cover how to convert iterable objects to arrays later in this chapter.

If all you want to do is iterate over a Map's entries, Maps are themselves iterable so you can simply use a for...of loop with the Map itself:

```js
for (let entry of m) {
    console.log(entry);
}
```

Since each Map entry is given as an array pair by the iterable, you can use destructuring to break it apart, in a similar fashion to how you destructure an object:

```js
for (let [key, value] of m) {
    console.log(key + ":", value);
}
```

You can use destructuring to extract values from any iterable, which we'll cover in detail later in the chapter.

There's also a `Map.prototype.forEach` method that takes a *callback* and applies that function to every entry of the map. A callback is a function you pass to another function as an argument that is then executed while the main function is itself executing. The callback to `forEach` takes as its parameters the entry value, the entry key, and the map itself (the latter 2 are optional):

```js
const m = new Map(Object.entries({ a: "hi", b: "bye" }));

m.forEach((val, key, map) => {
    console.log(key + ":", val);
});
```

Note that `forEach` does not return a value. It takes an optional second argument that sets the value of `this` while `forEach` is executing.

There is one big difference between using `forEach` and a for...of loop to iterate over a Map: with `forEach`, it will always iterate over the entire Map. You can't use `break` or `continue` to control the flow of the iteration.

In current standards-compliant JavaScript interpreters (as of April, 2023), the order of entries when you iterate over the Map will be the same order in which they were added. This was not true when Maps were first added to the language, so if you have to work with older versions of a browser or Node you may not be able to depend on that behavior.

## Sets

JavaScript Sets are similar to arrays, except that they are guaranteed to not contain duplicate values. Note that when dealing with a Set of objects the values are only considered to be duplicate if they are the exact same object or aliases of the same object.

### Constructing Sets

Just like with Maps, there is no literal syntax to create Sets. You have to use the Set constructor:

```js
let s = new Set();
```

If you pass an array or other iterable to the Set constructor, it will deduplicate the object's values and add each unique value to the Set:

```js
let s = new Set([ 1, 2, 3, 2, 1, 4, 3 ]); //-> Set { 1, 2, 3, 4 }
```

Like with Map keys or values, you should try to always have all the elements of your Set be the same type.

### Working with Sets

To get the number of elements in a Set, use the `size` property:

```js
s.size; //-> 4
```

Add a value to a Set with the `Set.prototype.add` method:

```js
s.add(5);
```

The `add` method both adds the value to the Set and returns the Set.

To check if a value is in a Set, use `Set.prototype.has`. Note that since there are no keys you check the value, not a key like you do with Maps:

```js
s.has(5); //-> true
```

You can delete a value from a Set with `Set.prototype.delete`:

```js
s.delete(5);
```

It returns `true` if the value is deleted, and `false` if nothing is deleted.

You can also clear a Set with `Set.prototype.clear`:

```js
s.clear();
s.size; //-> 0
```

### Iterating over Sets

Interestingly, a Set has `keys`, `values`, and `entries` methods just like a Map. `Set.prototype.keys` returns a SetIterator of the values in the Set. `Set.prototype.values` returns a SetIterator of the values in the Set. `Set.prototype.entries` returns a SetIterator that contains pairs that hold each value twice, as if it were created from a Map that had all its keys the same as their values.

You can also iterate over a Set with for...of:

```js
for (let value of s) {
    console.log(value);
}
```

Like Maps, Sets have a `forEach` method. The callback takes the value, key, and the set itself (the latter 2 are optional). Since Sets don't actually have keys, the key is the same as the value:

```js
const s = new Set([1, 2, 3, 4, 5]);

s.forEach((val) => {
    console.log(val);
});
```

Like `Map.prototype.forEach`, `Set.prototype.forEach` takes an optional second argument to set the value of `this` when `forEach` is executing.

Like Maps, in currently standards-compliant versions of JavaScript the iterator will go in the order in which the elements were added to the set. This behavior is not guaranteed in earlier versions, so if you have to support older browsers or older versions of Node you can't depend on that behavior.

## Arrays

Arrays are an ordered collection of elements. The elements of an array can be any type, including other arrays. Like with Maps and Sets, you should try to have all the elements of your array be the same type except for under certain circumstances. If you're using an array to represent a tuple of elements, and you know ahead of time which element of the array will be which type, then it's ok to mix types. A tuple is a collection of mixed-type elements, similar to an object's values, but without keys. It's just a collection of values.

### Constructing Arrays

The simplest way to construct an array is with the literal syntax:

```js
// an array of numbers
const nums = [ 1, 2, 3, 4 ];

// a tuple of mixed types
const person = [ "Jason", 42 ];
```

You can create an array of empty elements by passing an integer to the `Array` constructor:

```js
const empties = Array(5); //-> an array of 5 undefined elements
```

If you have an iterable object, you can convert it to an array using `Array.from`:

```js
const arr = Array.from(s); //-> [ 1, 2, 3, 4 ]
```

`Array.from` also takes an optional 2nd argument for a mapping callback function. This function will be applied to every element of the array, and can take 1 or 2 parameters. The first argument is the element of the iterable the function is being applied to, and the 2nd (optional) argument is the numeric index of the element being mapped:

```js
// note the use of _ to indicate an unused argument
// this is a convention used in many languages
const squares = Array.from(Array(4), (_, i) => i * i); //-> [ 1, 4, 9, 16 ];
```

`Array.from` can actually receive a 3rd argument, which sets the value of `this`, but I've never used it.

You can also pass any object with a `length` property to the first argument of `Array.from` and it will create an array with that many elements. You can use this to create ranges of numbers:

```js
function makeRange(start, stop, step) {
    return Array.from(
        { length: (stop - start) / step + 1 },
        (_, i) => start + (i * step)
    );
}
```

Finally, you can create an array from an arbitrary-length list of arguments using `Array.of`:

```js
const ofArray = Array.of(1, 2, 3, 4); //-> [ 1, 2, 3, 4 ]
```

### Accessing Array Elements

You access an array element by its numeric index in brackets (i.e. a member expression with bracket syntax). The first element of the array has the index 0.

```js
squares[2]; //-> 9
```

You can also add elements to an array by assigning to a member expression that has the number of an index not yet defined:

```js
squares[4] = 25; // array is now [ 1, 4, 9, 16, 25 ]
```

You can skip indexes when adding new elements. This will fill the spots between the last element in the array and the new element with `undefined`.

```js
squares[7] = 64;
// array is now [ 1, 4, 9, 16, 25, undefined, undefined, 64 ]
```

To know how many elements are in an array, including empty elements, use the `length` property:

```js
squares.length; //-> 8
```

### Bracket Access and Strings

Note that you can also use bracket notation to access a character in a string, starting with 0 as the index of the first character. In programming we start counting from 0 instead of 1, which can be confusing for beginners.

Even more confusingly, the index isn't based on Unicode scalars like it is when you iterate over a string. The index stands for the UTF-16 code unit found at the index. You'll remember from the last chapter that a UTF-16 character is made up of at least 1 16 bit (2 byte) numeric value mapped to text. In JavaScript these values are called char codes. For people who write in English or another language with letters derived from the Latin alphabet, there will be no difference between most char code and code point numeric values in most cases.

### Iterating over Arrays

You can iterate over an array in 2 ways: with a regular for loop or a for...of loop.

To iterate over an array with a for loop, use the loop counter as the array index:

```js
for (let i = 0; i < squares.length; i++) {
    console.log(squares[i]);
}
```

Unless you have a good reason not to, though, it's usually better to use a for...of loop because then you don't have to worry about a counter:

```js
for (let num of squares) {
    console.log(num);
}
```

I suppose you could also use a while loop with a counter if you **really** wanted to, but don't do that unless you have a **really** good reason.

## Array Methods

Arrays have many useful methods that allow you to perform many different operations on their elements. I've divided them into 2 categories: "simple" methods that do not take a function as one of their arguments (except for `sort`, which **can** take a function but doesn't have to), and "higher-order methods" that **do** take a function as one of their arguments.

### Simple Array Methods

Here are some simple array methods.

#### `Array.isArray`

To check if an object is an array:

```js
const s = new Set([1, 2, 3, 4]);
Array.isArray(s); //-> false
```

#### `Array.prototype.at`

Gets the element at a given array index. If you give it a negative number, it will count back from the end (so -1 gets the last element of the array and so on):

```js
squares.at(-1); //-> 64
```

#### `Array.prototype.concat`

Concatenates two or more arrays together:

```js
const a1 = [ 1, 2 ];
const a2 = [ 3, 4 ];
a1.concat(a2); //-> [ 1, 2, 3, 4 ]
```

The `concat` method returns a new array and does not modify any of the original arrays.

#### `Array.prototype.flat`

"Flattens" nested arrays into a single array:

```js
const arr = [ [ 1, 2, 3 ], 4, 5 ];
arr.flat(); //-> [ 1, 2, 3, 4, 5 ]
```

You can pass in an optional integer argument to tell `flat` how many levels to un-nest:

```js
const arr = [ [ 1, 2, [ 3 ] ], 4, 5];
arr.flat(1); //-> [ 1, 2, [ 3 ], 4, 5 ]
```

#### `Array.prototype.includes`

Checks to see if an array has an element with a certain value. Remember that with reference types it has to be the exact same object or an alias.

```js
const nums = [ 1, 2, 3, 4, 5 ];
nums.includes(15); //-> false
```

#### `Array.prototype.indexOf`

Returns the index of the element if the argument matches an element in the array; otherwise it returns -1:

```js
nums.indexOf(3); //-> 2
nums.indexOf(100); //-> -1
```

#### `Array.prototype.join`

Joins together all the elements in an array as a single string. Takes an optional argument of a string that will be inserted between elements.

```js
const words = [ "Every", "good", "boy", "does", "fine" ];
words.join(" "); //-> "Every good boy does fine"
```

#### `Array.prototype.lastIndexOf`

Like `indexOf`, only it returns the index of the last time an element is found in an array. Returns -1 if not found.

```js
const nums = [ 1, 2, 3, 1, 3, 1];
nums.lastIndexOf(1); //-> 5
```

#### `Array.prototype.pop`

Removes and returns the last element of an array. If the array is empty, returns `undefined`.

```js
const nums = [ 1, 2, 3, 4, 5 ];
nums.pop(); //-> 5
// array is now [ 1, 2, 3, 4 ]
```

#### `Array.prototype.push`

Pushes its argument onto the end of the array, then returns the argument:

```js
[ 1, 2, 3 ].push(4); //-> returns 4
// array is now [ 1, 2, 3, 4 ]
```

#### `Array.prototype.reverse`

Reverses an array. Mutates the original array **and** returns the reversed array:

```js
[ 1, 2, 3, 4 ].reverse(); //-> [ 4, 3, 2, 1 ]
```

#### `Array.prototype.shift`

Like `pop`, but for the first element in the array. Returns `undefined` if the array is empty.

```js
const nums = [ 1, 2, 3, 4 ];
nums.shift(); //-> 1
// array is now [ 2, 3, 4 ]
```

#### `Array.prototype.slice`

Creates a copy of the elements of an array from the first index argument to an optional second index argument (not inclusive of the last index). If you only give it 1 argument, it copies to the end of the array. If you use negative numbers, it counts that index from the end of the array.

```js
[ 1, 2, 3, 4, 5 ].slice(1, 4); //-> [ 2, 3, 4 ]
```

#### `Array.prototype.sort`

Sorts the elements in an array. Takes an optional function to determine how to sort the elements. Note that if you call `sort` on an array that contains any type other than strings, and if you don't pass a sorting function as an argument, it will coerce the elements to strings and then sort them. This is most noticeable with numbers, because you get something very different from what you'd expect when sorting numbers:

```js
[ "boy", "cat", "apple" ].sort(); //-> [ "apple", "boy", "cat" ]
[ 2, 1, 5, 10, 22 ].sort(); //-> [ 1, 10, 2, 22, 5 ]
```

To sort numbers properly, use a sorting function. Note that your sorting function takes 2 arguments, which are the 2 elements of the array to compare for sorting. If the a element should go before the b element, your function should return a positive number. If the b element should go before the a element, your function should return a negative number. If the two elements have equal values that are being checked, it should return 0.

```js
[ 2, 1, 5, 10, 22 ].sort((a, b) => a - b); //-> [ 1, 2, 5, 10, 22 ]

const people = [
    { name: "Jason", age: 42 },
    { name: "Gretchen", age: 39 },
    { name: "Daniel", age: 9 }
];

// sort by the age property
people.sort((a, b) => a.age - b.age);

// sort by the name property
people.sort((a, b) => a > b ? 1 : a < b ? -1 : 0);
```

Like `reverse`, sort both mutates the original array **and** returns the sorted array.

#### `Array.prototype.unshift`

Like `push`, but at the beginning of the array.

```js
const nums = [ 2, 3, 4 ];
nums.unshift(1); //-> returns 1
// array is now [ 1, 2, 3, 4 ]
```

### Higher Order Array Methods

#### `Array.prototype.every`

Returns `true` if every element in the array satisfies a *predicate* (function that checks to see if something is true or false):

```js
[ 2, 4, 6, 8 ].every((el) => el % 2 === 0); //-> true
```

#### `Array.prototype.filter`

Removes elements from an array that do not satisfy the predicate. The predicate takes the array element as an argument, plus optional arguments for the current index and the array itself.

```js
[1, 2, 3, 4, 5, 6].filter((el) => el % 2 === 0); //-> [ 2, 4, 6 ]
```

#### `Array.prototype.find`

Finds and returns the first array element that satisfies a predicate. Returns `undefined` if there is no matching value.

```js
const people = [
    { name: "Jason", age: 42 },
    { name: "Gretchen", age: 39 },
    { name: "Daniel", age: 9 }
];

people.find((person) => person.name === "Jason");
```

#### `Array.prototype.findIndex`

Like `find`, but returns the numeric index of the first matching element. Returns -1 if none is found.

```js
people.findIndex((person) => person.age > 50); //-> -1
```

#### `Array.prototype.findLast` and `Array.prototype.findLastIndex`

Like `find` and `findIndex`, except that they work on the **last** matching element. Obviously if there's only 1 matching element they return a value for it. Return `undefined` and -1 respectively if there is no matching element.

#### `Array.prototype.flatMap`

Applies a mapping function to every element of the array and then flattens it. Like using `map` followed by `flat(1)`, but somewhat more efficient.

```js
const words = [ [ "It's", "always" ], [ "sunny" ], [ "in Philadelphia" ] ];
// split is a method that splits a string into an array
words.flatMap((el) => el.split(" "));
//-> [ "It's", "always", "sunny", "in", "Philadelphia" ]
```

#### `Array.prototype.forEach`

Similiar to the `forEach` methods for Maps and Sets. The callback takes the current array element then optional arguments for the current index and the array itself.

```js
[ 1, 2, 3, 4, 5, 6 ].forEach((n, i, a) => {
    if (n % 2 === 0) {
        console.log("This array element is even");
    }

    if (i % 2 === 0) {
        console.log("This index is even");
    }

    if (i === a.length - 1) {
        console.log("This is the last element");
    }
});
```

#### `Array.prototype.map`

Applies a callback that maps each element of the array to a new value, then returns an array of the transformed values. Does not modify the original array. Callback can take the same arguments as `forEach`.

```js
const people = [
    { name: "Jason", age: 42 },
    { name: "Gretchen", age: 39 },
    { name: "Daniel", age: 9 }
];

people.map((person) => person.name); //-> [ "Jason", "Gretchen", "Daniel" ]
```

#### `Array.prototype.reduce`

Applies a reducer function and accumulates a value that is returned after the last iteration. The callback takes 2 required parameters: the accumulator value and the current value. Then it can take 2 optional arguments for the current index and the array itself. `reduce` takes an optional second argument that sets the initial value of the accumulator. If no initial value is given, it will use the 1st element's value. I recommend always passing in an initializer argument to avoid confusion and hard-to-diagnose bugs.

`reduce` is extremely powerful. You can actually implement any other higher-order array method's functionality using `reduce`.

```js
/**
 * Sums up an array of numbers
 * @param {number[]} numbers
 * @returns {number}
 */
function sum(numbers) {
    return numbers.reduce((sum, num) => sum + num, 0);
}

// uses the range function from above
// remember it doesn't include the
// end number in the range
sum(range(1, 11, 1)); //-> 55

// using reduce like map
people.reduce((names, person) => {
    // later in the chapter I'll show
    // you a better way to do this
    names.push(person.name);
    return names;
}, []);
```

There is also a `reduceRight` method that is exactly the same except that it iterates backwards, start from the last element of the array and going to the first.

#### `Array.prototype.some`

Checks to see if **any** element in the array satisfies a predicate:

```js
people.some((person) => person.age > 40); //-> true
```

### More Array Methods

For complete documentation of how to use arrays and all array methods, including a few not covered here, see [the MDN Array entry](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array).

## Working with Arrays

Here's an example that puts together several array methods to perform a task similar to something you might need to do at some point:

```js
// let's assume getUsers returns a list of users from a database
const users = getUsers();

const adults = users
                // note that to return an object from an arrow function
                // you must wrap it in parentheses if there's no
                // block statement with a return statement
                .map((user) => ({ email: user.email, name: user.name, age: user.age }))
                // reject all the users younger than 18
                .filter((user) => user.age >= 18)
                // sort from oldest to youngest
                .sort((a, b) => b.age - a.age);
```

The above example fetches a list of users from the database. We only care about 3 data attributes for each user, so we use `map` to cherry pick those 3 attributes from the initial user object. Then we only want users who are adults, so we `filter` out all the users who aren't at least 18 years old. Finally, we want them sorted from oldest to youngest so we pass a comparing function to `sort`.

## Iterable Objects

There are several objects in JavaScript that are iterable. You've seen Maps, Sets, MapIterators, SetIterators, MapEntries, and SetEntries objects already in this chapter. Later in this chapter you'll see the `arguments` object, which is another iterable object.

You can also create your own iterable objects by adding a special `[Symbol.iterator]` method to your object.

You'll often see people refer to iterable objects as "array-like objects."

### Spreading Iterables

Similar to how you can spread objects, you can spread arrays. You can spread an array into another array:

```js
const people = [ "Jason", "Gretchen", "Daniel" ];
const morePeople = [ ...people, "Josh", "Chris" ];
```

In one of the examples for `reduce` I used the `push` method to add an element to an array like this:

```js
people.reduce((names, person) => {
    names.push(person.name);
    return names;
}, []);
```

It's better to avoid mutating the accumulator whenever possible. In fact, it's generally better to avoid mutating arrays at all unless you make a copy first and then mutate the copy. You always want to have a way to get back to your original data unless it's absolutely impossible, which may be the case if you're working with absolutely massive arrays that would make it far too slow and resource-intensive to make a copy, but that's about the only time you'd need to do that.

You can copy and update the accumulator array by spreading it:

```js
people.reduce((names, person) => [ ...names, person.name ]);
```

You can also spread an array as some or all of the arguments to a function if the array contains the right number of compatible values for the function.

```js
/**
 * Adds 3 numbers
 * @param {number} n1
 * @param {number} n2
 * @param {number} n3
 * @returns {number}
 */
function add3(n1, n2, n3) {
    return n1 + n2 + n3;
}

const nums = [1, 2, 3];
add3(...nums); //-> 6
```

One neat thing about spreading is that it actually works with any iterable, not just arrays. That's because the interpreter uses the `[Symbol.iterator]` method to figure out how to spread out the elements.

### Iterable Destructuring

Like with objects, you can destructure array values:

```js
const names = [ "Jason", "Gretchen", "Daniel" ];
const [ jason, gretchen, daniel ] = names;
```

You can name the variables anything you want. Skip an array index by leaving an empty space between two commas:

```js
const [ jason, , daniel ] = names;
```

Finally, you can gather unused array elements from destructuring into another array. This is called a rest variable.

```js
const [ jason, ...rest ] = names;  //-> rest is [ "Gretchen", "Daniel" ]
```

You can also destructure any other iterable object. Do note that if you use a rest variable it will gather the remaining elements into an array, because destructuring doesn't know about the constructor for whatever kind of object you're destructuring from.

You can also combine destructuring arrays and objects:

```js
const { name, games: [ golf, ...games ] } = { name: "Jason", games: [ "Golf", "Chess", "D&D" ] };
```

## Variadic Functions

Now that you know how iterables work, you can see how to write *variadic functions*. A variadic function is one that can take any number of optional arguments after whatever required arguments are given.

### The `arguments` Object

One way to create variadic functions is by using the `arguments` object. This object is an iterable object that collects all the arguments passed into a function. You can use bracket syntax member expressions with integer indexes to access the items in `arguments`.

It's **not** an array, so if you want to do anything interesting with it you need to create an array from it:

```js
const argsArray = Array.from(arguments);
```

The `arguments` object will include all arguments passed into a function even if there are more arguments given than formal parameters defined for the function.

Note that only `function` functions, both function declarations and function expressions, have an `arguments` object. Arrow functions do not have `arguments`, so if you try to use it inside the body of an arrow function you'll get an error.

### Using a Rest Parameter

The other way to define a variadic function, which is the one I recommend you use because it works with any function and it's more flexible, is with a rest parameter.

A rest parameter is similar to a rest variable when destructuring. You use the same `...` operator before the parameter name. A rest parameter should always be last in your parameter list. Like with destructuring, arguments starting at the position of the rest parameter will be collected into an array. You can name the rest parameter whatever you want.

```js
/**
 * Sum up a variadic number of numbers
 * @param {...number} numbers
 * @returns {number}
 */
function sum(...numbers) {
    return numbers.reduce((sum, num) => sum + num, 0);
}

sum(1, 2, 3, 4, 5); //-> 15
```

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

Now, you might look at the above code and be confused: shouldn't the algorithm's running time be O(2n)? After all, there are 2 iterations for each name. One when the name is being entered, and one when it is being printed.

That's a good observation, and in real terms doubling the number of iterations could absolutely affect how long it takes an algorithm to run in real time.

However, in terms of Big O, it's still just O(n) because there is only one set of inputs. The running time of the code is a function of how large the input is, regardless of whether that input is processed 1, 2, or 1,000 times. The number of steps it takes to process the input in each iteration isn't important.

As a general rule with Big O, if you count the steps and come up with a constant value then you can substitute 1 for the constant.

Big O notation is an **approximation** of the running time, not a calculation of every little thing that affects how long it actually takes for the computation to run.

## Recap

In this chapter we covered the built-in collection types in JavaScript: Maps, Sets, and Arrays. We also introduced iterable objects, explained variadic functions, and learned about linear time algorithms.

The next chapter is our first project! We're going to build an old school text-based adventure game you can play in your console.

I hope you're as excited as I am!

## Exercises
