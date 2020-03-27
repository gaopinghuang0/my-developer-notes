## Javascript


### `this` and `prototype`
* [You don't know JS: this & Object Prototypes (1st ed) (Aug 2015)](https://github.com/getify/You-Dont-Know-JS/tree/1st-ed/this%20%26%20object%20prototypes).
* [[Review] You don't know JS: this & Object Prototypes (Jul 2017)](http://blog.ifyouseewendy.com/blog/2017/07/03/review-you-dont-know-js-this-and-object-prototypes), such as [Prototypes](http://blog.ifyouseewendy.com/blog/2017/07/03/review-you-dont-know-js-this-and-object-prototypes/#prototypes), and [What happened when we call new](http://blog.ifyouseewendy.com/blog/2017/07/03/review-you-dont-know-js-this-and-object-prototypes/#what-happened-when-we-callnew-)

### Promises
[Promises/A+](https://promisesaplus.com/) is a standard/protocol for compatible JS promises. There are a bunch of promise implementations, such as ES6 native Promises, [kriskowal/Q](https://github.com/kriskowal/q/blob/v1/design/README.md), [petkaantonov/Bluebird](https://github.com/petkaantonov/bluebird), and [then/promises](https://github.com/then/promise/blob/master/src/core.js). These implemetations are quite different.
* [Are there still reasons to use promise libraries like Q or BlueBird now that we have ES6 promises? (Jan 16)](https://stackoverflow.com/questions/34960886/are-there-still-reasons-to-use-promise-libraries-like-q-or-bluebird-now-that-we/34961040). The main takeaway is that ES6 promises only provide basic functions, and are slower than BlueBird in most environments.
* Bluebird [optimization killer (Jun 2017)](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers) explains its optimization. However, this is no longer right after Ignition and TurboFan are released. See [front-end/V8 Engine](../front-end.md) for more details.

### Async & Await
* [JavaScript: Promises and Why Async/Await Wins the Battle (Jul 2018)](https://dev.to/nickparsons/javascript-promises-and-why-asyncawait-wins-the-battle-1g8a).
* [Waiting for more than one concurrent await operation (Oct 19)](https://stackoverflow.com/questions/46889290/waiting-for-more-than-one-concurrent-await-operation).

### Dates
- [ ] [The definitive guide to JavaScript Dates (Jul 2018)](https://flaviocopes.com/javascript-dates/).

### Garbage Collection
* [How JavaScript works: memory management + how to handle 4 common memory leaks (Sep 2017)](https://blog.sessionstack.com/how-javascript-works-memory-management-how-to-handle-4-common-memory-leaks-3f28b94cfbec). In particular, mark-and-sweep algorithm.

### Event Loop, Microtask
- [ ] [How JavaScript works: Event loop and the rise of Async programming + 5 ways to better coding with async/await (Oct 2017)](https://blog.sessionstack.com/how-javascript-works-event-loop-and-the-rise-of-async-programming-5-ways-to-better-coding-with-2f077c4438b5)
- Very interesting video: [SmashingConf London — Jake Archibald on ‘The Event Loop’ (Feb 2018)](https://vimeo.com/254947206)

### Web Workers


### Service Workers



## TypeScript