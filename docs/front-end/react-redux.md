
## Overall architecture
* [Building React From Scratch (Sep 2016) by Paul O Shannessy](https://www.youtube.com/watch?v=_MAD4Oly9yg&ab_channel=ReactRally). The [source code](https://github.com/zpao/building-react-from-scratch) for this talk.
* [The React Source Code: a Beginner’s Walkthrough I (Apr 2017)](https://medium.com/@ericchurchill/the-react-source-code-a-beginners-walkthrough-i-7240e86f3030)
  * For React V15, "Haste" module system from Facebook was used. 
    * Each file contains a license header in which it uses `@providesModule ModuleName` to declare a unique module. 
    * No matter how deep those files/modules locate, eventually, they will be copied into a single flat directory called `lib/` with their unique names. Consequently, all `require("ModuleName")` will be auto converted into `require("./ModuleName")`. In other words, prepending all `require()` paths with `./`
  * The actual core code is in `src/isomorphic/React.js`. In the license header, we can see `@providesModule React`.
  * Some missing modules ("Warning") may exist in the `fbjs` package, which follows Haste module system as well. Find out why it's kept in another repository [here](https://www.npmjs.com/package/fbjs).
  * Testing is done via Jest.
* [Behind the Scenes: Improving the Repository Infrastructure (Dec 2017)](https://reactjs.org/blog/2017/12/15/improving-the-repository-infrastructure.html)
  * For React V16, a lot of changes have been made to the code structure.
  * [Monorepository](http://danluu.com/monorepo/)
  * Rollup for library
    * Flat bundle, rather than turning modules into functions like Webpack.
    * It is more readable.
  * Google Closure Compiler, even the "simple" mode can provide better dead code elimination and small function inline than Uglify.
  * Only test Public APIs, instead of internal modules
    * It is clear what is being tested from the user's perspective
    * You can run it even if you rewrite the implementation from scratch
    * You can run tests on compiled bundles.
  * Prevent infinite loop
    * The React 16 codebase contains many `while` loops for Fiber related code. These `while` loops make development really difficult: "Every time there is a mistake in an exit condition our tests would just hang, and it took a while to figure out which of the loops is causing the issue."
    * A Babel plugin is used: if a loop continues for more than the max threshold, the plugin throws an error.
    * However, if an error thrown from the Babel plugin gets caught (`try-catch`) and ignored up the call stack, the test will pass even though it has an infinite loop. To solve this pitfall, "we set a global field before throwing the error. Then, after every test run, we rethrow that error if the global field has been set. This way any infinite loop will cause a test failure, no matter whether the error from the Babel plugin was caught or not."
  * Tracking Bundle Size. After `yarn build`, a table will be printed out to show the changes of the sizes of all the bundles since the previous build.
  * Simplify Release Process
    * Automated scripts with two steps: *build* and *publish*.

## Virtual DOM
1. [Virtual DOM 真的比操作原生 DOM 快吗？](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/47)  Vue作者尤雨溪的回答：
   1. Angular的脏检查使得任何变动都有固定的`O(watcher count)`的代价，而Knockout/Vue都采用了依赖收集，在js和DOM层面都是`O(change)`；而React在js层面是`O(DOM)`，因为需要比较整个界面，在DOM层面操作反而是`O(change)`，因为只需要修改变动的部分。
   2. Virtual DOM真正的价值在于：
      1. 方便函数式的UI编程
      2. 可以渲染到DOM以外的backend，比如ReactNative。

## diff 算法


## Hooks
* [How to prevent re-renders on React functional components with React.memo()](https://linguinecode.com/post/prevent-re-renders-react-functional-components-react-memo). Functional component will be re-rendered every time when the parent has changed. To avoid, use React.memo() to convert the functional component into HOC.
   * Also, there is React.useMemo(), which is memorize one value inside a component.
* [Only Call Hooks at the Top Level](https://reactjs.org/docs/hooks-rules.html). We know that we should not call Hooks inside loops, conditions, or nested functions.  And also I know that Hooks is based on an array to maintain its order. My question is whether the array is a global array or an array that is bound to a function. 
   * If it uses a global array, then conditional Components will break the order. Also, React.memo() above will break the order because it will not enter the component with hooks so that no index/pointer will be updated on the global array if any. Anyway, there is a [good blog](https://www.netlify.com/blog/2019/03/11/deep-dive-how-do-react-hooks-really-work/) about using a global array to mockup React Hooks.
   * [ ] Therefore, it must be a local array. But how does React do it?  https://medium.com/the-guild/under-the-hood-of-reacts-hooks-system-eb59638c9dba
* [React Hooks(二): useCallback 之痛 (Dec 2019)](https://zhuanlan.zhihu.com/p/98554943)
  * 问题1：快照 or 最新值。当你的应用里同时存在Functional组件和class组件时，你就面临着UI的不一致性。Functional组件始终显示快照值，而class组件显示最新值。一旦你页面里混用了class组件和functional 组件（使用useref暂存状态也视为class组件），就存在的UI不一致性的可能
  * 问题2：一个最简单的case就是一个组件依赖了父组件的callback，同时内部useffect依赖了这个callback。在文章的代码示例里，每次Parent重渲染都会生成一个新的fetchData，因为fetchData是Child的useEffect的dep，每次fetchData变动都会导致子组件重新触发effect，一方面这会导致性能问题，假如effect不是幂等的这也会导致业务问题（如果在effect里上报埋点怎么办）
    * 提到了7种解法，比如 `useEventCallback`，核心是 `useRef` 来取最新值。但还是会导致一些一致性问题（useEffect和useLayoutEffect都不能完全保证安全，因为在commit和useEffect之间有个gap）。
    * 使用`mutable`，实际上这种做法就是放弃react的快照功能，变相放弃了concurrent mode。
    * `useReducer` 比较好，也是官方推荐的较为正统的做法。需要一些封装来处理异步。

## Suspense
1. [React Suspense in practice (Mar 2020)](https://css-tricks.com/react-suspense-in-practice/) shows a demo. 'Suspense' has `useTransition` hook.
   1. First, if an update needs three data and two of which have arrived, the Suspense will keep the update in the memory and do not show partial result in UI. This is to maintain UI consistency. At this moment, the old UI will be shown.
   2. However, if the third data takes too long, the old UI will be stale and misleading. So `useTransition` has a `timeout` option. If the total suspending time exceeds the `timeout`, it will use a fallback component to replace the old UI.
2. [React Suspense for data fetching (Nov 2019)](https://blog.logrocket.com/react-suspense-for-data-fetching/).
   1. Explains why Suspense is needed.
   2. Data fetching approaches
      1. Fetch-on-render.  The network request isn't triggered until the component renders. This can lead to a problem known as a "waterfall".
      2. Fetch-then-render.  Use Promise.all to fetch the data outside of App; namely, before App component renders. However, this will make all components wait for the slowest data fetch.
      3. Render-as-you-fetch. `fetchData` outside of App, then use Suspense on each component that needs the data from `fetchData`.


## React Fiber
* 为什么 React Fiber不用 js 提供的 async/await 和 generator呢？主要原因在于：
   * [ ] TODO：https://zhuanlan.zhihu.com/p/99977314
* [Inside Fiber: in-depth overview of the new reconciliation algorithm in React (Mar 2020)](https://indepth.dev/inside-fiber-in-depth-overview-of-the-new-reconciliation-algorithm-in-react/)
  * Optimization - Effects List: building a linear list of fiber nodes with effects for quick iteration. After the `render` phase, React enters the `commit` phase. Now it has 2 trees and the effects list. The first tree represents the state currently rendered on the screen. Then there’s an alternate tree built during the `render` phase. It’s called `finishedWork` or `workInProgress` in the sources and represents the state that needs to be reflected on the screen. And then, there’s an effects list — a subset of nodes from the `finishedWork` tree linked through the `nextEffect` pointer. Remember that the effect list is the result of running the `render` phase.
