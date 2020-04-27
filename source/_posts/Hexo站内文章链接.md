---
title: Hexo站内文章链接
tags: Hexo问题
categories: Hexo
abbrlink: d0ffdcbb
date: 2020-04-24 10:23:17
---

此文章来源于Hexo官方中文文档：https://hexo.io/zh-cn/docs/tag-plugins

## 引用文章

引用其他文章的链接。

```
{% post_path filename %}
{% post_link filename [title] [escape] %}
```

在使用此标签时可以忽略文章文件所在的路径或者文章的永久链接信息、如语言、日期。

例如，在文章中使用 ```{% post_link how-to-bake-a-cake %}``` 时，只需有一个名为 ```how-to-bake-a-cake.md``` 的文章文件即可。即使这个文件位于站点文件夹的 ```source/posts/2015-02-my-family-holiday``` 目录下、或者文章的永久链接是 ```2018/en/how-to-bake-a-cake```，都没有影响。

<!--more-->

默认链接文字是文章的标题，你也可以自定义要显示的文本。此时不应该使用 Markdown 语法 `[]()`。

默认对文章的标题和自定义标题里的特殊字符进行转义。可以使用`escape`选项，禁止对特殊字符进行转义。

**链接使用文章的标题** 

```
{% post_link hexo-3-8-released %}
```

[Hexo 3.8.0 Released](https://hexo.io/news/2018/10/19/hexo-3-8-released/) 

**链接使用自定义文字**

```
{% post_link hexo-3-8-released '通往文章的链接' %}
```

[通往文章的链接](https://hexo.io/news/2018/10/19/hexo-3-8-released/) 

**对标题的特殊字符进行转义** 

```
{% post_link hexo-4-released 'How to use <b> tag in title' %}
```

[How to use  tag in title](https://hexo.io/news/2019/10/14/hexo-4-released/)

**禁止对标题的特殊字符进行转义** 

```
{% post_link hexo-4-released '<b>bold</b> custom title' false %}
```

[**bold** custom title](https://hexo.io/news/2019/10/14/hexo-4-released/)

---

我练习一下：

{% post_link 我的小破站 %}

{% post_link 我的小破站 '小破站' %}

```public static void main(String[] ags)```

```
{% post_link 我的小破站 %}

{% post_link 我的小破站 '小破站' %}
```

