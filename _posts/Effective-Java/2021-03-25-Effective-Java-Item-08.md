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

title: "Effective Java Item 08"
excerpt: "finalizer와 cleaner 사용을 피하라"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Effective Java
tags:
  - [Effective Java, finalizer, cleaner]
date: 2021-03-20
last_modified_at: 2021-03-20
---
# finalizer와 cleaner 사용을 피하라

## finalizer란?
객체가 소멸될 때 호출하기로 약속된 메소드  
자바 9에서는 deprecated 되었으며 cleaner가 도입되었다.  
cleaner도 finalizer보단 다소 낫지만 되도록 사용하지 말자.  

## finalizer 공격을 막는 방법

### 1. 클래스를 final로 선언

상속을 막을 수 있다 (확장성은 포기해야 한다)

### 2. finalize 메소드를 final로 선언

하위 클래스에서 finalize 재사용을 막느다. 하지만 finalizer 객체가 더 오래 살아있게 된다.
*무슨 말인지 정확히 이해가 안간다. finalizer 스레드가 오래 살아 있는 다는 뜻일까?*

### 3. Boolean flag를 사용한다.

이 방법은 작성하기 번거롭고 실수하기 쉽다. 하위 클래스를 통한 공격을 막을 수도 없다.

### 4. 제일 좋은 방법은 객체 생성 전에 예외를 던지는 것이다.

>Object가 생성될 때 JVM은 다음과 같이 동작한다.
>1. 객체를 위한 공간을 할당한다.   
>2. 객체의 모든 인스턴스 변수를 기본값으로 설정. 여기에는 객체의 수퍼 클래스에 있는 인스턴스 변수가 포함된다.   
>3. 객체에 대한 파라미터 변수를 지정한다.   
>4. 명시적 또는 암시적 생성자 호출(생성자에서 this() 또는 super() 호출)을 처리한다.   
>5. 클래스의 변수를 초기화한다.   
>6. 생성자의 나머지 부분을 실행한다.

출처 - [Finalizer attack](https://yangbongsoo.tistory.com/8?category=919799)

파라미터 변수를 지정할 때 예외를 던지게 해주면 된다.

***
참고(Reference)  
[Finalizer attack](https://yangbongsoo.tistory.com/8?category=919799)  
[Finalizer Attack in Java](https://self-learning-java-tutorial.blogspot.com/2020/03/finalizer-attack-in-java.html)