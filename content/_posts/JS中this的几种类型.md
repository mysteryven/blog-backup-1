---
title: "JS中this的几种类型"
date: 2020-07-05T10:42:54+08:00
draft: false
tags:
  - "JS"
---

在 JS 中，`this` 是一个特别让人头疼的东西。起初，我还对能掌握 `this` 抱有激情，但随着时间的推移，老是有出人意料的场景出现，我开始想，这或许是一个比较糟糕的设计，而我们需要付出时间，为这个糟糕的设计埋单。

尽管如此，JS 已经发展到如此体量了，我们不去战胜它，也没有办法。

`this` 之所以困扰，就是因为它的规则太多了，很容易混淆。今天我们就按照优先级由低到高的顺序，来顺一遍 `this` 的规则。

## 默认绑定（default Binding）

这是优先级最低的方式，不过，只能在非严格模式 (strict mode) 下使用。这种形式也是我们不推荐使用的。

```js
function foo() {
	console.log( this.a );
}

var a = 2;

foo(); // 2
```

## 隐式绑定（Implicit Binding）

我们调用对象的一个函数，如果没有更高的 `this` 规则，那隐式绑定就会生效了。

我们还举例了 `JQuery` 的例子，这个是第三方库为我们添加的，这时候的 `this` 指向什么，要看他们的文档。

```js
function foo() {
	console.log( this.a );
}

var obj = {
	a: 2,
	foo: foo
};

obj.foo(); // 2

// 像 JQuery 这样的框架
$('#a').on('click', function doSomething() {
  console.log(this)
})
```

## 显示绑定（Explicit Binding）

显示绑定就是使用 `call`、`bind`、`apply` 这样的关键词去绑定，它只比最后一种用 `new` 声明的优先级低。

```js
function foo() {
	console.log( this.a );
}

var obj = {
	a: 2
};

foo.call( obj ); // 2

var bar = foo.bind(obj)

bar(); // 2
```

另外，`bind` 还有一个妙用：它可以预先设定函数的参数，和函数式编程的 [curry](https://lodash.com/docs/4.17.15#curry) 函数有点像。看下面这个例子：

```js
function add(x, y) {
  return x + y;
}

var add10 = add.bind(undefined, 10);

add10(2) // 12
```
## new 

没人比他还要高了。

```
var myCar = new Car(..);
```

## 总结

掌握了这些规则，以后再遇到 `this` 问题，就找找看问题里有最高优先级的规则是哪一个，套用就好了！

