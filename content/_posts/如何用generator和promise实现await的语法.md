---
title: "如何用 generator 和 promise 实现一个 async/await "
date: 2020-02-28T19:24:02+08:00
draft: false
tags:
  - "JS"
---

<!--more-->

 在使用 DVA 的时候往往有下面这样格式的代码：

```js
effects: {
  *save({ payload: todo }, { put, call }) {
    // Call saveTodoToServer, then trigger `add` action to save data
    yield call(saveTodoToServer, todo);
    yield put({ type: 'add', payload: todo });
  },
},
```

不知道你有没有好奇过它是怎么实现的，又或者说没有对于他怎么实现的，没有任何思路呢？

今天我就带大家来解析一下这段代码的实现。

我们先来讲理解上面那段代码最关键的部分，且看下面示例。

```js
function* add() {
  const n = 10;
  const a = yield n + 1;
  yield a + 1;
}

const adder = add();
adder.next(); // 会输出什么
adder.next(1); // 会输出什么
adder.next(); // 会输出什么
```

你觉得输出的结果分别是什么呢？

在看下面答案之前，可以试着先自己想一下:)

---

上面两条语句正确的执行结果如下：

```
{value: 11, done: false}
{value: 2, done: false}
{value: undefined, done: true}
```

你会不会好奇，为什么会这样！特别是第二个输出。

下面，我们一步步看看他是怎么做的。我们忽略掉定义 add 函数的部分。

1. 首先生成一个 generator，名叫 adder。

2. 调用 adder 的 next 方法，这时候会进入 adder 内部，去执行它。

3. 在 adder 内部，我们首先定义了一个 const 的变量 n，并给他赋值为 10。

4. 接下来定义一个 const 的变量叫 a，接下来为了给它赋值，要先执行等号右边的操作 `yield n + 1`。

5. 我们需要先计算 yield 后面的表达式，计算完 yield 后面的表达式后，根据 yield 的语法规定，代码便停在了 yield 这里，并把 yield 后面的结果返回出去。所以我们拿到了结果 {value: 11, done: false}。到这里，一次 next 就执行完了。**注意，此时我们只是定义了 a，但还没有走到给它赋值的那一步，这里很关键。**

6. 接下来我们继续调用 next 方法， 这里的入参是 1，此时代码会从上一次暂停住的 yield 继续运行，也就是在赋值给 a 变量之前的那个状态，此时这个值会做 yield 的返回值赋值给 a，也就是说此时 a 的值是 1。

7. 接着继续运行到下一个 yield 处，计算得到的值为 2，也返回出去。我们第二个结果也就拿到了 {value: 2, done: false}。这次的 next 方法也结束了。

8. 接下来，我们再调用一次 next 方法，整个函数就结束。也就拿到了最后的结果：{value: undefined, done: true}。

不知上面的逻辑你是否理解了。如果理解了上面的逻辑，稍加拓展就能解决我们今天的问题了。

我们把上面的代码稍加修改。

```js
function* getPostGenerator() {
  const res = yield fetchAPI("https://jsonplaceholder.typicode.com/posts/1"); // 这是一个promise 请求。

  console.log(res);
}

// 这个是 fetch 的官网介绍。
// https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch
function fetchAPI(url) {
  return fetch(url).then(response => {
    return response.json();
  });
}
```

上面的代码结构也类似于我们在文章开头看到的 DVA 的结构。

我们怎么让它实现类似于 async/await 的功能呢？那就要在调用 next 方法之后做文章了。

```js
var getPost = getPostGenerator();

// 此时的 value 是 fetch 的返回值，也就是一个 promise 对象。
// 第一次 next 就执行完了。
var returnValue = getPost.next().value;

// 我们要在 promise 对象的状态变为 resolved 的时候调用 getPost 的 next 方法。
// 这样的话，从接口拿到的值就能赋值给 res 了。也就能把它打印出来！
returnValue.then(res => {
  getPost.next(res);
});
```

你可以把上面的代码粘贴到 jsbin 里运行一下。说起来，它和我们上一段代码的逻辑是一样的。

这样就大致实现了我们的问题。

不过我们每一次都这么写，也能实现同步写代码这个需求，但是太麻烦了。JS 很贴心的给我们提供了语法糖，

我们可以使用更简洁的语法完成它：

```js
async function getPostGenerator() {
  const res = await fetchAPI("https://jsonplaceholder.typicode.com/posts/1");
  console.log(res);
}
```

清爽多了吧！

以上就是全部内容了，有任何问题，欢迎和我交流。
