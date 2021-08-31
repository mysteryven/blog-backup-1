---
title: "YDKJ 2th - Get Started - Chapter 3 笔记"
date: 2020-03-20T16:25:25+08:00
draft: false
---

本系列是《你不知道的 JS》第二版的读书笔记，本笔记属于 Get Started 一书的第二章，书籍传送门：[click me](https://github.com/getify/You-Dont-Know-JS/blob/2nd-ed/get-started/ch2.md)

<!--more-->

我们经常使用 spread 操作符（...），比如在进行数组浅拷贝的时候：`a = [...[1, 2, 3]]`; spread 操作符的作用是 _consuming iterators_，也就是只要是个 iterator ，就可以使用这个操作符。`for...of` 也是用来遍历 iterator 的。

除了数组外，常见的可以生成 iterator 的还有 `Map`、`Set`、 `String`。

```js
const map = new Map();
map.set("a", 1);
map.set("b", 2);

for (let [key, value] of map) {
  console.log(`${key}: ${value}`);
} // a: 1   b: 2
```

```js
const str = "hello world";
console.log([...str]);
// ["h", "e", "l", "l", "o", " ", "w", "o", "r", "l", "d"]
```

```js
const set = new Set();
set.add(1);
set.add(2);
for (let value of set) {
  console.log(value);
} // 1  2
```

另外，经过一些扩展，我们还可以在对象中使用它：

```js
const iteratorObj = {
  a: 1,
  b: 2,
  c: 3,
  [Symbol.iterator]() {
    const keys = Object.keys(obj);
    const len = keys.length;
    let step = 0;

    const iterator = {
      next() {
        if (len === 0) {
          return { value: undefined, done: true };
        } else if (step === len) {
          return { value: obj[keys[len - 1]], done: true };
        }

        return { value: obj[keys[step++]], done: false };
      }
    };
    return iterator;
  }
};

for (let v of iteratorObj) {
  console.log(v); // 1, 2, 3
}
console.log(...iterator); // 1, 2, 3
```

面试中，老是被问闭包是什么，现在有标准答案了：

> Closure is when a function remembers and continues to access variables from outside its scope, even when the function is executed in a different scope.

闭包是一个函数，尽管它不在当前作用域执行，依然可以继续访问在它作用域外部的变量。另外还有一点值得主要：闭包的外围不一定只是函数，还可能是 `for` 循环。

JS 中的 `this` 功能非常强大，但又非常令人困扰，很多人理解 `this` 是指向函数本身、指向实例，这都是不对的。this 是动态的，最好被定义为执行执行上下文（excution context）。

`Object.create(null)` 创建了一个没有原型链的对象（standalone object），某些时候很有用。

关于 `this`，还有一个很令我困惑的示例：

```js
var homework = {
  topic: "CSS",
  study() {
    console.log(`Please study ${this.topic}`);
  }
};

var jsHomework = Object.create(homework);
jsHomework.topic = "JS";
jsHomework.study();
```

你猜最后一行输出什么？

```java
Please study JS
```

没有输出 CSS 会不会让你感觉比较奇怪？事实上，尽管我们通过原型链调用的 `study` 方法委托到父类身上，但是 `this` 却指向被保留了下来。（method calls on objects which delegate through the prototype chain still maintain the expected this）。

对比而言，Java 就不会了，这算是 JS 的一个特色：

```Java
// Homework.java
public class Homework {
    public String topic = "Maven";

    public void study() {
        System.out.println("Please study " + this.topic);
    }
}

// JSHomework.java
public class JSHomework extends Homework {
    public String topic = "Java";

    public static void main(String[] args) {
        JSHomework jsHomework = new JSHomework();
        jsHomework.study();
    }
}

// Please study Maven
```

以上就是本节笔记的全部内容。
