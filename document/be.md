---
layout: page
title: Backend
related_posts:
  - /pages/back/sql/
  - /pages/back/java/
  - /pages/back/jsp/
description: >
  백앤드 수업
---

<h2>목록</h2>
<ul class="search-results">
  {% for post in page.related_posts %}
    {% assign related_page = site.pages | where: "url", post | first %}
    {% if related_page %}
      <li class="search-item">
        <a href="{{ related_page.url }}" class="search-link">
          <h3>{{ related_page.title }}</h3>
          <p class="description">{{ related_page.description }}</p>
          <span class="date">{{ related_page.date | date: "%Y-%m-%d" }}</span>
        </a>
      </li>
    {% endif %}
  {% endfor %}
</ul>
