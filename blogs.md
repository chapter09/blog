---
layout: page
title: Blogs
tagline:
comments: false
---
{% include JB/setup %}

<table class="table table-hover" style=" margin-left: -20px; ">
    <tbody>
        {% for post in site.posts %}
        <tr>
            <td style="border: none; padding: 0;text-align: right;">{{ post.date | date_to_string }}</td>
            <td style="border: none;"></td>
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
