---
layout: page
related_posts:
  - /be/
title: MyBatis
date: 2025-04-15
description: >
  springboot MyBatis 수업 정리
---

**EX17MyBatis**

- 프로젝트 생성시에 추가해야 함.

![image.png](/assets/img/pageimg/mybatis.png)

- 프로젝트 생성 후 build.gradle 에 implementation 아래 코드 추가

`implementation 'org.bgee.log4jdbc-log4j2:log4jdbc-log4j2-jdbc4.1:1.16'`

- [라이브러리를 가져오는 곳](https://central.sonatype.com/)
  - 최신버전이 있는지 확인을 위해 위 브라우저 검색창에 log4jdbc를 검색 했음 → 검색결과, 위의 버전이 최신인 것을 확인함
- db.sql 라는 파일에 해당 테이블 파일을 작성해둬서 db구조를 파악하는데 도움

- `log4jdbc.log4j2.properties` 파일 생성

  ```java
  log4jdbc.spylogdelegator.name=net.sf.log4jdbc.log.slf4j.Slf4jSpyLogDelegator
  log4jdbc.dump.sql.maxlinelength=0
  log4jdbc.drivers=com.mysql.cj.jdbc.Driver
  log4jdbc.auto.load.popular.drivers=false //우선적으로 탐지되는 driver
  ```

- `logback.xml` 파일 생성

  ```java
  <configuration>
      <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
          <encoder>
              <pattern>%d{yyyyMMdd HH:mm:ss.SSS} [%thread] %-3level %logger{5} - %msg %n</pattern>
          </encoder>
      </appender>

      <logger name="jdbc" level="OFF"/>

      <logger name="jdbc.sqlonly" level="OFF"/>
      <logger name="jdbc.sqltiming" level="DEBUG"/>
      <logger name="jdbc.audit" level="OFF"/>
      <logger name="jdbc.resultset" level="OFF"/>
      <logger name="jdbc.resultsettable" level="DEBUG"/>
      <logger name="jdbc.connection" level="OFF"/>

      <root level="INFO">
          <appender-ref ref="STDOUT" />
      </root>

  </configuration>
  ```

- 이전 실습 폴더에서 templates에 있는 html 파일 가져오기( index, join, modifyForm )

- com.study.springboot 밑에 controller,dao,dato 패키지와 폴더 만들기

  ![image.png](/assets/img/pageimg/mybatis1.png)

- html입력폼(post) <-> DTO <-> DAO(Entity)
  <-> Mapper XML(Repo 인터페이스) <-> DB

- resources>**mybatis>mapper>MemberDao.xml** 생성

  ```java
  // xml 문서라는 문구
  <?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  // 매핑 (파일 구조에 맞게 작성하기 주의)
  <mapper namespace="com.study.springboot.dao.IMemberDao">
      <!-- public List<MemberDto> list();   -->
      <!-- sql 문 뒤에 세미콜론은 찍지 않는다 -->
      <!--  id는 함수 이름이다  -->
      <select id="list" resultType="com.study.springboot.dto.MemberDto">
          SELECT * FROM member ORDER BY join_date DESC
      </select>
  </mapper>
  ```

- 데이터 연결이 됐으니, 데이터 를 화면에 띄우기 위해서 controller를 수정
  - main 매서드에 root경로를 list로 redirect로 반환함.
  - list 매서드엔 list 경로(root경로가 될 것임) 데이터를 넣어서 반환함 (Model 객체 사용)
- listcount 를 mybatis에서 불러오는 법

  1. `model.addAttribute("listcount",list.size());`
  2. MemberDao.xml 파일에 count 하는 sql문 코드 추가

     ```java
     <!-- int : _int   -->
     <!-- Integer : int   -->
     <select id="count" resultType="_int">
         SELECT COUNT(*) FROM member;
     </select>
     ```

     IMemberDao 에 코드 추가

     ```java
     public int count();
     ```

     HtmlController 파일에서 코드 변경

     ```java
     int listcount = memberDao.count();
     model.addAttribute("listcount",listcount);
     ```

- 회원가입 정보를 db 에 insert 하기

1.  MemberDao.xml 파일에 insert 하는 sql문 코드 추가

```java
<insert id="insert" parameterType="com.study.springboot.dto.MemberDto">
    INSERT INTO member
    VALUES (0,#{userId},#{userPw},#{userName},#{userRole},#{joinDate})
</insert>
```

IMemberDao 코드 추가

```java
public int insert(MemberDto dto);
```

2. map으로 부르기

```java
<insert id="insertMap" parameterType="map">
    INSERT INTO member
    VALUES (0,#{userId},#{userPw},#{userName},#{userRole},#{joinDate})
</insert>
```

IMemberDao 코드 추가

```java
public int insert(Map map);
```

controller 코드 작성

```java
@PostMapping("/joinAction")
@ResponseBody
public String joinAction(@ModelAttribute MemberDto dto) {
    try{
        int result = memberDao.insert(dto);
        System.out.println("result = " + result);
        if(result != 1) {
            return "회원가입 실패";
        }
    }
    catch (Exception e) {
        e.printStackTrace();
        return "<script>alert('회원가입실패');history.back();</script>";
    }
    return "<script>alert('회원가입성공');location.href='/list';</script>";
}
```

| Content-Type                      | 처리 방식 | 어노테이션                      |
| --------------------------------- | --------- | ------------------------------- |
| application.x-www-form-urlencoded | 폼 데이터 | @ModelAttribute , @RequestParam |
| application/json                  | JSON 본문 | @RequestBody                    |

- id로 회원정보 단일 호출하기

  MemberDao.xml 파일에 sql문 코드 추가

  ```java
  <!-- param1 : 첫번째 매개변수 예약어   -->
  <select id="findById" resultType="com.study.springboot.dto.MemberDto">
      SElECT * FROM member where id=#{ param1 }
  </select>
  ```

  IMemberDao 코드 추가

  ```java
  public Optional<MemberDto> findById(int i);
  ```

  controller

  ```java
  @GetMapping("/viewMember")
  public String viewMember(@RequestParam int id, Model model) {
      Optional<MemberDto> optional = memberDao.findById(id);
      if(optional.isPresent()) {
          model.addAttribute("member",optional.get());
      } else {
          System.out.println("회원정보가 없습니다");
          return "redirect:/list";
      }
      return "modifyForm";
  }
  ```

- update
  MemberDao.xml 파일에 sql문 코드 추가
  ```java
  <update id="update" parameterType="com.study.springboot.dto.MemberDto">
      UPDATE member SET user_id=#{userId}, user_pw=#{userPw},
      user_name=#{userName}, user_role=#{userRole}, join_date=#{joinDate}
      WHERE id=#{ id }
  </update>
  ```
  IMemberDao 코드 추가
  ```java
  public int update(MemberDto dto);
  ```
  controller
  ```java
  @PostMapping("/modifyAction")
  @ResponseBody
  public String modifyAction(@ModelAttribute MemberDto dto) {
      try {
          int result = memberDao.update(dto);
          if(result != 1) {
              return "<script>alert('회원수정실패');history.back();</script>";
          }
      }
      catch (Exception e) {
          e.printStackTrace();
          return "<script>alert('수정실패');history.back();</script>";
      }
      return "<script>alert('수정성공');location.href='/list';</script>";
  }
  ```
- delete

  1. MemberDao.xml 파일에 sql문 코드 추가

  ```java
  <delete id="delete" >
      DELETE FROM member WHERE id=#{param1}
  </delete>
  ```

  IMemberDao 코드 추가

  ```java
  public int delete(int id);
  ```

  2. map으로 부르기

  ```java
  <delete id="deleteMap" parameterType="map">
      DELETE FROM member WHERE id=#{id} and user_id=#{userId}
  </delete>
  ```

  IMemberDao 코드 추가

  ```java
  public int deleteMap(int id, String userId);
  ```

  controller

  ```java
  @GetMapping("deleteMember")
  @ResponseBody
  private String deleteMember(@RequestParam int id) {
      try{
          int result = memberDao.delete(id);
  //            int result = memberDao.deleteMap(id,"leehong");
          if (result != 1) {
              return "<script>alert('회원삭제실패');history.back();</script>";
          }
      } catch (Exception e) {
          e.printStackTrace();
          return "<script>alert('회원삭제실패');history.back();</script>";
      }
      return "<script>alert('회원삭제성공');history.back();</script>";
  }
  ```
