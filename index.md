---
layout: home
title: Welcome to My Blog
---

# Welcome to My Blog

This is my personal blog built with Jekyll and GitHub Pages.

## Recent Posts

{% for post in site.posts limit:5 %}
  - [{{ post.title }}]({{ post.url | relative_url }}) - {{ post.date | date: "%Y-%m-%d" }}
{% endfor %} 