---
title: Hexo搭建博客常见问题及解决
top: false
cover: false
toc: true
mathjax: false
date: 2021-01-24 10:39:30
password:
summary:
tags:
  - 代码
  - 问题
categories: 代码
copyright: true
---

*****

##### 1.**Q**：Hexo更换新主题无法成功安装，出错。

   **A**：首先，确保新主题在hexo生成的站点文件的themes文件夹下。其次，确保站点配置文件*_config.yml*中的theme：之后的字段名字和你下载主题库的文件夹名字相同。

##### 2.**Q**：Hexo静态页面报错，丢失样式，并提示{% raw %}"{% extends ‘_layout.swig‘ %} {% import ‘_macro/post.swig‘ as post_template %}" {% endraw %}

​    **A**：新版的hexo默认swig未安装，应该重新安装

```git
 npm i hexo-renderer-swig
```

##### 3.**Q**：Hexo静态页面本地版式没问题，deploy到github上出现乱版。

​    **A**：尝试将hexo缓存区清空后重试

```git
hexo clean
```

<!-- more -->

##### 4.**Q**：Hexo备份

​    **A**：hexo是在本地生成页面后deploy到Github page或者Gitee上的，所以push上去的文件不包括一些源文件，需要备份时可以在github新建一个branch hexo，然后将整个hexo push至github上托管即可。

##### 5.**Q**：Hexo菜单栏图标未启用（以**next**主题为例)

​    **A**：检查自己的站点是否运行在根目录下，运行在子目录下要删除下述代码中的'/'。(注意，**YAML**语言冒号后面都要有空格，否则会出错；而且/后面也不能有空格，否则会报错，5.0之后的版本已经测试过确实如此)'||'后面表示启用home对应的图标，没有||也是不行的，同时注意*menu_icons:*菜单栏图标对应关系是否出错。

```yaml
menu:
  home: /||home
```

##### 6.**Q**：每次撰写新文章时要配置tag、categories等信息很麻烦。

​    **A**：打开Hexo中生成的**scaffold**文档，修改里面三种模板文件draft、page、post的模板，这样以后用下述代码

```git
hexo new post "<文章标题>"
```

生成文章之后，会自动生成你配置好的md文件，以post为例，可以采用如下模板，模板上下的两条横杆不可以省略：
{% raw %}
```yaml
---
title: {{ title }}
date: {{ date }}
top: false
cover: false
password:
toc: true
mathjax: true
summary:
tags:
categories:
---
```
{% endraw %}

##### 7.**Q**：出现Template render error: (unknown path)报错或者出现err:Error [Nunjucks Error]

​    **A**：问题在于出现了Hexo无法识别的内容，比如问题6中出现的{% raw %}'{{'以及'}}'或者'{% %}'，{% endraw %}如果它不在代码块中，且没被注释，就会报错。还有一种可能就是5中所提到的，YAML代码冒号后面空格未加，也有可能报错。第一种可能的解决办法就是，增加两行识别代码或者将其注释掉。

```yaml
{% raw %}
识别报错部分
{% endraw %}
```

