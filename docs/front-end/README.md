
# Front-end


## Javascript and TypeScript
See [javascript.md](./../programming-languages/JavaScript/javascript.md) for more detail.

## V8 Engine
Understand the concepts of hidden class, garbage collection, and so on.
* [BlinkOn 6 Day 1 Talk 2: Ignition - an interpreter for V8 (Jun 2016)](https://www.youtube.com/watch?time_continue=58&v=r5OWCtuKiAk). A detailed talk about Ignition.
* [Launching Ignition and TurboFan (May 2017)](https://v8.dev/blog/launching-ignition-and-turbofan). Since V8 v5.9, Ignition and TurboFan replaced Full-codegen and Crankshaft.
* [Understanding V8’s Bytecode (Aug 2017)](https://medium.com/dailyjs/understanding-v8s-bytecode-317d46c94775). Ignition, the interpreter, generates bytecode from this syntax tree. TurboFan, the optimizing compiler, eventually takes the bytecode and generates optimized machine code from it.
* [How JavaScript works: inside the V8 engine + 5 tips on how to write optimized code (Aug 2017)](https://blog.sessionstack.com/how-javascript-works-inside-the-v8-engine-5-tips-on-how-to-write-optimized-code-ac089e62b12e).
* [ ] [JavaScript engine fundamentals: Shapes and Inline Caches (Jun 2018)](https://mathiasbynens.be/notes/shapes-ics).
* [JavaScript essentials: why you should know how the engine works (July 2018)](https://www.freecodecamp.org/news/javascript-essentials-why-you-should-know-how-the-engine-works-c2cc0d321553/). This blog has clear figures (*.gif).
* [ ] [JavaScript engine fundamentals: optimizing prototypes (Aug 2018)](https://mathiasbynens.be/notes/prototypes).
* [The story of a V8 performance cliff in React (Aug 2019)](https://v8.dev/blog/react-cliff). The main issue happens when V8 does not know how the infer the shape of an object after `Object.preventExtensions()`. It will create an *orphaned* shape for each object, which is a problem if there are a lot of objects.

## Browser (in particular, Chrome)
Check this blog site: https://blog.sessionstack.com/@zlatkov.
* [ ] Rendering engine
* [ ] Networking
* [ ] [How JavaScript works: Storage engines (Jun 2018)](https://blog.sessionstack.com/how-javascript-works-storage-engines-how-to-choose-the-proper-storage-api-da50879ef576)
* [ ] Shadow DOM
* [ ] WebRTC

## CSS
* CSS Framework, such as [Bootstrap](https://getbootstrap.com/).
* Preprocess, such as [SASS](https://sass-lang.com/guide).
* Styled Components
* [ ] BEM - Block Element Modifier

## Front-end Library and State Management
### 1) A brief history of libraries that I have used.
* 2014-2017, jQuery was used heavily.
* 2015-2017, Angular.js. At that time, [MEAN stack](https://thinkster.io/tutorials/mean-stack) was popular, which stands for MongoDB, Express.js, Angular.js, and Node.js.
* 2016-present, [d3.js](). I listed some [useful resources of using d3.js](./d3js.md).
* 2018, [three.js]()
* 2019, [fabric.js]() and [Vue.js]().
* 2018-present, [React](). State management via [Redux](). I listed some [useful resources of using React-Redux](./react-redux.md).

### 2) Blogs
* [ ] [How to visually design state in JavaScript](https://www.freecodecamp.org/news/how-to-visually-design-state-in-javascript-3a6a1aadab2b/).


## Build Tools
* 2017-18, [Grunt]() and [browserify]() were used.
* 2018-present, using [Webpack]() as module bundler

## Testing
* Jest

## Improve Web Performance
* [ ] An eBook: [Essential Image Optimization (2018)](https://images.guide/) by Addy Osmani.
* [ ] [Lazy Loading Images and Video (last updated Aug 2019)](https://developers.google.com/web/fundamentals/performance/lazy-loading-guidance/images-and-video/) by Jeremy Wagner.
* [ ] [亿万级访问量下的前端同构直出实践 (Sep 2017)](https://cloud.tencent.com/developer/article/1005973)

## Monitoring

## Web Assembly

## Client Side Rendering vs. Server Side Rendering

## Web Security

## Animation

## Virtual DOM

## References
1. Inspired by [Don’t be a Junior Developer: The Roadmap - June 2018](https://zerotomastery.io/blog/dont-be-a-junior-developer-the-roadmap/?utm_source=medium&utm_medium=dont-be-junior-the-roadmap)
2. Inspired by [Web developer roadmap - 2020](https://github.com/kamranahmedse/developer-roadmap)