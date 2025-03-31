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

### 인텔리제이 단축키

- 출력관련 템플릿
  - **sout** : System.out.println();
  - **soutv**: 변수와 함께 출력
  - **soutm**: 현재 메서드 이름 출력
  - **soutp**: 메서드 파라미터 출력
- 객체 생성 및 캐스팅
  - **psf**: public staic final
  - **psvm**: main 메서드
  - **thr**: throw new
- 반복문 관련 템플릿
  - **fori**: 기본 for루프
  - **itar**: 배열을 위한 for 루프
  - **iter**: 향상된 for 루프
  - **itli**: List를 위한 for 루프
- 조건문 관련 템플릿
  - **ifn**: null 체크 조건문
  - **inn**: not null 체크 조건문

### Scanner 클래스 함수들

- nextLine() : 문자열을 입력받되 엔터칠 때 까지 (공백포함)
- next() : 문자열의 공백까지만 입력받음
- nextInt() : 문자열을 받아서 정수로 반환해줌
- nextFloat() : 실수로 반환해줌
- nextDouble() : 실수로 반환해줌

### 조건문 (자바스크립트 문법과 거의 동일)

- 단일 if문
  ```
  if(조건절-비교,논리 연산자) {
       true일 때 수행문
  }
  ```
- if else문
  ```
  if(10>20)
      System.out.println("10>20");
  else System.out.println("10<=20");
  ```
- if else if문
  ```
  int score = 90;
  if(score == 90){
      System.out.println("90");
  } else if(score == 80){
      System.out.println("80");
  } else {
      System.out.println("그외의 수");
  }
  ```
- 중첩 if문
  ```
  if(true){ //조건1
      if(false){ //조건2
          //조건1이고(and) 조건2이다
      } else if(true){ //조건3
          //조건1이고 조건2는 아니고 조건3이다
      }
  }
  ```

### 조건문 switch문 (자바스크립트와 거의 유사함)

```
  int a = 10;
  switch (a){
      case 10:
          System.out.println("10 입니다");
          break;
      case 20:
          System.out.println("20 입니다");
          break;
      default:
          System.out.println("그외의 경우");
          break;
  }
```

### 반복문

- for , do-while
- 패턴

```

for( 초기화; 조건; 증감 ) {
  실행문;
}

```

- 무한반복문 만드는 법

```

for( ; ; ){ }
while(true){ }

```

### 배열 Array

- 같은 타입의 데이터를 연속적 공간에 나열해 놓은 데이터구조
- 생성시에 크기가 지정되고 이후에는 변경 불가
- 인덱스는 0부터 부여됨

### 객체지향 프로그래밍

- 모든 사물을 객체(물건,사물)로 추상화(모델링,설계)하여 프로그래밍 하는 기법
- 속성(변수,필드)과 행동(함수,메서드)로 정의한다.

```
//클래스 선언
class Car{
  //속성(변수)
  int price = 1000;
  //행동(메소드)
  void run(){
  System.out.println("차가 달린다");  } }

  public class ex19 {
  public static void main(String[] args) {
  // 클래스이름 객체(인스턴스) 이름 = new 클래스이름();
  Car car = new Car();

          //멤버변수 접근하려면, 객체이름 뒤에 점.을 찍는다.
          System.out.println(car.price);
          //멤버함수 접근하려면, 객체이름 뒤에 점.을 찍는다.
          car.run();

          // System.out.println(car.run()); //void 반환값을 출력하면 x
      }

  }
```

- static 변수/함수

- static 예약어 : 정적변수(객체)/함수를 지정할 때 사용
- 의미 : 프로그램 구동시에 고정된 메모리 번지에 들어감 (자동 new), 프로그램 종료시까지 변경되지 않음
- 사용이유

  - 시작점(Entery Point)를 지정할 때 사용함
  - 중요한 데이터를 안정적으로 저장할 때 주로 사용
  - 자주 사용하는 유틸성 클래스에 지정한다. new를 안 해도 클래스 함수 사용 가능

- 접근제한자

- 클래스,함수,변수 앞에 위치하여 접근을 제한할 때 사용

  | 접근제한자 | 설명                                                   |
  | ---------- | ------------------------------------------------------ |
  | public     | 같은 폴더(패키지)에서, 다른 폴더의 클래스에서 접근가능 |
  | protected  | 같은 플더 + 상속 관계 클래스에서 접근 가능             |
  | default    | 같은 폴더                                              |
  | private    | 같은 클래스 안에서 접근 가능(캡슐화,은닉)              |
  |            | Getter/Setter 함수를 통해서 접근 가능하도록 허용       |
  |            | ex. 은행 잔고를 창구를 통해서만 접근가능하도록         |

- 메서드 오버로딩(Overloading)

  - 매개변수의 타입과 갯수를 다르게 함으로 함수의 기능을 확장하는것
  - 같은이름의 함수를 여러번 사용하기 위함
  - ex. println(정수형) => printlnInt
  -     println(문자열) => printlnStr

- 매서드 오버라이딩(Over riding)

  - 상속관계에서 자식클래스의 매서드가 부모클래스의 메소드를 재정의 하는것
  - 싱글톤(Signleton)
  - 프로그램 안에서 유일한 클래스 객체

- 생성자 함수(Constructor)

  - 클래스 객체가 생성될 때(new) 자동으로 호출되는 메서드
  - 용도 - 클래스 필드가 초기화 할 때

- 클래스의 상속
  - 부모(상위) 클래스의 유산(자원-함수와 변수)를 자식(하위) 클래스가 물려받은 것
  - 사용이유
    - 코드 중복을 피할 수 있다
    - 계층적인 구조로 코드를 설계할 수 있다
