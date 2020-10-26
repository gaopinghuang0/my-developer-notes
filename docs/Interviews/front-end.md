
## 前端面试题

### 面试题汇总
1. [2018大厂高级前端面试题](https://juejin.im/post/6844903695411314696)
2. [每天一道前端题](https://github.com/Advanced-Frontend/Daily-Interview-Question/blob/master/datum/summary.md)

### react/redux
1. [React-router](https://juejin.im/post/6844904094772002823)

### 手写代码
1. 手写promise
   1. [blog1](https://juejin.im/post/6844903667791855623)：考虑了then返回promise
   2. [blog2](https://zhuanlan.zhihu.com/p/160315811)：考虑了nextTick实现microtask，而不是setTimeout
2. [手写deep copy](https://zhuanlan.zhihu.com/p/160315811)
   1. 仍有缺陷，比如没有释放cacheList
   2. cacheList可以用WeakMap加快查找速度，并且不用担心内存释放
   3. 不支持Map，Set，Buffer，Symbol等
   4. [另一种实现](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/148)
3. 手写new，apply，call，bind
   1. [手写bind](https://zhuanlan.zhihu.com/p/160315811)
   2. 见[我自己的总结](https://gaopinghuang0.github.io/2020/10/25/JS-my-new-call-apply-bind)
   3. 注意：new绑定this的优先级大于bind，所以函数内部在new的时候与bind传入的context无关。见[美团笔试题第二题](https://juejin.im/post/6845166890990436359)，挺有意思的。
4. [手写防抖节流](https://segmentfault.com/a/1190000017860819)。 简称：DTTV，意思是Debounce uses Timer，Throttle uses Variable.
5. [手写curry](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/134)
   1. 这道题用curry其实是不满足要求的。
   2. 有趣的是[用toString来表示输出值](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/134#issuecomment-497164320)。
6. [手写原生AJAX](https://www.jianshu.com/p/2be2e4f1fc8e)
   ```js
   xhr.open("POST","/try/ajax/demo_post2.php",true);
   xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded");
   xhr.send("fname=Zhou&lname=minghang");
   ```

### EventLoop
1. 浏览器
2. NodeJs

### 浏览器
1. [缓存](https://juejin.im/entry/6844903593275817998)：强制缓存和协商缓存。以前http/1.0使用expires来指定绝对时间，但可能因为本机电脑的时间不同导致失效；现在http/1.1用cache-control来指定相对时间(max-age=600)，拥有更高优先级。如果缓存失效，就把缓存标识符发回服务器，如果不需要更新，浏览器返回304；如果需要更新，就返回新内容。cache-control还包含public、private、no-cache、no-store等值，具体的见本条开头的链接或[这个链接](https://chinese.freecodecamp.org/news/an-introduction-to-the-browser-cache-mechanism/)。
   1. 缓存的地址。pushCache是http/2.0引入的。此外，还有memory cache和disk cache。在浏览器中，新打开的tab会从disk cache里加载，但会把js和图片放入memory cache里，如果此时刷新（不关闭tab），那么就直接从memory cache里拿。而css文件则每次都从disk cache里拿。

### CSS
* [分析比较 opacity: 0、visibility: hidden、display: none 优劣和适用场景](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/100#issuecomment-484420858)


### NodeJS 多进程多线程
1. cluster，多个进程监听同一个端口
   1. 内部其实是Master进程在listen端口，然后轮训出一个worker，然后通过内部的net来传handle

### 其他
1. [AMD, CMD, Common.js, ES6的区别](https://zhuanlan.zhihu.com/p/34093119). Common.Js是同步加载，适合本地文件，也是Node.js使用的方案。而AMD和CMD是异步加载，适合浏览器端，前者把所有的依赖都先定义好，而后者是在使用的地方再import。ES6的方案不太一样，可以支持静态的依赖分析。另外，在部分浏览器里可以使用如下html代码来引入module:
   ```html
   <script type="module" src="main.js"></script>
   <!-- or -->
   <script type="module">
      /* JavaScript module code here */
   </script>
   ```
2. JSONP and CORS
   1. Access-Control-Allow-Origin
   2. Access-Control-Request-Headers and Access-Control-Allow-Headers （可以调节自定义头部信息）
   3. Access-Control-Allow-Methods
   4. Access-Control-Allow-Credentials
   4. Access-Control-Max-Age  (秒)
3. Http options请求的作用
   1. 得到支持的Methods，例如CORS使用它来获取Access-Control-Allow-Methods