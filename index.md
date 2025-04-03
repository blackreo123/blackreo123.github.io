---
layout: home
title: Home
---

# 블로그에 오신 것을 환영합니다

Jekyll과 GitHub Pages로 만든 개인 블로그입니다.

## 최근 포스트

{% for post in site.posts limit:5 %}
  - [{{ post.title }}]({{ post.url | relative_url }}) - {{ post.date | date: "%Y-%m-%d" }}
{% endfor %} 