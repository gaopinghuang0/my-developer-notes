# JavaScript手写new, call, apply, bind

## 实现自己的 `new`

```js
function Student(name, id) {
    this.name = name;
    this.id = id;
}
Student.prototype.sayHello = () => { console.log('hello'); }

// ES6
function myNew(Constructor, ...rest) {
    const newObj = Object.create(Constructor.prototype);
    const result = Constructor.apply(newObj, rest);
    return result instanceof Object ? result : newObj;
}

// ES5
function myNew() {
    var newObj = {};
    var Constructor = Array.prototype.shift.call(arguments);
    newObj.__proto__ = Constructor.prototype;
    var result = Constructor.apply(newObj, arguments);
    return result instanceof Object ? result : newObj;
}

// Usage
const student1 = myNew(Student, 'Tom', 1);
```
注意，`myNew`的最后一行不能使用`typeof result === 'object'`，因为如果返回值是`null`而已知`typeof null === 'object'`为true，就会导致myNew返回null。而浏览器里原生的new是会return newObj的。对比见下图：
<img width="600" alt="myNew-comparison" src="myNew-js.jpg">

### 扩展：那什么时候用typeof，什么时候用instanceof呢？
见这个Stack Overflow问题：[typeof vs. instanceof](https://stackoverflow.com/questions/899574/what-is-the-difference-between-typeof-and-instanceof-and-when-should-one-be-used)。
简单来说：只有在判断简单的built-in types（string，boolean，number) 的时候，使用typeof，而不用instanceof + 大写的构造函数。其他时候都可以使用instanceof。
<img width="400" alt="typeof-vs-instanceof" src="typeof-vs-instanceof.jpg">