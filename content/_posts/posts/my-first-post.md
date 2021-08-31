---
title: "如何使用 Hugo 自建博客"
date: 2020-01-31T19:19:17+08:00
draft: false
---

<!--more-->

使用 Hugo 自建博客非常的简单，你只需要按照它的[文档](https://gohugo.io/getting-started/quick-start/)做下来即可。

下面整理几个需要注意的点：

## 1. 如何添加自定义页 ？

在 `config.toml` 文件添加如下格式的代码：

```
[[menu.main]]
    name = "自定义名"
    url = "/customePage/"
    weight = 1
```

接着运行下面命令即可:

```
hugo new /customerPage/articleName.md
```

## 2. Github 域名解析需要配置的 IP

你可以在[这个网址](https://help.github.com/en/github/working-with-github-pages/managing-a-custom-domain-for-your-github-pages-site)找到你想要的。

其他的没有了，因为用 hugo 的文档已经写的非常好了，多说也是啰嗦。
