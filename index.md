---
layout: page
title: 首页
tagline: Jimmy Study
---
{% include JB/setup %}


## 快速入口

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

## 友情链接
[hatter](https://code.google.com/p/hatter-source-code/wiki/Study_CPU_Intel)

[CoolShell](http://coolshell.cn/)

[并发编程](http://ifeve.com/java-concurrency-thread-directory/)

## 帮助

###### 1.切换镜像，解决下载问题

[镜像切换到taobao](http://ruby.taobao.org/)

###### 2.创建Github Website

[如何创建](https://pages.github.com/)

###### 3.选择Website template

[jekyll-bootstrap](https://github.com/plusjade/jekyll-bootstrap/)

###### 4.使用jekyll将markdown转化成静态博客网站
[jekyll](http://jekyllrb.com/docs/posts/)

[jekyll添加文章类目](http://pizn.github.io/2012/02/23/use-category-plugin-for-jekyll-blog.html)

###### 5.liquid语法
[liquid](https://github.com/shopify/liquid/wiki/liquid-for-designers)

###### 6.mac10.9安装jekyll错误
[错误链接](http://v5sheji.com/archives/mac-xcode5-1-gem-jekyll-error.html)







