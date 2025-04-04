---
layout: page
related_posts:
  - /be/
title: java
date: 2025-04-02
description: >
  springboot 수업 정리
---

### 스프링

- Spring은 Java를 기반으로 한 웹 어플리케이션 프레임워크다.

  - JDBC : Java Database Connection
  - ORM : Object Relational Mapping
  - OXM : Object XML Maooing
  - JMS : Java Messaging Service
  - Transacions : 인터페이스와 POJO들은 가지고 있는 클래스의 트랜잭션 매니징을 지원해준다.
  - AOP : Aspect-Oriented Programming

- 특징

  - 경량 컨테이너
    - 제어의 역전(IoC, Invertion of Control)
    - 의존성 주입(Di, Dependency Injection)
    - 관점지향 프로그래밍(AOP, Aspect-Oriented Programming)
  - POJO(Plain Old Java Object) 프로그래밍을 지향
    - 순수 Java만을 통해서 생성한 객체를 의미

### 스프링부트

- LomBok(롬복)

  - 지원 어노테이션

    - @Getter : getter 자동생성
    - @Setter : setter 자동생성
    - @NoArgsConstructor : 매개변수 없는 기본생성자 자동생성
    - @AllArgsConstructor : 모든 필드를 파라미터로 받는 생성자 자동생성
    - RequiredArgsConstructor : final이나 @NonNull인 필드만 매개변수로 받는 생성자 자동생성, 생성자 주입에 사용
    - @NotNull : null을 허용하지 않은 객체 Bean 자동생성
    - Nullable : null을 허용하는 객체 Bean 자동생성
    - Date : @Getter, @Setter, @ReauiredArgsConstructor, @ToString, @EqualsAndHashCode을 한꺼번에 설정해주는 어노테이션
    - @ToString : toString 메소드 자동생성
    - @EqualsAndHashCode : equals, hashCode 메서드 생성

  - 모든 어노테이션 임포트 문구 : `import lombok.*;`
