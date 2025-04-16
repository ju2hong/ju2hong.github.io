---
layout: page
related_posts:
  - /be/
title: Session Login
date: 2025-04-15
description: >
  springboot Session Login 수업 정리
---

**Ex18LoginJoinDB**

- 프로젝트 생성

  ![image.png](/assets/img/pageimg/sessionlogin.png)

- application.properties 에 추가

  ```java
  # server port
  server.port=8080

  # thymeleaf
  spring.thymeleaf.cache=false

  # jpa
  spring.jpa.hibernate.ddl-auto=none
  spring.jpa.generate-ddl=false
  spring.jpa.show-sql=true
  spring.jpa.database-platform=org.hibernate.dialect.MySQL8Dialect
  # spring.jpa.hibernate.use-new-id-generator-mappings=false

  # pretty sql format
  spring.jpa.properties.hibernate.format_sql=true
  spring.jpa.properties.hibernate.use_sql_comments=true

  # mysql
  spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
  spring.datasource.hikari.jdbc-url=jdbc:mysql://localhost:3306/mydb?characterEncoding=UTF-8&serverTimezone=Asia/Seoul
  spring.datasource.username=root
  spring.datasource.password=mysql123

  # logging
  logging.level.org.hibernate.type.description.sql.BasicBinder=trace
  logging.level.org.hibernate.SQL=debug
  # Hibernate 6.1.5 updated in springboot 3.x
  logging.level.org.hibernate.orm.jdbc.bind=trace
  ```

- templates> index.html 파일 작성

  ![image.png](/assets/img/pageimg/sessionlogin1.png)

- com.study.springboot > controller,dto,entity 폴더 만들기

  ![image.png](/assets/img/pageimg/sessionlogin2.png)

- controller

  - loginaction - HttpSession 객체를 파라미터로 받아 세션으로 사용자저장
    ```java
    @GetMapping("/")
    public String main(HttpSession session) {
        //로그인 성공시 액션
        session.setAttribute("isLogin",true);
        session.setAttribute("userId","jui");
        return "index";
    }
    ```
  - logout 액션 - 세션을 `invalidate` 를 통해 로그아웃 시킴
    ```java
    @GetMapping("/logoutAction")
    public String logoutAction(HttpSession session) {
        session.invalidate();
        return "index";
    }
    ```

- entity > MemberEntity,MemberRepository 생성

- dto> MemberJoinDto 생성
  - if ) 사용자가 테이블과 다르게 추가적으로 입력 창에 데이터를 보낼 수도 있음. 이곳에 테이블에 없는 정보를 넣음
- dto> MemberLoginDto 생성

- 동작 구조

  html 입력 폼 ↔ DTO(@Controller) ↔ DTO(@Service) ↔ DAO(@Repository) ↔ DAO(Entity) ↔ DB 테이블

- 유효성 확인 vaildation 관련 어노테이션
  - @NotNull : null 불가
  - @Null : Null 만 입력 가능
  - @NotEmpty : Null, 빈 문자열 불가
  - @NotBlank Null, 빈 문자열, 스페이스만 있는 문자열 불가
- 로그인 처리 controller 구현

  - 로그인 시에 아이디가 잘못된 경우와 비밀번호가 잘못된 경우를 따로 나눠서 처리를 해줌. → 아이디가 틀렸는지 비밀번호가 틀렸는지 정확하게 알려줌. 사실 이런 로직 처리는 프론트에서 주로 함.

  - controller에 작성함 (아이디가 틀린 경우)
    ```java
     Optional<MemberEntity> optional =
                    memberRepository.findByUserId(dto.getUserId());
            if(!optional.isPresent()) {
                return "<script>alert('아이디가 없습니다. 회원가입 해주세요.');history.back();</script>";
            }
            return "<script>alert('로그인 성공');location.href='/';</script>";
    ```
  - MemberRepository 작성함

    ```java
    Optional<MemberEntity> findByUserId(String userId);
    ```

  - controller에 작성함 (비밀번호가 틀린 경우)
    ```java
    Optional<MemberEntity> optional1 =
                  memberRepository.findByUserIdAndUserPw(dto.getUserId(),dto.getUserPw());
          if(!optional1.isPresent()) {
              return "<script>alert('암호가 맞지 않습니다.'); history.back();</script>";
          }
    ```
  - MemberRepository 작성함
    ```java
    Optional<MemberEntity> findByUserIdAndUserPw(String userId,String userPw);
    ```

- 로그인시에 server에 JSESSION을 발급받는다. 로그아웃시엔 JSESSION이 삭제된다.
- 관리자페이지 일반회원페이지 로그인 나누기
  ```java
  String userRole = optional1.get().getUserRole();
        if(userRole.equals("ROLE_ADMIN")) {
            return "<script>alert('관리자 로그인 성공'); location.href='/admin';</script>";
        } else {
            return "<script>alert('로그인 성공'); location.href='/';</script>";
        }
  ```
- 회원가입폼 이동하기

  - joinForm.html 생성
  - 회원가입 get
    ```java
    @GetMapping("/joinForm")
    public String joinForm() {
        return "joinForm";
    }
    ```
  - 회원가입 post

    ```java
    @PostMapping("/joinAction")
      @ResponseBody
      public String joinAction(@Valid @ModelAttribute MemberJoinDto dto,
                               BindingResult bindingResult) {
          if(bindingResult.hasErrors()) {
              String detail = bindingResult.getFieldError().getDefaultMessage();
              String bindResultCode = bindingResult.getFieldError().getCode();
              return "<script>alert('"+detail+"'); history.back();</script>";
          }
          System.out.println("dto = " + dto.getUserId());
          try {
              MemberEntity entity = dto.toSaveEntity();
              MemberEntity newEntity = memberRepository.save(entity);
          } catch (Exception e) {
              e.printStackTrace();
              return "<script>alert('회원가입 실패'); history.back();</script>";
          }
          return "<script>alert('회원가입 성공'); location.href='/';</script>";
      }

    ```

    ➕오류가난 원인은 entity를 `AUTO` 타입으로 해둬서 오류가 남. `*IDENTITY` 로 바꿔서 오류 해결.

- 회원정보 수정/삭제
  - id 별 get
    ```java
    @GetMapping("/viewMember")
    public String viewMember(@RequestParam int id, Model model) {
        Optional<MemberEntity> optional =
                memberRepository.findById((long)id);
        if(!optional.isPresent()) {
            return "redirect:/admin"; //관리자 메인화면
        }
        optional.ifPresent((entity)->{
            model.addAttribute("member",entity.toSaveDto());
        });
        return "modifyForm";
    }
    ```
  - 수정 post
    ```java
    @PostMapping("modifyAction")
    @ResponseBody
    public String modifyAction(@ModelAttribute MemberJoinDto dto) {
        try{
            MemberEntity entity = dto.toUpdateEntity();
            memberRepository.save(entity);
        } catch (Exception e) {
            e.printStackTrace();
            return "<script>alert('회원수정 실패'); history.back();</script>";
        }
        return "<script>alert('회원수정 성공'); location.href='/admin';</script>";
    }
    ```
  - 삭제 post
    ```java
    @GetMapping("deleteMember")
    @ResponseBody
    public String deleteMember(@RequestParam int id) {
        Optional<MemberEntity> optional =
                memberRepository.findById((long)id);
        if(!optional.isPresent()) {
            return "<script>alert('회원삭제실패'); history.back();</script>";
        }
        MemberEntity entity = optional.get();
        try {
            memberRepository.delete(entity);
        } catch (Exception e) {
            e.printStackTrace();
            return "<script>alert('회원삭제실패'); history.back();</script>";
        }
        return "<script>alert('회원삭제성공'); location.href='/admin';</script>";
    }
    ```
