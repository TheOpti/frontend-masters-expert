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

#### Microtask queue

Asynchronous tasks need proper management. For that, the ECMA 
standard specifies an internal queue PromiseJobs, more often 
referred to as the “microtask queue” (ES8 term).

## Iterators

Iterator - Object that knows how to access items from a collection one 
at a time, while keeping track of its current position within that sequence.

Example of iterator:
```js
function returnIterator(arr) {
  let i = 0;
  return function() {
    const elem = arr[i];
    i++;
    return elem;
  }
}
```

In order to be iterable, an object must implement the @@iterator method,
meaning that the object (or one of the objects up its prototype chain)
must have a property with a `Symbol.iterator`.

Built-in iterators in JavaScript:
- Map
- Set
- Array
- TypedArray
- String

Syntax expecting iterables:
- `for.. of..` loop
- spread operator
- generator functions

Read more:
- [A Simple Guide to ES6 Iterators in JavaScript with Examples](https://codeburst.io/a-simple-guide-to-es6-iterators-in-javascript-with-examples-189d052c3d8e)

## Generators

Generators are functions that can be exited and later re-entered. 
Their context (variable bindings) will be saved across re-entrances (from MDN).

Generators can return (“yield”) multiple values, one after another, on-demand. 
They work great with iterables, allowing to create data streams with ease.

```js
function* generateSequence() {
  yield 1;
  yield 2;
  return 3;
}

let generator = generateSequence();

let one = generator.next();

alert(JSON.stringify(one)); // {value: 1, done: false}
```

Example with generators - Fibonacci sequence:

```js
function *fib() {
  let current = 0;
  let next = 1;
  while(true) {
    yield current;
    [current, next] = [next, current + next];
  }
}
```

We can use the ability to pause createFlow's running and then restart it 
only when our data returns

```js
function doWhenDataReceived(data) {
  returnNextElement.next(data);
}

function* createFlow() {
  const data = yield fetch('http://twitter.com/user/1');
  console.log(data);
}

const returnNextElement = createFlow();
const futureData = returnNextElement.next();

futureData.then(doWhenDataReceived);
```

Read more:
- [function*@MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*)


## Final


