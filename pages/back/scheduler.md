---
layout: page
related_posts:
  - /be/
title: Scheduler&FileUpload
date: 2025-04-21
description: >
  springboot Scheduler&FileUpload 수업 정리
---

**Ex22Scheduler**

### 주기적으로 동작하는 프로그램

- 사용 용도 예시

  - 정기적인 알림 전송 ( 특정 시간마다 사용자에게 **푸시 알림**이나 **이메일**을 보낼 때 )
  - 데이터 백업 또는 정리 작업 ( 매일/매주/매월 자동으로 DB 백업을 하거나 오래된 로그 파일 삭제 등 )
  - 배치 처리 ( 매 시간마다 외부 API에서 데이터 수집하여 DB 저장 )
  - 자동 결제 처리 ( 특정 날짜에 정기결제 실행 )
  - 주기적인 상태 점검 (서버 상태, 외부 시스템 연결 상태 등을 주기적으로 점검)
  - 통계 및 자동 생성 (매일밤, 매주말에 리포트 생성해서 저장하거나 이메일 전송)

- Application 설정

- `@Scheduler` 를 사용하기 위해서 `@EnableScheduling` 를 설정 해줘야한다.

- Scheduler

  - `@Component` : Spring Bean으로 등록해준다. 스프링이 이 클래스를 자동으로 인식하고 관리합니다.
  - `@EnableAsync` : 비동기 처리를 활성화한다. `@Async` 어노테이션과 함께 쓰면 메서드가 별도의 스레드에서 실행된다.
  - `@Slf4j` : 로그 찍게 도와주는 어노테이션이다.
  - `@Scheduled` 속성
    - fixedDelay : 작업 수행시간과 상관없이 일정 주기마다 메소드를 호출하는 것. (`+String` : 문자열로 값을 표현하겠다는 의미)
    - fixedRate : (작업수행 시간을 포함하여) 작업을 마친 후부터 주기 타이머가 돌아 메소드를 호출 (`+String` : 문자열로 값을 표현하겠다는 의미)
    - initialDelay : 스케줄러에서 메소드가 등록 되자마자 수행하는 것이 아닌 초기 지연시간을 설정 (`+String` : 문자열로 값을 표현하겠다는 의미)
    - cron : Corn 표현식을 사용하여 작업을 예약

- SchedulerConfig
  - `@Configuration` : 클래스에서 외부 라이브러리에 대한 설정을 한다.
  - `@Slf4j` : 로그 찍게 도와주는 어노테이션이다.

**Ex23FileUpload**

### 파일 업로드

- application.properties 에 설정 - yml 파일과 같은 역할 (설정하는 코드는 다를 수 있음)

  ```yaml
  # file upload
  spring.servlet.multipart.enabled=true
  #각자 로컬에 맞는 설정으로 경로를 바꿔야 함 (window는 경로를 \\ 해줘야함)
  spring.servlet.multipart.location=C:\\Users\\campus2H003\\Documents\\springboot\\Ex26FileUpload\\src\\main\\resources\\static\\upload
  spring.servlet.multipart.max-file-size=10MB
  spring.servlet.multipart.max-request-size=50MB

  # static resource
  spring.mvc.static-path-pattern=/static/**
  spring.resources.static-locations=classpath:/static/
  spring.web.resources.static-locations=classpath:/static/upload/
  #스프링이 기본 정적 리소스 매핑을 자동으로 설정한다.
  # /static/image.png -> http://localhost:8080/image.png
  spring.resources.add-mappings=true
  ```

- WebCongiguration

  - `@Configudation`
  - class 파일에 extends `WebMvcConfigurationSupport` 해준다.
  - 생성>매서드 재정의> `addResourceHandlers` 선택> 확인

  - 정적파일 uploadForm.html

    ```html
    <!DOCTYPE html>
    <html xmlns:th="http://www.thymeleaf.org">
      <head>
        <meta charset="UTF-8" />
        <title>파일 업로드</title>
      </head>
      <body>
        <h1>file upload</h1>
        <form method="post" action="/upload" enctype="multipart/form-data">
          <!-- multiple : 여러 파일 선택 가능 -->
          <input type="file" name="uploadFile" multiple="multiple" />
          <input type="submit" value="Upload" />
        </form>
      </body>
    </html>
    ```

  - HtmlController

    ```java
    @PostMapping("/upload")
    public String upload(@RequestParam MultipartFile[] uploadFile,
                         Model model) throws IOException {
        for(MultipartFile file : uploadFile) {
            if(!file.isEmpty()) {
                //새로운 파일이름으로 file을 기록한다.
                String uuid = UUID.randomUUID().toString(); //16진수 랜덤한 문자열 생성
                log.info("uuid: {}",uuid);
                // 1828zx72bz_image.png
                File newFilename = new File(uuid + "_" +file.getOriginalFilename());
                //물리적으로 파일에 기록한다.
                file.transferTo(newFilename);
            }
        }
        return "upload";
      }
    }
    ```

  - 결과 - upload 폴더에 파일이 업로드 됨

    ![image.png](/assets/img/pageimg/sc1.png)

  - **파일 리스트 다운로드**

  - result.html 생성

    ```html
    <!DOCTYPE html>
    <html xmlns:th="http://www.thymeleaf.org">
      <head>
        <meta charset="UTF-8" />
        <title>result.html</title>
      </head>
      <body>
        <h1>upload file 목록</h1>
        <ul th:each="file: ${files}">
          <li>
            <img th:src="|/upload/${file.uuid}_${file.fileName}|" />
          </li>
          <li>
            <span th:text="${file.fileName}"></span>
            <a th:href="@{'/upload/' + ${file.uuid} + '_' + ${file.fileName}}"
              >[download]</a
            >
          </li>
        </ul>
      </body>
    </html>
    ```

  - controller

    ```java
    @PostMapping("/upload2")
    public String upload2(@RequestParam MultipartFile[] uploadFile,
                          Model model) throws IOException {
        List<FileDto> list = new ArrayList<>();
        for(MultipartFile file: uploadFile) {
            if(!file.isEmpty()) {
                //FileDto 생성
                FileDto dto = new FileDto(UUID.randomUUID().toString(),
                        file.getOriginalFilename(),
                        file.getContentType());
                list.add(dto);

                //File을 물리적으로 기록하기
                File newFileName = new File(dto.getUuid()+"_"+dto.getFileName());
                file.transferTo(newFileName);

                //DB에 파일이름을 기록한다.
            }
        }
        model.addAttribute("files",list);
        return "result";
    }
    ```

  - 결과 - 업로드된 파일들이 list로 뜨고 download를 누르면 사진 다운이 되도록 함

    ![image.png](/assets/img/pageimg/sc2.png)
