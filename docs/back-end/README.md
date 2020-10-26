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


## Version Control (Git)
* [深入理解Git实现原理](https://zhuanlan.zhihu.com/p/45510461)
   * 使用底层命令 `git hash-object -w file.txt` 往git数据库中写入内容。写入会返回一个40位的 hash (SHA1 key, 例如 "83baae...30"），同时内容会存储成一个 blob 文件，存放成 `.git/objects/83/baae...30`。可以看到，前两位 hash 作为了文件夹名字，后38位作为了文件名称。
     * 可以使用 `git cat-file -t <hash-key>` 查看类型，并用 `git cat-file -p <hash-key>` 来查看实际内容。
     * 如果改变了文件的内容，再写入数据库，那么会返回一个新的key，以及存储一个新的blob文件，两个blob文件同时存在。
     * 注意，此时只保留了文件内容，忽略了很多信息，比如：文件名、变更时间、顺序、变化说明等。
   * 为了保存文件名和其他有用信息，Git 利用树对象 （tree object）来管理数据（blob）对象。一个树对象包含一条或多条记录，每条记录含有一个指向 blob 或其他树对象的指针，以及相应的模式、类型、文件名。
     * Git会根据某一时刻的**暂存区**来创建树对象。为了创建树对象，首先要创建一个**暂存区**，保存在 `.git/index`里。一开始，`.git/index` 不存在。
     * 使用底层命令 `git update-index --add file.txt` 来把 file.txt 的第一个版本放入暂存区。这时`.git/index` 文件被创建。可以使用底层命令 `git ls-files --stage` 来查看暂存区内容。
     * 使用底层命令 `git write-tree`来把暂存区内容写入一个树对象。跟 blob 类似，这个写入会返回一个40位的key，然后用前两位创建文件夹，后38位当做文件名。此时，区别在于，这个文件的类型是tree，可以使用 `git cat-file -t <hash-key>` 查看类型，并用 `git cat-file -p <hash-key>` 来查看实际内容。
     * 如果又添加一个新的文件 `new.txt`，直接调用 `git update-index --add new.txt`, 那么文件的内容会自动保存成一个blob对象，如果再调用 `git write-index`, 会创建相应的 tree 对象。**注意，暂存区并没有清空**。使用底层命令 `git ls-files --stage`可以看到，file.txt 和 new.txt 都还在暂存区中。这是因为添加文件是append操作，不会清空暂存区。同样，也意味着，tree对象的内容里包含了file.txt 和 new.txt两个文件。
     * 对于*文件夹*的操作类似，只不过不能添加空文件夹，而是添加文件夹里的文件。但 tree 对象里会保存文件夹的名称。
   * 接下来，需要记录下各个版本的时序关系和注释，就能得到功能比较强大的Git了。
     * Git 使用提交对象（commit object）来记录版本间的时序关系和注释。
     * 使用底层命令 `git commit-tree <hash-of-tree-object> -m "first commit"` 来将某个树对象提交为 commit 对象。跟常用的`git commit`命令一样，`-m`选项后面就是注释。commit对象的创建同样会返回一个40位的key，规则跟blob和tree几乎一样，因此使用 `git cat-file [-t] [-p] <hash>` 就可以查看类型或内容了。
     * 此外，可以指定两次提交对象的时序：`git commit-tree <hash-of-tree-object-v2> -p <hash-of-commit-object> -m "second commit"`。这样，第二个树对象就和第一个树对象的提交历史联系起来了，`-p` 指定了父commit。
   * 最后，因为直接使用 hash 不够方便，所以 Git 提供了引用（references）来简化这个过程。引用保存在 `.git/refs`下。比如我们最常见的`master`就指向当前最新一个提交的hash。类似的，分支（branch）也可以用不同的引用来方便定位。
   * 当然，还有些很重要的问题这篇文章没有解决，比如，
     * 怎么压缩文件，保证这些存储的对象不会占用太多空间。尤其是上文中提到每个版本都被 blob 对象完整地记录下来，这样一旦有很多版本，必然会导致相同的信息被重复记录了多次。
     * 怎么checkout到某个版本。如果不压缩的话，直接就用对应版本的commit对象，找到tree对象，再找到相应的blob对象；但如果压缩了，就需要考虑怎么计算出需要的版本。
* 关于压缩文件(pack)，可以看一下[这篇文章](https://hiberabyss.github.io/2018/03/28/git-internal/)。
  * Git会定期或在特定条件下（例如 push 的时候）对对象文件进行打包处理。这时会查找命名及大小相近的文件, 然后保存最新的文件的完整内容, 历史文件则按照 diff 的方式进行保存。同时会删掉无用的对象文件。
  * 可以使用 `git gc` 来手动触发打包过程。之后，原来的 blob 文件都不存在了，但在 pack 目录里生产了一个 `.idx` 和 `.pack` 文件。

## Docker and Container

## Monitoring

## Manage Sign In and Sessions

## Infrastructure as a Service and Platform as a Service

## Continuous Integration, Delivery, and Deployment


## References
1. Inspired by [Don’t be a Junior Developer: The Roadmap - June 2018](https://zerotomastery.io/blog/dont-be-a-junior-developer-the-roadmap/?utm_source=medium&utm_medium=dont-be-junior-the-roadmap)
2. Inspired by [Web developer roadmap - 2020](https://github.com/kamranahmedse/developer-roadmap)