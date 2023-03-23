# Built-In Collection Types: Maps, Sets, and Arrays

JavaScript has 3 built-in *collection* types that we will look at in this chapter. A collection type allows you to group data together in a structured manner. The 3 built-in collection types are Maps, Sets, and Arrays.

## Maps

A JavaScript Map is an object made up of key/value pairs. It's conceptually similar to an object, except that with an object you can only have property keys that are strings or symbols. The keys of a Map entry can be any type, including objects and even other Maps.

You can mix and match the types of your keys and values, but don't. Maps are most effective when you have the same type for all keys and the same type for all values. Obviously the key and value types don't have to be the same. If you **absolutely** need different types for values and you need keys with a type other than string or symbol, you have to use a Map. You should avoid getting into situations where this is necessary. If you need entry values with different types, an object is more appropriate than a Map.

## Constructing Maps

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

## Working with Maps

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

## Iterating over A Map

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

In current standards-compliant JavaScript interpreters (as of April, 2023), the order of entries when you iterate over the Map will be the same order in which they were added. This was not true when Maps were first added to the language, so if you have to work with older versions of a browser or Node you may not be able to depend on that behavior.

## Sets

JavaScript Sets are similar to arrays, except that they are guaranteed to not contain duplicate values. Note that when dealing with a Set of objects the values are only considered to be duplicate if they are the exact same object or aliases of the same object.

## Constructing Sets

Just like with Maps, there is no literal syntax to create Sets. You have to use the Set constructor:

```js
let s = new Set();
```

If you pass an array or other iterable to the Set constructor, it will deduplicate the object's values and add each unique value to the set:

```js
let s = new Set([ 1, 2, 3, 2, 1, 4, 3 ]); //-> Set { 1, 2, 3, 4 }
```

## Working with Sets

## Iterating over A Set

## Arrays

## Constructing Arrays

## Iterating over Arrays

## Simple Array Methods

## Higher Order Array Methods

## Iterable Objects

## Spreading Iterables

## Iterable Destructuring

## Variadic Functions

## The `arguments` Object

## Using a Rest Parameter

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

Now, you might look at the above code and be confused: shouldn't the algorithm's running time be O(2n)? After all, there are 2 iterations for each name. One when the name is being entered, and one when it is being printed.

That's a good observation, and in real terms doubling the number of iterations could absolutely affect how long it takes an algorithm to run in real time.

However, in terms of Big O, it's still just O(n) because there is only one set of inputs. The running time of the code is a function of how large the input is, regardless of whether that input is processed 1, 2, or 1,000 times. The number of steps it takes to process the input in each iteration isn't important.

Big O notation is an **approximation** of the running time, not a calculation of every little thing that affects how long it actually takes for the computation to run.

## Recap

## Exercises
