# Asynchronous Programming in JavaScript (with Rx.js Observables)

Content:
1. [Building Blocks](#building-blocks)
2. [Observables](#observables)
3. [Creating Array Functions](#creating-array)
4. [Creating Trees](#creating-trees)
5. [Handling Events with Observables](#handling-events)
6. [Handling HTTP Requests with Observables](#handing-http)
7. [Observable In-Depth](#observable-in-depth)

Async seems hard:
- Race conditions
- Memory leaks
- Complex state machines
- Uncaught Async Errors

# Building Blocks

### Functional JavaScript:
1. `map()`
2. `filter()`
3. `reduce()`
4. `concatAll()` - `flat()` in ES2019
5. `zip()` - ???

### Writing code in two steps:
1. Building a collection we want
2. Consuming a data from this collection and doing something with it

### What's the difference between Arrays and Events?

They are both collections. We should approach them in the same way. 
`map`, `filter` and `concatAll` can be implemented over an Iterator.

The only difference between Iterator/Observer is who's in control.

### So many Push APIs / stream APIs:
1. DOM Events
2. WebSockets
3. Server-sent Events
4. Node Streams
5. Service Workers
6. jQuery Events
7. XMLHttpRequest
8. setInterval


### Introducing Observables
> Observable === Collection + Time

Array isn't an Iterator. It's something you can ask to GIVE YOU an iterator.
Observable is a collection that arrives over time. 

# Observables

Observables can model:
- Events
- Async Server Requests
- Animations

Example:
```js
/**
  fromEvent(
    t: EventTargetLike, 
    event: string, 
    selector: function
  ): Observable 
*/
var mouseMoves = Observable.fromEvent(element, 'mousemove');
```

### Hot vs Cold Observable:

Cold - is when your observable creates the producer (Producers crated *inside*)

```js
var cold = new Observable((observer) => {
  var producer = new Producer();
  // have observer listen to producer here
});
```

Hot - is when your observable closes over the producer (Producers created *outside*)

```js
var producer = new Producer();
var hot = new Observable((observer) => {
  // have observer listen to producer here
});
```

Read more - [link](https://medium.com/@benlesh/hot-vs-cold-observables-f8094ed53339).

Using observables can help eliminate race conditions since they sequence 
the incoming streams. They also are able to handle scenarios with nested 
observables (observables of observables).

#### `takeUntil`

The takeUntil method copies all the data from a source observable 
into a new observable. However, it completes once it reaches 
a “stop observable” which is passed as a parameter. 
 
This allows a seemingly infinite stream of data become finite.
 
 Example:
 ```js
import { fromEvent, interval } from 'rxjs';
import { takeUntil } from 'rxjs/operators';

const source = interval(1000);
const clicks = fromEvent(document, 'click');
const result = source.pipe(takeUntil(clicks));
result.subscribe(x => console.log(x));
```