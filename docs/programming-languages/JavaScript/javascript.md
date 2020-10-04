## Javascript


### `this` and `prototype`
* [You don't know JS: this & Object Prototypes (1st ed) (Aug 2015)](https://github.com/getify/You-Dont-Know-JS/tree/1st-ed/this%20%26%20object%20prototypes).
* [[Review] You don't know JS: this & Object Prototypes (Jul 2017)](http://blog.ifyouseewendy.com/blog/2017/07/03/review-you-dont-know-js-this-and-object-prototypes), such as [Prototypes](http://blog.ifyouseewendy.com/blog/2017/07/03/review-you-dont-know-js-this-and-object-prototypes/#prototypes), and [What happened when we call new](http://blog.ifyouseewendy.com/blog/2017/07/03/review-you-dont-know-js-this-and-object-prototypes/#what-happened-when-we-callnew-)

### Promises
[Promises/A+](https://promisesaplus.com/) is a standard/protocol for compatible JS promises. There are a bunch of promise implementations, such as ES6 native Promises, [kriskowal/Q](https://github.com/kriskowal/q/blob/v1/design/README.md), [petkaantonov/Bluebird](https://github.com/petkaantonov/bluebird), and [then/promises](https://github.com/then/promise/blob/master/src/core.js). These implemetations are quite different.
* [Are there still reasons to use promise libraries like Q or BlueBird now that we have ES6 promises? (Jan 16)](https://stackoverflow.com/questions/34960886/are-there-still-reasons-to-use-promise-libraries-like-q-or-bluebird-now-that-we/34961040). The main takeaway is that ES6 promises only provide basic functions, and are slower than BlueBird in most environments.
* Bluebird [optimization killer (Jun 2017)](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers) explains its optimization. However, this is no longer correct after Ignition and TurboFan are released. See [front-end#V8 Engine](../../front-end/README.md) for more details.

### Async & Await, Event Loop, Microtask
* [How JavaScript works: Event loop and the rise of Async programming + 5 ways to better coding with async/await (Oct 2017)](https://blog.sessionstack.com/how-javascript-works-event-loop-and-the-rise-of-async-programming-5-ways-to-better-coding-with-2f077c4438b5)
* Video (interesting). [What the heck is the event loop anyway? by Philip Roberts (Oct 2014)](https://www.youtube.com/embed/8aGhZQkoFbQ)
* Video (interesting). [SmashingConf London — Jake Archibald on ‘The Event Loop’ (Feb 2018)](https://vimeo.com/254947206)
* [JavaScript: Promises and Why Async/Await Wins the Battle (Jul 2018)](https://dev.to/nickparsons/javascript-promises-and-why-asyncawait-wins-the-battle-1g8a).
* [Waiting for more than one concurrent await operation (Oct 19)](https://stackoverflow.com/questions/46889290/waiting-for-more-than-one-concurrent-await-operation).

### Dates
- [ ] [The definitive guide to JavaScript Dates (Jul 2018)](https://flaviocopes.com/javascript-dates/).

### Garbage Collection
* [How JavaScript works: memory management + how to handle 4 common memory leaks (Sep 2017)](https://blog.sessionstack.com/how-javascript-works-memory-management-how-to-handle-4-common-memory-leaks-3f28b94cfbec). In particular, mark-and-sweep algorithm.

### Web Workers


### Service Workers

### WeakMap and WeakSet
WeakMap holds "weak" references to key objects, which means that they do not prevent garbage collection in case there would be no other reference to the key object. This also means the values in the WeakMap can be garbage collected.  

1. WeakMap keys are not enumerable.
2. WeakMap keys must be of the type `Object` only, while the values can be of any type.
3. WeakMap has no `size` property, but only `set`, `has`, `delete` methods.

Real use cases:
1. WeakMap的实际例子：一个graph里的node，记录node被点击的次数和时间。优点是不需要修改node本身，而是把node作为WeakMap的key，然后添加次数和时间等额外信息。如果node从graph里删掉了，这个WeakMap里也会自动删掉。
2. Hiding implementation details. Code below is from [this blog (2014)](https://fitzgeraldnick.com/2014/01/13/hiding-implementation-details-with-e6-weakmaps.html). Here, only `Public` is exported, so privates are hiden from consumers.
   ```js
   const privates = new WeakMap();

   function Public() {
     const me = {
       // Private data goes here
     };
     privates.set(this, me);
   }

   Public.prototype.method = function () {
     const me = privates.get(this);
     // Do stuff with private data in `me`...
   };

   module.exports = Public;
   ```

3. Store custom data for DOM nodes.
4. Memo/cache for immutable objects. See this [Stack Overflow answer](https://stackoverflow.com/a/46263541)

## TypeScript