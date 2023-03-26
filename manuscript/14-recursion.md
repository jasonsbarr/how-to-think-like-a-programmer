{class:part}

# Algorithms and Program Design

Now that you've grasped the basics of programming in JavaScript, we'll level up your program analysis and design skills. First, you'll build on the knowledge about algorithms you've already gained. We'll dig into some more complicated algorithms and data structures, and also see how to analyze algorithms for efficiency. Then you'll learn about testing and writing tests, so you can make your applications as bulletproof as possible. Then we'll take a deep dive into program design and problem solving. Finally, we'll put it all together with one of the most complex projects for any developer: we'll create our own programming language.

# Recursion

## The Call Stack

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
