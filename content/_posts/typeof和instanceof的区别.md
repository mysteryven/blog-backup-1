---
title: "在 JS 中，如何判断一个变量是 string 类型"
date: 2020-03-02T16:25:12+08:00
draft: false
tags:
  - "JS"
---

<!--more-->

## typeof

typeof 的语法规则是：`typeof operand`。返回的就是字符串。

我们可以直接使用 `typeof a === 'string'` 来判断。

另外，列几个这个操作符比较特殊的情况：

```js
typeof Null; // 'object'
typeof NaN; // 'number'
typeof Array; // 'object'
```

## instanceof

instanceof 的语法规则是 `object instanceof constructor`。返回值是 boolean 类型。

instanceof 的工作原理是看构造器的 `prototype` 属性是否存在于该对象的原型链上。这样也就意味着它只能判断对象类型。

如果我们使用 `new String("I am string")` 这样的方式构造一个字符串，也能使用 instanceof 来判断。如下：

```js
new String("I am string") instanceof String;
```

## Object.prototype.toString.call()

这个方法默认会返回 "[object type]", 其中的 type 就是数据的类型。值得说明的是，我们调用的时候必须要用上 call。详情可以点击 [MDN 文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/toString)。

```js
Object.prototype.toString.call("1") === "[object String]";
```

## 借助库

最后就是借助一些库了，比如，我们可以使用 lodash 的 isString 方法判断：

```js
_.isString("I am string") === true;
```

以上就是我能想到的全部了！
