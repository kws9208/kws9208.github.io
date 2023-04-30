---
title: "Spring 입문"
layout: single
categories: spring
tags: spring
toc: true
toc_label: "Spring 입문" 
---

# Spring 입문

멋쟁이사자처럼 대학 11기 백엔드 스프링부트 교육 과정 중 인프런 강좌,  
`스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술`을 수강하고 정리한 포스트입니다.  

## 1. 프로젝트 환경설정

### 프로젝트 생성

개발환경: Java 17, IntelliJ Ultimate  

스프링 프로젝트 생성: https://start.spring.io  

![image](https://user-images.githubusercontent.com/82245973/234819578-a1ea73ef-58ba-43ac-8b2a-065ce17f404c.png)  

- Project: Gradle - Groovy  
필요한 라이브러리들을 가져오고 빌드하는 라이플사이클까지 관리해주는 도구  
과거에는 Maven을 썼지만 요즘에는 Gradle 사용  
- Language: Java  
개발 언어인 Java 선택  
- Spring Boot: 3.0.6  
Snapshot: 아직 만들고 있는 버전, M1: 정식 릴리즈 버전 X  
정식 릴리즈 버전 중 가장 최신인 3.0.6 선택  

- Project Meta  
Group: 보통 기업 도메인명을 작성  
Aritifact: 빌드 후 나오는 결과물, 프로젝트명 같은 것  

- Dependencies: Spring Web(웹 프로젝트), Thymeleaf(HTML을 만들어주는 템플릿 엔진)  

잘 선택한 이후 GENERATE하여 다운로드 받은 zip 파일을 작업하는 폴더에 압축풀고 IntelliJ로 실행한다.  

아래는 IntelliJ Ultimate로 스프링 프로젝트 생성하는 방법이다.  

![image](https://user-images.githubusercontent.com/82245973/234816019-c7c94762-2d8c-43db-a8d8-aa8c108bfb9a.png)  

![image](https://user-images.githubusercontent.com/82245973/234816221-23534203-c0c7-410d-8804-6c2ed1f7d0a6.png)  

IntelliJ에서 프로젝트를 임포트한 후 외부에서 라이브러리를 다운받아야하기 때문에 인터넷에 연결되어있어야 한다.  

**전체 프로젝트 구조**  

![image](https://user-images.githubusercontent.com/82245973/234824913-ee125665-6e3e-42da-b133-10f199c5ebc1.png)  

- /gradle: 그래들이 사용하는 폴더
- /src: 표준화된 폴더 구조(main/java)
- /src/main/java: 자바 소스 폴더
- /src/main/resources: 자바 코드를 제외한 설정파일이나 HTML 등을 저장하는 폴더
- /src/test: 테스트 코드 폴더
- build.gradle: gradle 설정 파일

**build.gradle**

```gradle
plugins {
    id 'java'
    id 'org.springframework.boot' version '3.0.6'
    id 'io.spring.dependency-management' version '1.1.0'
}

group = 'com.example'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '17'

repositories {
    // 라이브러리들을 다운 받는 공간(사이트)
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    // 기본 내장 테스트 라이브러리: JUnit5
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

tasks.named('test') {
    useJUnitPlatform()
}
```

**스프링부트 실행**  

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }

}
```

DemoApplication.java 파일을 실행하면 스프링부트에 내장된 톰캣 웹서버가 실행되면서 스프링부트가 같이 실행되며 웹브라우저 localhost:8080에서 확인 가능하다.  

![image](https://user-images.githubusercontent.com/82245973/234898012-e648c973-bcf9-4475-9bf6-a7e8e6a3f487.png)  

아직 아무것도 없어서 에러 페이지만 나온다.  

### 라이브러리 살펴보기

gradle이나 maven 같은 빌드 도구들은 의존관계를 관리해주는데 어떤 라이브러리를 추가하면  라이브러리가 필요로 하는 라이브러리들을 자동으로 추가해준다.  

![image](https://user-images.githubusercontent.com/82245973/234895108-f9146ff4-31e1-4ef5-a59b-0ac96f66be72.png)

IntelliJ 우측의 코끼리 모양 아이콘(gradle)을 클릭하면 라이브러리들 간의 의존성을 확인할 수 있다.  

- `spring-boot-starter-web` → `spring-boot-starter-tomcat`  / `spring-webmvc`  

스프링부트는 웹서버 톰캣을 내장하고 있어서 따로 웹서버 설정없이 서버 프로그램을 실행하면 자동으로 웹서버가 실행되고 8080 포트로 접근이 가능하다.  

- `spring-boot-starter-thymeleaf` → `thymeleaf-spring`  

타임리프와 관련된 라이브러리를 가지고 있다.

- `spring-boot-starter` → `spring-boot-starter-logging` / `spring-boot` / `spring-core`  

spring-boot-starter는 스프링 프로젝트를 하면 웬만하면 다 의존관계로 임포트가 된다.   
스프링부트와 관련된 라이브러리를 쓰면 spring-core까지 스프링과 관련된 라이브러리들을 가져온다.  

그리고 현업에서는 System.out.println 으로 출력하지 않고 log로 출력하는데 로그를 남겨야 로그들을 따로 보거나 파일로 관리할 수 있기 때문이다. 로깅 라이브러리로 slf4j나 logback을 거의 표준처럼 사용한다.  

- `spring-boot-starter-test` → `junit5` / `mockito` / `assertj` / `spring-test`  

자바 진영에서 테스트할 때 junit5 라이브러리를 주로 사용한다. mockito나 assertj는 테스트를 편리하게 하는데 도와주는 라이브러리이고 spring-test는 스프링과 통합해서 테스트할 때 도움을 주는 라이브러리이다.  

### View 환경설정

간단한 welcome page를 만들어 본다.  

/resource/static 폴더에 index.html을 만들어두면 스프링부트가 index.html을 웰컴 페이지로 보여준다.  
웰컴 페이지란 웹브라우저 주소창에 도메인 입력 시 보이는 첫 화면을 말한다.  

**정적 페이지**

```html
<!DOCTYPE HTML>
<html>
<head>
    <title>Hello</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
    Hello
    <a href="/hello">hello</a>
</body>
</html>
```

![image](https://user-images.githubusercontent.com/82245973/234915134-f6055f75-edd1-4769-b8e9-f21849062c3f.png)  

**동적 페이지**

thymeleaf라는 템플릿 엔진을 이용하여 동적 페이지를 만든다.  

- 컨트롤러 작성  
컨트롤러는 웹 애플리케이션의 첫 번째 진입점이다.  

```java
@Controller
public class HelloController {

    @GetMapping("hello")    // /hello라는 요청이 들어오면 아래의 메서드를 호출함
    public String hello(Model model){   // 웹 MVC의 Model
        // 모델의 data라는 속성에 hello!!라는 값을 넣음
        model.addAttribute("data", "hello!!");
        // 모델을 /resources/template 폴더 안에 있는 html 파일로 전달하여 렌더링
        return "hello";
    }
}

```

- 템플릿 작성

```html
<!DOCTYPE HTML>
<!-- thymeleaf 템플릿 엔진 선언 후 템플릿 구문 사용 가능 -->
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Hello</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
    <!-- thymeleaf 구문 -->
    <!-- 컨트롤러가 넘겨준 모델의 속성 사용 -->
    <p th:text="'안녕하세요. ' + ${data}" >안녕하세요. 손님</p>
</body>
</html>
```

**동작 과정**

![image](https://user-images.githubusercontent.com/82245973/234929465-0522280b-42d1-4bb0-8849-243a767e15e3.png)  

1. 웹브라우저에서 /hello 요청을 하게되면 스프링부트의 내장된 톰켓이 요청을 받아서 스프링 컨테이너에서 해당 요청에 맞는 컨트롤러를 매칭하여 메서드를 실행시킨다.  
2. 스프링은 요청이 올 때 모델을 만들어서 메서드에 넣어주는데 메서드는 이 모델의 속성에 값을 넣어 viewName을 리턴한다.  
3. 그러면 viewResolver는 /resources/template에서 해당 viewName과 맞는 html 파일을 찾아 model을 넘겨주고 템플릿을 렌더링한다.  
4. 렌더링된 html 파일을 웹브라우저에 전달한다.  

### 빌드하고 실행하기

빌드를 하면 실행할 수 있는 파일 만들어진다. 여태까지 한 것은 IntelliJ에서만 실행한 것이다.  
cmd 창을 키고 다음 명령어를 입력한다. (window 기준)  

```
cd project/Java-Study-01/우석/demo  // 프로젝트 폴더로 이동
gradlew build // 스프링 프로젝트 빌드
dir // 파일 및 폴더 목록 확인
cd build/libs   // jar 파일이 있는 폴더로 이동
java -jar demo-0.0.1-SNAPSHOT.jar // jar 파일 실행
```

나중에 서버에 배포할 때는 프로젝트를 빌드하고 나온 jar 파일만 배포하고 실행시키면 된다.  
그러면 서버에서도 스프링을 실행할 수 있게 된다.  

## 2. 스프링 웹 개발 기초

### 정적 컨텐츠

html 파일을 그대로 웹 브라우저에 전달해 주는 것  

스프링부트는 정적 컨텐츠 기능을 제공한다.  
/static 폴더에 html 파일을 만들면 해당 html로 접근할 수 있다.  

**hello-static.html**

```html
<!DOCTYPE HTML>
<html>
<head>
    <title>Hello</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
Hello
<a href="/hello">hello</a>
</body>
</html>
```

**정적 컨텐츠 이미지**

![image](https://user-images.githubusercontent.com/82245973/235051341-8afbe75f-35d0-4d7b-a6d8-10a97b00ac14.png)  

1. 웹브라우저에서 요청이 들어오면 스프링부트의 내장 톰캣 서버가 요청을 받는다.  
2. 요청을 스프링에게 넘기고 스프링 컨테이너에서 해당 리소스와 매핑이 되는 컨트롤러를 찾는다.  
3. 만약 없다면 /resources/static 에서 해당 html 파일을 찾아서 웹브라우저에 전송해준다.  

### MVC와 템플릿 엔진

JSP, php 등 템플릿 엔진을 이용하여 html을 서버에서 동적으로 변형하여 전달해 주는 것  

이러한 개발 패턴을 MVC라고 하는데 Model, View, Controller의 줄임말이다.  

**HelloController.java**

```java
@Controller
public class HelloController {
    @GetMapping("hello-mvc")
    // @RequestParam: 파라미터를 받는 어노테이션(웹사이트에서 url 파라미터를 받음)
    public String helloMVC(@RequestParam("name") String name, Model model){
        model.addAttribute("name", name);
        return "hello-template";
    }
}
```

**hello-template.html**

```html
<html xmlns:th="http://www.thymeleaf.org">
<body>
    <p th:text="'hello ' + ${name}">hello! empty</p>
</body>
</html>
```

그냥 /hello-mvc로 접근하면 
`WARN 9032 --- [nio-8080-exec-8] .w.s.m.s.DefaultHandlerExceptionResolver : Resolved [org.springframework.web.bind.MissingServletRequestParameterException: Required request parameter 'name' for method parameter type String is not present]` 와 같은 오류가 발생한다.  

웹브라우저에서 get방식으로 파라미터를 넘길때 url 뒤에 `?[변수명]='값'` 을 추가해주면 파라미터가 같이 전달된다. `localhost:8080/hello-mvc?name=spring`  

![image](https://user-images.githubusercontent.com/82245973/235060772-dceac4d5-53c6-4743-a7d4-211d1b73f9c2.png)  

**MVC, 템플릿 엔진 이미지**

![image](https://user-images.githubusercontent.com/82245973/235060444-98872487-d18c-4107-bf78-68f834d07b51.png)  

1. 요청이 오면 내장 톰캣 서버가 스프링에게 요청을 넘긴다.  
2. 스프링은 스프링 컨테이너에서 요청과 관련된 컨트롤러를 찾고 메서드를 실행한다.  
3. 메서드는 입력받은 파라미터를 모델에 저장하고 viewName을 리턴한다.  
4. viewResolver가 viewName과 맞는 html 파일을 찾아주고 모델을 토대로 html을 렌더링하여 브라우저에 전달한다.  

### API

html 파일이 아니라 json이라는 데이터 포맷으로 클라이언트나 서버에 데이터를 전달하는 것  

**문자열 반환**

```java
@Controller
public class HelloController {
    @GetMapping("hello-string")
    // HTTP 응답의 Body 부분에 리턴값을 넣어 전달함
    @ResponseBody
    // view가 없어서 매개변수로 Model을 추가하지 않음
    public String helloString(@RequestParam("name") String name){
        return "hello " + name;
    }
}
```

![image](https://user-images.githubusercontent.com/82245973/235066453-37c6468a-0b9a-47f1-b6b5-d30ca3e10d18.png)  

html 파일이 아니라 그냥 문자열 자체가 전달된다.  

**객체 반환**

```java
@Controller
public class HelloController {
    @GetMapping("hello-api")
    @ResponseBody
    // 객체를 리턴
    public Hello helloApi(@RequestParam("name") String name){
        Hello hello = new Hello();
        hello.setName(name);
        return hello;
    }

    // Hello 클래스
    static class Hello{
        private String name;

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }
    }

}
```

![image](https://user-images.githubusercontent.com/82245973/235076599-085ae20e-99d4-4e3e-8eed-b3b6f7e451a9.png)  

json 객체가 전달되는데 json은 key-value로 이루어진 자료구조로 스프링에서 객체를 반환하면 json으로 변환하여 전달한다.  

![image](https://user-images.githubusercontent.com/82245973/235077805-afc057eb-3541-4ec2-877d-5d5eff7b1562.png)  

1. 요청이 오면 톰캣은 요청을 스프링에 전달한다.  
2. 스프링은 요청과 관련된 컨트롤러를 찾는다.  
3. @ResponseBody 어노테이션을 만나면 메서드의 리턴값을 HttpMessageConverter로 넘긴다.  
4. 객체를 Json 형식으로 바꾸고 요청한 웹브라우저나 서버에 전달한다.  

리턴값이 문자면 StringHttpMessageConverter 로, 객체면 MappingJackson2HttpMessageConverter 로 넘겨준다.
객체를 json 형식으로 바꿔주는 라이브러리는 크게 두 가지로, Jackson2이랑 Gson이 있지만 스프링은 기본적으로 Jackson을 사용한다.  

## 3. 회원 관리 예제 - 백엔드 개발

### 비즈니스 요구사항 정리

가장 단순한 비즈니스 요구사항
- 데이터: 회원ID, 이름
- 기능: 회원 등록, 조회
- 데이터베이스: 아직 결정되지 않음

**웹 애플리케이션 계층 구조**

![image](https://user-images.githubusercontent.com/82245973/235151208-9b14bac4-5774-47eb-a845-bf2a87520bb0.png)  

- 컨트롤러: 웹 MVC의 컨트롤러
- 서비스: 핵심 비즈니스 로직
- 리포지토리: 데이터베이스에 접근하여 도메인 객체를 저장하고 관리하는 인터페이스
- 도메인: 데이터베이스에 저장되는 비즈니스 도메인 객체

**클래스 의존관계**

![image](https://user-images.githubusercontent.com/82245973/235151283-755dfcef-8a44-4cf9-a73e-bfd371412d07.png)  

데이터베이스가 아직 결정되지 않아 인터페이스로 구현한다.  
개발 초기에는 가벼운 메모리 기반의 데이터 저장소를 사용한다.  

### 회원 도메인과 리포지토리 만들기

**회원 객체**

```java
public class Member {
    // 데이터를 구분하기 위해 시스템이 저장하는 임의의 값
    private Long id;
    // 이름
    private String name;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

**회원 리포지토리 인터페이스**

```java
public interface MemberRepository {
    Member save(Member member);
    // Optional<>: null 값을 다루는 객체
    Optional<Member> findById(Long id);
    Optional<Member> findByName(String name);
    List<Member> findAll();
}
```

**회원 리포지토리 메모리 구현체**

```java
public class MemoryMemberRepository implements MemberRepository{
    // 데이터를 저장하는 변수
    // 동시성 문제를 해결하려면 ConcurrentHashMap 사용
    private static Map<Long, Member> store = new HashMap<>();
    // 키 값을 생성해주는 변수
    // 동시성 문제를 고려하면 AtomicLong 사용
    private static long sequence = 0L;

    @Override
    public Member save(Member member) {
        // id 세팅
        member.setId(++sequence);
        // 저장
        store.put(member.getId(), member);
        return member;
    }

    @Override
    public Optional<Member> findById(Long id) {
        // null이 반환될 가능성이 있는 객체를 Optional로 감싸기
        return Optional.ofNullable(store.get(id));
    }

    @Override
    public Optional<Member> findByName(String name) {
        // stream을 이용하여 이름이 같은 객체 찾기
        return store.values().stream()
                .filter(member -> member.getName().equals(name))
                .findAny();
    }

    @Override
    public List<Member> findAll() {
        // return store.values().stream().collect(Collectors.toList());
         return new ArrayList<>(store.values());
    }
}
```

### 회원 리포지토리 테스트 케이스 작성

JUnit을 활용한 테스트 케이스 작성  

패키지 레벨, 클래스 레벨에서 테스트 케이스를 동시에 돌려 볼 수 있지만, 테스트의 순서는 보장되지 않아서 테스트가 끝날 때마다 데이터가 지워지는 코드를 만들어두어야 한다.  

테스트는 서로 의존관계 없이 설계되어야하므로 하나의 테스트가 끝날 때마다 공용 저장소를 초기화해줘야 문제가 발생하지 않는다.  

**회원 리포지토리 메모리 구현체 테스트**

```java
public class MemoryMemberRepositoryTest {
    MemoryMemberRepository repository = new MemoryMemberRepository();

    // @AfterEach: @Test 메서드가 끝날 때마다 동작하는 콜백 함수
    @AfterEach
    public void afterEach(){
        repository.clearStore();
    }

    // @Test: 메서드 테스트
    @Test
    public void save(){
        Member member = new Member();
        member.setName("spring");

        repository.save(member);

        Member result = repository.findById(member.getId()).get();

        // Test pass시 출력되는 것은 없으나 초록불, fail시 빨간불
        // Assertions.assertEquals(member, result);

        // AssertJ static method: assertThat
        assertThat(member).isEqualTo(result);
    }

    @Test
    public void findByName(){
        Member member1 = new Member();
        member1.setName("spring1");
        repository.save(member1);

        Member member2 = new Member();
        member2.setName("spring2");
        repository.save(member2);

        Member result = repository.findByName("spring1").get();

        assertThat(result).isEqualTo(member1);
    }

    @Test
    public void findAll(){
        Member member1 = new Member();
        member1.setName("spring1");
        repository.save(member1);

        Member member2 = new Member();
        member2.setName("spring2");
        repository.save(member2);

        List<Member> result = repository.findAll();

        assertThat(result.size()).isEqualTo(2);
    }
}
```

### 회원 서비스 개발

회원 도메인과 리포지토리를 활용해 실제 비즈니스 로직 작성  

**회원 서비스**

```java
public class MemberService {
    private final MemberRepository memberRepository = new MemoryMemberRepository();

    // 회원가입
    public Long join(Member member){
        // 중복 회원 검증
        validateDuplicateMember(member);
        memberRepository.save(member);
        return member.getId();
    }

    // extract method
    private void validateDuplicateMember(Member member) {
        memberRepository.findByName(member.getName())
                // Optional 객체의 값이 존재하면
                .ifPresent(m -> {
                    throw new IllegalStateException("이미 존재하는 회원입니다.");
                });
    }

    // 전체 회원 조회
    public List<Member> findMembers(){
        return memberRepository.findAll();
    }

    // 개인 회원 조회
    public Optional<Member> findOne(Long memberId){
        return memberRepository.findById(memberId);
    }
}
```

### 회원 서비스 테스트

**기존 회원 서비스 메모리 회원 리포지토리**

```java
public class MemberService {
    private final MemberRepository memberRepository = new MemoryMemberRepository();
}
```

**회원 리포지토리가 회원 서비스 코드에 DI가 가능하도록 변경**

```java
public class MemberService {
    private final MemberRepository memberRepository;

    // DI(Dependency Injection): MemberRepository를 외부에서 주입하도록 변경
    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
}
```

**회원 서비스 테스트 케이스 작성**

```java
class MemberServiceTest {
    MemberService memberService;
    MemoryMemberRepository memberRepository;

    // @BeaforEach: 테스트 메서드가 실행되기 전에
    @BeforeEach
    public void beforeEach(){
        // MemoryMemberRepository 생성
        memberRepository = new MemoryMemberRepository();
        // MemberService에 MemoryMemberRepository 주입
        memberService = new MemberService(memberRepository);
    }

    // @AfterEach: 테스트 메서드를 실행한 후
    @AfterEach
    public void afterEach(){
        // memberRepository 초기화
        memberRepository.clearStore();
    }

    @Test
    // 테스트 함수는 한글로 작성해도 무방함
    // 빌드될 때 테스트 코드는 실제 코드에 포함되지 않음
    void 회원가입() {
        // given: 주어진 상황에서
        Member member = new Member();
        member.setName("hello");

        // when: 무언가 실행했을 때
        Long savedId = memberService.join(member);

        // then: 이런 결과가 나와야 함
        Member findMember = memberService.findOne(savedId).get();
        assertThat(member.getName()).isEqualTo(findMember.getName());
    }

    @Test
    public void 중복_회원_예외() {
        // given
        Member member1 = new Member();
        member1.setName("spring");

        Member member2 = new Member();
        member2.setName("spring");

        // when
        memberService.join(member1);
        IllegalStateException e = assertThrows(IllegalStateException.class, () -> memberService.join(member2));

        // then
        assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");
    }

    @Test
    void findMembers() {
        // given
        Member member1 = new Member();
        member1.setName("spring1");
        memberRepository.save(member1);

        Member member2 = new Member();
        member2.setName("spring2");
        memberRepository.save(member2);

        // when
        List<Member> result = memberService.findMembers();

        // then
        assertThat(result.size()).isEqualTo(2);
    }

    @Test
    void findOne() {
        // given
        Member member = new Member();
        member.setName("spring");
        memberRepository.save(member);

        // when
        Member result = memberService.findOne(member.getId()).get();

        // then
        assertThat(result).isEqualTo(member);
    }
}
```

## 4. 스프링 빈과 의존관계

### 컴포넌트 스캔과 자동 의존관계 설정

```java
// @Controller: 스프링이 실행될 때, 스프링 컨테이너가 컨트롤러 객체를 만들고 스프링 빈으로 등록함
@Controller
public class MemberController {

    private final MemberService memberService;

    // DI(Dependency Injection): 스프링 컨테이너에 있는 MemberService를 연결시켜줌
    @Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
    }
}

// @Service: 스프링이 실행될 때, 스프링 컨테이너가 서비스 객체를 만들고 스프링 빈으로 등록함
@Service
public class MemberService {
    private final MemberRepository memberRepository;

    @Autowired
    // DI(Dependency Injection): MemberRepository를 외부에서 주입하도록 변경
    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
}

// @Repository: 스프링이 실행될 때, 스프링 컨테이너가 리포지토리 객체를 만들고 스프링 빈으로 등록함
@Repository
public class MemoryMemberRepository implements MemberRepository{
}

// @SpringBootApplication은 @ComponentScan 어노테이션을 포함함
// @ComponentScan: 이 클래스가 포함된 패키지 내에서 @Component 어노테이션이 작성된 클래스들을 스캔해서 자동으로 스프링 빈으로 등록해줌
// @Controller, @Service, @Repository 어노테이션은 @Component 어노테이션을 포함하기 때문에 자동으로 스프링 빈에 등록됨
// 스프링 컨테이너에서 스프링 빈으로 등록할 때 싱글톤으로 등록함(유일하게 하나만 등록하여 공유함)
@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }

}
```

### 자바 코드로 직접 스프링 빈 등록하기

회원 서비스와 회원 리포지토리의 @Service, @Repository, @Autowired 제거  

```java
@Configuration
public class SpringConfig {
    // @Bean: 메서드를 실행하여 반환되는 객체를 스프링 빈에 등록
    @Bean
    public MemberService memberService(){
        // 스프링 빈에 등록된 memberRepository를 넣어줌
        return new MemberService(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository(){
        return new MemoryMemberRepository();
    }
}
```

DI의 세 가지 방법  
- 필드 주입: 외부에서 접근이 불가능하다.
- setter 주입: 중간에 객체가 바뀔 가능성이 있다.
- 생성자 주입: 생성 시점에 생성자로 한번만 주입한다. 

스프링 빈에 등록되지 않은 객체는 @Autowired가 동작하지 않는다.  

## 5. 회원 관리 예제 - 웹 MVC 개발

### 회원 웹 기능 - 홈 화면 추가

**홈 컨트롤러 추가**

```java
@Controller
public class HomeController {
    // 도메인
    @GetMapping("/")
    public String home(){
        return "home";
    }
}
```

**회원 관리용 홈**

```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<body>
<div class="container">
    <div>
        <h1>Hello Spring</h1>
        <p>회원 기능</p>
        <p>
            <a href="/members/new">회원 가입</a>
            <a href="/members">회원 목록</a>
        </p>
    </div></div> <!-- /container -->
</body>
</html>
```

![image](https://user-images.githubusercontent.com/82245973/235296897-120b4cdd-c606-4463-9c76-b1833eaa2e62.png)  

도메인과 연결된 HomeController가 있어서 /resources/static/index.html은 무시된다.  

### 회원 웹 기능 - 등록

**회원 등록 폼 컨트롤러**

```java
// @Controller: 스프링이 실행될 때, 스프링 컨테이너가 컨트롤러 객체를 만들고 스프링 빈으로 등록함
@Controller
public class MemberController {
    private final MemberService memberService;

    // DI(Dependency Injection): 스프링 컨테이너에 있는 MemberService를 연결시켜줌
    @Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
    }

    // 회원가입 창으로 이동
    @GetMapping("/members/new")
    public String createForm(){
        return "members/createMemberForm";
    }
}
```

**회원 등록 폼 HTML**

```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org"><body>
<div class="container">
    <!-- form 태그 /members/new로 post 요청 -->
    <form action="/members/new" method="post">
        <div class="form-group">
        <!-- 폼 라벨 -->
        <label for="name">이름</label>
        <!-- input 태그, text를 입력받음 -->
        <!-- 입력한 값이 name 속성으로 스프링으로 전달됨-->
        <input type="text" id="name" name="name" placeholder="이름을 입력하세요">
        </div>
        <!-- 요청 전송 -->
        <button type="submit">등록</button>
    </form>
</div> <!-- /container -->
</body>
</html>
```

![image](https://user-images.githubusercontent.com/82245973/235296861-24a66125-c4d9-4a16-9787-dfa054acc624.png)  

**회원 폼 클래스**

```java
public class MemberForm {
    // <input type="text" id="name" name="name" placeholder="이름을 입력하세요">
    // 위 input 태그의 name 속성과 매칭됨
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

**회원 컨트롤러에서 등록 기능**

```java
@Controller
public class MemberController {
    // 회원가입
    @PostMapping("/members/new")
    // 스프링이 전달받은 값을 MemberForm 클래스의 setter 메서드로 값을 설정
    public String create(MemberForm form){
        // 회원 만들기
        Member member = new Member();
        member.setName(form.getName());

        // 회원 등록
        memberService.join(member);

        // 홈 화면으로 이동
        return "redirect:/";
    }
}
```

### 회원 웹 기능 - 조회

**회원 컨트롤러에서 조회 기능**

```java
@Controller
public class MemberController {
    // 회원목록
    @GetMapping("/members")
    public String list(Model model){
        List<Member> members = memberService.findMembers();
        model.addAttribute("members", members);
        return "members/memberList";
    }
}
```

**회원 리스트 HTML**

```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<body>
<div class="container">
  <div>
    <table>
      <thead> <tr>
        <th>#</th>
        <th>이름</th>
      </tr>
      </thead>
      <tbody>
      <!-- model 안에 있는 members를 가져옴 -->
      <!-- th:each 루프를 돌음 -->
      <!-- members에서 하나씩 꺼내서 member에 넣음-->
      <tr th:each="member : ${members}">
        <!-- member의 id와 name을 가져옴 -->
        <!-- Member 클래스의 getter 메서드로 id와 name을 가져옴 -->
        <td th:text="${member.id}"></td>
        <td th:text="${member.name}"></td>
      </tr>
      </tbody>
    </table>
  </div>
</div> <!-- /container -->
</body>
</html>
```

![image](https://user-images.githubusercontent.com/82245973/235297085-352c23bd-5eb0-415d-b2a2-b022d67a4007.png)  

데이터는 메모리에만 존재해서 서버를 재부팅하게 되면 데이터는 사라져있다.  
그래서 데이터들을 파일이나 데이터베이스에 저장해야한다.  

## 6. 스프링 DB 접근 기술

### H2 데이터베이스 설치

[https://www.h2database.com/html/download-archive.html](https://www.h2database.com/html/download-archive.html) 에서 `1.4.200` 버전을 설치한다.  

설치 후 `C:\Program Files (x86)\H2\bin` 에서 `h2.bat` 를 실행한다. (window 기준)  

**테이블 생성하기**

```sql
drop table if exists member CASCADE;
create table member
(
 id bigint generated by default as identity,
 name varchar(255),
 primary key (id)
);
```

### 순수 JDBC

**환경 설정**

```gradle
dependencies{
    // JDBC 드라이버
    implementation 'org.springframework.boot:spring-boot-starter-jdbc'
    // H2 데이터베이스 클라이언트
    runtimeOnly 'com.h2database:h2'
}
```

```properties
# H2 데이터베이스 연결 정보
spring.datasource.url=jdbc:h2:tcp://localhost/~/test
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.username=sa
```

**JDBC 회원 리포지토리**

```java
public class JdbcMemberRepository implements MemberRepository{
    // DB에 접속하기 위한 데이터소스
    private final DataSource dataSource;

    // 스프링으로부터 DataSource 주입받기
    public JdbcMemberRepository(DataSource dataSource) {
        this.dataSource = dataSource;
    }

    @Override
    public Member save(Member member) {
        String sql = "insert into member(name) values(?)";

        Connection conn = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;

        try {
            conn = getConnection();
            pstmt = conn.prepareStatement(sql, Statement.RETURN_GENERATED_KEYS);

            pstmt.setString(1, member.getName());

            pstmt.executeUpdate();
            rs = pstmt.getGeneratedKeys();

            if (rs.next()) {
                member.setId(rs.getLong(1));
            } else {
                throw new SQLException("id 조회 실패");
            }
            return member;
        } catch (Exception e) {
            throw new IllegalStateException(e);
        } finally {
            close(conn, pstmt, rs);
        }
    }

    @Override
    public Optional<Member> findById(Long id) {
        String sql = "select * from member where id = ?";

        Connection conn = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;

        try {
            conn = getConnection();
            pstmt = conn.prepareStatement(sql);
            pstmt.setLong(1, id);

            rs = pstmt.executeQuery();

            if(rs.next()) {
                Member member = new Member();
                member.setId(rs.getLong("id"));
                member.setName(rs.getString("name"));
                return Optional.of(member);
            } else {
                return Optional.empty();
            }
        } catch (Exception e) {
            throw new IllegalStateException(e);
        } finally {
            close(conn, pstmt, rs);
        }
    }

    @Override
    public List<Member> findAll() {
        String sql = "select * from member";

        Connection conn = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;

        try {
            conn = getConnection();
            pstmt = conn.prepareStatement(sql);

            rs = pstmt.executeQuery();
            List<Member> members = new ArrayList<>();
            while(rs.next()) {
                Member member = new Member();
                member.setId(rs.getLong("id"));
                member.setName(rs.getString("name"));
                members.add(member);
            }

            return members;
        } catch (Exception e) {
            throw new IllegalStateException(e);
        } finally {
            close(conn, pstmt, rs);
        }
    }

    @Override
    public Optional<Member> findByName(String name) {
        String sql = "select * from member where name = ?";

        Connection conn = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;

        try {
            conn = getConnection();
            pstmt = conn.prepareStatement(sql);
            pstmt.setString(1, name);

            rs = pstmt.executeQuery();

            if(rs.next()) {
                Member member = new Member();
                member.setId(rs.getLong("id"));
                member.setName(rs.getString("name"));
                return Optional.of(member);
            } return Optional.empty();
        } catch (Exception e) {
            throw new IllegalStateException(e);
        } finally {
            close(conn, pstmt, rs);
        }
    }

    private Connection getConnection() {
        return DataSourceUtils.getConnection(dataSource);
    }

    private void close(Connection conn, PreparedStatement pstmt, ResultSet rs)
    {
        try {
            if (rs != null) {
                rs.close();
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        try {
            if (pstmt != null) {
                pstmt.close();
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        try {
            if (conn != null) {
                close(conn);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    private void close(Connection conn) throws SQLException {
        DataSourceUtils.releaseConnection(conn, dataSource);
    }
}

```

**스프링 설정 변경**

```java
@Configuration
public class SpringConfig {
    // 스프링 빈이 데이터 소스를 만들어 줌
    private DataSource dataSource;

    // DataSource 주입
    @Autowired
    public SpringConfig(DataSource dataSource) {
        this.dataSource = dataSource;
    }

    // @Bean: 메서드를 실행하여 반환되는 객체를 스프링 빈에 등록
    @Bean
    public MemberService memberService(){
        // 스프링 빈에 등록된 memberRepository를 넣어줌
        return new MemberService(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository(){
//        return new MemoryMemberRepository();
        // MemberRepository 구현체만 JDBC 리포지토리로 변경
        return new JdbcMemberRepository(dataSource);
    }
}
```

스프링의 DI를 이용하여 기존 코드의 변경없이 설정만으로 구현 클래스를 변경할 수 있다.  

또한 데이터를 이제 DB에 저장하므로 스프링 서버를 재부팅해도 데이터가 안전하게 보호된다.  

### 스프링 통합 테스트

이전까지 순수한 자바 코드 테스트(단위 테스트)를 진행했었다.  

이제는 스프링 컨테이너와 DB까지 연결한 통합 테스트를 진행한다.  

**회원 서비스 스프링 통합 테스트**

```java
// @SpringBootTest: 스프링부트를 실행하여 스프링 컨테이너와 같이 테스트
@SpringBootTest
// @Transactinal: 테스트가 끝나면 트랜잭션을 롤백해줌 -> @BeforeEach, @AfterEach 제거
@Transactional
class MemberServiceIntegrationTest {
    // 테스트 케이스의 경우 필드 주입 가능
    @Autowired
    MemberService memberService;
    @Autowired
    MemberRepository memberRepository;

    @Test
    // 테스트 함수는 한글로 작성해도 무방함
    // 빌드될 때 테스트 코드는 실제 코드에 포함되지 않음
    void 회원가입() {
        // given: 주어진 상황에서
        Member member = new Member();
        member.setName("hello");

        // when: 무언가 실행했을 때
        Long savedId = memberService.join(member);

        // then: 이런 결과가 나와야 함
        Member findMember = memberService.findOne(savedId).get();
        assertThat(member.getName()).isEqualTo(findMember.getName());
    }

    @Test
    public void 중복_회원_예외() {
        // given
        Member member1 = new Member();
        member1.setName("spring");

        Member member2 = new Member();
        member2.setName("spring");

        // when
        memberService.join(member1);
        IllegalStateException e = assertThrows(IllegalStateException.class, () -> memberService.join(member2));

        // then
        assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");
    }

    @Test
    void findMembers() {
        // given
        Member member1 = new Member();
        member1.setName("spring1");
        memberRepository.save(member1);

        Member member2 = new Member();
        member2.setName("spring2");
        memberRepository.save(member2);

        // when
        List<Member> result = memberService.findMembers();

        // then
        assertThat(result.size()).isEqualTo(2);
    }

    @Test
    void findOne() {
        // given
        Member member = new Member();
        member.setName("spring");
        memberRepository.save(member);

        // when
        Member result = memberService.findOne(member.getId()).get();

        // then
        assertThat(result).isEqualTo(member);
    }
}
```

### 스프링 JdbcTemplate

**스프링 JdbcTemplate 회원 리포지토리**

JdbcTemplate는 JDBC API에서 반복적으로 사용되는 코드를 제거해주지만 SQL은 직접 작성해야한다.  

```java
public class JdbcTemplateMemberRepository implements MemberRepository{
    private final JdbcTemplate jdbcTemplate;

    // @Autowired: 생성자가 하나일 때, 생략가능
    // DataSource 주입
    public JdbcTemplateMemberRepository(DataSource dataSource) {
        this.jdbcTemplate = new JdbcTemplate(dataSource);
    }

    @Override
    public Member save(Member member) {
        SimpleJdbcInsert jdbcInsert = new SimpleJdbcInsert(jdbcTemplate);
        jdbcInsert.withTableName("member").usingGeneratedKeyColumns("id");

        Map<String, Object> parameters = new HashMap<>();
        parameters.put("name", member.getName());

        Number key = jdbcInsert.executeAndReturnKey(new MapSqlParameterSource(parameters));
        member.setId(key.longValue());
        return member;
    }

    @Override
    public Optional<Member> findById(Long id) {
        List<Member> result = jdbcTemplate.query("select * from member where id = ?", memberRowMapper(), id);
        return result.stream().findAny();
    }

    @Override
    public Optional<Member> findByName(String name) {
        List<Member> result = jdbcTemplate.query("select * from member where name = ?", memberRowMapper(), name);
        return result.stream().findAny();
    }

    @Override
    public List<Member> findAll() {
        return jdbcTemplate.query("select * from member", memberRowMapper());
    }

    private RowMapper<Member> memberRowMapper(){
        return (rs, rowNum) -> {
            Member member = new Member();
            member.setId(rs.getLong("id"));
            member.setName(rs.getString("name"));
            return member;
        };
    }
}
```

**JdbcTemplate을 사용하도록 스프링 설정 변경**

```java
@Configuration
public class SpringConfig {
    // 스프링 빈이 데이터 소스를 만들어 줌
    private DataSource dataSource;

    // DataSource 주입
    @Autowired
    public SpringConfig(DataSource dataSource) {
        this.dataSource = dataSource;
    }

    // @Bean: 메서드를 실행하여 반환되는 객체를 스프링 빈에 등록
    @Bean
    public MemberService memberService(){
        // 스프링 빈에 등록된 memberRepository를 넣어줌
        return new MemberService(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository(){
//        return new MemoryMemberRepository();
        // MemberRepository 구현체만 JDBC 리포지토리로 변경
//        return new JdbcMemberRepository(dataSource);
        // MemberRepository 구현체만 JDBC Template 리포지토리로 변경
        return new JdbcTemplateMemberRepository(dataSource);
    }
}
```

### JPA

JPA를 사용하면 기존의 반복 코드를 줄여줄 뿐만 아니라 기본적인 SQL도 작성해준다.  
JPA는 SQL과 데이터 중심 설계에서 객체 중심 설계로 패러다임을 전환하여 개발 생산성을 크게 높여준다.  

**JPA 라이브러리 추가**

```gradle
// JPA
implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
```

**스프링부트 JPA 설정 추가**

```properties
# JPA 설정
# JPA가 만드는 SQL 출력
spring.jpa.show-sql=true
# 자동 테이블 생성 기능 끄기
spring.jpa.hibernate.ddl-auto=none
```

**JPA 엔티티 매핑**

```java
// JPA는 인터페이스로 그 구현체로 Hibernate가 스프링에서 쓰임
// JPA는 ORM 기술로 객체(Object)와 릴레이션(Relation)을 매핑(Mapping)시킴
// @Entity: JPA가 관리하는 엔티티로 지정
@Entity
public class Member {
    // Pk 지정
    @Id
    // Identity 전략: DB가 id를 자동으로 생성해 주는 것
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    // 데이터를 구분하기 위해 시스템이 저장하는 임의의 값
    private Long id;
    // 이름
    private String name;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

**JPA 회원 리포지토리**

```java
public class JpaMemberRepository implements MemberRepository{
    // JPA는 모든게 EntityManger로 동작함
    private final EntityManager em;

    // EntityManager 주입
    public JpaMemberRepository(EntityManager em) {
        this.em = em;
    }

    @Override
    public Member save(Member member) {
        em.persist(member);
        return member;
    }

    @Override
    public Optional<Member> findById(Long id) {
        Member member = em.find(Member.class, id);
        return Optional.ofNullable(member);
    }

    @Override
    public Optional<Member> findByName(String name) {
        List<Member> result = em.createQuery("select m from Member m where m.name = :name", Member.class)
                .setParameter("name", name)
                .getResultList();
        return result.stream().findAny();
    }

    @Override
    public List<Member> findAll() {
        return em.createQuery("select m from Member m", Member.class).getResultList();
    }
}
```

**서비스 계층 트랜잭션 추가**

```java
// @Transactional: JPA에서 모든 데이터 변경은 transaction 안에서 실행되어야함
@Transactional
public class MemberService {}
```

**JPA를 사용하도록 스프링 설정 변경**

```java
@Configuration
public class SpringConfig {
    // 스프링 빈이 엔티티 매니저를 만들어 줌
    private EntityManager em;

    // EntityManager 주입
    @Autowired
    public SpringConfig(EntityManager em) {
        this.em = em;
    }

    // @Bean: 메서드를 실행하여 반환되는 객체를 스프링 빈에 등록
    @Bean
    public MemberService memberService(){
        // 스프링 빈에 등록된 memberRepository를 넣어줌
        return new MemberService(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository(){
//        return new MemoryMemberRepository();
        // MemberRepository 구현체만 JDBC 리포지토리로 변경
//        return new JdbcMemberRepository(dataSource);
        // MemberRepository 구현체만 JDBC Template 리포지토리로 변경
//        return new JdbcTemplateMemberRepository(dataSource);
        // MemberRepository 구현체만 JPA 리포지토리로 변경
        return new JpaMemberRepository(em);
    }
}
```

### 스프링 데이터 JPA

스프링 데이터 JPA는 구현 클래스 없이 인터페이스만으로 개발이 가능하다.  

**스프링 데이터 JPA 회원 리포지토리**

```java
// JpaRepository를 상속한 인터페이스가 있으면 스프링 데이터 JPA가 구현체를 자동으로 만들어서 스프링 빈에 등록해줌
public interface SpringDataJpaMemberRepository extends JpaRepository<Member, Long>, MemberRepository {
    @Override
    Optional<Member> findByName(String name);
}
```

**스프링 데이터 JPA 회원 리포지토리를 사용하도록 스프링 설정 변경**

```java
@Configuration
public class SpringConfig {
    private final MemberRepository memberRepository;

    @Autowired
    // 스프링 데이터 JPA가 만든 구현체 주입
    public SpringConfig(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

    // @Bean: 메서드를 실행하여 반환되는 객체를 스프링 빈에 등록
    @Bean
    public MemberService memberService(){
        // 스프링 빈에 등록된 memberRepository를 넣어줌
        return new MemberService(memberRepository);
    }
}
```

**스프링 데이터 JPA 제공 클래스**

![image](https://user-images.githubusercontent.com/82245973/235310395-538f610c-596d-4648-8c8c-c03896f77f8a.png)  

스프링 데이터 JPA는 인터페이스를 통해 기본적인 CRUD 뿐만 아니라 메서드 이름만으로 조회하는 기능을 제공한다.  

## 7. AOP

### AOP가 필요한 상황

예를 들어, 모든 메소드의 호출 시간을 측정하고 싶다면?  

모든 메서드에 호출 시간 로직을 추가해줘야 한다.  

![image](https://user-images.githubusercontent.com/82245973/235312093-1026eee9-13d8-4619-b940-59b9e24dd564.png)  

이처럼 핵심 로직이 아닌 공통 로직은 만들거나 변경하기도 어렵고 유지보수도 어렵다.  

**MemberService 회원 조회 시간 측정 추가**

```java
@Transactional
public class MemberService {
    private final MemberRepository memberRepository;

    // DI(Dependency Injection): MemberRepository를 외부에서 주입하도록 변경
    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

    // 회원가입
    public Long join(Member member){
        long start = System.currentTimeMillis();
        try{
            // 중복 회원 검증
            validateDuplicateMember(member);
            memberRepository.save(member);
            return member.getId();
        }finally {
            long finish = System.currentTimeMillis();
            long timeMs = finish - start;
            System.out.println("join = " + timeMs + "ms");
        }
    }

    // extract method
    private void validateDuplicateMember(Member member) {
        memberRepository.findByName(member.getName())
                // Optional 객체의 값이 존재하면
                .ifPresent(m -> {
                    throw new IllegalStateException("이미 존재하는 회원입니다.");
                });
    }

    // 전체 회원 조회
    public List<Member> findMembers(){
        long start = System.currentTimeMillis();
        try{
            return memberRepository.findAll();
        }finally {
            long finish = System.currentTimeMillis();
            long timeMs = finish - start;
            System.out.println("findMembers = " + timeMs + "ms");
        }
    }
}
```

### AOP 적용

AOP(Aspect Oriented Programming)는 공통 관심사항과 핵심 관심사항을 분리하는 것이다.  

![image](https://user-images.githubusercontent.com/82245973/235312102-6f7a53b4-8619-4cbf-9b76-fb702d501c4b.png)  

**시간 측정 AOP 등록**

```java
// @Aspect: AOP 사용
@Aspect
// AOP를 스프링 빈에 등록
@Component
public class TimeTraceAop {
    // @Around: 공통 관심사항 지정(패키지 하위에 대해 모두 적용)
    @Around("execution(* com.example.demo..*(..))")
    private Object execute(ProceedingJoinPoint joinPoint) throws Throwable{
        long start = System.currentTimeMillis();
        System.out.println("START: " + joinPoint.toString());
        try {
            return joinPoint.proceed();
        }finally {
            long finish = System.currentTimeMillis();
            long timeMs = finish - start;
            System.out.println("END: " + joinPoint.toString() + " " + timeMs + "ms");
        }
    }
}
```

AOP를 이용하면 핵심 관심 사항과 공통 관심 사항을 분리하여 별도의 공통 로직을 만들 수 있고 원하는 적용 대상을 선택할 수 있다.  

**AOP 적용 전 전체 그림**

![image](https://user-images.githubusercontent.com/82245973/235312185-664a4844-4a2e-48e1-9fe7-b1a25e4362b9.png)  

AOP 적용 전에는 의존 관계에 따라서 호출한다.  

**AOP 적용 후 전체 그림**

![image](https://user-images.githubusercontent.com/82245973/235312145-ce4c7a3c-5412-439f-8a90-a159ab3828ca.png)  

스프링 컨테이너 AOP가 적용된 클래스에 대해 가짜 스프링 빈을 만들고 프록시를 통해 AOP를 실행한 뒤 joinPoint.proceed()에서 진짜가 호출된다.  