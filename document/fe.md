---
layout: page
related_posts:
  - /pages/front/nextjs/
title: Frontend
description: >
  프론트 수업
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
