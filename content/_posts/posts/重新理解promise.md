---
title: "重新理解promise"
date: 2020-02-19T16:36:56+08:00
draft: false
---

<!--more-->

```
function display(res) {
	console.log(res);
}

const dataFetch = fetch('/tweet/mystery/');
dataFetch.then(display)
```

当我们执行到 `dataFetch...` 这一行的时候，我们的 `dataFetch` 这个变量会立即拿到如下格式的一个数组：

```
{
	value: undefined,
	fullfillment: []
}
```

这里的 `value` 存储的就是我们将要从接口拿到的值，不过在此时是空而已。

而`then` 后面的方法都会被放到 `fullfillment` 这个数组里面。当 `value` 里的值变化时，就会去触发数组里面的方法。如果 `then` 换个名字，换成 `auto-trigger-on-value-be-updated` 会更好理解一点。不过这个名字倒是太长了。

这就是 promise 的实现机制。
