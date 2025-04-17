---
layout: page
title: HK-TOSS Study
description: >
  한국경제신문 X 토스뱅크 부트캠프
---

### 📌 Study Log 목록

🐥 [FE 수업정리](/document/fe/)

🐣 [BE 수업정리](/document/be/)

👉 [관련코드 저장소](https://github.com/ju2hong/2025htboot.git)

<br>

<hr>

### 📌 Project 목록

📎 [월급 쪼개기 시스템 bufl](https://github.com/Toss-middle-project)

### 🆕 최신 글

<ul class="search-results">
  {% assign recent_posts = site.posts | sort: 'date' | reverse | slice: 0, 5 %}
  {% for post in recent_posts %}
    <li class="search-item">
      <a href="{{ post.url }}" class="search-link">
        <h3>{{ post.title }}</h3>
        <p class="description">{{ post.description }}</p>
        <span class="date">{{ post.date | date: "%Y-%m-%d" }}</span>
      </a>
    </li>
  {% endfor %}
</ul>
