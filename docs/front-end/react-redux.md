
## Virtual DOM
1. [Virtual DOM 真的比操作原生 DOM 快吗？](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/47)  Vue作者尤雨溪的回答：
   1. Angular的脏检查使得任何变动都有固定的`O(watcher count)`的代价，而Knockout/Vue都采用了依赖收集，在js和DOM层面都是`O(change)`；而React在js层面是`O(DOM)`，因为需要比较整个界面，在DOM层面操作反而是`O(change)`，因为只需要修改变动的部分。
   2. Virtual DOM真正的价值在于：
      1. 方便函数式的UI编程
      2. 可以渲染到DOM以外的backend，比如ReactNative。

## diff 算法


## Hooks
1. [How to prevent re-renders on React functional components with React.memo()](https://linguinecode.com/post/prevent-re-renders-react-functional-components-react-memo). Functional component will be re-rendered every time when the parent has changed. To avoid, use React.memo() to convert the functional component into HOC.
   1. Also, there is React.useMemo(), which is memorize one value inside a component.
2. [Only Call Hooks at the Top Level](https://reactjs.org/docs/hooks-rules.html). We know that we should not call Hooks inside loops, conditions, or nested functions.  And also I know that Hooks is based on an array to maintain its order. My question is whether the array is a global array or an array that is bound to a function. 
   1. If it uses a global array, then conditional Components will break the order. Also, React.memo() above will break the order because it will not enter the component with hooks so that no index/pointer will be updated on the global array if any. Anyway, there is a [good blog](https://www.netlify.com/blog/2019/03/11/deep-dive-how-do-react-hooks-really-work/) about using a global array to mockup React Hooks.
   2. [ ] Therefore, it must be a local array. But how does React do it?  https://medium.com/the-guild/under-the-hood-of-reacts-hooks-system-eb59638c9dba

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