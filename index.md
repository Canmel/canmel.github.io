---
layout: page
title: Meedesidy Blog
tagline: Myblog
email: 892379244@qq.com
github: Canmal
---
{% include JB/setup %}


<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
