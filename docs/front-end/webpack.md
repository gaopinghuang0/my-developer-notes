

## Webpack

- [Webpack](#webpack)
  - [Loaders and Plugins](#loaders-and-plugins)
  - [Optimization](#optimization)

### Loaders and Plugins
Loader 可以看作具有文件转换功能的翻译员，配置里的 module.rules 数组配置了一组规则，告诉 Webpack 在遇到哪些文件时使用哪些 Loader 去加载和转换。比如，针对 `.css` 文件，`css-loader` 读取CSS文件，`style-loader`把CSS内容注入到JavaScript里，之后将其插入到`HTML head style`标签里。

Plugin 用来扩展 Webpack 功能，在整个构建流程的一些事件中注入钩子，实现某种功能。完整的事件列表可以在[这里](https://webpack.wuhaolin.cn/5%E5%8E%9F%E7%90%86/5-1%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86%E6%A6%82%E6%8B%AC.html)找到。最常用的是 `compilation` 事件，指当 Webpack 调用 Loader 完成了每个模块的转换工作后。在这里注入钩子，可以得到一个对应的 Compilation 对象，包含了当前的模块资源、编译生成资源、变化的文件等。除此之外，Plugin 还要接收一个 Compiler 的对象，包含了Webpack环境的配置信息，全局唯一，可以看做是 Webpack 本身的一个 Singleton 实例。

一个最基础的 Plugin 的代码就会利用以上的两个对象：
```js
class BasicPlugin{
  constructor(options){
  }

  apply(compiler){
    compiler.plugin('compilation',function(compilation) {
    })
  }
}

module.exports = BasicPlugin;
```
Webpack在构造函数中传入该插件的options，在apply方法中传入compiler对象。接下来，在compiler对象上注册钩子函数。可以看到，compiler.apply 和 compiler.plugin 就类似于 eventEmitter.trigger 和 eventEmitter.on，作用就是广播和监听。 其实Compilation对象本身也可以注入钩子，方法跟compiler类似，也是使用apply和plugin。这里就不展开了。

使用上面的Plugin时：
```js
const BasicPlugin = require('./BasicPlugin.js');
module.export = {
  plugins:[
    new BasicPlugin(options),
  ]
}
```


### Optimization
优化可以分为优化开发体验和优化输出质量两部分，参考了[深入浅出Webpack](https://webpack.wuhaolin.cn/4%E4%BC%98%E5%8C%96/)。

1. 优化开发体验
   1. 优化构建速度
      1. 缩小文件搜索范围：使用test, include, exclude等；或者使用`resolve.alias`来直接指定依赖文件，比如最小化的文件（react.min.js)。
      2. 使用DLLPlugin：大量复用的模块的动态链接库只需要编译一次，比如react，react-dom等常用第三方模块，只要不升级，就不用重新编译。
      3. 使用HappyPack：多进程执行编译，尤其是最耗时的Loaders。
      4. 使用ParallelUglifyPlugin：多进程的代码压缩，可以引入cache来缓存结果。
   2. 优化使用体验
      1. 使用自动刷新
      2. 开启热模块替换
2. 优化输出质量
   1. 减少首屏加载时间
      1. 区分环境：即区分生产和开发环境
      2. 压缩代码
      3. CDN加速
      4. 使用Tree Shaking：依赖静态的ES6模块化语法。
      5. 提取公共代码：利用好浏览器缓存，加快访问速度；同时减少后续的网络传输流量，节约成本。
      6. 按需加载：可以结合React-Router，自建一个AsyncComponent，在componentDidMount中异步`import(*)`. Webpack 内置了对 `import(*)` 语句的支持，当 Webpack 遇到了类似
         ```js
         import(/* webpackChunkName: "[name]" */ './[name].js')
         ```
         时，会将`./[name].js`为入口新生成一个Chunk，当执行到这个import时才加载对应的Chunk文件，并返回一个Promise. 同时，需要在webpack里相应地设置。细节见[按需加载](https://webpack.wuhaolin.cn/4%E4%BC%98%E5%8C%96/4-12%E6%8C%89%E9%9C%80%E5%8A%A0%E8%BD%BD.html)。
   2. 提升流畅度
      1. Prepack：提前将某些源代码求值并用结果来替换源代码。保持结果一致。比较激进的方案。还不成熟。
      2. 开启Scope Hoisting：作用域提升。分析模块之间的依赖关系，尽可能的把打散的模块合并到一个函数中。为了不造成代码冗余，只有那些被引用了一次的模块才能被合并。同时，因为需要分析模块间的依赖，所以源码必须是ES6。
