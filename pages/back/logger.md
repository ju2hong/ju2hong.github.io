---
layout: page
related_posts:
  - /be/
title: Logger
date: 2025-04-16
description: >
  springboot Logger 수업 정리
---

**Ex20Logger (** [참고자료](https://www.notion.so/cbd03811493d4eb997126a10a8ba1173?pvs=21) )

- 프로젝트 생성

  ![image.png](/assets/img/pageimg/logger1.png)

- 추가 빌드파일에 넣기(꼭 anntationProcessor 아랫줄에 복붙하기)
  ```java
  testCompileOnly 'org.projectlombok:lombok' // 테스트 코드에서 lombok사용
  testAnnotationProcessor 'org.projectlombok:lombok' // 테스트 코드에서 lombok사용
  ```
- main>resources 에 application.yml 생성(띄어쓰기 주의해야함)

  ```java
  logging:
    level:
      com:
        study:
          ExLogger: debug
    pattern:
      file: "[%d{HH:mm:ss.SSS}][%-5level][%logger.%method:line%line] - %msg%n"
      level:

    file:
      name: logs/mylog.log

    logback:
      max-history: 30
      file-name-pattern: "logs/mylog.%d{yyyy-MM-dd}.%i"
  ```

  - root 밑에 **log>mylog.log** 생성
  - logback-spring.xml 파일 생성(주신 파일 복붙)
    - 자바 역사가 길다보니 로그를 찍는 방법이 다양하다.

- LogTest 파일 생성 , (파일명)ApplicationTests 을 extends 해온다.

  ```java
  package com.study.springboot;

  import lombok.extern.slf4j.Slf4j;
  import org.junit.jupiter.api.Test;
  import org.slf4j.Logger;
  import org.slf4j.LoggerFactory;

  @Slf4j
  class LogTest extends Ex20LoggerApplicationTests{
      static int count = 0;
      @Test
      public void testSlf4j() {
          log.trace("slf4j trace 로깅 {}",count++);
          log.debug("slf4j debug 로깅 {}",count++);
          log.info("slf4j info 로깅 {}",count++);
          log.warn("slf4j warn 로깅 {}",count++);
          log.error("slf4j error 로깅 {}",count++);
      }
      @Test
      public void TestLogger() {
          //class 객체는 클래스에 대한 모든 정보(멤버함수,필드,어노테이션,생성자)
          //담고있다.

          Logger logger = LoggerFactory.getLogger(LogTest.class);
          logger.trace("trace 로깅 {}", count++);
          logger.debug("debug 로깅 {}", count++);
          logger.info("info 로깅 {}", count++);
          logger.warn("warn 로깅 {}", count++);
          logger.error("error 로깅 {}", count++);
      }
  }
  ```

결과

![image.png](/assets/img/pageimg/logger2.png)
