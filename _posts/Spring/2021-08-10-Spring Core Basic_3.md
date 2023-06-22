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

title: "Spring Core Basic_3"
excerpt: "스프링 핵심 기본_3"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Spring
tags:
  - [Spring, 김영한, 스프링 핵심 기본]
date: 2021-08-10
last_modified_at: 2021-08-10

---

## 다양한 의존관계 주입 방법

[스프링 입문편](https://punsoo.github.io/spring/Introduction-To-Spring/#%EC%8A%A4%ED%94%84%EB%A7%81-bean-%EB%93%B1%EB%A1%9D-%EB%B0%A9%EB%B2%95) 에서 한번 다뤘던 주제이다.

### 생성자 주입

- 컴포넌트 스캔으로 클래스가 빈으로 등록이 될 때 생성자가 호출된다.

- 이 때 생성자에 @Autowired가 있으면, 스프링이 연관된 객체를 스프링 컨테이너에서 찾아서 넣어준다.

- 그러니까 Bean 등록이 되기 전, Bean이 생성될 때 의존 관계에 있는 Bean들이 주입된다.

- 생성자가 1개만 있으면 @Autowired는 생략할 수 있다.

- 불변 의존관계를 만든다.
  - 따로 setter 메서드를 만들거나 외부접근을 허용하지 않는한 불변 의존관계이다.
  
  - 수정자/필드 주입과 다르게 해당 필드를 final로 선언할 수 있다. (final은 객체가 생성된 뒤에 호출되면 안된다.)
  
    생성자에서 혹시라도 값이 설정되지 않는 오류(누락)를 컴파일 시점에 막아준다. (컴파일 오류는 바로 잡기 아주 편하다)
  
  - 대부분의 경우 의존관계를 변경할 일이 없고 변경되면 안되는 경우도 많다.
  
  - 생성자 호출시점에(객체를 생성할 때) 딱 1번만 호출되는 것이 보장된다.(불변)
  
- 생성자에 명시해줌으로써 필수 의존관계라고 할 수 있다.  (생성자는 무조건 불러야함으로)

- private 선언한 필드는 왠만하면 세팅을 해주어야 한다는 뜻이다.

- 왠만하면 생성자에는 NULL을 허용하지 않는다 - (문서에 따로 적혀있지 않는한!)

- 독릭적으로 인스턴스화가 가능한 POJO(Plain Old Java Object)를 구현한다. ([참고1](https://madplay.github.io/post/why-constructor-injection-is-better-than-field-injection)) ([참고2](https://coding-start.tistory.com/250))

- 생정자 주입 방식은 프레임 워크에 의존하지 않고, 순수한 자바 언어의 특징을 잘 살리는 방법이다.



### 수정자(Setter) 주입

- setter 메소드에 @Autowired가 있으면, 스프링이 연관된 객체를 스프링 컨테이너에서 찾아서 넣어준다.

- Bean 등록이 되고(Bean 생성이 되고) 의존 관계에 있는 Bean들을 주입한다.

- setter 메소드는 public으로 외부 접근을 허용하기 때문에 언제든 변경되게 할 위험이 있다.
  - (의문?) private 으로 설정하면 변경에 닫혀있는 것인가? (아니면 그런 경우는 애시당초 setter메소드 의미가 없나?)
  
-  @Autowired 붙어있음에도 주입해야할 Bean이 Spring에 등록이 되어있지 않을 수도 있는데,

  이렇게 주입할 대상이 없는 경우에는 오류가 발생한다.

- @Autowired(required=false) 를 통해 주입할 대상이 없어도 동작하게 할 수 있다. 즉 수정자 주입은 **선택적** 이다.

  다시 말해 주입해주는 빈에 대해 필수가 아니라고 선언하는 것이다.(있어도 되고 없어도 된다.)

- 수정자 주입의 일반적인 방법이 일반 메소드 주입이다.

- 일반 메소드 주입이란 아무 메소드에나 @Autowired를 붙여서 의존관계 주입을 하는 것이다.

> 자바빈 프로퍼티
>
> 자바에서는 과거부터 필드의 값을 직접 변경하지 않고, setter, getter 메소드를 통해서 값을 읽거나 수정하는 규칙을 만들었는데
>
> 이것이 자바빈 프로퍼티 규약이다.



### 필드(Field) 주입

- field에 @Autowired가 있으면, 스프링이 연관된 객체를 스프링 컨테이너에서 찾아서 넣어준다.

- Bean 등록이 되고(Bean 생성이 되고) 의존 관계에 있는 Bean들을 주입한다. ([필드 주입은 생성자 수행이 완료되고 호출된다](https://namocom.tistory.com/772))

- 안티패턴이다. (사용하지 말자!)

  - 외부에서 변경할 수가 없어서, 스프링 없이는 (DI 프레임 워크 없이는) 테스트가 매우 불편하다.
  - 임의의 mock 객체로 바꿔서 테스트해주고 싶어도 방법이 없다. (순수한 자바코드로는 불가능)
  - 임시 방편으로 setter메소드를 만들어서 테스트를 해줄 수 있곘지만, 그럴 바엔 수정자 주입을 하는 편이 낫다.

- @SpringBootTest (스프링 컨테이너에서 테스트할 수 있게 해주는 곳)와 같이 애플리케이션과 상관없는 테스트 코드나

  스프링 설정을 목적으로 하는 @Configure 클래스 같은 곳에서만 특수한 목적으로만 사용한다. (가급적 쓰지말자!)

> 스프링 컨테이너 관리하는 스프링 빈 클래스 (ex @Component를 붙여준) 내에서 @Autowired 어노테이션이 효과가 있다.
>
> 일반 클래스 내에서는 작동하지 않는다.
>
> 바이트 코드를 조작하는 기능이 자바에 있어서 일반 클래스 내에서도 @Autowired가 작동하게 할 수 있지만 일반적으로 쓰이지 않는다.



> 스프링 레퍼런스에는 강제화되는(필수적인) 의존성의 경우를 비롯해 기본적으로 생성자 주입을 권장한다.  
>
> 합리적인 디폴트를 부여할 수 있고 선택적인 의존성을 사용할 때만(옵션이 필요하면) 수정자 주입을 사용하기를 권장한다 
>
> 그렇지 않으면 의존성을 사용하는 모든 코드에 null 체크를 구현해야 한다.
>
> 필드 주입은 사용하지 않는게 좋다.
>
> ([참고](https://coding-start.tistory.com/250))



### 순환 참조 오류

**수정자/필드 주입**

- 수정자 주입이나 필드 주입은 객체가 메모리에 적재된 후에 빈을 주입하게 되는 것이다. 즉 객체를 이미 생성한 이후에 빈을 주입한다.

- 다시 말해, Bean이 생성되서 Bean Factory 에 등록되고 나서 의존관계의 Bean을 주입한다.

- 런타임에 의존성을 주입하기 떄문에 의존성을 주입하지 않아도 객체가 생성될 수 있다.

- 다시 말해, 참조 변수만 선언하고 Bean 인스턴스가 주입되지 않았으니 정상적으로 초기화가 되는 것이다.

  이것을 참조하는 객체를 선언하지 않았으므로, 혹은 객체를 생성하지 않았으므로 라고 표현하기도 하던데([참고](https://jurogrammer.tistory.com/79)) 

  부정확한 표현이라고 생각이 든다. 두고두고 생각해보고 공부해보자... 

  (그러니까 객체 선언과 빈 생성은 되었고 빈 주입이 안 되었다고 해야지 맞는 표현 아닌가 싶다. )

- 객체 생성 후 비즈니스 로직에서 (메소드 호출시) stackoverflow가 발생하며 에러를 확인하게 된다. 즉 런타임에 에러를 알게 된다.

- 필드/수정자 주입은 생성자가 수행이 완료되고 호출된다. ([참고](https://namocom.tistory.com/772))

**생성자 주입**

- 생성자 주입은 객체를 생성하는 동시에 빈을 주입한다. 
- 그래서 생성자 주입은 컴파일 시점(객체의 생성 시점)에 BeanCreationException 을 발생시킨다. 서로 참조하는 객체가 생성되지 않았는데 그 빈을 참조하기 떄문이다.
- 다르게 표현하면,  생성자 주입에서 객체가 생성이 되려면 주입하려는 Bean이 정상적인 상태여야 한다. (그렇지 않아서 에러 발생)

### Lombok

- @Getter, @Setter를 붙이기만 해도 getter, setter 메소드를 생성해준다.
- @RequiredArgsConstructor 를 붙이기만 해도 final 필드들에 대해서 생성자를 자동으로 만들어준다.

### 옵션 처리

주입할 스프링 빈이 없어도 (스프링에 등록이 안되어있어도) 동작해야할 때가 있다.

@Autowired 의 required 옵션이 디폴트로 true이기에, 자동 주입 대상이 없으면 오류가 발생한다.

- @Autowired(required=false): 자동 주입할 대상이 없으면 수정자 메서드 자체가 호출이 되지 않는다.

  ```java
  @Autowired(required = false)
  public void setNoBean1(Member member) {
  System.out.println("setNoBean1 = " + member);
  }
  ```

- org.springframework.lang.@Nullable: 자동 주입할 대상이 없으면 null이 입력된다.

  ```java
  @Autowired
  public void setNoBean2(@Nullable Member member) {
  System.out.println("setNoBean2 = " + member);
  }
  ```

- Optional<>: 자동 주입할 대상이 없으면 Optional.empty가 입력된다.

  ```java
  @Autowired(required = false)
  public void setNoBean3(Optional<Member> member) {
  System.out.println("setNoBean3 = " + member);
  }
  ```



### 롬복과 최신 트렌드

- @Getter, @Setter를 붙이기만 해도 getter, setter 메소드를 생성해준다.

- @RequiredArgsConstructor 를 붙이기만 해도 final 필드들에 대해서 생성자를 자동으로 만들어준다.

- build.gradle에 다음과 같이 lombok 설정을 추가해준다.

  ```tex
  configurations {
      compileOnly {
      	extendsFrom annotationProcessor
      }
  }
  ```

- lombok plugin 을 설치한다.
- Preferences에서 Annotation Processors를 검색한 후, Enable annotation processing을 체크한다.

### 빈 조회 결과가 여러 개일 때

- @Autowired는 Type으로 조회하기 때문에, 여러 개의 하위 타입 빈이 조회될 수 있다.
- 그렇다고 구체적인 구현 객체 클래스 타입(하위 타입)으로 지정하는 것은 DIP를 위반하고 유연성이 떨어진다.
- 게다가 이름만 다르고 완전히 똑같은 타입의 스프링 빈이 2개 있을 때도 해결이 안된다.
- 다음과 같이 3가지 방법으로 해결할 수 있다.

1. @Autowired 필드명(파라미터명) 매칭을 사용할 수 있다.

   먼저 타입 매칭을 시도하고, 여러 빈이 있을 때 추가로 필드 명, 파라미터 명으로 빈 이름을 매칭한다.

2. @Qulifier 추가 구분자를 붙여준다

   - @Component나 @Bean으로 빈 등록시 @Qualifier를 붙여준다.
   - 주입시에도 @Qualifier를 붙여주고 등록한 이름을 적어준다.
   - 만약 등록한 이름(ex @Qualifier("mainDiscountPolicy"))를 찾지 못하면, 빈 이름으로 추가로 찾는다.
   - 하지만 @Qualifier는 @Qualifer를 찾는 용도로만 사용하는 게 명확하고 좋다.
   - @Qulifer끼리 매칭 - 빈 이름으로 매칭 - NoSuchBeanDefinitionException 발생

3. @Primary 를 사용해준다.

   - 여러 빈이 매칭되면 @Primary가 우선권을 가진다.
   - @Qualifier가 @Primary보다 우선권이 높다. (자동보다는 수동이, 넓은 범위보다는 좁은 범위가, 상세할수록 우선권이 높다)
   - 메인 데이터베이스의 커넥션을 획득하는 빈은 @Primary를, 서브 데이터베이스의 커넥션을 획득하는 빈은 @Qualifier를 지정해서 명시적으로 획득 하는 방식으로 사용하면 코드를 깔끔하게 유지할 수 있다.

- 조회한 빈이 모두 필요할 때 Map, List 와 같은 컬렉션 프레임워크를 사용해주면 된다.

  ```java
  private final Map<String, DiscountPolicy> policyMap;
  private final List<DiscountPolicy> policies;
  ```

  이런 식으로 필드를 선언해 주입받으면 된다. (물론 이왕이면 생성자 주입으로!)

### Annotation 직접 만들기

- @Qualifier("mainDiscountPolicy") 이렇게 직접 문자로 적으면 컴파일시 타입 체크가 안된다.
- 직접 어노테이션을 만들면 타입 체크는 물론, Intellij에서 사용한 부분 코드 추적에도 용이하다.
- 애노테이션에는 상속이라는 개념이 없고, 이렇게 여러 애노테이션을 모아서 사용하는 기능은 스프링에서 지원해주는 기능이다.

```java
@Target({ElementType.FIELD, ElementType.METHOD, ElementType.PARAMETER,
ElementType.TYPE, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Qualifier("mainDiscountPolicy")
public @interface MainDiscountPolicy {
}
```

>  이렇게 여러 애노테이션을 조합해주는데, 나는 @Qualifier 안에 @Target, @Retention, @Documented 가 다 들어있어서 @Qualifier만 적어줘도 잘 동작하지 않을까 생각했었다. 테스트해보니 안된다... 일단 다 붙여줘야 동작하는 걸로 이해하자



### 자동, 수동의 올바른 실무 운영 기준

- 자동을 기본으로 사용하자

  - 스프링은 @Component 뿐 아니라 @Controller, @Service, @Repository 처럼 계층에 맞추어 일반 애플리케이션 로직을 자동으로 스캔할 수 있도로 지원한다.

  - 최근 스프링 부트는 컴포넌트 스캔을 기본으로 사용하고, 스프링 부트의 다양한 빈들도 조건이 맞으면 자동으로 등록하도록 설계하였다.

  - 수동으로 빈을 등록하는 과정은 번거롭고, 빈이 많아질수록 설정 정보를 관리하는 것 자체가 부담이 된다.

  - 자동만으로도 OCP, DIP를 지킬 수 있다.

  - 문제가 발생해도 어떤 곳에서 문제가 발생하였는지 파악하기 쉬운 업무 로직에서 사용하기 좋다.

  - 다형성을 활용할 떄 자동 등록을 사용한다면, 최소한 같은 패키지에 묶어두도록 하자

  - 스프링과 스프링 부트가 자동으로 등록하는 수 많은 빈들 또한 스프링을 잘 이해하고 스프링의 의도대로 (자동으로) 잘 사용하자

    

- 수동은 언제 사용할까?

  - 기술 지원 로직은 업무 로직에 비해 로직 수 자체도 적고, 보통 애플리케이션 전반에 걸쳐서 광범위하게 영향을 미친다.
  - 기술 지원 로직은 수동 빈 등록으로 명확하게 들어내서 디버깅과 적용 여부를 명확하게 알 수 있도록 하자
  - 비즈니스 로직 중에서도 다형성을 적극적으로 활용할 때 (ex Map<String, DiscountPolicy>) 사용해도 좋다.



## 빈 생명주기 콜백





## Reference

이 글은 김영한님의 [스프링 핵심 원리 - 기본편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8/dashboard)을 보고 정리해서 작성하였습니다.

[1] [Spring - Field vs Constructor vs Setter Injection 그리고 순환참조(Circular Reference)](https://coding-start.tistory.com/250)

[2] [[Spring\] 생성자 주입을 사용해야 하는 이유](https://mangkyu.tistory.com/125)

[3] [Spring "Field Injection"? or "Constructor Injection"?](https://interconnection.tistory.com/124)

[4] [[Spring] Field Injection에 의한 순환 참조 오류](https://jurogrammer.tistory.com/79)

[5] [[Spring] Bean 생성시 필드주입 시점은?](https://namocom.tistory.com/772)

