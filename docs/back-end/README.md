# Back-end

## Node.js
* Monitoring - [Node.js Monitoring Done Right (Nov 2016) by RisingStack](https://medium.com/hackernoon/node-js-monitoring-done-right-70418ecbbff9) gives a list of what we should monitor and why.
* Stream. Complete the [`stream-adventure` at NodeSchool](https://nodeschool.io/#workshopper-list).
* Node Event-loop
  * [(Official) The Node.js Event Loop, Timers, and process.nextTick()](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/)
  * [深入理解NodeJS事件循环机制](https://juejin.im/post/6844903999506923528)
  * 我的总结：
    * 每一个event-loop有六个阶段（phases）： timers，pending callbacks，idle prepare，poll，check，close callbacks。其中pending callbacks是处理系统的一些回调，比如TCP链接错误啥的，不用太关心；而idle、prepare是node内部使用，不需要关心。close callbacks是处理一下socket.on('close'，callback)之类的回调。这样，重点需要关心的是三个阶段：timers，poll，和check。
      * timers：这一阶段主要是执行已经到期的setTimeout和setInterval的回调函数。如果没有到期的，就经过pending、idle到poll阶段。
      * poll（轮训）：处理除了timers/check/close callback阶段以外的其他回调，比如I/O相关的回调。
        * 如果 poll 队列不空，event loop会遍历队列并同步执行回调，直到队列清空或执行的回调数到达系统上限；
        * 如果 poll 队列空了，会有两种情况：
          * 如果已经有setImmediate，那么会进入到check阶段
          * 如果没有setImmediate，那么会计算需要等待多长时间，然后block这么长时间，之后加入到队列里立即执行
        * [ ] 如果这时候有timers的时间到期了，那么会返回到timers阶段。（TODO：有个疑问，这个到底是啥时候检查的。没有一个博客写清楚了的。）
      * check：执行setImmediate的回调函数
    * 另外，process.nextTick和Promise.resolve都会创建microtask。这些microtask不属于任何一个阶段，只要在某个阶段产生了，就会在本阶段结尾，下一阶段开始前执行。这就像插队。为了防止无穷尽的插队导致的下一阶段饿死，设置了一个最大tick深度（比如1000）
    * 小细节：setTimeout(0)和setImmediate的顺序。如果不在一个I/O周期内，那么两者的顺序还不完全确定；如果在一个I/O周期内，那么setImmediate会先执行，因为在poll阶段执行了I/O的回调，然后注册了setImmediate和setTimeout。但在check阶段就会执行setImmediate，而只有到了timers阶段才会执行setTimeout。可以参考上面的那篇博客里的例子。

## Backend frameworks
* [Web.py]() in Python
* [Flask] in Python
* [Express.js] in JS.

## Linux
* Bash
* SSH

## Cache
* [Why Redis is so fast?](https://zhuanlan.zhihu.com/p/57089960)

## Networks
See [networks.md](./../general-cs/networks.md) for more detail.

## Security
- [Meltdown and Spectre](https://spectreattack.com/). Two hardware vulnerabilities

## Testing

## Optimize Performance
* [1万属性，100亿数据，每秒10万吞吐，架构如何设计？ (May 2019)](https://mp.weixin.qq.com/s?__biz=MzI1NDQ3MjQxNA==&mid=2247489011&idx=1&sn=1b99b5801d0cec32d10c288e5bad4397&chksm=e9c5ec42deb265546d67ec2dee678fbd64607a417328b1925ca96bd6d1cf695341df385a5737&scene=21#wechat_redirect)
* [大家都见过哪些让你虎躯一震的代码？ - BI3OJW的回答 - 知乎](https://www.zhihu.com/question/287421003/answer/528275532). See the second part about optimizing image processing.


## Docker and Container

## Monitoring

## Manage Sign In and Sessions

## Infrastructure as a Service and Platform as a Service

## Continuous Integration, Delivery, and Deployment


## References
1. Inspired by [Don’t be a Junior Developer: The Roadmap - June 2018](https://zerotomastery.io/blog/dont-be-a-junior-developer-the-roadmap/?utm_source=medium&utm_medium=dont-be-junior-the-roadmap)
2. Inspired by [Web developer roadmap - 2020](https://github.com/kamranahmedse/developer-roadmap)