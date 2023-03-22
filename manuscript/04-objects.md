# Working with Compound Data Types

You know enough JavaScript now to do some pretty complex stuff. It's time to grow your knowledge of data types in JavaScript to match the skills you've learned so far. In this chapter we're going to cover *compound data types*, which are data types made up of amalgamations of other data, including both primitive types and other compound types.

One thing to note as we go through this chapter: the `typeof` operator isn't going to be terribly useful here because all the types we're about to explore will evaluate to "object" if you use `typeof` on them.

That's because according to the JavaScript type system we have 2 different kinds of types: primitive types and objects.

You'll also remember that the special value `null` has type `object` as well, because it represents an empty value that **could** be an object. One that **would** be an object if it wasn't empty.

In this chapter we'll look at Objects (obviously), Arrays, Maps, Sets, and Dates. We'll learn how to use Arrays and Array-like objects to write variadic functions (functions that take a variable number of arguments), and we'll also expand our JavaScript evaluation model to include some things that are very important for evaluating objects and executing code that works on objects.

## Objects

The simplest kind of JavaScript object is simply `Object`.

Object is a constructor, just like String and Number. We'll explore constructors later in the chapter, but for now it's enough to know that all objects get properties from Object.

Some languages make you define object types with classes before you can construct actual instances of objects. JavaScript does not. We **do** have classes in JavaScript, but they work differently from classes you may have used in other programming languages.

Instead of constructing objects from classes, JavaScript has *prototype* based objects. We'll get deep into prototypes and how they work later in the book, but for now it's enough to know that any object can be the prototype for any other object.

You can also create object literals using curly braces:

```js
const obj = {};
```

The above snipped creates an object with no additional properties besides the default ones that come on every object (almost, we'll see exceptions later in the book).

You can define properties as key/value pairs on an object literal:

```js
const person = {
    name: "Jason",
    age: 42
};
```

Then you access the property data using member expressions:

```js
person.name //-> "Jason"
```

An object property's key can be any string or symbol. If the key is a string that is a valid variable name, you can define it without quotation marks. If the key is not a valid variable name, you have to use quotation marks:

```js
const invalidVariableNames = {
    "+": function(a, b) {
        return a + b;
    },
    "my name": "Jason"
}
```

Remember, a function can be used as just another data value, so it's perfectly acceptable to assign a function to an object property as a method. In this case, we need to use bracket notation to call the method since "+" isn't a valid variable name:

```js
invalidVariableNames["+"](2, 3); //-> 5
```

Actually, that's not quite true. You can use numbers as property keys:

```js
const people = {
    0: {
        name: "Jason",
        age: 42
    },
    1: {
        name: "Daniel",
        age: 9
    }
}
```

Then access the value with bracket notation:

```js
people[0]; //-> { name: "Jason", age: 42 }
```

You can also use bracket notation to compute object property names. This works well with using symbol property keys:

```js
const person = {
    [Symbol.for("name")]: "Jason",
    ["a" + "g" + "e"]: 42
}
```

### Aliasing Objects

You can alias (make a new name for) an object simply by assigning it to a new variable. Look out, though, because if you make any changes to (mutate) the new alias it will also mutate the original object as well. That's because objects are *reference types*.

Primitives like number are value types, which means the value is immutable and when you assign it to a variable it essentially copies the data to the new variable. Immutable means it can't be changed, and numbers are immutable because 10 is always 10 no matter what you do to it. Since objects are reference types, when you alias an object no data is copied. The interpreter just creates a new pointer to the same object in memory, which is shared between the different variables.

```js
let a = 10;
let b = a; // a === 10
b = 15;
a; //-> still 10

const person = { name: "Jason" };
const jason = person;
jason.age = 42;
person; //-> { name: "Jason", age: 42 }
```

Note that the `person` object is still mutated **even though it was declared with `const`**. That's because `const` only creates a constant **binding**. It does **not** create immutable data. If you want an object to be immutable, you have to use `Object.freeze`:

```js
const person = { name: "Jason" };
Object.freeze(person);
person.age = 42 // ERROR
```

### Simple Data Objects, a.k.a. POJOs

Simply defining properties with values on object literals is the simplest way to use objects in JavaScript. If you're familiar with functional programming languages like Elm or F#, this is similar to how records work in those languages.

These simple data objects are also known as POJOs, which stands for "Plain Old JavaScript Objects."

If you never learned anything else about JavaScript except what we've covered so far in this book, you could still write any program you ever wanted to write in JavaScript. True, we haven't covered Arrays yet, but you can simply think of Arrays as objects with numeric property keys and everything will work just fine.

You can create a factory function to construct Person objects:

```js
/**
 * @typedef Person
 * @property {string} name
 * @property {number} age
 */

/**
 * Constructs a Person
 * @param {string} name
 * @param {number} age
 * @returns {Person}
 */
function makePerson(name, age) {
    return { name, age };
}
```

Notice that in the above function we don't use `name: name` or `age: age` like you might expect. That's because JavaScript allows *shorthand property definitions*. When a variable and object property have the same name, you only have to include the variable name when constructing your object literal.

### Methods

As you saw previously, you can simply define a function as an object property's value and use it as a method. JavaScript has an abbreviated form of this when creating object literals:

```js
const person = {
    name: "Jason",
    age: 42,
    greet(name) {
        return "Hello, " + name + "!";
    }
}

person.greet("Vikram"); //-> "Hello, Vikram!"
```

If you want your `Person` factory function to return objects with the method on them, simply add the method to the object returned by the function:

```js
/**
 * Constructs a Person
 * @param {string} name
 * @param {number} age
 * @returns {Person}
 */
function makePerson(name, age) {
    return {
        name,
        age,
        greet(name) {
            return "Hello, " + name + "!";
        }
    };
}
```

Now all your `Person` objects will have the "greet" method.

### The `this` Keyword

You can see how all this is useful, I'm sure, but what if you want the object to be able to reference itself?

Let's say you want your Person object to have an "introduce" method that allows a Person to tell someone else their name.

You do this using the `this` keyword:

```js
/**
 * Constructs a Person
 * @param {string} name
 * @param {number} age
 * @returns {Person}
 */
function makePerson(name, age) {
    return {
        name,
        age,
        introduce() {
            return "Hello, I'm " + this.name;
        }
    }
}
```

You reference a property on `this` in exactly the same way as you would on any other object.

It seems simple, but `this` can actually get surprisingly complex in JavaScript. I'll explain more about that later in the chapter.

### Iterating over Objects

You can iterate over all an object's properties. This can be done in 2 ways:

```js
const person = {
    name: "Jason",
    age: 42,
    nationality: "United States",
    nativeLanguage: "English"
};

// iterate using a for...in loop
for (let key in person) {
    console.log(key + ": " + person[key]);
}

// iterate using Object.keys
for (let key of Object.keys(person)) {
    console.log(key + ": " + person[key]);
}

```

Note that `Object.keys` only works with string keys that are *own properties* on the object. An object's own properties are the ones defined directly on the object itself, and not inherited from another object as prototype properties.

### Configuring Object Properties

Both of these techniques only work with properties that are enumerable. An object's properties are set to enumerable by default, but you can use the `Object.defineProperty` method to define non-enumerable properties:

```js
const person = {
    name: "Jason",
    age: 42
};

Object.defineProperty(person, "name", { enumerable: false });
```

In addition to the `enumerable` attribute, you can also use the 3rd argument to `Object.defineProperty` to set whether a property is configurable and writeable. Configurable means the property's attributes can be further edited. Writeable means the value of the property can be overwritten with something else. Trying to edit a non-writeable property will throw an error.

The other things you can define through the 3rd argument to `Object.defineProperty` are the property's value, a getter function, and a setter function. Here's an example of a getter function for a property "fullName" that isn't defined directly on the object itself:

```js
const person = {
    firstName: "Jason",
    lastName: "Barr",
    age: 42,
    get fullName() {
        return this.firstName + " " + this.lastName;
    }
};

// use with a simple member expression
person.fullName; //-> "Jason Barr"
```

You can use getters and setters for properties already defined on an object or for "virtual" properties like this one that have not been defined on the object. To use `Object.defineProperty` to set the above getter:

```js
Object.defineProperty(person, "fullName", {
    get: function() {
        return this.firstName + " " + this.lastName;
    }
});
```

When you create a setter function, you can set the value of that property using a simple member expression and the function's logic will be applied to the value:

```js
const person {
    // ...
    set firstName(name) {
        return name[0].toUpperCase() + name.slice(1).toLowerCase();
    }
};

person.firstName = "rObErT";
person.firstName; //-> Robert
```

You can also use `Object.defineProperty` for setters just like you can with getters.

Note that you can also use bracket notation to access a character in a string, starting with 0 as the index of the first character. In programming we start counting from 0 instead of 1, which can be confusing for beginners.

Even more confusingly, the index isn't based on Unicode scalars like it is when you iterate over a string. The index stands for the UTF-16 code unit found at the index. You'll remember from the last chapter that a UTF-16 character is made up of at least 1 16 bit (2 byte) numeric value mapped to text. In JavaScript these values are called char codes. For people who write in English or another language with letters derived from the Latin alphabet, there will be no difference between most char code and code point numeric values in most cases.

### Constructor Functions and The `new` Keyword

You saw above how to use a factory function to create objects of a certain type. JavaScript also has special functions called constructors that can be used to create objects. You define a constructor function just like any other function, but assign values to properties using the `this` keyword:

```js
/**
 * @typedef Person
 * @property {string} name
 * @property {number} age
 */
/**
 * Constructs a Person
 * @constructor {Person}
 * @param {string} name
 * @param {number} age
 */
function Person(name, age) {
    this.name = name;
    this.age = age;
}
```

To use a constructor function to create an object, use the `new` keyword:

```js
const jason = new Person("Jason", 42);
```

### Prototypes

What if you want your Person object to have methods on them?

One way to do this is simply to define the methods in the constructor:

```js
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.greet = function(name) {
        return "Hello, " + name + "!";
    };
}
```

This works, but it wastes space because every copy of the Person type has its own copy of the function. That's no big deal when you're talking about 2 or 3 instances of your object, but what if you're building something like a game where you could have thousands or even hundreds of thousands of instances? In that case, the extra memory used by multiple copies of the same function could cause problems.

Since the function is always the same, there's no need to have a separate copy of it for every object instance. A single copy that's shared by all the instances is just what we need. In JavaScript, we do this through prototypes.

There are a few different ways to assign methods to an object type's prototype.

First, we can assign them directly to the "prototype" property of the Person constructor:

```js
/**
 * @typedef Person
 * @property {string} name
 * @property {number} age
 */
/**
 * Constructs a Person
 * @constructor {Person}
 * @param {string} name
 * @param {number} age
 */
function Person(name, age) {
    this.name = name;
    this.age = age;
}

Person.prototype.greet = function(name) {
    return "Hello, " + name + "!";
};
```

Second, we can create an object with methods to be shared by the instances of our Person type and then set it to be the constructor's "prototype" property:

```js
const personProto = {
    greet(name) {
        return "Hello, " + name + "!";
    },

    introduce() {
        return "Hello, I'm " + this.name;
    },

    haveBirthday() {
        this.age++;
    }
};

// definition of Person constructor
Person.prototype = personProto;
```

Either way works just as well.

### Creating a Static Constructor

I personally am not a huge fan of the `new [Constructor]` syntax. I prefer a more functional interface for object creation, so I like to add a *static* constructor method to my constructors.

A static method or property is simply one that's attached to the constructor itself, instead of the instance.

To create a static "new" method that constructs an object, all you have to do is abstract away the `new` syntax in its own method:

```js
Person.new = function(name, age) {
    return new Person(name, age);
}

// now create a new Person
const jason = Person.new("Jason", 42);
```

You don't have to do it this way if you prefer using the `new` syntax to construct objects; it's just my personal preference.

#### The Constructor Property

Using a constructor this way gives Person instances a constructor property that resolves to the Person constructor:

```js
const jason = Person.new("Jason", 42);
jason.constructor; //-> Person {}
jason.constructor.name; //-> "Person"
```

### Extending and Inheriting Objects

If you have any experience with object oriented programming, you know that inheritance is an important way of extending an object's functionality. In JavaScript, inheritance works through prototypes. You can create an object with another object as its prototype using the `Object.create` method, and thus create a subtype of the first object:

```js
const programmerProto = Object.create(Person.prototype);
```

Now the `programmerProto` object has `Person.prototype` as its prototype, so it has all the properties and methods of that object. Next you can add additional methods to the prototype using `Object.assign`, which adds all the properties of one object to another object:

```js
const programmerMethods = {
    writeCode() {
        console.log("I am writing a program");
    }
};

Object.assign(programmerProto, programmerMethods);
```

Note that `Object.assign` mutates its target directly, adding properties right onto the leftmost object. Mutating an object can be problematic, because you might assume you have one kind of data when in fact you have something different, so it's common to use `Object.assign` with an empty object as its target. Note that you can pass as many arguments to `Object.assign` as you need. It will simply write the properties to the first object, starting with the 2nd argument and continuing rightward. If 2 objects have the same property key, the last one on the right "wins."

```js
// assume the Programmer constructor exists
Programmer.prototype = Object.assign({}, programmerProto, programmerMethods);
```

There is one problem with setting up inheritance this way with `Object.create`: it obfuscates the "constructor" property on the subtype's constructor.

You can use `Object.setPrototypeOf` to solve this problem:

```js
/**
 * @typedef Programmer
 * @property {string} name
 * @property {number} age
 * @property {string[]} languages
 */
/**
 * Constructs a Programmer
 * @param {string} name
 * @param {number} age
 * @param {string[]} languages
 */
function Programmer(name, age, languages) {
    this.name = name;
    this.age = age;
    this.languages = languages;

    Object.setPrototypeOf(Programmer.prototype, Person.prototype);
    Object.setPrototypeOf(Programmer, Person);
}

// merge new methods onto Programmer.prototype
Programmer.prototype = Object.assign(Programmer.prototype, programmerMethods);

// add a static constructor (optional)
Programmer.new = function(name, age, languages) {
    return new Programmer(name, age, languages);
};
```

Now you have a fully-featured Programmer type that inherits from the Person type.

Note that `string[]` in the docblock above denotes an array of strings. We'll talk about arrays in the next chapter.

Using `Object.setPrototypeOf` like this properly sets up the inheritance chain both for the Programmer constructor and the prototype of Programmer instances. `Object.create` still works perfectly fine if all you want to do is have one object inherit from another object.

### Classes

Prototypal inheritance is extremely powerful and flexible, but sometimes you don't need all that. There's a simpler way to do all this prototypal inheritance if all you want to do is set up a simple inheritance chain, and that's using classes. Classes were introduced to JavaScript in ES2015, and abstract away the messy details of setting prototypes an inheritance. To create a Person class, just use the `class` keyword:

```js
/**
 * @class Person
 * @property {string} name
 * @property {number} age
 */
class Person {
    /**
     * Constructs a Person
     * @param {string} name
     * @param {number} age
     */
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    /**
     * Static constructor method
     * @param {string} name
     * @param {number} age
     * @returns {Person}
     */
    static new(name, age) {
        return new Person(name, age);
    }

    /**
     * Getter for name property
     * @returns {string}
     */
    get name() {
        return this.name;
    }

    /**
     * Setter for name property
     * @param {string} newName
     */
    set name(newName) {
        this.name = newName[0].toUpperCase() + newName.slice(1).toLowerCase();
    }

    /**
     * Greet another Person
     * @param {string} name
     * @returns {string}
     */
    greet(name) {
        return "Hello, " + name + "!";
    }

    /**
     * Introduce yourself to another Person
     */
    introduce() {
        return "Hello, I'm " + this.name + ".";
    }
}

// create a new Person
const jason = new Person("Jason", 42);

// or use the static method
const daniel = Person.new("Daniel", 9);
```

Now if you want to create a Programmer class that inherits from the Person class, you need to use the `extends` and `super` keywords:

```js
/**
 * @class Programmer
 * @extends Person
 * @property {string} name
 * @property {number} age
 * @property {string[]} languages
 */
class Programmer extends Person {
    /**
     * Constructs a Programmer
     * @param {string} name
     * @param {number} age
     * @param {string[]} languages
     */
    constructor(name, age, languages) {
        super(name, age);
        this.languages = languages;
    }

    /**
     * How do you know if a person is a programmer?
     * Do nothing and they'll tell you!
     * @returns {string}
     */
    introduce() {
        const superGreeting = super.greet();
        return superGreeting + " I am a programmer!";
    }
}
```

`extends` tells the interpreter you want to set up prototypal inheritance between the Programmer and Person classes, and `super` means you're accessing either the constructor (when called as if it were a function) or a property (when used as if it were an object) on the class you're inheriting from.

Note that classes don't actually introduce new functionality into JavaScript. They're just a convenient shorthand for prototypes and prototypal inheritance.

### Native Constructors

Now you know what I mean when I say String, Number, etc. are constructors. There's a small difference between these native constructors and ones you'll define, though.

You should avoid doing things like this:

```js
const five = new Number(5);
```

It's technically legal, and you can use the resulting value in much the same way as you could use a primitive number, but then there's this:

```js
typeof five; //-> "object"
```

Were you expecting "number"? Whoops!

On top of that, it will throw an error if you try to use `new` with the constructors of the 2 newer primitive types, BigInt and Symbol.

As a general rule, using the constructors of primitive types to create objects is a bad idea; however, you **can** use the constructors as simple functions to cast values from one type to another:

```js
Number("5"); //-> 5
String(true); //-> "true"
Boolean(""); //-> false
BigInt(10); //-> 10n
```

You can also extend a native constructor as a class, though that's not something you're going to need to do very often (if ever):

```js
class MyArray extends Array {}
```

### Objects as Function Parameters

You can pass objects to any function just like you can with any other value, however there are a couple of important things to take note of:

1. Objects are passed by reference, which means when you pass an object into a function you're not passing a copy of the data, but a reference to the actual object itself which means anything you change in the function will change on the object itself both in and outside of the function
2. Objects are mutable by default, which means you can easily change anything about the object and JavaScript won't stop or warn you

So if you do something like this:

```js
const person = new Person("Jason", 42);

/**
 * Change the name of a Person instance
 * @param {Person} person
 * @param {string} name
 * @returns {Person}
 */
function changeName(person, name) {
    person.name = name;
    return person;
}

changeName(person, "Bob");
```

it will actually mutate the object everywhere it is used in the program, which may not be what you want.

It's usually better to create a copy of the object and return that. You can do that with `Object.assign`:

```js
function changeName(person, name) {
    return Object.assign({}, person, { name });
}
```

You can also do it with the spread operator, which we'll cover in the next section.

### The Spread Operator

### Destructuring Objects

## Dates

## JavaScript Evaluation Model

### Function Calls and `this`

### The Differences between `function` Functions and Arrow Functions

## Recap

## Exercises
