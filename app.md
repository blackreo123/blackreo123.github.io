---
layout: page
title: APP
permalink: /app/
---

# APP 개발 기록

여기에 앱 개발 관련 내용들을 정리할 예정입니다.

## 최근 포스트

<ul class="post-list">
  {% assign app_posts = site.posts | where: "categories", "app" %}
  {% for post in app_posts %}
    <li>
      <span class="post-meta">{{ post.date | date: "%Y-%m-%d" }}</span>
      <h3>
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      </h3>
      {% if post.excerpt %}
        {{ post.excerpt }}
      {% endif %}
    </li>
  {% endfor %}
</ul>

## 프로젝트 현황

### 진행 중인 프로젝트
- My First App (Swift, SwiftUI)

### 완료된 프로젝트
- 준비 중

### 아이디어
- 새로운 프로젝트 구상 중 