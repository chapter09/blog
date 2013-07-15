---
layout: page
title: Posts
tagline: Supporting tagline
comments: false
---
{% include JB/setup %}

<ul >
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
