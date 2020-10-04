
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
3. 手写new，apply，call，bind
   1. [手写bind](https://zhuanlan.zhihu.com/p/160315811)
   2. 见[我自己的总结](../programming-languages/JavaScript/new-call-apply-bind.md)
   3. 注意：new绑定this的优先级大于bind，所以函数内部在new的时候与bind传入的context无关。见[美团笔试题第二题](https://juejin.im/post/6845166890990436359)，挺有意思的。
4. 手写...

### EventLoop
1. 浏览器
2. NodeJs

### 浏览器
1. [缓存](https://juejin.im/entry/6844903593275817998)：强制缓存和协商缓存。以前使用expire来指定绝对时间，但可能因为本机电脑的时间不同导致失效；现在用cache-control来指定相对时间，拥有更高优先级。如果缓存失效，就把缓存标识符发回服务器，如果不需要更新，浏览器返回304；如果需要更新，就返回新内容。


### 其他
1. [AMD, CMD, Common.js, ES6的区别](https://zhuanlan.zhihu.com/p/34093119). Common.Js是同步加载，适合本地文件，也是Node.js使用的方案。而AMD和CMD是异步加载，适合浏览器端，前者把所有的依赖都先定义好，而后者是在使用的地方再import。ES6的方案不太一样，可以支持静态的依赖分析。另外，在部分浏览器里可以使用如下html代码来引入module:
   ```html
   <script type="module" src="main.js"></script>
   <!-- or -->
   <script type="module">
      /* JavaScript module code here */
   </script>
   ```
