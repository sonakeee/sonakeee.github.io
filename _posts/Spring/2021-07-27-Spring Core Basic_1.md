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

title: "Spring Core Basic_1"
excerpt: "스프링 핵심 기본_1"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Spring
tags:
  - [Spring, 김영한, 스프링 핵심 기본]
date: 2021-07-27
last_modified_at: 2021-07-31
---

## 스프링의 탄생 배경

옛날에는 자바 진영에서 EJB (Enterprise Java Beans) 가 정파의 기술로 여겨졌다.

안정적인 기술로 여겨져서 기술 영업도 잘되었고 오픈 소스는 안정성 문제 때문에 도입이 안되던 시절이었다.

EJB는 트랜잭션, 분산 서버, Entity Bean이라는 ORM 기술을 가지고 있었다. (나름 장점이 많았다)

하지만 비용이 너무 비쌌을 뿐더러 (서버 한대에 수천만원?) 너무 어렵고 복잡했다.

너무 EJB 의존적으로 개발하다보면 코드 또한 지저분해지고 복잡해졌다... 😢😫

무엇보다 객체지향이 가진 좋은 장점을 다 잃어버리게 되었다.

그러다보니 POJO(Plain Old Java Object) 라는 구호가 나오기 시작했는데 오래된 방식의 간단한 자바 오브젝트라는 말이다. 한마디로 순수 과거 자바 스타일로 돌아가자는 뜻이었다. EJB라는 무거운 프레임워크에 종속된 무거운 객체를 만들게 된 것에 개발자들이 반발해서 사용하게 된 용어이다.

결국 Gavin King이라는 사람이 하이버네이트라는 오픈 소스를 만들기 시작했다.

하이버네이트는 EJB 엔티티빈 기술을 대체하였고 JPA(Java Persistence API) 의 새로운 표준을 정의하였다. 

JPA는 표준 인터페이스이고 이것을 구현한 하이버네이트, EclipseLink 등의 벤더들이 있다.

현재 자바 ORM 시장은 거의 JPA가 차지하고 있고 JPA 구현체 중에서도 하이버네이트가 주로 쓰인다.

Rod Johnson이 EJB 를 대체하자는 책을 출간한 이후 Juergen Hoeller(유겐 휠러), Yann Caroff(얀 카로프)가 Rod Johnson에게 오픈 소스프로젝트를 제안하였다.

결국 Rod Johnson 이라는 사람이 EJB 컨테이너를 대체할 스프링 오픈소스를 만들기 시작했다.

스프링의 핵심 코드의 상당수는 Juergen Hoeller가 개발하고 있고 지금도 현재진행형이다.

스프링의 이름은 전통적인 EJB(J2EE)라는 겨울을 넘어 새로운 시작이라는 뜻이다.



## 스프링 버전별 특징

- 2003년 스프링 프레임워크 1.0 출시 - XML (당시 대세)
- 2006년 스프링 프레임워크 2.0 출시 - XML 편의 기능 지원
- 2009년 스프링 프레임워크 3.0 출시 - 자바 코드로 설정(XML 없이)
- 2013년 스프링 프레임워크 4.0 출시 - 자바 8 지원
- 2014년 스프링 부트 1.0 출시 - 설정이 너무 간편해졌고 서버 내장방식을 택함
- 2017년 스프링 프레임워크 5.0 출시, 스프링 부트 2.0 출시 - 리액티브 프로그래밍 지원(비동기 non-blocking 지원 - Node.js처럼)
- 2021년 7월 현재 스프링 프레임워크 5.3.9, 스프링 부트 2.5.3

## 스프링이란?

스프링이란 여러가지 기술의 모음이다.

- 스프링 프레임워크
  - 핵심 기술: 스프링 DI 컨테이너, AOP, 이벤트, 기타
  - 웹 기술: 스프링 MVC, 스프링 WebFlux
  - 데이터 접근 기술: 트랜잭션, JDBC, ORM 지원, XML 지원
  - 기술 통합: 캐시, 이메일, 원격접근, 스케줄링
  - 테스트: 스프링 기반 테스트 지원
  - 언어: 코틀린, 그루비
- 스프링 부트
  - 스프링을 편리하게 사용할 수 있도록 지원, 최근에는 기본
  - 단독으로 실행할 수 있는 스프링 애플리케이션을 쉽게 생성
  - Tomcat 같은 웹 서버를 내장해서 별도의 웹 서버를 설치하지 않아도 됨
  - 손쉬운 빌드 구성을 위한 starter 종속성 제공
  - 스프링과 외부(3rt part) 라이브러리 자동 구성
  - 메트릭, 상태 확인, 외부 구성 같은 프로덕션 준비 기능 제공
  - 관례에 의한 간결한 설정
- 스프링 데이터 (crud를 편리하게 사용할 수 있게 도와주는 것)
- 스프링 세션 (세션 기능을 편하게 사용하게 해주는 것)
- 스프링 시큐리티 (보안과 관련된 것)
- 스프링 Rest Docs (API 문서랑 테스트를 편하게 엮어서 API 문서를 편리하게 해주는 것)
- 스프링 배치 (배치처리에 특화된 기술)
- 스프링 클라우드 (클라우드에 특화된 기술)

스프링이란 단어는 문맥에 따라 다르게 사용된다.

- 스프링 DI 컨테이너 기술 (ex 소스코드 상에서 스프링 컨테이너라고 언급할 때)
- 스프링 프레임워크
- 스프링 부트, 스프링 프레임워크 등을 모두 포함한 스프링 생태계

### 스프링의 진짜 핵심

- 스프링은 객체지향 언어인 자바 기반의 프레임워크
- 스프링은 객체 지향 언어가 가진 강력한 특징을 살려내는 프레임워크
- 스프링은 좋은 객체 지향 애플리케이션을 개발할 수 있게 도와주는 프레임워크



## 객체 지향 프로그래밍

- 객체 지향 프로그래밍은 컴퓨터 프로그램을 명령어의 목록으로 보는 시각에서 벗어나 여러 개의 독립된 단위, 즉 '객체'들의 모임으로 파악하고자 하는 것이다. 각각의 객체는 메시지를 주고받고, 데이터를 처리할 수 있다. (협력)
- 객체 지향 프로그래밍은 프로그램을 유연하고 변경이 용이하게 만들기 때문에 대규모 소프트웨어 개발에 많이 사용된다.

### 다형성의 본질

- 인터페이스를 구현한 객체 인스턴스를 실행 시점에 유연하게 변경할 수 있다.
- 다형성의 본질을 이해하려면 협력이라는 객체사이의 관계에서 시작해야한다.
- 클라이언트를 변경하지 않고, 서버의 구현 기능을 유연하게 변경할 수 있다.
- 스프링에서 이야기하는 IOC(제어의 역전), DI(의존관계 주입)은 다형성을 활용해서 역할과 구현을 편리하게 다룰 수 있도록 지원한다.

### 역할과 구현을 분리

- 실세계의 역할과 구현이라는 편리한 개념을 다형성을 통해 가져올 수 있다.
- 유연하고, 변경이 용이하며 확장 가능한 설계이다.
- 클라이언트에 영향을 주지 않고 변경이 가능하다.
- 인터페이스를 안정적으로 잘 설계하는 것이 중요하다. (인터페이스 자체의 변경은 파급효과가 크다.)

### SOLID 원칙

- SRP 단일 책임 원칙 (Single responsibility principle)
  - 한 클래스는 하나의 책임만 가져야 한다는 뜻인데, 이 기준은 모호하다.
  - 중요한 기준은 변경이다. 변경이 있을 때 파급 효과가 적으면 단일 책임 원칙을 잘 따른 것이다.
  - ex) UI 변경으로 sql코드부터 애플리케이션 코드를 다 고치면 SRP 원칙에 어긋난 것이다.
  - ex) 객체의 생성과 사용을 분리하도록 하자.

- OCP 개방 폐쇄 원칙 (Open Closed Principle)
  - 확장에는 열려 있으나 변경에는 닫혀 있어야 한다.
  - 다형성을 활용한다.
  - 인터페이스를 구현한 새로운 클래스를 만듬으로써 새로운 기능을 추가한다(확장)
  - 객체를 생성하고, 연관관계를 맺어주는 별도의 조립, 설정자가 필요하다.(스프링 컨테이너가 이 역할을 한다!)
- LSP 리스코프 치환 원칙 (Liskov Substitution Principle)
  - 객체는 하위 타입의 인스턴스로 바꾸더라도 프로그램 동작에 문제가 없어야 한다.
  - 다형성을 지원하기 위한 원칙으로, 하위 클래스가 인터페이스 규약을 다 지키도록 하여 구현체를 신뢰할 수 있게 한다.
- ISP 인터페이스 분리원칙 (Interface Segregation Principle)
  - 특정 클라이언트를 위한 인터페이스 여러 개가 범용 인터페이스 하나보다 낫다.
  - 인터페이스의 역할이 명확해지고, 대체하기 쉬워진다.
- DIP 의존관계 역전 원칙 (Dependency Inversion Principle)
  - 구체화(구현 클래스)에 의존하면 안되고, 추상화(인터페이스)에 의존해야 한다.
  - 다시 말해 역할에 의존하게 해야 한다는 뜻이다.
  - 구현체에 의존하게 되면 변경이 아주 어려워진다.

```java
public class MemberService  {
    private MemberRepository memberRepository = new MemoryMemberRepository();
}
```

이와 같은 코드는 MemberService가 인터페이스인 MemberRepository 뿐 아니라 구현체인 MemoryMemberRepository까지 의존하고 있기에 DIP 원칙을 위반한다. 그래서 구현체를 변경하려고 하면 OCP 원칙을 위반하게 된다.

즉, 객체 지향의 핵심이 다향성이지만, 다형성만으로는 OCP와 DIP를 지킬 수 없게 된다.

그래서 스프링에서는 

- DI(Dependency Injection)
- DI 컨테이너 (객체들을 넣어놓고 의존 관계를 서로 연결해주고 주입해주는 기술)

를 제공해준다.

> 이상적으로는  모든 설계에 인터페이스를 부여하는 것이 좋지만, 추상화라는 개발자의 노력 비용이 들어간다.
>
> 즉,  구체 구현 클래스가 무엇인지 코드를 한번더 살펴봐야 한다.
>
> 김영한 선생님은 변경 가능성이 없는 것은 바로 구체 클래스를 직접 사용하고, 향후 꼭 필요할 때 리팩토링을 통해 인터페이스를 도입하는 것을 추천하신다.



## 스프링 예제

### ConcurrentHashMap

HashMap은 멀티스레드 환경에서 동시성을 보장할 수 없고, SynchronizedMap은 속도가 떨어진다.

실무에서는 속도도 보장하면서 동시성을 보장하는 자료구조가 ConcurrentHashMap가 쓰인다.

CocurrentHashMap이 내부적으로 사용하는 방식은 다음과 같다.

- [Compare and Swap](http://tutorials.jenkov.com/java-concurrency/compare-and-swap.html) 방식 (빈 버킷으로의 노드 삽입)
- 부분적인 lock 으로 스레드 경합 최소화 (버킷의 첫번째 노드 기준)
- 버킷 단위로 lock을 사용하여 버킷의 수 == 동시작업 가능한 쓰레드 수이다
  - 여러 쓰레드에서 삽입, 참조하더라도 그 데이터가 다른 세그먼트에 위치하면 서로 락을 얻기 위해 경쟁하지 않는다.([참고](https://devlog-wjdrbs96.tistory.com/269))
- volatile로 구현된 Node

>  [링크](https://pplenty.tistory.com/17)가 잘 정리되어 있어서 공부했지만 그럼에도 이해가 안되는 부분이 많다. 라이브러리 이해하기는 쉬운 일이 아니구나...

### 인터페이스명 + Impl

인터페이스의 구현체가 하나만 있을 때는 관례상 인터페이스 이름에 Impl을 붙여 작명한다.

### OCP, DIP 위반

우선 객체지향적으로 구현하기 위해 노력한 점을 살펴보자.

- 코드가 전체적으로 역할(인터페이스), 구현(구체 클래스)로 잘 나뉘어졌다.
  - 역할(인터페이스) : MemberService, MemberRepository, OrderService, DiscountPolicy 
  - 구현(구체 클래스) : MemberServiceImpl, MemopryMemberRepository, OrderServiceImpl, FixDiscountPolicy, RateDiscountPolicy
- 다형성을 활용하였다.

그렇다면 뭐가 문제일까? OrderServiceImpl 에서 할인 정책을 바꾼다고 생각해보자.

```java
public class OrderServiceImpl implements OrderService {
// private final DiscountPolicy discountPolicy = new FixDiscountPolicy();
private final DiscountPolicy discountPolicy = new RateDiscountPolicy();
}
```

OrderService에서 코드의 변경이 일어났다! OCP 원칙을 위반한 것이다.

게다가 OrderService에서는 FixDiscountPolicy랑 RateDiscountPolicy의 존재를 알고 있어야 한다. 즉 의존관계이다.

역할(인터페이스)에만 의존해야한다는 DIP 원칙까지 위반하고 있었던 것이다.

MemberSerivceImpl 에서의 상황도 마찬가지이다.

```java
public class MemberServiceImpl implements MemberService {
    private MemberRepository memberRepository = new MemoryMemberRepository(); // 구체 클레스에 의존하고 있다.
}
```

그렇다면 인터페이스에만 의존하도록 만들고 싶은데, 어떻게 해야할까? 

외부에서 구현 객체를 대신 생성하고 주입해주면 된다! 이를 의존성 주입(DI : dependency Injection)이라고 부른다.

### 책임 분리

구현 객체를 생성하고, 연결(주입)하는 책임을 하는 클래스 AppConfig 를 하나 만들자

```java
public class AppConfig {
    public MemberService memberService() {
    	return new MemberServiceImpl(new MemoryMemberRepository());
    }
    public OrderService orderService() {
	    return new OrderServiceImpl(new MemoryMemberRepository(),new FixDiscountPolicy());
    }
}
```

MemberServiceImpl, OrderServiceImpl 에서 더 이상 구체 클래스에 의존하지 않고 생성자를 통해 주입 받을 수 있다.

```java
public class MemberServiceImpl implements MemberService {
    private final MemberRepository memberRepository;

    public MemberServiceImpl(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
}
```



```java
public class OrderServiceImpl implements OrderService{
    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;

    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
}
```

### 역할과 구현을 좀더 명료하게 구분하도록 AppConfig를 리팩토링 할 수 있다.

```java
public class AppConfig {
    public MemberService memberService() {
        return new MemberServiceImpl(memberRepository());
    }

    private MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }

    public OrderService orderService() {
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }

    public DiscountPolicy discountPolicy() {
        return new FixDiscountPolicy();
    }
}
```

new MemoryMemberRepository() 중복도 없애고, 메소드 이름으로 역할이 분명하게 구분되어지기 때문에 역할과 구현이 명료하게 구분된다.

### IoC (Inversion of Control: 제어의 역전)

IoC란 객체의 생성, 생명주기의 관리까지 모든 객체에 대한 제어권이 바뀌었다는 것을 의미한다.

- 클라이언트 구현 객체(OrderServiceImpl, MemberServiceImpl)가 가지고 있던 제어권을 AppConfig로 옮겨주었다.
- 클라이언트 구현 객체들은 자신의 로직만 실행할 뿐, 호출하는 인터페이스를 통해 어떤 구현 객체들(MemberServiceImpl, MemopryMemberRepository, OrderServiceImpl 등)이 생성되고 실행될지 모른다.
- 이렇게 프로그램의 제어 흐름을 직접 제어하는 것이 아니라 외부에서 관리하는 것을 제어의 역전(IOC)이라고 한다.



### 프레임워크 vs 라이브러리

- 내가 작성한 코드를 제어하고, 대신 실행하면 프레임워크이다 (JUnit)
- 내가 작성한 코드를 직접 제어한다면, 라이브러리이다.



### 의존관계 주입 (DI: Denpendency Injection)

- 정적인 클래스 의존관계 
  - import 코드만 보고도 의존관계 파악이 가능하다.(실행이 필요없다!)
  - 하지만 이러한 클래스 의존관계만으로는 실제 어떤 구현 객체가 클라이언트 구현 객체에서 사용될지 알 수 없다.
- 동적인 객체 인스턴스 의존 관계
  - 애플리케이션 실행 시점(런타임)에 실제 생성된 객체 인스턴스의 참조가 연결된 의존 관계이다.
  - 클라이언트 구현 객체(OrderServiceImpl, MemberServiceImpl)의 외부에서 실제 구현 객체(MemberServiceImpl, MemopryMemberRepository, OrderServiceImpl 등)를 생성하고 클라이언트에 전달하여 실제 의존 관계가 연결 되는 것을 의존 관계 주입(DI)라고 한다.
- DI를 사용하면 정적인 클래스 의존관계는 변경하지 않은채, 동적인 객체 인스턴스 의존관계를 쉽게 변경할 수 있다.
- DI를 사용하면 클라이언트 코드를 변경하지 않고 호출하는 대상의 타입 인스턴스를 변경할 수 있다.

### IOC 컨테이너, DI 컨테이너

- AppConfig 처럼 객체를 생성하고 관리하면서 의존관계를 연결해주는 것을 'IoC 컨테이너' 또는 'DI 컨테이너' 라고 한다.

- 최근에는 의존관계 주입에 초점을 맞춰 주로 DI 컨테이너라 한다.
- 어셈블러, 오브젝트 팩토리 등으로 불리기도 한다.

### 스프링 컨테이너

- ApplicationContext 가 스프링 컨테이너이다
- 스프링 컨테이너는 @Configuration이 붙은 클래스(AppConfig)를 설정(구성) 정보로 사용한다.
- AppConfig에서 @Bean이라 붙은 메서드를 모두 호출해서 반환된 객체를 스프링 컨테이너 등록한다. 
- 스프링 컨테이너에 등록된 객체를 스프링 빈이라고 한다.
- 스프링 빈은 기본적으로 메서드의 명을 이름으로 사용하지만, @Bean(name = "beanName") 과 같이 따로 설정해 줄 수 있다.
- 개발자는 스프링 컨테이너의 getBean() 메서드를 통해서 필요한 스프링 빈(객체)을 찾을 수 있다.

## Reference

이 글은 김영한님의 [스프링 핵심 원리 - 기본편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8/dashboard)을 보고 정리해서 작성하였습니다.

[1] [[java] ConcurrentHashMap 동기화 방식](https://pplenty.tistory.com/17)

[2] [[Java] ConcurrentHashMap이란 무엇일까?](https://devlog-wjdrbs96.tistory.com/269)

[3] [동시성에 대해서 생각해보자.](https://happy-coding-day.tistory.com/151)

[4] [자바 volatile 키워드](https://parkcheolu.tistory.com/16)[](https://parkcheolu.tistory.com/16)

