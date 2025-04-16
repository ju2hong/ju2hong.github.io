---
layout: page
related_posts:
  - /be/
title: ThymeleafFragment
date: 2025-04-16
description: >
  springboot Thymeleaf Fragment 수업 정리
---

**Ex19ThymeleafFragment**

- 프로젝트 생성시 추가 → [홈페이지](https://start.spring.io/)

  ![image.png](/assets/img/pageimg/fragment.png)

- 패키지 생성시 겹치는 경우 고치는 법

  ![image.png](/assets/img/pageimg/fragment1.png)

  ![image.png](/assets/img/pageimg/fragment2.png)

- 폴더>파일 생성하기

  ![image.png](/assets/img/pageimg/fragment3.png)

- head.html 작성 → css나 js 파일을 포함시키기

  ```html
  <!DOCTYPE html>
  <html xmlns:th="http://www.thymeleaf.org">

  <th:block th:fragment="headFragment">
    <meta charset="UTF-8">
    <title>head.html의 타이틀</title>

    <link rel="stylesheet" href="/css/myglobal.css">
    <script src="/js/myglobal.js"></script>

    <link rel="stylesheet" href="/css/header.css">
    <link rel="stylesheet" href="/css/footer.css">

  </th:block>
  </html>
  ```

- header 작성 (footer도 비슷하게 작성)
  ```html
  <!DOCTYPE html>
  <html xmlns:th="http://www.thymeleaf.org">
    <header th:fragment="headerFragment">
      <header id="header">헤더 입니다.</header>
    </header>
  </html>
  ```
- first 작성 → head와 header,footer를 받아옴

  ```html
  <!DOCTYPE html>
  <html xmlns:th="http://www.thymeleaf.org">
    <head>
      <title>first.html의 타이틀</title>
      <!-- 자체 타이틀이 있으면, head.html의 타이틀은 무시됨. -->

      <th:block th:replace="fragments/head :: headFragment"></th:block>
    </head>
    <body>
      <header th:replace="fragments/header :: headerFragment"></header>

      <p>first.html의 자체 콘텐츠</p>

      <footer th:replace="fragments/footer :: footerFragment"></footer>
    </body>
  </html>
  ```

결과

![image.png](/assets/img/pageimg/fragment4.png)
