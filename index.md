---
layout: page
title: Meedesidy Blog
tagline: 不是每一个问题都是钉子，不是每一个问题都是锤子
email: 892379244@qq.com
github: Canmal
---
{% include JB/setup %}


<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
