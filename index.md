---
layout: page
title: Posts
tagline: Supporting tagline
comments: false
---
{% include JB/setup %}

<!--<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>-->

<table class="table table-hover">
    <tbody>
        {% for post in site.posts %}
        <tr>
            <td style="border: none; padding: 0;">{{ post.date | date_to_string }}</td>
            <td style="border: none; padding: 0;">
                &raquo; 
                <a href="{{ BASE_PATH }}{{ post.url }}">
                    {{ post.title }}
                </a>
            </td>
        </tr>
        {% endfor %}
    </tbody>
</table>
