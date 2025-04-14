---
layout: page
related_posts:
  - /be/
title: JSP
date: 2025-04-14
description: >
  springboot jsp 수업 정리
---

**Ex15Jsp**

- jsp 설정은 수동으로 해야함
  ```java
  //JSP
    implementation 'jakarta.servlet:jakarta.servlet-api'
    implementation 'jakarta.servlet.jsp.jstl:jakarta.servlet.jsp.jstl-api'
    implementation 'org.apache.tomcat.embed:tomcat-embed-jasper'
    implementation 'org.glassfish.web:jakarta.servlet.jsp.jstl'
  ```
  ```java
  # application.properties
  # jsp
  spring.mvc.view.prefix=/WEB-INF/views/
  spring.mvc.view.suffix=.jsp
  ```
- java>src>main> webapp>WEB-INF>views
  ![image.png](/assets/img/pageimg/jsp.png)
- ex01.jsp 생성 후 vscode로 열기

`<%@ page language="java" contentType="text/html; charset=UTF-8"pageEncoding="UTF-8" %>`

이 코드를 정적파일 문서 맨 위에 넣기

- controller 작성은 html 배울 때랑 똑같이 써도 됨. application.properties에 파일 확장자를 지정해뒀기 때문에 .jsp를 쓰지 않아도 됨
- 태그
  - 페이지 지시어 <%@ %>
  - 출력문(표현식) <%= %>
  - 전역변수, 함수선언부 <%! %>
  - 자바코드(스크립트릿) <% %>
- 지시어 작성 후 html 작성해서 페이지 소스를 보면 지시어 부분에 화이트스페이스가 남음
  - `<%@ page trimDirectiveWhitespaces="true" %>` 지시어 부분에 작성
- JSTL (Java server page standard tag library)
  - 페이지 맨 위 영역에 추가 `<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>`
  - jsp에서 추가 확장 기능을 제공한다 <c>
  - header/footer 분리하는 기능을 제공
  - ex) `<c:import url="./ex06_header.jsp" />`
- JSP 페이지 간의 데이터 전달
  - jsp 액션태그
    - `<c:import url="./ex06_header.jsp" />` /ex/07/ 시에 /ex/08페이지가 반환됨
- EL : JSP 표현식 (값출력)이 복잡
  - 연산자
    - 수치연산자 : +, -, \*, / or div, % or mod, 단항연산자 -부호
    - 비교연산자 : == or eq, != or ne, < or lt, > or gt,<= or le, >= or ge
    - 논리연산자 : && or and, || or or, ! or not
    - empty 연산자 : empty <값> (null,빈 문자열,길이가 0,빈 Map,빈 Collection 은 true 이 외는 flase)
    - 삼항연산자 : ? 연산자
- JSTL : HTML처럼 표현식을 간단히 출력하기 위해 만들었음
  → JSTL + EL 형태로 표현식을 간편하게 사용하기 위함
  예) 표현식 <%= student %> → EL ${ student }
  표현식 <%if %> → JSTL <c: if >
  표현식 <%for %> → JSTL <c: for>
  - core 태그
    - < c :set > : 변수선언
    - < c :out > : 출력
    - < c :if > : 조건문
    - < c :choose > : switch 문과 유사
    - < c :when > : case문과 유사
    - < c :otherwise > : default문과 유사
    - < c :foreach > : for문(향상된)
  - forEach 태그 출력 예시
    - 리스트 출력 시
      ```java
      <c:forEach var="i" begin="0" end="4" step="1">
          i : ${ i } <br>
      </c:forEach>
      ```
    - 배열 출력 시
      ```java
      <c:set var="intArray" value="<%= new int[]{10,20,30} %>" />
      <c:forEach var="intValue" items="${ intArray }">
          intValue : ${ intValue } <br>
      </c:forEach>
      ```
    - varStatus 사용
      ```java
      <c:forEach var="value" items="${ intArray }" varStatus="status">
          value : ${ value }<br>
          index : ${ status.index }<br>
          count : ${ status.count }<br>
          current : ${ status.current }<br>
      </c:forEach>
      ```
    - 자바의 map데이터 출력
      ```
      <%
          java.util.Map<String, Object> map =
              new java.util.HashMap<>();
          map.put("name", "홍길동");
          map.put("now", new java.util.Date());
      %>
      <br>
      <c:set var="mapData" value="<%= map %>"/>
      <c:forEach var="mapKV" items="${ mapData }" >
          ${ mapKV.key } = ${ mapKV.value }<br>
      </c:forEach>
      ```
      **연습문제**
    ```java
     <!-- 1. 1부터 100까지 합 출력 -->
     <c:set var="sum" value="0" />
     <c:forEach var="i" begin="1" end="100" step="1">
          <c:set var="sum" value="${sum + i}" />
     </c:forEach>
     1부터 100까지 합 : ${sum}
    ```
    ![image.png](/assets/img/pageimg/jsp1.png)
    ```java
    <!-- 2. 구구단 7단 출력 -->
    <c:forEach var="i" begin="1" end="9" step="1">
      7 x ${i} = ${7*i} <br>
    </c:forEach>
    ```
    ![image.png](/assets/img/pageimg/jsp2.png)
    ```java
    <!-- 3. 1부터 100 사이의 2배수이면서 5배수 -->
    <c:forEach var="i" begin="1" end="100" step="1">
      <c:if test ="${i%2==0 && i%5==0}">
         1부터 100 사이의 2배수이면서 5배수 : ${i} <br>
      </c:if>
    </c:forEach>
    ```
    ![image.png](/assets/img/pageimg/jsp3.png)

**Ex16JSPCRUD**

- html문서(thymeleaf) 를 jsp문서로 바꾸기 위해서, 기존 Ex14ReadCRUD 폴더를 복붙해서 생성함
  - setting.gradle에서 [`rootProject.name`](http://rootProject.name) 를 바꾼다.
  - application.properties에서 [`spring.application.name`](http://spring.application.name) 를 바꾼다.
  - 실행파일 에서 -Application 앞 부분을 변경 후 실행하면 복붙폴더 이름변경 성공.
- src>java>main 아래 webapp>WEB-INF>views 폴더를 생성 후 이전 templates 폴더에 있던 정적 `.html` 파일을 가져와 `.jsp` 파일로 바꾼다.
  - jsp폴더에서 thymeleaf 코드 부분을 다 변경해주면 된다. (대부분 th 코드를 지워주면 됨)
