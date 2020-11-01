
# Front-end

- [Front-end](#front-end)
  - [Javascript and TypeScript](#javascript-and-typescript)
  - [V8 Engine](#v8-engine)
  - [Browser (in particular, Chrome)](#browser-in-particular-chrome)
  - [CSS](#css)
  - [Front-end Library and State Management](#front-end-library-and-state-management)
    - [1) A brief history of libraries that I have used.](#1-a-brief-history-of-libraries-that-i-have-used)
    - [2) Blogs](#2-blogs)
  - [Build Tools](#build-tools)
  - [Testing](#testing)
  - [Improve Web Performance](#improve-web-performance)
  - [Monitoring](#monitoring)
  - [Web Assembly](#web-assembly)
  - [Client Side Rendering vs. Server Side Rendering](#client-side-rendering-vs-server-side-rendering)
  - [Web Security](#web-security)
  - [Animation](#animation)
  - [References](#references)


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
* [ ] [Rendering engine (Mar 2018)](https://blog.sessionstack.com/how-javascript-works-the-rendering-engine-and-tips-to-optimize-its-performance-7b95553baeda)
* [ ] [Networking (Apr 2018)](https://blog.sessionstack.com/how-javascript-works-inside-the-networking-layer-how-to-optimize-its-performance-and-security-f71b7414d34c)
* [ ] [How JavaScript works: Storage engines (Jun 2018)](https://blog.sessionstack.com/how-javascript-works-storage-engines-how-to-choose-the-proper-storage-api-da50879ef576)
* [ ] [Shadow DOM (Jun 2018)](https://blog.sessionstack.com/how-javascript-works-the-internals-of-shadow-dom-how-to-build-self-contained-components-244331c4de6e)
* [ ] [WebRTC (Jul 2018)](https://blog.sessionstack.com/how-javascript-works-webrtc-and-the-mechanics-of-peer-to-peer-connectivity-87cc56c1d0ab)
* [浏览器渲染过程与性能优化 (Oct 2017)](https://juejin.im/post/6844903501953237006)
  * HTML -> DOM
  * CSS -> CSSOM
  * DOM + CSSOM -> Rendering tree
  * Reflow （回流）： 计算每个节点在窗口内的位置和大小。
  * Repaint （重绘）： 绘制到屏幕。
  * [CSS阻塞渲染，但不阻塞DOM解析。JS阻塞DOM解析，自然也会阻塞渲染。](https://ljf0113.github.io/2017/09/24/how-css-and-js-block-dom/)
    * 可以使用defer或async属性。
    * 当浏览器遇到`<script>`标签时，会触发页面渲染。即每次遇到`<script>`时，都会渲染一次页面。因此，如果前面的CSS资源还没加载完毕，浏览器会等它加载完毕再执行`script`.
  * 优化CSS：添加媒体查询。
* [减少DOM的回流和重绘](https://juejin.im/post/6844904114841714696)
  * 元素批量修改，统一添加到页面中;
  * 动画效果position使用absolute等脱离文档流，采用transform;
  * 少调用computedStyle这些，不然会强制渲染一次;
  * 分离读写操作。在现代浏览器的渲染队列会自动合并几次写操作。如果在中间读style，就会打断这个合并。例如
    ```js
    // 一次回流重绘
    box.style.width = '200px';
    box.style.height = '200px';
    box.style.margin = '20px'; 
    console.log(box.style.width);
    console.log(box.offsetHeight); 
    ```
    而不是
    ```js
    // 三次回流和重绘
    box.style.width = '200px';
    console.log(box.style.width); //=>中断渲染队列，立即渲染一次，引发一次DOM回流和重绘  200px
    box.style.height = '200px'; 
    console.log(box.offsetHeight);
    box.style.margin = '20px'; 
    ```
* DevTools
  * Debug JS sources.
    * [An overview](https://developers.google.com/web/tools/chrome-devtools/javascript), which is good to read.
    * [ ] [Different ways of setting breakpoints](https://developers.google.com/web/tools/chrome-devtools/javascript/breakpoints)


## CSS
* CSS Framework, such as [Bootstrap](https://getbootstrap.com/).
* Preprocess, such as [SASS](https://sass-lang.com/guide).
* Styled Components
* BEM - Block Element Modifier

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
* 2018-present, using [Webpack](./webpack.md) as module bundler

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
1. [浅说 XSS 和 CSRF](https://github.com/dwqs/blog/issues/68)， XSS写得不太好。CSRF写得还行，但也不完整，尤其是没提到一般是在不同网站下发起的请求，利用了浏览器自动发cookie的特性。
2. XSS类型
   1. 反射型
   2. 存储型：比如在评论或发表文章里加入恶意JS代码。
3. 防范XSS
   1. 为cookie加httpOnly，禁止JS访问cookie。
   2. 检查输入，过滤特殊字符`<`，转义或编码
   3. 检查输出，转义或编码
4. 防范CSRF
   1. 加随机token
   2. 检查Referer，如果不同源，就直接拒绝。这个也能放在图片盗链。
   3. 加验证码。不能用一个简单的URL发起请求，强迫用户手动填验证码。

## Animation

## References
1. Inspired by [Don’t be a Junior Developer: The Roadmap - June 2018](https://zerotomastery.io/blog/dont-be-a-junior-developer-the-roadmap/?utm_source=medium&utm_medium=dont-be-junior-the-roadmap)
2. Inspired by [Web developer roadmap - 2020](https://github.com/kamranahmedse/developer-roadmap)