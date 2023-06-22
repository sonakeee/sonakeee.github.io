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

title: "Effective Java Item 05"
excerpt: "자원을 직접 명시하지 말고 의존 객체 주입을 사용하라"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Effective Java
tags:
  - [Effective Java, 의존 객체 주입]
date: 2021-03-21
last_modified_at: 2021-04-03
---

# 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라

## 필요한 자원(or 팩토리)를 생성자(or 정적 팩토리, 빌더)에 넘겨주자

멀티스레드 환경에서 안전하게 공유할 수 있으면서 (final을 유지하면서) 여러 자원을 사용할 수 있다.  
특히 Supplier<T> 인터페이스가 팩터리의 대표적인 예이다.  
팩토리란 호출할 때마다 특정 타입의 인스턴스를 반복해서 만들어주는 객체를 말한다.  

책과는 조금 다르게 일반적으로 `Map< String, Supplier<T> >` 형식으로 supplier <T>를 사용해주어 
팩토리 패턴을 구현해주는 것 같다.

***
참고(Reference)  
[what-is-the-most-effective-way-of-writing-a-factory-method](https://stackoverflow.com/questions/23126207/what-is-the-most-effective-way-of-writing-a-factory-method)  
[Java/자바 - Supplier<T> interface](https://m.blog.naver.com/zzang9ha/222087025042)    
[생성 패턴 팩토리 패턴(Factory Pattern) 이해 및 예제](https://readystory.tistory.com/117)