# Introduction

You are about to embark on an amazing journey.

Learning to program is a highly-sought after skill in today's technology-driven world. By learning to program, you can dramatically increase your income in a short period of time while you get to work on interesting and challenging projects. Not only that, but programming can even be lots of fun! There's nothing quite like encountering a problem, working through a solution, and building it in code.

Most importantly, you'll learn thinking skills that can be applied to any problem domain, code-related or not.

This book is a bit different from most "learn to code" books. Too many programming books for beginners teach you the syntax and basics of a programming language, but that's where it ends.

This book does more.

Yes, you'll learn the syntax of JavaScript and how to write programs both in the browser and on the server, but you'll get much more than that.

The goal of this book is to go beyond learning the language and teach you how to *think like a programmer*.

Thinking like a programmer means adopting the mental practices a programmer uses to build effective solutions in code to the real world problems they face.

It means having the ability to break a problem down into its smallest pieces, understanding what needs to happen to solve the problem, and then (and *only* then) recognizing how to write the code.

It means understanding the *exact steps* your program needs to take to compute the result you're looking for.

The series of steps your program needs to take in order to compute is called an *algorithm*. There are several different kinds of algorithms, and you'll see many of them in this book. You'll also learn how to create your own algorithms and analyze them to see how they might be improved.

As we work through this book, we'll progressively develop mental models of computation to help you understand how JavaScript works at a deep level. We'll create rules that govern how programs will run. Essentially, we'll construct a model for a JavaScript interpreter.

An interpreter is another program, outside the JavaScript programs you'll write, that executes the programs written in some programming language (in this case, JavaScript).

You'll also learn how to design rock-solid, efficient programs and how to solve any coding problem so you'll ace your technical interviews when it's time to get a programming job.

## Software Engineering

Thinking like a programmer means thinking like an engineer. You are *engineering* solutions in code to the problems you face.

Software engineering is often a back-and-forth process.

You solve a little bit of the problem, then test your solution.

Then you solve another piece of the problem, and test again.

Lather, rinse, and repeat until you have a bulletproof program that comprehensively solves the entire problem.

You won't just understand how to write programs after working through this book (and I do **highly** suggest you work through the exercises and projects).

You'll understand how the computer works more deeply than most beginning (and even intermediate) programmers. You'll also know how to analyze your programs and, whenever possible, make them more efficient.

Also, if you work through this whole book you'll cover about as much as you would learn from an introductory computer science course at a top university. You'll work through some pretty advanced programming problems!

## About The Book

The book is divided into 3 sections:

1. Basic JavaScript Programming
2. Intermediate Programming
3. Final Project

The sections are divided into 6 or 7 chapters each.

Each chapter covers an important topic you need to know to become a proficient JavaScript programmer.

There are exercises at the end of each chapter, and there's a project at the end of each section.

These project chapters don't just walk you through the code for the project. They show you the steps of breaking down the problem and thinking through the solution. Just like what you'll need to do in real life once you get your first programming job.

Finally, there's a massive final project that will make a worthy addition to any portfolio and give you the experience of taking a major project from design to deployment.

## How To Get The Most out of This Book

This is not a book for cramming. It is a book to be digested in pieces, preferably no larger than 1 chapter at a time.

Our brains learn best by breaking large, complex units of information into small "chunks." I've divided each chapter into topics and subtopics to try to help you with your chunking. Once you have an understanding of the concepts, you need to reinforce them through repetition, recall, and deliberate practice. That's where the exercises and projects come in.

You can help your brain learn the material more completely and permanently if you set aside time each day to work on it.

Practicing programming for 30 minutes a day 5 times a week is better than practicing for 2 and a half hours once a week.

It's also vital that you get as much high-quality sleep as possible, because that's when our brains really solidify the new neural connections we're creating through the learning process.

This should go without saying, but code along with the examples. Do the exercises. Even the harder ones. Do the projects. Expand on the projects using what you know. Get creative with it. The best way to learn to program well is to write programs.

The best way to get better at programming is to write more programs.

But you can't just throw up some code and be done with it; you have to make the effort to write **good** programs.

The better you get at writing programs, the more effective your practice will be at helping you learn this material deeply.

One of my main goals in this book is to help you not just learn a programming language, but to learn what **good** programs look like.

That way you have a measuring stick to hold your own work up against.

Keep at it, and one day you'll get better than me.

I hope you do!

Learning the basics of how to use a programming language is easy. Learning how to be an excellent programmer is really hard and takes a **lot** of work.

This book will demystify large parts of the process for you, but you have to put in the work yourself to really learn the material.

You have to practice writing lots of programs in order to get good at writing programs.

There are no shortcuts to mastery, but this book can serve as a trusted guide along the path.

## Programs and Programming Languages

We've talked a lot about writing and reasoning about programs, but what is a program? Moreover, what is a programming language?

A program is simply a combination of data and logic that computes a result.

To compute means to calculate. It's usually associated with numbers, although modern computers can map numbers onto text, images, and many other kinds of data. It's all numbers at the most basic level, though.

Computers only process numbers in their CPU (Central Processing Unit).

We abstract away those bare metal-level processes by writing programs that are broken down into code in a machine language the CPU can understand. Machine code consists only of binary numbers (0s and 1s). Breaking higher-level code down into binary machine code is known as compiling.

The machine code contains instructions that tell the CPU what to do, what data to use, and how to produce the result of the program.

Since the CPU doesn't process the programming language code directly, we have to build interpreters that can do that. Interpreters are themselves written in programming languages, and each level of execution runs in its own interpreter until it gets all the way down to the level of the CPU.

You can think of the CPU as a hardware interpreter that executes machine code.

There are dozens, maybe even hundreds of programming languages out there. All of them follow the same basic process. The programs written in a programming language either execute directly in the interpreter itself or are compiled down to machine code so the CPU can execute them.

Some languages are very low level, like assembly languages. Assembly languages operate directly at the level of instructions a CPU can perform. When you write code in assembly, you literally tell the CPU how to move data at the bits and bytes level.

Others, like C, are higher level, but still require you to work with memory and primitive computer processes.

Then there are still higher level languages, like JavaScript, where the details of memory management and CPU operations are hidden away and all you have to do is write the high level logic of the program to get the result you're looking for. The computer handles all the details for you.

## The Fundamental Elements of A Programming Language

Every programming language has 4 fundamental elements:

1. A set of primitive constructs
2. Syntax
3. Static semantics
4. Semantics

The primitive constructs are things like basic data types and built-in *functions*. Functions are like miniature programs that perform some action on some data. Sometimes you'll see them called "subroutines" or "procedures." Primitives are defined in the interpreter itself.

5 is a primitive in JavaScript. So is `+`, which is an operator. An operator is a kind of function that can be applied to its operands.

Syntax is simply the symbols used to write programs in a language, and the rules that define well-formed statements in it.

A construct called a *binary expression* has the syntactic form `operand operator operand`, so you can see that `5 + 5` is a syntactically valid statement in JavaScript; however, `5 5 +` is invalid. Entering invalid syntax into a program always causes an error that halts the program, and it is the most common type of error for beginning programmers.

Producing an error that halts the program is known as "throwing an error."

The static semantics of a language defines which syntactically well-formed statements have a valid meaning. For example, `"hello"` is another primitive, as is the operator `*`. `"hello" * 5` is a syntactically valid statement, because it follows the form operand operator operand, but it is static-semantically invalid because `"hello" * 5` does not produce a valid meaning in the language. In JavaScript, the interpreter signals to you that the statement is invalid by producing the value `NaN`, which stands for "Not a Number."

Static semantics also includes things like the type system of a language, and any other characteristics of the language that can be statically verified&mdash;that is, verified before the code is compiled or executed. Some languages even make it possible to verify parts of a program's logic ahead of time.

There are languages that do a great deal of pre-execution static semantic checking, like Java and Haskell. For better or worse, JavaScript does very little in that regard.

Finally, semantics is simply the actual meaning produced by a valid statement. That `5 + 5` produces 10 is a fact of semantics. You get semantic errors when a program produces a valid meaning, but it's not the one you intended. This includes things like logic errors in the code and implementing an algorithm incorrectly. For example, let's say you have the code `5 + "5"`, where you have the number 5 and try to add it to the text "5." JavaScript will let you do this; the result is the text "55." If you intended to add 2 numbers together, this is a semantic error and you'll have to find the source of the bug.

Semantic errors are the worst kind of error in a program because they can be very difficult to find. Sometimes they will cause the interpreter to throw an error, but often they don't. You don't even know they exist until you run the program that produces the wrong value.

You can reduce the number of semantic errors in your programs by writing tests, which we will do for some of the programs in this book.

## Abstraction and Combination

In addition to primitives like 5 and `+`, they also give you means of *abstraction* and *combination*.

Abstraction means hiding the details so the person using a thing doesn't need to know exactly how it works.

It's like driving a car. You don't need to know exactly how the accelerator pedal connects to the various mechanisms that make the car go faster. You just need to know how to push the pedal down to get to the speed you want. The actual process of how the car goes faster is abstracted away.

In a programming language, abstractions are bits of logic that allow you to treat them like primitives. They are often combinations of language primitives, as well as other abstractions defined by programmers.

Combination is exactly what it sounds like: you take primitives from the language and combine them together to create new functionality to augment what the language gives you.

You can combine primitives as well as other abstractions and combinations you (or other programmers) have defined.

Every program is made up of abstractions and combinations in some form.

## About JavaScript

JavaScript was originally designed for simple scripts you could use in web pages to provide interactivity and animation. Eventually it came to be used as a fully-fledged programming language for the creation of large and complex applications.

Brendan Eich, then a developer for Netscape, created the first version of the language in just 10 days back in 1995.

It was an amazing accomplishment, but as you'll see in this book it's also resulted in some strange quirks and behaviors other languages don't have.

Some of them are good, but some of them can be extremely frustrating. I'll give you a heads up when there's something you need to know that fits that description.

JavaScript most often runs in interpreters. The code is executed as the interpreter reads in and processes the code, rather than being compiled ahead of time into binary executables you can run later.

JavaScript has been influenced by several other programming languages, most notably Scheme and Self. From Scheme, we get first-class functions, closures, and other functional programming features. From Self, we get objects with prototypes. If you've programmed in another object oriented language, prototypes may be confusing at first but I'll demystify them for you in the relevant chapter. If you have no programming experience and have no idea what anything I've just said means, don't worry about it! I'll explain everything you need to know when we get there.

One last thing about JavaScript: you may see the term "ECMAScript" on occasion. Shortly after JavaScript was released people realized there needed to be a standard for it. The standards group that oversees JavaScript is ECMA, or the European Computer Manufacturers Association. ECMAScript is the **official** specification for what is and is not valid JavaScript.

The [Wikipedia article about JavaScript](https://en.wikipedia.org/wiki/JavaScript) covers the history of the language in more detail, if you're curious.

Note that every new update to JavaScript needs to be backwards-compatible with previous versions of the language, so once something gets into the language we're stuck with it forever.

I have chosen JavaScript as the language to use in this book because it is probably the most commonly-used programming language in the world right now, and it's likely that most if not all of the programming jobs you'll ever have will require you to use it in some way.

Plus it's a relatively easy language to learn, which makes it suitable for beginners.

Note that, while we will use some packages from NPM (a repository for JavaScript libraries) in this book, we will not use any major frameworks or libraries (like React or Angular). Our focus is on learning how to program, not how to use the popular framework of the week. Once you understand how to write substantial programs, you'll be able to quickly learn how to use any framework. Likewise, once you are proficient with JavaScript you'll be able to learn additional programming languages relatively easily.

## Installing Node.js

To run JavaScript code on your computer outside of a browser, you need Node.js.

The easiest way to install Node is simply to go to the [Node.js website](https://nodejs.org/) and download the installer from there.

You'll see they have 2 versions for you to choose from: the LTS (Long Term Support) and Current versions.

If you want to have the latest and greatest stable features as they're incorporated into Node, you'll want the Current version. If you prefer safety and stability, and don't mind possibly being slightly behind on new features, the LTS version is for you. Either will work equally well with this book. While I will use modern JavaScript syntax and commonly-accepted best practices, I won't use cutting-edge features that either aren't (yet) fully part of the standard or that have been released after April of 2023. That means you should be able to run all the code in this book on a recent version of any modern browser or Node.js.

Simply run the installer you've chosen and respond to any prompts it gives you.

If you're on a Linux PC or Mac, there's a tool called NVM (Node Version Manager) that will let you manage and use different versions of Node.

I use NVM on both my work and personal computers so I can test my code against multiple versions to make sure my applications will work properly with different versions. You can learn more about NVM at [their GitHub repo](https://github.com/nvm-sh/nvm).

## JavaScript in The Browser

You don't need to do anything to use JavaScript in the browser; unless you've deliberately disabled JavaScript, it will just work.

You'll be able to use all the language features your browser supports right out of the box.

I test all my browser code in Chrome, Firefox, Safari, and Edge to make sure there's a consistent user experience across browsers. I also test on tablet and mobile devices, both Android and iOS. All the code in this book should be compatible with the latest version of every modern browser.

JavaScript is usually loaded in a web page either from a `script` element or an external file. Most of the time you'll want to use an external file so you can share code between multiple web pages.

To load an external JavaScript file, you just need to include it in a `script` element:

```html
<script src="path/to/file.js"></script>
```

That's all you need to do! You may see examples elsewhere that include the attribute and value `type="text/javascript"` in the opening tag, but that hasn't been required for well over a decade.

If you don't know HTML and CSS, don't worry; I'll include all the code you need either in the book itself or in the code for the relevant projects.

## Installing Packages with NPM

One of the nice things about using a popular programming language is there are tons of things you might need to do in a program that someone else has already done. For most of these things, you can find at least one code library to use instead of reinventing the wheel yourself. These libraries are also known as packages.

Node.js comes with a package manager called NPM, which supposedly does **not** mean "Node Package Manager," even though it totally does.

The NPM package registry has over 1 million packages published that you can install and use in any project you want. We'll use a few in this book so you can get the hang of how the JavaScript ecosystem works.

To install a package from NPM, just type this into your terminal (or command prompt if you're on Windows): `npm install [package name]`.

You can also install packages globally, which you may want to do if there's a utility you want to use from the command line or in many projects. To do that, simply add the `-g` flag to your install command: `npm install -g [package name]`.

## Choosing The Right Code Editor

You need a good text editor for writing code.

You can't write code in a word processor like Microsoft Word, because it applies formatting to the text that will prevent an interpreter from reading it in the way you intend.

If you're on Windows, the first text editor you think of might be Notepad.

Don't use Notepad.

Download an editor specifically designed for programming.

A good code editor will include, at a minimum, things like syntax highlighting and error notifications.

There are several good, free editors available.

Some popular ones are [Visual Studio Code](https://code.visualstudio.com/) (a.k.a. VS Code) and [Brackets](https://brackets.io/). [Sublime Text](https://www.sublimetext.com/) is another good one. It's not free, but it does have a free trial that has no defined end time. That means you can try it out as long as you want before you decide to pay for it.

There are plugins available for each of these 3 editors, which extend their functionality in some way.

Each has a vibrant developer ecosystem and new features are being added regularly.

I personally use VS Code the most, but you can't go wrong with any of these 3 choices. Feel free to try them all out and see which one suits you the best.

## Your First Program

Ok, with all that said, it's finally time to write your first JavaScript program!

Open your terminal (or command prompt), type `node`, and hit enter.

This will load the Node REPL and give you a prompt where you can enter code.

REPL stands for "Read Eval Print Loop," and it's exactly what it sounds like: it reads the code you type in, executes it, prints the result of the computation, and then goes back to the beginning to do it all over again until you close out the REPL.

You can also write this program in your browser's console. In most browsers you can open the console with CTRL (or CMD on a Mac) + Shift + j, and you'll be given a prompt where you can enter code into the console. For the most part, the console works in the same way as the Node REPL.

Ready for your first program? Here it is, so type it into your REPL (or console):

```js
console.log("Hello, world!");
```

And hit ENTER.

From left to right, you've entered an object's name, the member expression operator, a method on that object, an argument to the method, and the semicolon is a statement terminator. You should use a semicolon to end most statements in JavaScript.

Confused by those terms? I'll explain what all those words mean in the next chapter!

You'll see 2 things in your output:

First, the string "Hello, world!". "String" is what programmers say when they mean text, because text is a string (or sequence) of characters.

Second, you'll see `undefined`.

![Hello, world! in Node.js](node-hello-world.png)

![Hello, world! in the browser console](console-hello-world.png)

Printing "Hello, world!" is obvious, because that's what you told it to print, but what's the deal with `undefined`?

`undefined` is a special value in JavaScript that represents a value that has not been defined. All functions and methods in JavaScript must evaluate to a value. That's called "returning" from the function. Since `console.log` just prints the argument(s) you give it, there is no return value. The method returns `undefined` to signal that there is no specific value returned by the function.

Now that you've written your first program, let's dive in!
