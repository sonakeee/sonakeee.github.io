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

title: "Effective Java Item 11"
excerpt: "equals는 일반 규약을 지켜 재정의하라"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Effective Java
tags:
  - [Effective Java, equals]
date: 2021-04-11
last_modified_at: 2021-04-11
---
# equals는 일반 규약을 지켜 재정의하라

## equals 추이성

> 구체 클래스를 확장해 새로운 값을 추가하면서 equals 규약을 만족시킬 방법은 존재하지 않는다.  
> 상속 대신 컴포지션을 사용하는 것이 괜찮은 우회 방법이다.

### 무한 재귀, StackOverFlowError 위험

ColorPoint 가 Point를 상속하였고 equals 내에 다음과 같은 if 조건이 있다고 하자.
```java
if(!(o instanceof ColorPoint)) return o.equals(this);
```
비슷하게 SmellPoint도 equals내에서
```java
if(!(o instanceof SmellPoint)) return o.equals(this);
```
두 if 조건문은 서로 equals를 호출하며 반복 실행된다.(무한재귀, StackOverFlowError)

***
참고(Reference)  
[Item 10. Equals는 일반 규약을 지켜 재정의하라](https://jaehun2841.github.io/2019/01/10/effective-java-item10/#%EB%A6%AC%EC%8A%A4%EC%BD%94%ED%94%84-%EC%B9%98%ED%99%98-%EC%9B%90%EC%B9%99-solid)  
[Java HashMap은 어떻게 동작하는가?](https://d2.naver.com/helloworld/831311)  
[해쉬 테이블의 이해와 구현 (Hashtable)](https://bcho.tistory.com/1072)
