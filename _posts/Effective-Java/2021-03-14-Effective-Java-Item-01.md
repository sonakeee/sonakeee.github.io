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

title: "Effective Java Item 01"
excerpt: "생성자 대신 정적 팩터리 메서드를 고려하라"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Effective Java
tags:
  - [Effective Java, 정적 팩터리 메소드]
date: 2021-03-14
last_modified_at: 2021-03-29
---

# 생성자 대신 정적 팩터리 메서드를 고려하라  

## 인스턴스 통제는 플라이웨이트 패턴의 근간이 된다.

플라이 웨이트 패턴이란 **공유를 통해 대량의 객체들을 효과적으로 다루는 패턴**이다.   
생성되는 객체의 수가 많을수록, 사용되는 회수가 많을수록, 메모리에 오래 올라가 있을수록 사용하기 적합하다.  
HashMap<key, 객체> 하나를 팩토리 클래스가 들고 있어 기존에 생성된 객체를 재활용한다고 생각하면 되겠다.  
대표적으로 플라이웨이트 패턴이 사용되는 예시로는 자바의 모든 래퍼 클래스의 valueOf() 메소드와 String Pool이 있다.

>java 6 이하에서는 PermGen 영역에 String Pool을 저장해 놓았었다.   
>하지만 PermGen영역은 Runtime에 확장될 수 없는 고정된 capacity(32M~96M) 를 가지고 있기 때문에 
> String.intern()이 많이 호출되면 OutOfMemoryException가 발생할 확률이 높았었다.   
> 그래서 java7부터 main heap space에 저장 하게 변경되었다.  
> Perm 영역은 Java8을 기점으로 완전히 사라졌다.  

[참고 글]  
[자바의 String Pool](https://www.nakjunizm.com/2017/07/25/String_Pool/), [JDK 8에서 Perm 영역은 왜 삭제됐을까](https://johngrib.github.io/wiki/java8-why-permgen-removed/),   
[JVM Heap 영역](https://swiftymind.tistory.com/112), [Java Garbage Collection](https://d2.naver.com/helloworld/1329)


## 정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.

이런 유연함은 서비스 제공자 프레임워크(service provider framework)를 만드는 근간이 된다.  
대표적인 서비스 제공자 프레임워크인 JDBC를 예로 들자면 DriverManager.getConnection 메소드를 작성하는 시점에는 Connection 인터페이스를 구현한 클래스가 
존재하지 않아도 된다는 것이다.  
클라이언트는 어떤 제공자(Mysql, Oracle, SqlServer - 서비스의 구현체)가 사용되건 상관없이 Driver, connection 서비스 인터페이스를 통해 구현체를 사용할 수 있다.   
각각의 DBMS의 벤더사에서 Driver, Connection 인터페이스를 구현하여 제공해줄 것이다.  
**즉 Driver, connection 인터페이스와 실제 그 인터페이스를 구현하는 구현체 클래스가 완전히 분리되어 제공되는 것이다.**  

***서비스 제공자 프레임워크****의 구성요소 ex) JDBC*  

*구현체를 나타내는(represents implementations, 구현체의 동작을 정의하는) ***서비스 인터페이스***  
ex) Connection  
제공자가 구현체를 등록할 때 사용하는 ***제공자 등록 API***   
ex) DriverManager.registerDriver  
클라이언트가 서비스의 인스턴스를 얻을 때 사용하는 ***서비스 접근 API***  
ex) DriverManager.getConnection  
서비스 인터페이스의 인스턴스를 생성하는 팩터리 객체(DriverManager)를 설명해주는 ***서비스 제공자 인터페이스***  
ex) Driver*

사족) Class.forName(각각의 벤더사에서 Driver인터페이스를 구현한 구현체 클래스)를 통해  
클래스를 로드하고 등록해준다. 등록하는 과정은 클래스를 로드할 때 자동으로 실행되는 static 블록에서  
DriverManager.registerDriver를 통해 처리된다.

>브리지 패턴과 의존 객체 주입(DI)가 서비스 제공자 프레임워크 패턴의 변형이라고 한다  
이해가 잘 안되서 나중에 공부해봐야겠다

___
참고(Reference)

[[구조 패턴] 플라이웨이트 패턴(Flyweight Pattern) 이해 및 예제](https://readystory.tistory.com/137)  
[이펙티브 자바 01. 정적 팩토리 메소드와 서비스 제공자 인터페이스 (JDBC 예제)](https://plposer.tistory.com/61)  
[[Java] Class.forName(String className) 그리고 Service Provider Interface](https://devyongsik.tistory.com/294)  
[[Java 궁금증] Class.forName()은 어떻게 동작할까?](https://kyun2.tistory.com/23)  
[클래스로더 1, 동적인 클래스 로딩과 클래스로더](https://javacan.tistory.com/entry/1)  
[SPI와 API 차이](https://m.blog.naver.com/PostView.nhn?blogId=miniwikibook&logNo=60164835890&proxyReferer=https:%2F%2Fwww.google.com%2F)