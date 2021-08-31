---
title: "YDKJ 2th - Get Started - Chapter 2 笔记"
date: 2020-03-16T21:39:56+08:00
draft: false
---

本系列是《你不知道的 JS》第二版的读书笔记，本笔记属于 Get Started 一书的第二章，书籍传送门：[click me](https://github.com/getify/You-Dont-Know-JS/blob/2nd-ed/get-started/ch2.md)

<!--more-->

我们通常认为 `const` 的作用是在声明一个不可变的变量，但准确的说，`const` 声明的是一个不能被重新赋值（re-assigned）的变量，而不是不可变(unchangeable)。

函数有如下两种声明方式：

```javascript
function foo(coolThings) { // 第一种声明方式 - function declaration
    // ..
    return 'Hi';
}

const foo = function(coolThing) {...} // 第二种声明方式 - function expression
```

这两种声明方式有一个很容易被忽略的区别：

对于第一种来说，函数名（identifier）和函数值（function value）的绑定发生在代码编译阶段（compile phase of the code）。而第二种呢，函数名和函数值只在运行到当前语句的时候才会建立关系。

JS 中有 `==` 和 `===` 两个判断相等的运算符。通常在咱们项目中，我们会使用更严格的 `===`。毕竟 `==` 一不小心就造成 bug 了。和 `==` 相比，我们通常认为 `===` 既比较值，也比较类型，所以更严格一点。

```javascript
1 == "1"; // true
1 === "1"; // false
```

事实上，两者都会比较类型，真正的不同点在于：`===` 不允许任何类型转化（type conversion），而 `==` 允许，并且倾向于转化为 `number` 类型。

如果相比较的两个值的类型是相同的，二者做的事情一模一样。

`===` 也有两个判断不准的值。

```javascript
NaN === NaN; // false
0 === -0; // true
```

对于第一个问题，我们使用 `Number.isNaN` 来判断，对于第二个，我们使用 `Object.is` 方法。

`<` 、`>` 、`if`、`while` 都会有发生像 `==` 的隐式类型转化，这点太坑了。

比如下面这个例子，挺难发现问题吧。

```
var arr = [ "1", "10", "100", "1000" ];
for (let i = 0; i < arr.length && arr[i] < 500; i++) {
    // 会运行三次
}
```

虽说我们可以用 `===` 来避免相等判断时的隐式类型转化问题，但是上面提到的这些咋办？ 所以说，这个问题很难被逃避，与其畏惧他，还不如去面对它！去了解它的规则，去征服他！ 我记得这本书后面的部分有更详细的解释，到那时候我们再整理。

就目前来说，我们组织 JS 代码有两种方式，一种是 `Class`，另一种是使用 `ES Module`。使用 `ES Module` 的方法如下：

```
// 文件 A.js
export function printHello() {
    console.log("hello")
}

// 文件 B.js
import {printHello} from '/A.js';

printHello();

// 文件 C.js
import {printHello} from '/A.js';

printHello();
```

这里有个要特别注意的地方，不管我们引用了 A 文件中的 `printHello` 几次，都只是引用的一个地址，他们都是同一个，我们 import 进来的都是同一个实例。也叫做 singletons。

之前我在项目中就踩过坑，那时候不清楚这个规则，无意间修改了同事的接口名，导致出了线上问题，血淋淋的教训。

如果想解决这个问题，我们就要借助下面这个组织方法：

```javascript
function printDetails(pub, URL) {
  pub.print();
  console.log(URL);
}

export function create(title, author, pubDate, URL) {
  var pub = createPub(title, author, pubDate);

  var publicAPI = {
    print() {
      printDetails(pub, URL);
    }
  };

  return publicAPI;
}
```

以上就是这一节的全部笔记内容了。
