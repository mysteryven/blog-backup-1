---
title: "JS里new的用途"
date: 2019-03-01T13:49:24+08:00
draft: false
---

今天看到了一片博文讲 `new` 讲的非常不错，于是我再简略的概括一下亦当复习了。

<!--more-->

首先要明白，`new`其实是一个语法糖。

如果我们创建了一个构造函数叫 `Food`，当我们执行 `new Food` 的时候会发生什么呢？

1. js 帮我们在 `Food` 内部声明一个临时的空对象
2. 把 `Food` 里面 this 指向的属性或者方法都放到临时对象里面
3. 让临时对象的一个叫做 `_proto` 的属性指向 `Food` 的 `prototype`
4. 返回这个临时对象

当然了，我们也可以不用 `new` 操作符，如果不怕麻烦的话。

另外的，一个新的对象都有一个 `constructor` 属性，作用是为了指名构造它的是哪一个对象。它不会自动修改。

在构造对象的时候如果我们使用下面的方式修改原型

```
    Food.prototype = {
        ...
    }
```

这样之后，`prototype` 的指向就完全变了，那 `constructor` 也就变了，此时我们还需要手动改回来，在 `Food.prototype` 添加 `constructor` 属性，或者不让 `Food.prototype` 是一个新对象，直接如下方式修改：

```
Food.prototype.toString = ...
```

（完）
