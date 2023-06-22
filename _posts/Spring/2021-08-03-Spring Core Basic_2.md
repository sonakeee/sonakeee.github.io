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

title: "Spring Core Basic_2"
excerpt: "스프링 핵심 기본_2"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Spring
tags:
  - [Spring, 김영한, 스프링 핵심 기본]
date: 2021-08-03
last_modified_at: 2021-08-08
---

## 스프링 컨테이너

- ApplicationContext, BeanFactory 를 스프링 컨테이너라 한다.
  - BeanFactory를 직접 사용하는 경우는 거의 없으므로 일반적으로 ApplicationContext를 스프링 컨테이너라고 한다.
- ApplicationContext는 인터페이스이다. [(BeanFactory 인터페이스를 상속받는다)](https://jhhan009.tistory.com/71)
- BeanFactory는 스프링 컨테이너의 최상위 인터페이스다.

  - getBean()을 비롯한 많은 기능(빈 관리 기능)을 BeanFactory가 제공한다.
- ApplicationContext가 BeanFactory가 제공하는 기능 이외에 추가적으로 제공하는 기능들은 다음과 같다.

  - MessageSource 를 활용한 국제화 기능
  - 로컬 개발 환경, 테스트 서버 환경(개발 환경), 실제 프로덕션에 나가는 운영 환경, 스테이지 환경(운영과 가장 비슷한 환경) 별로 연결해야하는 데이터베이스가 다르다. 그러한 환경 변수와 관련된 정보를 처리해주는 기능
  - 애플리케이션 내 이벤트를 발행하고 구독하는 모델을 편리하게 지원해주는 기능
  - 파일, 클래스패스, 외부 등에서 추상화를 통해 리소스를 편리하게 조회하는 기능
- 스프링 컨테이너는 다양한 형식의 설정 정보를 받아드릴 수 있게 유연하게 설계되어 있다. (자바 코드, XML, Groovy)
  - XML
  
    - 컴파일 없이 Bean 설정 정보를 변경할 수 있다.
    - 빈과 의존관계 등을 일일히 설정하는 것이 상당히 번거롭다.
    - 코드를 실행해야 설정 정보 오류를 확인할 수 있다.
    - 스프링 부트 도입 이후 잘 사용하지 않는다.
    - [링크1](https://atoz-develop.tistory.com/entry/Spring-%EC%8A%A4%ED%94%84%EB%A7%81-XML-%EC%84%A4%EC%A0%95-%ED%8C%8C%EC%9D%BC-%EC%9E%91%EC%84%B1-%EB%B0%A9%EB%B2%95-%EC%A0%95%EB%A6%AC) , [링크2](https://ktrlnr.tistory.com/entry/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88Spring-Container%EB%A5%BC-%EB%A7%8C%EB%93%9C%EB%8A%94-2%EA%B0%80%EC%A7%80-%EB%B0%A9%EB%B2%95-XML-Annotation)
  
  - Annotation 방식 (자바 코드로 설정)
  
    - 설정 정보를 변경하려면 컴파일이 필요하다.
    - 컴파일러의 도움을 받기 때문에, 오타 등의 설정 정보 오류를 미리 알 수 있다.
  
- 스프링은 BeanDefinition이라는 추상화를 통해 다양한 설정 형식을 지원한다.
- 자바코드(Annotation 방식)이든, Xml이든, Groovy이든 스프링 컨테이너는 BeanDefinition 인터페이스(역할)을 통해 정보를 받아들인다.
- BeanDefinion을 빈 설정 메타정보라고 한다.
  - @Bean, <bean>당 각각 하나씩 메타정보가 생성된다.
  - BeanDefinition 생성
    - XML 은 GenericXmlApplicationContext의 XmlBeanDefinitionReader를 통해 appConfig.xml 설정 정보를 읽고 BeanDefinition을 생성한다.
    - 자바코드(Annotation) 방식은 AnnotationConfigApplicationContext의 AnnotatedBeanDefinionReader를 통해 appConfig.class 를 읽고 BeanDefiniton을 생성한다.
  - BeanDefinition 정보
    - BeanClassName: 생성할 빈의 클래스 명( 자바 설정처럼 팩토리 역할의 빈을 사용하면 없음(null) )
      - xml 경우에는 hello.core.member.MemberServiceImpl 와 같이 클래스 명이 구체적으로 들어있다.
      
    - factoryBeanName: 팩토리 역할의 빈을 사용할 경우(자바 코드(Annotation)방식으로 등록하는 경우) ex) appConfig

    - factoryMethodName: 빈을 생성할 팩토리 메서드 ex)memberService, memberRepository, orderService, discountPolicy

    - Scope: 빈 스코프란 빈이 사용되는(존재할 수 있는) 범위를 말한다. 

       @Scope 어노테이션 (ex : @Scope(value = "prototype") )을 사용해서 빈 스코프를 정의해 줄 수 있다.

      xml에서는 <bean ... scope ="prototype"> 이런 식으로 정의한다.

      [(참고 링크1)](https://juns-lee.tistory.com/entry/SpringFramework%EC%97%90%EC%84%9C%EC%9D%98-%EC%8B%B1%EA%B8%80%ED%86%A4-%EC%A0%84%EB%9E%B5) [(참고 링크2)](https://velog.io/@probsno/Bean-%EC%8A%A4%EC%BD%94%ED%94%84%EB%9E%80) [(참고 링크3)](https://gmlwjd9405.github.io/2018/11/10/spring-beans.html) [(참고 링크4)](https://mangkyu.tistory.com/117) [(참고 링크5)](https://cantcoding.tistory.com/47)

      - singleton P: 기본 스코프, 스프링 컨테이너의 시작과 죵료까지 유지되는 가장 넓은 범위의 스코프

      - prototype:

        - 빈의 생성과 의존관계 주입까지만 관여하고 더는 관리하지 않는 매우 짧은 범위의 스코프
        - 하나의 빈 정의에 대해서 다수의 객체가 존재할 수 있음
        - 요청이 오면 항상 새로운 인스턴스를 생성하여 반환하고 이후에 관리하지 않음
        - 빈을 받은 클라이언트가 빈을 관리해야 함

      - request: 일반 Spring 애플리케이션이 아닌 Spring MVC Application 에서만 제공

        - Web-aware Spirng ApplicaitonContext 안에서만 유효
        - 각각의 HTTP 요청별로 인스턴스화 되며 요청이 끝날 때까지 유지되는 스코프
        - 각각의 HTTP 요청마다 별도의 빈 인스턴스가 생성되고 관리됨.

      - session: 일반 Spring 애플리케이션이 아닌 Spring MVC Application 에서만 제공

        - Web-aware Spirng ApplicaitonContext 안에서만 유효
        - 하나의 Bean 정의에 대해서 하나의 HTTP Session의 생명 주기 안에 단 하나의 객체만 존재
        - HTTP Session 과 동일한 생명주기를 가지는 스코프
        - HTTP Session 별로 인스턴스화 되고 세션이 끝나면 소멸됨

      - global session: 일반 Spring 애플리케이션이 아닌 Spring MVC Application 에서만 제공

        - Web-aware Spirng ApplicaitonContext 안에서만 유효
        - 하나의 Bean 정의에 대해서 하나의 global HTTP Session의 생명주기 안에 단 하나의 객체만 존재
        - 일반적으로 portlet context 안에서 유효
        - [포틀릿](http://daplus.net/java-%ED%8F%AC%ED%8B%80%EB%A6%BF%EA%B3%BC-%EC%84%9C%EB%B8%94%EB%A6%BF%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9E%85%EB%8B%88%EA%B9%8C/) 스펙은 하나의 포틀릿 웹 응용 프로그램을 구성하는 모든 포틀릿들 사이에서 공유되는 글로벌 세션의 개념을 정의한다. 

      - application: 일반 Spring 애플리케이션이 아닌 Spring MVC Application 에서만 제공

        - Web-aware Spirng ApplicaitonContext 안에서만 유효
        - 웹의 서블릿 컨텍스트(ServeletContext)와 같은 범위로 유지되는(동일한 생명주기를 가지는) 스코프

      - websocket: 일반 Spring 애플리케이션이 아닌 Spring MVC Application 에서만 제공

        - Web-aware Spirng ApplicaitonContext 안에서만 유효
        - 웹 소켓과 동일한 생명주기를 가지는 스코프

      - thread:

        - 새 스레드에서 요청하면 새로운 인스턴스 생성
        - 같은 스레드의 요청에는 항상 같은 인스턴스가 반환됨

      - custom:

        - org.springframewrok.beans.factory.confgi.Scope 를 구현하여 커스텀 스코프를 스프링 설정에 등록하여 사용

        

    - lazyInit: 스프링 컨테이너를 생성할 때 빈을 생성하는 것이 아니라, 실제 빈을 사용할 때까지 최대한 생성을 지연처리 하는지 여부

    - InitMethodName: 빈을 생성하고, 의존관계를 적용한 뒤에 호출되는 초기화 메서드 명

    - DestoryMethodName: 빈의 생명주기가 끝나서 제거하기 직전에 호출되는 메서드 명

    - Constructor arguments, Properties: 의존관계 주입에서 사용한다. (자바 설정처럼 팩토리 역할의 빈을 사용하면 없음)

### 스프링 컨테이너의 생성 과정

1. 스프링 컨네이터 생성

   - 스프링 컨테이너 안에는 빈 이름을 KEY 값으로, 빈 객체를 VALUE로 가지는 스프링 빈 저장소가 있다.

   - 스프링 컨테이너를 생성할 때는 구성 정보(XML 파일이나 Configuration 어노테이션을 붙인 자바 클래스)를 넘겨주어야 한다.

2. 스프링 빈 등록

   - 빈 저장소에 빈을 등록해준다.
     - @Configuration 자바 클래스의 경우, @Bean을 붙인 메소드 이름이 빈 이름이 되고 반환 객체가 빈 객체가 된다.
     - 빈 이름을 직접 부여할 수도 있다. ( @Bean(name = "beanName") )
     - 빈 이름은 중복되어서는 안된다. 다른 빈이 무시되거나, 기존 빈을 덮어버려서 설정에 따라 오류가 발생한다.

3. 스프링 빈 의존관계 설정 - 준비

4. 스프링 빈 의존관계 설정 - 완료

   - 설정 정보를 참고해서 의존관계를 주입(DI)한다.

> 스프링 빈을 생성하고, 의존관계를 주입하는 단계가 나누어져 있다.
>
> 하지만 이렇게 자바 코드로 빈을 등록하면 생성자 호출과정에서 의존관계 주입도 한번에 처리된다.

### 스프링 빈 등록 확인

```java
AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
String[] beanDefinitionNames = ac.getBeanDefinitionNames();
for (String beanDefinitionName : beanDefinitionNames) {
	BeanDefinition beanDefinition = ac.getBeanDefinition(beanDefinitionName);

	if (beanDefinition.getRole() == BeanDefinition.ROLE_APPLICATION) {
        Object bean = ac.getBean(beanDefinitionName);
        System.out.println("name = " + beanDefinitionName + " object = " + bean);
    }
}
```

- BeanDefinition.ROLE_APPLICATION : 직접 등록한 애플리케이션 빈
- BeanDefinition.ROLE_INFRASTRUCTURE: 스프링이 내부에서 사용하는 빈

`ac.getBeanDefinitionNames ()`: 스프링에 등록된 모든 빈 이름을 조회한다.

`ac.getBean()`: 빈 이름으로 빈 객체(인스턴스)를 조회한다.

 - `ac.getBean(빈이름, 타입)`
 - `ac.getBean(타입)`

두 가지 방법으로 조회가 가능하다.

`ac.getBeansOfType(SomeType.class)` 을 사용하면 해당 타입의 모든 빈을 조회한다.

부모 타입으로 조회하면, 자식 타입을 포함하여 조회한다.



## 싱글톤 컨테이너

- 매 요청마다 객체를 새로 생성하면 메모리 낭비가 심하다.
- 객체를 1개만 생성해서 공유하면 해결할 수 있다. (싱글톤 패턴)



### 싱글톤 패턴 문제점

- 구현 코드량이 많다
- 구체 클래스에 의존해서 DIP,  OCP를 위반한다. (구체클래스. getInstance() 이런 식으로) [(참고 링크)](https://juns-lee.tistory.com/entry/SpringFramework%EC%97%90%EC%84%9C%EC%9D%98-%EC%8B%B1%EA%B8%80%ED%86%A4-%EC%A0%84%EB%9E%B5)
- 테스트가 어렵다. [(참고 링크)](https://punsoo.github.io/effective%20java/Effective-Java-Item-03/)
- 내부 속성을 변경하거나 초기화 하기 어렵다
- private 생성자로 자식 클래스를 만들기 어렵다.
- 유연성이 떨어진다.
- 안티패턴이다.



### 스프링 컨테이너의 싱글톤 (싱글톤 컨테이너)

스프링 컨테이너는 싱글톤 패턴의 문제점은 해결하면서 장점은 유지한다.

- 지저분한 코드가 필요없다
- DIP,OCP를 위반하지 않으고 테스트가 용이하며 private 생성자로부터 자유롭다.

싱글톤 객체를 생성하고 관리하는 기능(싱글톤 레지스트리)를 가지고 있다.



### 싱글톤 방식의 주의점

- 싱글톤 방식은 여러 클라이언트에 공유되기 때문에 상태를 유지(stateful)하게 설계하면 안된다.
- 무상태(stateless)로 설계해야 한다.
  - 특정 클라이언트에 의존적인 필드가 있으면 안된다.
  - 특정 클라이언트가 값을 변경할 수 있는 필드가 있으면 안된다.
  - 가급적 읽기만 가능해야 한다.
  - 필드 대신에 자바에서 굥유되지 않는 지역변수, 파라미터, ThreadLocal 등을 사용해야 한다.
- 스프링 빈의 필드에 공유 값을 설정하면 정말 큰 장애가 발생할 수 있다.



### @Configuration

@Configuration은  스프링 컨테이너가 싱글톤 레지스트이도록 만들어준다.

- @Configuration 어노테이션을 붙이면, 스프링이 CGLIB이라는 바이트코드 조작 라이브러리를 사용해서 AppConfig 를 상속받은 임의의 다른 클래스를 빈으로 등록한다.
- 그 임의의 다른 클래스가 싱글톤이 보장되도록 해준다.
- @Configuration이 없으면, 매번 새로 인스턴스가 생성되고 (new MemoryMemberREpository()), 이 인스턴스들은 스프링 컨테이너로 관리되는 빈도 아니다.
- `@Autowired MemberRepository memberRepository;` 이런식으로 @Autowired를 사용하여 의존관계 주입을 구현할 수도 있다.
- 스프링 설정 정보는 항상 `@Configuration`을 사용하자.



## 컴포넌트 스캔

@Bean이나 XML의 <Bean> 으로 빈을 등록하다보면, 빈의 숫자가 많아질수록 설정 정보가 커진다.

그리고 그에 따라 누락하는 문제와 반복이 발생한다.

그래서 스프링은 설정 정보가 없어도 자동으로 스프링 빈을 등록하는 컴포넌트 스캔이라는 기능을 제공한다.

또 의존관계도 자동으로 주입하는 `@Autowired` 라는 기능도 제공한다.

컴포넌트 스캔을 사용하려면 @ComponentScan을 @Configuration과 함께 설정 정보에 붙여주면 된다.

그러면 @Component 애노테이션이 붙은 클래스들을 스캔해서 빈으로 등록한다.

> excludeFilters를 이용해서 컴포넌트 스캔 대상에서 제외할 수 있다.
>
> ```java
> @ComponentScan(excludeFilter = @Filter(type = FilterType.ANNOTATION, classes= Configuration.class))
> ```

@Bean으로 직접 설정 정보를 작성했을 때는, 의존 관계가 메소드 안에서 명시되었지만,

@ComponentScan의 경우에는 @Autowired로 의존관계를 자동으로 주입해준다. (생성자나 Setter, 필드에 붙인다. 주로 생성자에!)

컴포넌트 스캔을 할 때, @Component가 붙은 클래스의 클래스명을 기본 빈의 이름으로 사용하되, 맨 앞글자만 소문자를 사용한다.

- 기본 빈 이름 : RateDiscountPolicy -> rateDiscountPolicy
- 빈 이름 직접 지정: @Component("rateDiscountPolicy2") 이런 식으로 직접 이름을 부여할 수도 있다.



### 탐색 위치와 기본 스캔 대상

```java
@ComponentScan(
    basePackages = "hello.core.member",
    basePackageClasses = AutoAppConfig.class,
    excludeFilter = @Filter(type = FilterType.ANNOTATION, classes= Configuration.class)
)
```

- basePackages : 탐색할 패키지의 시작 위치를 지정한다. 이 패키지를 포함해서 하위 패키지를 모두 탐색한다.

  - ```java
    basePackages ={"hello.core", "hello.service"}
    ```

    이렇게 여러 패키지의 시작위치를 지정할 수도 있다.

- basePackageClasses: 지정한 클래스의 패키지를 탐색 시작 위치로 지정한다.

- 아무것도 지정하지 않으면 @ComponentScan이 붙은 설정 정보 클래스의 패키지가 시작 위치가 된다.

  - 이런 성질을 활용하여, 아무것도 지정하지 않고 정 정보 클래스의 위치를 프로젝트 최상단에 두는 것이 권장된다.
  - 프로젝트 메인 설정 정보는 프로젝트를 대표하는 정보이기 때문이라도 프로젝트 시작 루트 위치에 두는 것이 좋다고 여겨진다.
  - 스프링 부트의 대표 시작 정보인 @SpringBootApplication를 이 프로젝트 시작 루트 위치에 두는 것이 관례이다.
  - @SpringBootApplication 안에 @ComponentScan이 들어있다.

- 컴포넌트 스캔은 @Component 뿐만 아니라 다음 어노테이션들에도 포함되어있다. (해당 클래스의 소스 코드를 살펴보자)

  - @Controller: 스프링 MVC 컨트롤러로 인식
  - @Service:
    - 스프링 비즈니스 로직에서 사용하는데 사실 @Service는 특별한 처리를 하지 않는다.
    - 대신 개발자들이 핵심 비즈니스 로직이 여기에 있겠구나 라고 비즈니스 계층을 인식하는데 도움이 된다.
  - @Repository: 스프링 데이터 접근 계층으로 인식하고, 데이터 계층의 예외를 추상화하여 스프링 예외로 변환해준다.
  - @Configuration: 스프링 설정 정보로 인식하고, 스프링 빈이 싱글톤을 유지하도록 추가 처리를 한다.

- 애노테이션에는 상속관계라는 것이 사실 없다. 이렇게 애노테이션이 특정 애노테이션을 들고 있는 것을 인식할 수 있는 것은 자바 언어가 지원하는 기능이 아니고, 스프링이 지원하는 기능이다.

### 필터

```java
package hello.core.scan.filter;

import java.lang.annotation.*;

@Target(ElementType.TYPE) //ElementType.TYPE: 클래스 레벨에 붙는다는 뜻
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface MyExcludeComponent {
}
```

이런 식으로 애노테이션을 만들어 줄 수 있다.

```java
@ComponentScan(
            includeFilters = @Filter(type = FilterType.ANNOTATION, classes = MyIncludeComponent.class),
            excludeFilters = @Filter(type = FilterType.ANNOTATION, classes = MyExcludeComponent.class)
)
```

이렇게 빈으로 등록할 클래스와 등록하지 않을 클래스를 지정할 수 있다.

### 필터 옵션 (FilterType의 5가지 옵션)

- ANNOTATION: 기본값, 애노테이션을 인식해서 동작
- ASSIGNABLE_TYPE: 지정한 타입과 자식 타입을 인식해서 동작한다.
- ASPECTJ: AspectJ 패턴 사용
- REGEX: 정규 표현식
- CUSTOM: TypeFilter 라는 인터페이스를 구현해서 처리

> @Component로 충분해서 includeFilters는 거의 사용하지 않는다.
>
> excludeFilters는 가끔 사용한다.
>
> 최근 스프링 부트는 컴포넌트 스캔을 기본으로 제공하기에, 옵션을 변경하여서 사용하기보다 스프링의 기본 설정에 최대한 맞추어 사용하는 것을 권장한다.



### 중복 등록과 충돌

- 자동 빈 등록 vs 자동 빈 등록 충돌은 에러가 난다.

  - ```java
    예를들어 @Component("service") 를 두개의 클래스에 붙여줬을 때
    ```

- 수동 빈 등록 vs 자동 빈 등록 충돌은 수동 빈 등록이 우선권을 가진다.(수동 빈이 자동 빈을 오버라이딩 해버린다.)

  - ```java
    @Bean(name = "memoryMemberService") 로 등록하여 자동 빈 등록과 겹칠 때
    ```

  - 이 때 다음과 같은 로그를 남겨준다.

    - ```text
      Overriding bean definition for bean '클래스이름' with a different definition: replacing
      ```

  - 테스트 코드를 통해 Spring을 돌리면 이와 같이 되지만 스프링 부트에서는 오류를 낸다.

  - 위와 같은 결과는(오버라이딩 하는 결과는) 개발자가 의도하지 않고 발생할 수도 있고, 이럴 경우 정말 잡기 어려운 버그가 된다.

  - 그래서 최근 스프링부트는 오류가 나도록 기본 설정 값을 바꾸어 주었다.

    - ```text
      Consider renaming one of the beans or enabling overriding by setting spring.main.allow-bean-definition-overriding=true
      ```

  - 스프링 부트에서도 오버라이딩 하게 바꾸고 싶다면, resources/application.properties에 다음과 같은 설정을 적어준다.

    - ```text
      spring.main.allow-bean-definiton-verriding=true
      ```

  - 개발할 때는 명확하지 않은 것은(애매한 것은) 최대한 피하자! 개발은 혼자 하는 것이 아니다.

    어설픈 추상화는 잡기 어려운 버그가 된다.



- Reference

이 글은 김영한님의 [스프링 핵심 원리 - 기본편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8/dashboard)을 보고 정리해서 작성하였습니다.

[1] [스프링 코어(3) - 스프링 컨테이너 & 빈](https://jhhan009.tistory.com/71)

[2] [[Spring] 스프링 XML 설정 파일 작성 방법 정리](https://atoz-develop.tistory.com/entry/Spring-%EC%8A%A4%ED%94%84%EB%A7%81-XML-%EC%84%A4%EC%A0%95-%ED%8C%8C%EC%9D%BC-%EC%9E%91%EC%84%B1-%EB%B0%A9%EB%B2%95-%EC%A0%95%EB%A6%AC)

[3] [스프링 컨테이너(Spring Container)를 만드는 2가지 방법 (XML, Annotation) 출처](https://ktrlnr.tistory.com/entry/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88Spring-Container%EB%A5%BC-%EB%A7%8C%EB%93%9C%EB%8A%94-2%EA%B0%80%EC%A7%80-%EB%B0%A9%EB%B2%95-XML-Annotation)

[4] [SpringFramework에서의 싱글톤 전략](https://juns-lee.tistory.com/entry/SpringFramework%EC%97%90%EC%84%9C%EC%9D%98-%EC%8B%B1%EA%B8%80%ED%86%A4-%EC%A0%84%EB%9E%B5)

[5] [[Spring] Spring Bean의 개념과 Bean Scope 종류](https://gmlwjd9405.github.io/2018/11/10/spring-beans.html)

[6] [[Spring] Bean Scope(빈 스코프)의 종류](https://mangkyu.tistory.com/117)

[7] [Bean 스코프란?](https://velog.io/@probsno/Bean-%EC%8A%A4%EC%BD%94%ED%94%84%EB%9E%80)

[8] [스프링 Bean,Singleton,Bean Scope](https://cantcoding.tistory.com/47)

[9] [[java] 포틀릿과 서블릿의 차이점은 무엇입니까?](http://daplus.net/java-%ED%8F%AC%ED%8B%80%EB%A6%BF%EA%B3%BC-%EC%84%9C%EB%B8%94%EB%A6%BF%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9E%85%EB%8B%88%EA%B9%8C/)

[10] [[[Spring\] Spring Bean Scope 종류](https://charming-jung.tistory.com/54)](https://charming-jung.tistory.com/54)
