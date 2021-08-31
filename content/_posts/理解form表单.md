---
title: 关于form表单的一些事情
date: 2018-08-23
draft: false
---

我一直不敢用 form 表单，总觉得对它有种不熟悉的感觉，但是当我写登录注册页面的时候，觉得用 form 表单才是正道，因为我用 div 写连回车
触发登录的功能都没有，所以我就专门学习了一下 form，现在对它也不在畏惧了，以下是我整理的几条：

<!--more-->

## 监听 form 表单的事件

正确的做法是设置 button 的 type 为 submit，并监听 form 表单的 submit 事件，这样我们回车也能触发 submit 事件。  
如果 button 的 type 是其他的那么就不会触发 submit 了，但是不填 type 默认也是 submit

```
 <form @submit="test">
    <h3>注册</h3>
    <svg class="icon close" aria-hidden="true" @click="registerVisible = false">
        <use xlink:href="#icon-close"></use
    </svg>
    <div class="row">
        <label for="">邮箱</label>
        <input type="text">
    </div>
    <div class="row">
        <label for="">密码</label>
        <input type="text">
    </div>
    <div class="actions">
        <button type='submit'>注册</button>
        <button>注册</button>
    </div>
</form>
```

## 要设置阻止默认事件

要在 test 函数里面写 preventDefault，不然的话会执行 form 的默认事件，这样就刷新页面了。以前人们会利用 form 可以发请求的功能来实现类似 jsonp 的功能，现在一般都用 script 来做。

```
test(e) {
    e.preventDefault()
    console.log('run test')
}
```

vue 文档推荐把事件修饰符写在 html，这样就能保持 js 不操作 Dom

## 阻止冒泡事件

我  以前会把阻止默认事件和这个弄混，这个是不让事件继续传播了，和那个根本不是一个东西
`e.stopPagination`

(完)
