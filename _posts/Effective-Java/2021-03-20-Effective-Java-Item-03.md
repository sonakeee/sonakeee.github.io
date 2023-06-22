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

title: "Effective Java Item 03"
excerpt: "private 생성자나 열거 타입으로 싱글톤임을 보증하라"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Effective Java
tags:
  - [Effective Java, 싱글톤]
date: 2021-03-20
last_modified_at: 2021-03-20
---

# private 생성자나 열거 타입으로 싱글톤임을 보증하라

## 싱글톤 인스턴스를 가짜 구현으로 대체하기 힘들다.

### 1. 생성자가 private이기 때문에 상속할 수 없고 테스트에서 싱글톤 인스턴스의 생성을 통제할 수 없다.
### 2. getInstance 메소드의 경우 static 이기 때문에 가짜 객체를 싱글톤 대신 주입하기 힘들다.
mocking frameworks는 상속(Inheritance)과 다형성(polymorphism)을 기반으로 하고 있는데 2가지 모두 문제가 된다.  
`사실 다형성이 왜 문제가 되는지는 아직 이해가 안된다... `  
 [setter 메소드를 추가해서 테스트 ](https://blog.jayway.com/2010/01/15/learn-to-stop-worrying-and-love-the-singleton/) 가능하게 하거나
[interception과 AOP concept에 기반한 최신 mocking framework ](http://www.weblogism.com/item/254/mocking-static-method-calls) 를 이용하는 방법이 있겠다.   
[PowerMock](https://github.com/powermock/powermock) 을 쓰는 것도 하나의 방법이다.  
(PowerMock을 쓰면 일반적으로 mocking이 힘든 static, fianl, private 메소드 mocking이 가능하다)

여기서는 인터페이스를 구현하는 싱글톤에 대해서 자세히 알아보자.  
```java
interface Singleton {
    private static final myInstance;
    public static Singleton getInstance() { return myInstance; }
    public static void setInstance(Singleton newInstance) { myInstance = newInstance; }
    // public method declarations
}

// Used in production
class RealSingleton implements Singleton {
    // public methods
}

// Used in unit tests
class FakeSingleton implements Singleton {
    // public methods
}

class ClientTest {
    private Singleton testSingleton = new FakeSingleton();
    @Test
    public void test() {
        Singleton.setSingleton(testSingleton);
        client.doSomething();
        // ...
    }
}
```
위 코드는 'Working Effectively With Legacy Code'책에서 "Introduce Static Setter"라고 불리는 방법이다.

인터페이스를 구현하면 다음과 같은 관점에서도 테스트하기 용이하다.

이런 코드 대신에
```java
void foo() {
   Bar bar = Bar.getInstance();
   // etc...
}
```
인터페이스를 구현하면 이렇게 쓸 수 있다.
```java
void foo(IBar bar) {
   // etc...
}
```
가짜 bar 객체를 통해서 foo 메소드를 테스트할 수 있게 된것이다.
다시 말하면, 의존성을 제거해서 bar를 테스트 하지 않고 foo를 테스트 할 수 있게 된 것이다.

***
참고(Reference)  
[mocking-a-singleton-class](https://stackoverflow.com/questions/2302179/mocking-a-singleton-class)  
[advantage-of-singleton-implementing-an-interface](https://stackoverflow.com/questions/17988251/advantage-of-singleton-implementing-an-interface)  
[why-doesnt-mockito-mock-static-methods](https://stackoverflow.com/questions/4482315/why-doesnt-mockito-mock-static-methods)  
[static method와 Override hiding 대한 정리](https://wedul.site/457)  
[static method를 mocking 하기](https://roybatty.tistory.com/11)  
[스태틱 클래스 테스트하기](https://sungminoh.github.io/posts/development/java-mock-static-method/)
