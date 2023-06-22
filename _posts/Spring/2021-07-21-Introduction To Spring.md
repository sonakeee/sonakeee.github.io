---
defaults:
  - scope:
      path: ""
      type: posts
    values:
      layout: single
      author_profile: true
      comments: true
      share: true
      related: true

title: "Intoduction To Spring"
excerpt: "스프링 입문 김영한"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Spring
tags:
  - [Spring, 김영한, 스프링입문]
date: 2021-07-21
last_modified_at: 2021-07-21
---

## 라이브러리

 이전에는 웹 서버(WAS)를 직접 서버에 설치해놓고 (ex. Tomcat) 그 곳에 JAVA 코드를 밀어넣는 식의 시스템이었다. 

이 때는 웹 서버랑 개발 라이브러리가 완전히 분리가 되어있었다. 

Tomcat 서버에 들어가서 라이브러리들을 설치해주어야 했다.  

그러나 지금은 소스 라이브러리에서 웹 서버를 들고 있다(웹 서버를 임베디드, 내장하고 있다.)

라이브러리 하나 빌드해서 서버에 올리면 끝난다. (따로 Tomcat 설치를 하지 않아도 된다.)

- spring-boot-starter-web
  - spring-boot-strater-tomcat: 톰캣 (웹서버)
  - spring-webmvc: 스프링 웹 MVC
- spring- boot-starter-thymeleaf: 타임리프 템플릿 엔진(View)
- spring-boot-starter(공통): 스프링 부트 + 스프링 코어 + 로깅
  - spring-boot
    - spring-core
  - spring-boot-starter-logging
    - logback,slf4j
- spring-bot-starter-test
  - junit: 테스트 프레임워크
  - mockito: 목 라이브러리
  - assertj: 테스트 코드를 좀 더 편하게 작성하게 도와주는 라이브러리
  - spring-test: 스프링 통합 테스트 지원
- spring-boot-devtools
  - html 파일을 컴파일만 해주면 서버 재시작 없이 View 파일 변경이 가능하다.



### 로그 관련 라이브러리

slf4j(인터페이스)  

logback(실제 로그를 어떤 구현체로 출력하는 부분)

이 두가지 조합을 많이 쓴다. (현재 표준)



## VIEW



### 홈페이지 (Welcome Page)

스프링 부트에서 홈페이지 (Welcome Page)는 먼저 static 폴더에서 index.html을 찾아보고 없으면 index 템플릿을 찾아본다.

[Welcome Page 공식문서](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.developing-web-applications.spring-mvc.welcome-page)



### 템플릿 엔진

스프링 부트는 템플릿 엔진으로 FreeMarker, Groovy, Thymeleaf, Mustache를 지원한다.

[thymeleaf 공식 사이트](https://www.thymeleaf.org/)



#### 작동방식

1. 웹 브라우저에서 url을 입력하면 내장 톰켓 서버로 연결된다.

2. 내장 톰켓 서버는 스프링 컨테이너에 url을 전달한다.

3. 스프링 컨테이너는 @Controller 어노테이션을 한 클래스들 안에서 @GetMapping으로 해당 url 이 입력된 메소드를 찾는다.

4. 조건에 맞는 메소드를 찾으면, 그 메소드는 html 파일 이름을 viewResolver에게 넘긴다.

   이 때 데이터인 담은 model 객체도 같이 넘겨준다.

5. viewResolver는 view를 찾아주고 템플릿엔진에 연결시켜준다.

   - resources:templates/{ViewName}.html

6. 템플릿 엔진은 넘겨진 (넘겨진 Model 데이터와 함께) html 파일을 변환해서 웹 브라우저에게 넘겨준다.



### 정적 컨텐츠

#### 작동방식

1. 웹 브라우저에서 url을 입력하면 내장 톰켓 서버로 연결된다.
2. 내장톰켓 서버는 스프링 컨테이너에서 먼저 전달받은 url을 찾아본다. (컨트롤러가 우선순위를 가진다.)
3. 스프링 컨테이너에서 찾지 못하면 static 폴더에서 해당 html 파일을 찾는다.
4. 찾은 html을 웹 브라우저에게 넘겨준다.

[정적 컨텐츠 공식문서](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.developing-web-applications.spring-mvc.static-content)



### API

#### 작동방식

1. 웹 브라우저에서 url을 입력하면 내장 톰켓 서버로 연결된다.

2.  @ResponseBody 어노테이션이 붙어있으면 http의 BODY에 직접 데이터를 반환한다.

3. 이 때 HttpMessageConverter가 동작한다.

4. 반환된 데이터가 단순 문자이면 StringConverter가 동작하고, 객체이면 JsonConverter가 동작한다.

   StringConverter 실제 이름은 StringHttpMessageConverter

   JsonConverter 실제 이름은 MappingJackson2HttpMessageConverter

   byte처리 등등 기타 여러 HttpMessageConverter가 기본으로 등록되어 있다.

   이 때 HTTP Accept 헤더가 꼭 XML 타입으로 받아야겠다고 적혀있다면, XmlConverter가 동작한다.

   (HTTP Accept 헤더와 서버의 컨트롤러 반환 타입 정보 둘을 조합해서 HttpMessageConverter가 선택된다.)

5. 바꾼 데이터를 웹 브라우저에게 넘겨준다.



## 웹 애플리케이션 구조

<img width="100%" src="https://user-images.githubusercontent.com/68231412/126516436-73bcdf15-838e-4a0e-841b-d37822b9219e.png" />

- 컨트롤러: 웹 MVC의 컨트롤러 역할
- 서비스: 핵심 비즈니스 로직 구현
- 리포지토리: 데이터베이스에 접근, 도메인 객체를 DB에 저장하고 관리
- 도메인: 비즈니스 도메인 객체, 예) 회원, 주문, 쿠폰 등등 주로 데이터베이스에 저장하고 관리됨



## 테스트

자바의 main 메서드를 통해서 실행하거나 웹 애플리케이션의 컨트롤러를 통해 테스트를 하는 방법도 있다.

하지만 이런 방법들은 준비하고 실행하는데 오래 걸리고, 반복 실행과 여러 테스트를 한번에 실행하기가 어렵다.  

자바는 JUnit이라는 프레임워크로 테스트를 실행해서 이러한 문제를 해결한다.

>  실무에서는 빌드 툴이랑 엮어서, 빌드 툴에서 빌드할 때 테스트 코드를 통과하지 못하면 다음 단계로 넘어가지 못하게 한다. 
> (회원 리포지토리 테스트 케이스 작성 07:30)

각각의 테스트는 서로 의존관계 없이 독립적으로 시행되어야 한다.

이를 위해 afterEach() 나 beforeEach() 등을 이용하자 (ex. 공용 리포지토리를 초기화 해준다)

예외처리 test는 assertThrows 를 이용해주자



### TDD란?

[TDD 링크](https://punsoo.github.io/tdd%20&%20clean%20code/TDD/)



### given/when/then  패턴

- given : 준비

- when: 실행
- then: 검증



### Spring 통합 테스트

@SpringBootTest, @Transactional 어노테이션을 활용해주자

@SpringBootTest 

스프링 컨테이너와 테스트를 함께 실행한다.

@Transactional

테스트 시작 전에 transaction 을 시작하고 테스트 완료 후에 항상 rollback을 해준다.

@Commit 어노테이션을 달아준 메소드는 롤백하지 않고 커밋시켜준다.

DB에 데이터가 남지 않으므로 다음 테스트에 영향을 주지 않는다.

> Spring 통합 테스트가 좋지만, 순수 자바 코드로 하는 단위 테스트가 더 유용한 경우가 많다.
>
> 기능을 단위별로 쪼개서 테스트 하도록 단위 테스트 설계를 하자.







## 스프링 Bean 등록 방법

생성자에 @Autowired가 있으면 스프링이 연관된 객체를 스프링 컨테이너에서 찾아서 넣어준다.

(생성자가 1개만 있으면 @Autowired는 생략할 수 있다.)

객체 의존관계를 외부에서 넣어주기에 DI(Dependency Injection)이다.

다만, 스프링이 의존관계를 넣어주려면, 넣어지는 대상이 스프링 Bean으로 등록되어 있어야 한다.

스프링 Bean으로 등록하는 방법에는 2가지 방법이 있다. 

(XML로 하는 방식도 있지만 최근에는 잘 사용하지 않는다.)

>  참고: 스프링은 스프링 컨테이너에 스프링 Bean을 등록할 때, 기본으로 싱글톤으로 등록한다.
>
> 설정으로 싱글톤이 아니게 설정할 수 있지만, 특별한 경우를 제외하면 대부분 싱글톤을 사용한다.



### 1. 컴포넌트 스캔 방법

- @Component 애노테이션이 있으면 스프링 빈으로 자동 등록된다.
- @Controller, @Service, @Repository  모두 @Component를 포함하기 때문에 컴포넌트 스캔이 된다.
- 정형화된(일반적인) 컨트롤러, 서비스, 리포지토리 같은 코드는 컴포넌트 스캔을 사용한다.

> 컴포넌트 스캔의 범위는 @SpringBootApplication 어노테이션이 붙은 클래스가 들어있는 패키지와 하위 패키지들이다.
>
> 설정으로 패키지 밖도 가능하기는 하다.



### 2. 자바 코드로 직접 등록하기

- @Configuration 어노테이션을 달은 클래스 안에서 @Bean 어노테이션을 달은 메소드를 통해 객체를 반환받음으로써 Bean으로 등록한다.
- 정형화 되지 않거나 상황에 따라 구현 클래스를 변경해야 하면 설정을 통해(자바 코드로 직접) 스프링 빈을 등록한다.



<br />

> 동적으로 의존관계를 바꿔주는 것은 좋지 않다. 보통은 한번 세팅되고 나면, 되도록 바꾸지 않는다. 
>
> 등록한 Bean을 의존 주입 해주는 방법에는 생성자 주입 말고도 필드 주입, setter주입이 있다.
>
> 필드 주입, setter 주입은 생성 시점 이후에도 불필요한 변경의 가능성이 있을 뿐더러, 
>
> 객체 생성시점에는 순환참조가 일어나는지 아닌지 발견할 수 있는 방법이 없다.
>
> 반면에 생성자 주입은 순환참조가 생기면 오류를 띄워 사전에 방지해준다.
>
> 이외에도 생성자 주입은 Null Pointer Exception을 방지해주며 테스트를 용이하게 해준다.
>
> [링크1](https://yaboong.github.io/spring/2019/08/29/why-field-injection-is-bad/), [링크2](https://madplay.github.io/post/why-constructor-injection-is-better-than-field-injection), [링크3](https://interconnection.tistory.com/124) [링크4](https://mangkyu.tistory.com/125) [링크5](https://interconnection.tistory.com/124) [링크6](https://velog.io/@gillog/Spring-DIDependency-Injection-%EC%84%B8-%EA%B0%80%EC%A7%80-%EB%B0%A9%EB%B2%95), [링크7](https://coding-start.tistory.com/250), [링크8](https://jurogrammer.tistory.com/79)



## 스프링의 DI

스프링의 DI(Dependencies Injection)을 사용하면 기존 코드를 전혀 손대지 않고, 설정만으로 구현 클래스를 변경할 수 있다.

Spring은 통일된 인터페이스를 통해 구현체만 바꾸어서 코드를 확장하는 다형성을 활용하기 좋다.

> 개방 폐쇄 원칙(OCP, Open-Closed Principle) 
>
> 확장에는 열려있고, 수정, 변경에는 닫혀있다.



## JdbcTemplate

템플릿 메소드 패턴을 내부적으로 사용한다.

JDBC API에서 본 반복 코드를 대부분 제거해준다. 하지만 SQL은 직접 작성해야 한다.

DataSource 빈을 주입받는다.

> MyBatis 라이브러리와 비슷하다.



## JPA

- 기존의 반복 코드는 물론이고, 기본적인 SQL도 직접 만들어서 실행해준다.
- 리포지토리에 구현 클래스 없이 인터페이스 만으로 개발을 완료할 수 있다.
- SQL과 데이터 중심의 설계에서 객체 중심의 설계로 패러다임을 전환을 할 수 있다.
- 개발 생산성을 크게 높일 수 있다.
- Spring에서의 관계형 데이터베이스는 JPA를 기본으로 한다고 보면 된다.
- JPA란 것은 자바의 표준 인터페이스이고 구현체로 hibernate, eclipse 등의 벤더들이 있다 (주로 hibernate를 쓴다)



application.properties 파일에서 spring.jpa.hibernate.ddl-auto = none (= create) 설정은 객체를 보고 테이블을 자동으로 생성해줄지를 선택한다.

@Entity 어노테이션으로 (javax.persistence.Entity) JPA가 관리하는 entity가 된다.

키가 될 필드를 @Id로 지정해주고 자바 코드로 직접 키값을 할당해주는 방식(직접 할당) 외에도 3가지 자동 생성 방식이 있다.

자동 생성 방식에는 IDENTITY, SEQUENCE, TABLE 방식이 있고, @GeneratedValue(strategy=GenerationType.IDENTITY) 와 같은 형식으로 사용한다.

- IDENTITY : 기본 키 생성을 데이터베이스에 위임한다.
- SEQUENCE : 데이터베이스 시퀀스를 사용해서 기본 키를 할당한다.
- TABLE : 키 생성 테이블을 사용한다.

@Column 어노테이션으로 DB의 column이랑 Mapping 할 수 있다. (@Column(name = 컬럼명))

SpringBoot가 property에서 세팅한 정보랑 DB 커넥션 정보 등등을 종합한 EntityManager를 만들어준다. JPA를 쓰려면 이 EntityManager를 주입받아야 한다.

EntityManger는 원래 사양에 적혀있길 @PersistenceContext 어노테이션을 em에 달아주어야 하지만, 생성자 주입 (@Autowired)만으로도 Spring에서 알아서 주입해준다.

항상 @Transactional 어노테이션이 필요하다.

트래픽이 큰 실무 현장에서도 잘 활용되고 있다.

복잡한 동적 쿼리는 Querydsl 이라는 라이브러리를 사용하면 된다.

Querydsl을 사용하면 쿼리도 자바 코드로 안전하게 작성할 수 있고, 동적 쿼리도 편리하게 작성할 수 있다.

JPA와 심지어 Querydsl로도 해결하기 어려운 쿼리는 JPA가 제공하는 네이티브 쿼리를 사용하거나, JdbcTemplate을 사용하면 된다.



>  pk기반으로 쿼리할 때는 쿼리문을 작성할 필요도 없다.
>
> 그 외에는 jpql이라는 객체지향 쿼리언어를 통해 테이블 대신 객체 자체를 대상으로 쿼리를 날린다.(내부적으로 sql로 번역된다)
>
> JPA 기술을 Spring으로 한번 감싸서 제공해주는 기술(SpringDataJPA)을 통해 pk 기반이 아닌 경우에도 쿼리문을 작성하지 않는다!
>
> 다시말해, 기본적인 CRUD는 제공되어진다. (인터페이스 이름을 수정해주기만 하면 된다!)
>
> (내부적으로 메소드 이름을 리플렉션으로 읽어들여서 동작한다)
>
> 또한 페이징 기능도 자동으로 제공한다.
>
> SpringDataJPA는 import org.springfraemework.data.jpa.repository.JpaRepository 를 통해 인터페이스 대한 구현체를 스스로 만들어준다. 그리고 구현체를 자동으로 빈으로 등록해준다.



## AOP

AOP란 Aspect Oriendted Programming의 약자이다.

공통 관심 사항(cross-cutting concern)과 핵심관심 사항(core concern)을 분리한다.

@Aspect 어노테이션을 달아줌으로서 AOP클래스를 만들어준다. 

빈 등록은 역시나 @Component로 컴포넌트 스캔하거나 @Configuration 에서 @Bean으로 등록해준다.

@Around 어노테이션으로 적용할 대상을 설정할 수 있다. (주로 패키지 레벨별로 설정한다.)

실제 구현체 대신 가짜 구현체 (프록시)를 의존 관계 사이에 만들어 줌으로써 AOP를 구현한다.



## Reference

이 글은 김영한님의 [스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/dashboard)을 보고 정리하였습니다.

