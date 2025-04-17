---
layout: page
related_posts:
  - /be/
title: TDD and BDD
date: 2025-04-17
description: >
  springboot TDD,BDD 수업 정리
---

| 특성      | TDD                                | BDD                                       |
| --------- | ---------------------------------- | ----------------------------------------- |
| 접근 방식 | 테스트를 먼저 작성하고 코드를 구현 | 행동을 기술하고 이를 기반으로 테스트 작성 |
| 초점      | 코드의 기능 검증                   | 사용자 행동과 비즈니스 가치에 초점        |
| 언어      | 기술적으고 개발자 중심의 언어      | 비지니스 도메인 언어 사용                 |

**Ex21TDD**

### TDD( Test Driven Development ) 방법론 (테스트 주도 개발)

- 개발주기

  ![image.png](/assets/img/pageimg/test.png)

중요한 것은 실패하는 테스트 코드를 작성할 때까지 실제 코드를 작성하지 않는 것과, 실패하는 테스트를 통과할 정도의 최소 실제 코드를 작성해야 하는 것이다.

### 일반 개발 방식 vs TDD 개발 방식

- 보통 방식

  - 요구 사항 분석 → 설계 → 개발 → 테스트 → 배포
  - 개발을 느리게 하는 잠재적 위험
  - 소스 코드의 품질이 저하될 수 있다.

- TDD 개발
  - 디자인 → 테스트 코드 작성 → 코드 개발
  - 테스트 코드를 작성한 뒤에 실제 코드를 작성함.
  - 디버깅 시간을 단축할 수 있음( 데이터가 잘못 나올 때 DB에 문제인지, 비지니스 레이어의 문제인지 등등 ) 을 손 쉽게 찾아낼 수 있다.
- Junit5 : 자바 프로그래밍 언어용 단위 테스트 프레임워크 → [참고 글](https://nextmoveon.tistory.com/271)

  - 어노테이션
    - `@Test` , `@ParameterizedTest` , `@RepeatedTest` 등등

- 실습 프로젝트 생성
  ![image.png](/assets/img/pageimg/test1.png)

  - build 파일에 직접 의존성 추가 ( 줄 맞춤 주의 오류가 날 수도 있음 )

    ```java
    implementation 'com.google.code.gson:gson' //gson 의존성 추가 : json구조를 띄는 직렬화된 데이터를 JAVA의 객체로 역직렬화, 직렬화 해주는 자바 라이브러리
    testCompileOnly 'org.projectlombok:lombok' // lombok 테스트 의존성 추가
    testAnnotationProcessor 'org.projectlombok:lombok' // lombok 테스트 의존성 추가
    ```

  - index.html 생성

    ```java
    <!DOCTYPE html>
    <html xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="UTF-8">
        <title>index.html</title>
    </head>
    <body>
        로그인 사용자의 아이디는 <span th:text="${ member.loginId }"></span> <br>
        로그인 사용자의 암호는 <span th:text="${ member.loginPw }"></span> <br>
    </body>
    </html>
    ```

- LifeCycleTest

  - test폴더에 LifeCycleTest 생성, 설정에 빌드>빌드도구에서 빌드와 테스트 실행을 다 gradle → intelliJ IDEA로 변경

    ```java
    public class LifeCycleTest {
        @Test
        @DisplayName("테스트1")
        void testMethod() {
            System.out.println("테스트1");
        }
    }
    ```

    ![image.png](/assets/img/pageimg/test5.png)

  - 비활성화 테스트는 `@Disabled` test class 전체 실행시 실행되지 않는다. 단일 테스트는 실행이 됨.

  - 테스트 클래스 기준으로,

    테스트 메소드들이 실행되기 전에 한 번 실행 : `@Before`

    테스트 메소드들이 실행된 후에 한 번 실행 : `@After`

    ![image.png](/assets/img/pageimg/test2.png)

  - 각 테스트 메소드가

    실행되기 전에 매번 실행 : `@BeforeEach`

    실행된 후에 매번 실행 : `@AfterEach`

    ![image.png](/assets/img/pageimg/test3.png)

- 단정 (Assert) 메서드 : 테스트 케이스의 수행 결과를 판별하는 용도로 사용
  AssertTest 파일 생성

  ```java
  class Adder {
      public int add(int x, int y) {
          return x+y;
      }
  }

  public class AssertTest {
      private final Adder adder = new Adder();
      private final Adder adder2 = new Adder();
      private final String[] arr1 = {"a","b"};
      private final String[] arr2 = {"a","b"};
  }

  ```

  - assertArrayEquals(a, b) : 배열 a와 b가 일치함을 확인한다.

    ```java
    @Test
    void assertTest() {
        assertArrayEquals(arr1,arr2);
    }
    ```

  - assertEquals(a, b) : 객체 a와 b가 일치함을 확인한다. (객체에 정의되어 있는 equals를 통해 비교한다.)

    ```java
    @Test
    void test2() {
        assertEquals(3,adder.add(1,2));
    }
    ```

  - assertSame(a, b) : 객체 a와 b 가 같은 객체임을 확인 한다. (객체 자체를 비교한다. == )

    ```java
    @Test
    void test3() {
        assertSame(adder,adder);
        assertSame(adder,adder2); //주소값이 다름(서로 다른 객체임)
    }
    ```

  - assertTrue(a) : 조건 a가 참인지를 확인한다.
  - assertFalse(a) : 조건 a가 거짓인지를 확인한다.

    ```java
    @Test
    void test4() {
        assertTrue(3<10); //성공
    //        assertTrue(3>10); //실패
    //        assertFalse(3<10); //실패
    }
    ```

  - assertNotNull(a) : 객체 a가 null인지 확인한다.
    ```java
    @Test
    void test5() {
        assertNotNull(adder);
    }
    ```
  - assertAll() : 모든 종류의 assert를 각각 다 실행한다. 중간에 멈추지 않음.
    ```java
    @Test
    void test6() {
        assertAll(
                //람다식 코드로 한 번에 테스트함
                () -> { assertTrue(true,"assertTrue fail"); },
                () -> {
                    Object obj = new Object();
                    assertNotNull(obj,"obj Not Null");
                }
        );
    }
    ```

### BDD (Behavior-Driven Development)

- 행동주도 개발로, 개발자와 비즈니스 이해관계간의 협업을 강화하고 소프트웨어의 품질을 높이는데 초점을 맞춘 접근 방식이다.

- AssertJTest
  test폴더에 LifeCycleTest 생성

  ```java
  @Getter @Setter
  @AllArgsConstructor
  class User{
      private String name;
      private String nickname;
      private String email;
  }

  public class AssertJTest {

  }
  ```

  - AssertJ : Junit5의 Assert 함수와 비슷한 라이브러리, assertThat으로 시작하는 함수

    - 메소드 체이닝을 지원하기 때문에 더 깔끔하고 읽기 쉬운 테스트 코드를 작성할 수 있습니다.
    - 개발자가 테스트를 하면서 필요하다고 상상할 수 있는 거의 모든 메소드를 제공한다.
    - 디펜던시 : `testCompile 'org.assertj:assertj-core:3.26.3'`

  - test1

    ```java
    @Test
    void test1() {
        //DBB : 행동주도 개발
        //given(초기값), when(조건), then(기대되는 결과값)

        //given
        //when
        User user = new User("hong", "", "gildong@mail.com");
        //then
        Assertions.assertThat("".isEmpty()).isTrue();
        Assertions.assertThat(user.getName().isEmpty()).isFalse();
        Assertions.assertThat(user.getNickname().isEmpty()).isTrue();
    }
    ```

  - test2

    ```java
    @Test
    @DisplayName("list 테스트")
    void test2() {
        //given
        //when
        List<String> list = Arrays.asList("1","2","3");
        //then
        Assertions.assertThat(list).contains("1");
        Assertions.assertThat(list).contains();
        Assertions.assertThat(list).startsWith("1");
        Assertions.assertThat(list)
                .isNotEmpty()
                .contains("1")
                .doesNotContainNull()
                .containsSequence("2","3"); //2다음은 3인가?
    }
    ```

  - test3

    ```java
    @Test
    @DisplayName("객체 비교 테스트")
    void test3() {
        //given when
        User user1 = new User("hong","도둑","test@mail.com");
        User user2 = new User("hong","도둑","test@mail.com");
        //then
        //객체 주소 비교(얇은 비교)
        Assertions.assertThat(user1).isEqualTo(user1);
        //객체 내용 비교(깊은 비교)
        Assertions.assertThat(user1).usingRecursiveComparison().isEqualTo(user2);
    }
    ```

  - test4

    ```java
    import static org.assertj.core.api.Assertions.entry;
    @Test
    @DisplayName("맵 테스트")
    void test4() {
        //given when
        Map<String,String> map = new HashMap<>();
        map.put("name","hong");
        //then
        Assertions.assertThat(map)
                .isNotEmpty()
                .containsKey("name")
                .doesNotContainKeys("age")
                .contains(entry("name","hong"));
    }
    ```

  - test5

    ```java
    @Test
    @DisplayName("예외테스트")
    void test5() {
        //given
        String input = "abc";
        //when then
        //"java.lang.StringIndexOutOfBoundsException: String index out of range: 3" 라는 오류
        Assertions.assertThatThrownBy( () -> { input.charAt(3);} )
                .isInstanceOf(StringIndexOutOfBoundsException.class)
                .hasMessageContaining("String index out of range")
                .hasMessageContaining(String.valueOf(2)); //3으로 바꾸면 오류 안남
    }
    ```

  - test6, test7

    ```java
    import static org.assertj.core.api.Assertions.offset;
    @Test
        @DisplayName("문자열 테스트")
        void test6(){
            //given when
            String msg = "Hello, world! Nice to meet you.";
            //then
            Assertions.assertThat(msg)
                    .isNotEmpty()
                    .contains("Nice")
                    .contains("world")
                    .doesNotContain("zzz")
                    .startsWith("Hello")
                    .endsWith("u.")
                    .isEqualTo("Hello, world! Nice to meet you.");
      }
    @Test
    @DisplayName("숫자 테스트")
    void test7(){
        //given when then
        Assertions.assertThat(3.14d)
                .isPositive()
                .isGreaterThan(3)
                .isLessThan(4)
                .isEqualTo(3, offset(1d)) //일의 자릿수까지 비교
                .isEqualTo(3.1, offset(0.1d)) //소숫점 첫째자리까지 비교
                .isEqualTo(3.14);
    }
    ```

  - test8
    ```java
    @Test
    @DisplayName("SoftAssertions 사용하기")
    void test8() {
        //given when
        User user = new User("hong","thief","bank@mail.com");
        //then
        //SoftAssertions : 동시에 여러 테스트를 진행하고, 중간에 에러가 나도
        //  모든 검사를 수행한 후에 결과를 보여줌
        SoftAssertions soft = new SoftAssertions();
        soft.assertThat(user).isNotNull();
        soft.assertThat(user.getName()).isEqualTo("hong");
        soft.assertThat(user.getNickname()).isEqualTo(""); //thief 입력시 오류 X
        soft.assertThat(user.getEmail()).isEqualTo("bank@mail.com");
        soft.assertAll();
    }
    ```

### 계산기 실습 Test 해보기

Calc 파일 생성

```java
package com.study.springboot;

//사칙연산 계산기 클래스
public class Calc {
    public int add(int x, int y) {
        return x + y;
    }
    public int sub(int x, int y) {
        return x - y;
    }
    public int mul(int x, int y) {
        return x * y;
    }
    public int div(int x, int y) {
        return x / y;
    }
}

```

CalcTest 생성 (Calc 파일 → 우클릭 → 생성 → 테스트 → 전체선택 → 생성)

```java
package com.study.springboot;

import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;

class CalcTest {
    static Calc calc;

    @BeforeAll
    static void init() {
        System.out.println("init();");
        if(calc==null) {
            calc = new Calc();
        }
    }

    @Test
    void add() {
        assertEquals(12,calc.add(10,2));
    }

    @Test
    void sub() {
        assertEquals(8,calc.sub(10,2));
    }

    @Test
    void mul() {
        assertEquals(20,calc.mul(10,2));
    }

    @Test
    void div() {
        assertEquals(5,calc.div(10,2));
    }
}
```

- Test Code는 localhost로 직접 실행해서 결과를 확인하지 않아도 된다.

  - 잘 되는지 test 코드 확인 방법
    - `@WebMvcTest` : 가상 HTTP 요청을 통해 테스트 한다.
    - `@DataJpaTest` : Jpa DataSoruce 테스트
    - `@SpringBootTest` : 스프링부트 Application의 모든 빈을 로드한다.
      : 실제 서비스와 유사한 상태로 만들어 준다.
    - `MockMvc` : 웹 API 테스트를 할 때 가상의 HTTP 요청을 만들어주는 클래스이다.
    - `@MockitoBean` : API Controller에서 주입받은 Bean 객체에 대해서 Mock 형태로 객체 생성
  - Controller

    ```java
    @Controller
    public class HtmlController {

        @GetMapping
        public String main(Model model,MemberDto dto) {
            dto.setLoginId("hong");
            dto.setLoginPw("1234");
            model.addAttribute("member",dto);

            List<String> list = new ArrayList<>();
            list.add("hong");
            list.add("lee");
            model.addAttribute("list",list);

            return "index";
        }
    }
    ```

  - ControllerTest

    ```java
    @WebMvcTest // 가상 HTTP 요청을 통해 테스트한다.
    class HtmlControllerTest {
    //  MockMvc : 웹 API 테스트를 할 때 가상의 HTTP 요청을 만들어주는 클래스이다.
        @Autowired
        private MockMvc mockMvc;

        @Test
        @DisplayName("main 테스트")
        void main() {
            MemberDto dto = new MemberDto("hong","1234");
        try {
                mockMvc.perform(get("/"))
                        .andExpect(status().isOk()) //HTTP응답 200 ok
                        .andExpect(view().name("index"))
                        .andExpect(model().attributeExists("member"))
                        .andExpect(model().attribute("list", Matchers.contains("hong","lee")))
                        .andDo(print());
                } catch (Exception e) {
                e.printStackTrace();
                fail("테스트 실패");
            }
        }
    }
    ```

    결과
    ![image.png](/assets/img/pageimg/test4.png)

    ➕ 로그인 Action Test 함.
