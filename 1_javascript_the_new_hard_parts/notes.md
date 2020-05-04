# JavaScript: The New Hard Parts

Content:
1. [Asynchronous JavaScript](#asynchronous-javascript)
2. [Promises](#promises)
3. [Iterators](#iterators)
4. [Generators](#generators)
5. [Final](#final)

Execution context - execution context is an abstract concept 
of an environment where the Javascript code is evaluated 
and executed. Whenever any code is run in JavaScript, 
it’s run inside an execution context.

Types: 
- Global
- Functional
- Eval

## Asynchronous JavaScript

#### Javascript is single-threaded

JavaScript is single threaded (one command executing at a time)
and has a synchronous execution model (each line is executed 
in order the code appears).

In its most basic form, JavaScript is a synchronous, blocking, 
single-threaded language, in which only one operation can be in progress 
at a time. 

Web browsers define functions and APIs that allow us 
to register functions that should not be executed synchronously, 
and should instead be invoked asynchronously when some kind of 
event occurs (the passage of time, the user's interaction with 
the mouse, or the arrival of data over the network, for example). 

This means that you can let your code do several things at the same 
time without stopping or blocking your main thread.

#### Microtask & Macrotask queue:

JS has three "stacks":

- standard stack for all synchronous calls (one function calls another, etc)
- microtask queue (or job queue or microtask stack) for all async operations 
with higher priority (process.nextTick, Promises, Object.observe, MutationObserver)
- macrotask queue (or event queue, task queue, macrotask queue) for all
 async operations with lower priority (setTimeout, setInterval, 
 setImmediate, requestAnimationFrame, I/O, UI rendering)

[Read more on StackOverflow - Difference between microtask and macrotask within an event loop context
](https://stackoverflow.com/questions/25915634/difference-between-microtask-and-macrotask-within-an-event-loop-context)

## Promises

A Promise is an object representing the eventual completion or 
failure of an asynchronous operation.

A Promise is in one of these states:

- pending: initial state, neither fulfilled nor rejected.
- fulfilled: meaning that the operation completed successfully.
- rejected: meaning that the operation failed.

Features:
- Callbacks will never be called before the completion of the current run
 of the JavaScript event loop.
- Callbacks added with then() even after the success or failure of the 
asynchronous operation, will be called, as above.
- Multiple callbacks may be added by calling then() several times. 
Each callback is executed one after another, in the order 
in which they were inserted.

## Iterators

## Generators

## Final


