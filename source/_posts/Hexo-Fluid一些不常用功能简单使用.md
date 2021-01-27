---
title: Hexo-Fluid一些不常用功能简单使用
toc: true
tags:
  - 代码
  - 问题
categories:
  - [代码, 博客相关]
sticky: false
mermaid: true
date: 2021-01-26 15:19:48
---

Hexo-Fluid本身集成了一些有趣的功能，但是使用比较少，本文旨在简述这些功能的简单用法，也避免之后遗忘等问题。

<!-- more -->

## 脚注

​	Fluid主题内置了脚注语法，有时方便引用的一些写法，脚注一般写在文末更规范美观。

栗子：

```markdown
脚注，是可以附在文章页面的最底端的，对文本加以补充说明的注文[^1]。
[^1]:参考资料-百度百科
```

效果：

​	脚注，是可以附在文章页面的最底端的，对文本加以补充说明的注文[^1]。(尾注自动生成至文末)

[^1]:参考资料-百度百科

## Tag标签

### 便签

​		两种方式使用Tag插件，在markdown中使用如下代码

栗子一：使用markdown特殊代码段，注意两个特殊代码需要单独成一行

```markdown
{% note success %}
文字 或者 `markdown` 均可
{% endnote %}
```

栗子二：使用HTML形式

```html
<p class="note note-primary">标签</p>
```

示栗：

{% note primary %}
primary
{% endnote %}
{% note secondary %}
secondary
{% endnote %}
{% note success %}
success
{% endnote %}

<p class="note note-danger">danger</p>
<p class="note note-warning">warning</p>
<p class="note note-info">info</p>
<p class="note note-light">light</p>

### 行内标签

栗子一：使用markdown语法，加标签那段话不能以@开头

```markdown
这段话第十个字之后要{% label primary @加标签 %}啦！
```

这段话第十个字之后要{% label primary @加标签 %}啦！     ←效果

栗子二：使用HTML形式

```html
这段话第十个字之后要<span class="label label-warning">加标签</span>啦！
```

这段话第十个字之后要<span class="label label-warning">加标签</span>啦！   ←效果

### 勾选框

在markdown后面加上下述代码使用Checkbox

```markdown
{% cb text, checked?, incline? %}
```

text：显示的文字

checked：默认是否勾选，默认false

incline：是否内联(内联元素不会独占一行)，默认false

示栗：

```markdown
{% cb 普通示例 %}

{% cb 默认选中, true %}

{% cb 内联示例, false, true %} 后面文字不换行

{% cb false %} 也可以只传入一个参数，文字写在后边（这样不支持外联)
```

{% cb 普通示例 %}

{% cb 默认选中, true %}

<div>
<span>{% cb 内联示例, false, true %}</span><span>{% cb false %}也可以只传入一个参数，文字写在后边（这样不支持外联) </span>

</div>

### 按钮

可以在markdown中加入如下代码来使用Button：

```markdown
{% btn url, text, title %}
```

也可以使用HTML形式

```html
<a class="btn" href="url" title="title">text</a>
```

url：跳转链接

text：显示文字

title：鼠标悬停于button时显示的文字

{% btn https://github.com/novpath, 我是按钮, 点击进入我的github主页 %}   



### 组图

该功能以实现多张图片按一定布局组合显示。

```markdown
{% gi total n1-n2-... %}
  ![](url)
  ![](url)
  ![](url)
  ![](url)
  ![](url)
{% endgi %}
```

total：图片总数量，对应中间包含的图片 url 数量
n1-n2-...：每行的图片数量，可以省略，默认单行最多 3 张图，求和必须相等于 total，否则按默认样式

效果：

{% gi 3 3 %}
  ![图片1](/img/avatar.png)
  ![图片2](/img/avatar.png)
  ![图片3](/img/avatar.png)

{% endgi %}

## LaTeX 数学公式

### 运算符