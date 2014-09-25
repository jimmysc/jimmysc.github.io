---
layout: page
title: Hello World!
tagline: Supporting tagline
---
{% include JB/setup %}

Read [Jekyll Quick Start](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)

Complete usage and documentation available at: [Jekyll Bootstrap](http://jekyllbootstrap.com)

## Update Author Attributes

In `_config.yml` remember to specify your own data:
    
    title : My Blog =)
    
    author :
      name : Name Lastname
      email : blah@email.test
      github : username
      twitter : username

The theme should reference these variables whenever needed.
    
## Sample Posts

This blog contains sample posts which help stage pages and blog data.
When you don't need the samples anymore just delete the `_posts/core-samples` folder.

    $ rm -rf _posts/core-samples

Here's a sample "posts list".

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

## To-Do

This theme is still unfinished. If you'd like to be added as a contributor, [please fork](http://github.com/plusjade/jekyll-bootstrap)!
We need to clean up the themes, make theme usage guides with theme-specific markup examples.


## Blog 资料 
[开通github个人站点](https://pages.github.com/)

[github blog模板](https://github.com/plusjade/jekyll-bootstrap/)

[jekyll使用说明](http://www.zhanxin.info/jekyll/2013-08-07-jekyll-basic-usage.html)

[jekyll添加文章类目](http://pizn.github.io/2012/02/23/use-category-plugin-for-jekyll-blog.html)

[mac10.9安装jekyll错误](http://v5sheji.com/archives/mac-xcode5-1-gem-jekyll-error.html)

[jekyll](http://jekyllrb.com/docs/posts/)

[liquid](https://github.com/shopify/liquid/wiki/liquid-for-designers)


## 蒋海滔的学习笔记
[海涛笔记](https://code.google.com/p/hatter-source-code/wiki/Study_CPU_Intel)
