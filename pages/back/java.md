---
layout: page
related_posts:
  - /be/
title: java
date: 2025-03-28
description: >
  java 수업 정리
---

### JDK,인텔리제이 설치

💨 jdk 설치하기

👻 [oracle 링크](https://www.oracle.com/java/technologies/javase/jdk17-archive-downloads.html)

- jdk 설치 이후 자바 환경변수 변경

  1. `ctrl+R` -> `sysdm.cpl` 고급 시스템 변수 실행
  2. 환경변수 추가 JAVA_HOME 로 /jdk 경로를 붙여넣기
  3. cmd에 `javac -version`으로 확인

  ![image](/assets/img/pageimg/java.png)

  💨 인텔리제이 설치하기

- 인텔리제이는 바로 설치를 하지 않고 젯브레인 제품군을 관리해주는 툴박스 앱을 통해 다운로드 합니다. 👉 [툴박스 설치하기](https://www.jetbrains.com/ko-kr/toolbox-app/)

- 설치 후, 윈도우즈 오른쪽 아래 윗꺽쇠 버튼을 클릭해 젯브레인 툴박스 앱을 확인합니다. 그 후엔 아래 사진의 도구를 설치합니다.

  ![image](/assets/img/pageimg/java1.png)

- 앱 실행 후 플러그인으로 korean 과 Lombok 을 설치합니다.

- customize>All setting

  - 에디터> 파일 인코딩 모두 utf-8로 변경
  - 애디터> 일반> 자동 가져오기 java에 있는 빈 체크박스 체크

- 주석 색상 변경

- Editor 속 Color Scheme에 Java> comment 를 찾음

  - Line comment 에서 inherit 체크 해제 후 Foreground를 D7D052로 바꾼 후 apply
  - Block comment 에서 inherit 체크 해제 후 Foreground를 F574AE로 바꾼 후 apply

- 콘솔 한글 설정
  - 에디터> 파일 인코딩에서 전부 UTF-8로 변경, 명확한 ~ 체크박스 체크
  - 사용자 지정 VM 옵션 편집... 에서 아래 코드 추가후 저장
  ```
   -Dfile.encoding=UTF-8
   -Dconsole.encoding=UTF-8
  ```
  - 파일> 캐시 무효화 클릭하면 인텔리제이가 재실행 되면서 프로젝트를 초기화함 -> 한글출력 가능 해집니다.
