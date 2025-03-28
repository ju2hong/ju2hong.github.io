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

### 자바 용어 설명

- JVM (Java Virtual Muchine): 자바 가상머신 - 운영체제마다 설치 되는 코드 해석기
- API (Application Program Interface): 프로그램 개발에 필요한 함수목록
- 클래스 라이브러리: 클래스 도서관, ap나 함수,클래스의 모음

### 자바실습

- `//`는 한 줄 주석문, `/* */`은 여러줄 주석문
- `sout`시에 `System.out.println();` 가 출력됨
- 문자열 연결시 `"문자"+"문자"` 하면 됨
- 연산자는 왼쪽이나 오른쪽에 문자열이 있으면 문자열 연결 연산자로 동작함
  - `System.out.println("화면" + 10 + 20);` 의 결과값은 `화면1020`
  - `System.out.println(10 + 20 + "화면");` 의 결과값은 `30화면`
- println 은 문자열 한 줄 출력 + 줄바꿈 , print는 줄바꿈 없음, printf는 형식화된 출력문
- 데이터 타입(기본 자료형 8개)
  - 정수형 : int(4), long(8), short(2), byte(1)
  - 실수형 : float(4), double(8)
  - 논리형 : boolean(1)
  - 문자형(내부적으로 숫자형) : char(2)
- 형변환 type casting
  - 형변환 공식
    1. 작은 정수형 -> 큰정수형(문제없음)
    2. 큰 정수형 -> 작은 정수형(표현범위 벗어나면 값잘림)
    3. 실수형 -> 정수령(값잘림,소숫점 날라감)
    4. 정수형 -> 실수형(문제없음)
  - 지동 형변환 : 대입(산술)연산자를 통해 자동으로 형변환 됨
  - 수동 형변환 : 형변환 연산자(타입)을 통해 형변환 할때
- 연산자 종류 - 이항연산자(산비논대)
  - 단항 : ++ -- !(논리반전) (타입) ~(비트반전) 우선순위가 가장 높다
  - 산술 : + - \* / % << >> >>>(비트단위 이동연산자)
  - 비교 : < > <= >= == != instanceof(객체비교연산자)
  - 논리 : && or & or ^ (비트단위 논리연산자)
  - 삼항 : ? :
  - 대입 : = 복합대입(+= -= \*= /= %= ...) 우선순위가 가장 낮다.
- 비교연산자
  - A > B : A가 B보다 큰가? true/false
  - A < B : 작은가?
- 논리 연산자
  - AND &&: ~이고,~이면서
  - OR ㅣㅣ : ~이거나, ~또는, ~중의 하나
  - NOT ! l ~아니다
  - T && T : T 둘다 참이면 참
  - F ㅣㅣ F : F 둘다 거짓이면 거짓
- 상황 연산자 : ? : 물음표 연산자
  - 패턴: (조건절) ? A값 : B값
- 대입연산자
  - A = B : B값을 A에 덮어쓰기 한다
  - 값의 전달 방향이 오른쪽에서 왼쪽
  - 단항연산자이므로 연산순서도 오른쪽에서 왼쪽으로
- 복합대입연산자
  - A += B : A = A + B
  - A -= B : A = A - B
