---
title: "Html常用标签"
date: 2020-07-12T10:16:05+08:00
draft: false
---

ps：点击标题可跳转到 MDN 链接

## [a 标签](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/a)

<a> 元素（或称锚元素）可以创建通向其他网页、文件、同一页面内的位置、电子邮件地址或任何其他 URL 的超链接。

```html
<a href="https://example.com">Website</a>
```

锚点标签常常通过将 href 属性设置为 "#" 或 "javascript:void(0)" 来创造一个能阻止页面刷新的伪按钮的方式被滥用。 

## [img 标签](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img)

<img> 元素将一份图像嵌入文档

```html
<img src="/avatar.png">
```
src 属性是必须的，它包含了你想嵌入的图片的文件路径。

alt 属性包含一条对图像的文本描述，这不是强制性的，但对可访问性而言，它难以置信地有用——屏幕阅读器会将这些描述读给需要使用阅读器的使用者听，让他们知道图像的含义。如果由于某种原因无法加载图像，普通浏览器也会在页面上显示alt 属性中的备用文本：例如，网络错误、内容被屏蔽或链接过期时。

## [table 标签](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/table)

table 元素表示表格数据 — 即通过二维数据表表示的信息。

```html
<table>
    <thead>
        <tr>
            <th colspan="2">The table header</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>The table body</td>
            <td>with two columns</td>
        </tr>
    </tbody>
</table>
```